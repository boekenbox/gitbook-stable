---
description: Code your own trading strategies using JavaScript
---

# Custom strategies

Build completely custom trading strategies, using [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript). For spot and futures trading.

Use built-in trading methods, candle and indicator data to get started quickly.   
For advanced usage it's possible to install and use your own additional javascript modules.

A strategy runs as an async function, executed once every time a trading pair 'cycles' in Gunbot.  


{% hint style="success" %}
This is available in Gunbot Ultimate.
{% endhint %}

## Technical info

To get a better idea of what you can and can't do, here's a few facts:

* Gunbot takes care of the exchange connection, collecting data and making it available to a strategy
* You save strategies in your own .js files inside the Gunbot root folder
* There is no Gunbot specific syntax, strategy code is plain JavaScript
* Strategy code has access to a single outside object: `gb`, which contains a number of trading methods and lots of data to work with
* There are basically no restrictions to what you can do with the provided trading methods, but of course exchange defined restrictions apply \(API usage rate limits, open orders limits, etc.\)
* There are no protections against double orders, or any kind of other unwanted behavior
* Make sure to use JavaScript code compatible with [Node v14.4.0](https://nodejs.org/ca/blog/release/v14.4.0/)
* Using console.log\(\) logs to the main Gunbot console logs. Expect to console logs all the things
* Code runs inside an async function, you can use the `await`keyword to collect additional data like candle data for a different period
* A strategy does not have to return anything, but you can use return statements to easily stop your code at a certain point
* A strategy can use custom JavaScript modules. Save these in a folder named user\_modules inside your Gunbot folder.

{% hint style="danger" %}
**Custom code can be very risky!**

Because external modules can be used, be extremely careful when running strategy code you did not write yourself. In theory a malicious actor can gain access over your whole system.
{% endhint %}

## Strategy examples

Coding a strategy can be as difficult as you want it to be.   
Even with [very ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)[basic ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)[JavaScript ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#comparison_operators)[code](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements#declarations) it's possible to create complex strategies. 

The following examples show a few possible ways to code simple strategies. They are silly in functionality, and are not usable for actual trading. Each next example requires a bit more JavaScript knowledge.

{% hint style="warning" %}
Some examples can lead to immediate large trades. Use them to learn, not to trade.
{% endhint %}

### Example 1

* Trade USDT-BTC
* Invest 100 USDT per trade, allow max 1 buy order
* Market buy when price is below lowBB and fastSma is below ema1 \(using readily available indicator data\)
* After the buy order fills, place a sell order 1% above the break even price 
* Do nothing until this sell order fills

```javascript
// check if there are any open orders for this pair, 
// wait until they get filled before doing anything else
if (gb.data.openOrders.length > 0){
    // print a message to the console logs
    console.log('There is an open order, waiting...')
    // stop doing things
    return
}


// balance "settings": invest 100 USDT, convert into quote amount 
// needed for calling the buy method
var baseAmount = 100
var buyAmount = baseAmount / gb.data.bid


// check if no bag is held yet AND price, 
// lowBB and moving average criteria are true
if (
    !gb.data.gotBag &&
    gb.data.bid < gb.data.lowBB &&
    gb.data.fastSma < gb.data.ema1
    ){
    // conditions are true, fire a market buy order 
    // using the amount calculated in buyAmount
    gb.method.buyMarket(buyAmount, gb.data.pairName)
}
// place a limit sell order if we have a bag
else if ( gb.data.gotBag ) {
    // fire the sell order, pass in the amount, price and pair name. 
    // price is calculated as breakEven * 1.01
    gb.method.sellLimit(gb.data.quoteBalance, gb.data.breakEven * 1.01, gb.data.pairName)
}


```

### Example 2

* Keep the basic workings from example 1
* Additionally calculate a Hull Moving Average \(HMA\) using the last 50 candle close prices as input
* Place the sell order 1% above bid price, at the moment bid goes above HMA

```javascript
// check if there are any open orders for this pair, 
// wait until they get filled before doing anything else
if (gb.data.openOrders.length > 0) {
    // print a message to the console logs
    console.log('There is an open order, waiting...')
    // stop doing things
    return
}


// balance "settings": invest 100 USDT, convert into 
// quote amount needed for calling the buy method
var baseAmount = 100
var buyAmount = baseAmount / gb.data.bid

// define a variable to save the Hull Moving Average to
var hma

// get indicator data from tulind
// use candlesClose data as input, calculate a 50 period Hull Moving Average
gb.method.tulind.indicators.hma.indicator([gb.data.candlesClose], [50], function (err, results) {

    // tulind returns an array with hma values, 
    // assign the most recent value to the hma variable
    // notation is a bit awkward, the ".length -1" part 
    // makes it select the very last value
    hma = results[0][results[0].length - 1]
});


// check if no bag is held yet AND price, 
// lowBB and moving average criteria are true
if (
    !gb.data.gotBag &&
    gb.data.bid < gb.data.lowBB &&
    gb.data.fastSma < gb.data.ema1
) {
    // conditions are true, fire a market buy order 
    // using the amount calculated in buyAmount
    gb.method.buyMarket(buyAmount, gb.data.pairName)
}
// place a limit sell order if we have a bag
// make sure the sell method is only called when bid > hma
else if (gb.data.gotBag && gb.data.bid > hma) {
    // fire the sell order, pass in the amount, price and pair name. 
    // price is calculated as bid * 1.01
    gb.method.sellLimit(gb.data.quoteBalance, gb.data.bid * 1.01, gb.data.pairName)
}


```

### Example 3

* Trade USDT-BTC
* Invest 100 USDT per trade, allow max 1 buy order
* Code a bit more readable, assigning variables for everything used in conditionals
* Buy when a 50 period ema has crossed over a 200 period ema, AND within at most 2 candles later the candle open price is above ema1
* Place a sell order 5% above break even

```javascript
// check if there are any open orders for this pair, 
//wait until they get filled before doing anything else
if (gb.data.openOrders.length > 0) {
    // print a message to the console logs
    console.log('There is an open order, waiting...')
    // stop doing things
    return
}


// balance "settings": invest 100 USDT, convert into 
// quote amount needed for calling the buy method
var baseAmount = 100
var buyAmount = baseAmount / gb.data.bid


// needed variables for conditional logic
// some are left undefined, to set them later when the values can be calculated
// emaCrossup is intentionally set false, 
// only to be set true when a recent crossup is detected
var gotBag = gb.data.gotBag
var bid = gb.data.bid
var ema50
var ema200
var emaCrossup = false


// get indicator data from tulind
// use candlesClose data as input, calculate a 50
// and 200 period Exponential Moving Average
gb.method.tulind.indicators.ema.indicator([gb.data.candlesClose], [50], function (err, results) {

    // tulind returns an array with ema value, 
    // save full array to be able to look for crosses later
    ema50 = results[0]
});

gb.method.tulind.indicators.ema.indicator([gb.data.candlesClose], [200], function (err, results) {

    // tulind returns an array with ema value, 
    // save full array to be able to look for crosses later
    ema200 = results[0]
});


// figure out if there was a crossover of ema50 > ema200 in the last two candles
// do this in two conditionals, once for the most recent candle 
// and once for the previous candle
// if true, save the result in the emaCrossup variable
if (
    ema50[ema50.length - 1] > ema200[ema200.length - 1] &&
    ema50[ema50.length - 2] < ema200[ema200.length - 2]
) {
    emaCrossup = true
}
else if (
    ema50[ema50.length - 2] > ema200[ema200.length - 2] &&
    ema50[ema50.length - 3] < ema200[ema200.length - 3]
) {
    emaCrossup = true
}


// buy if there is no bag, and there was a recent ema crossup 
// and bid price is above ema50
if (!gotBag && bid > ema50[ema50.length - 1] && emaCrossup) {
    // conditions are true, fire a market buy order using the amount calculated in buyAmount
    gb.method.buyMarket(buyAmount, gb.data.pairName)
}
// place a limit sell order if we have a bag
// make sure the sell method is only called when 
else if (gotBag) {
    // fire the sell order, pass in the amount, price and pair name. Price is calculated as bid * 1.01
    gb.method.sellLimit(gb.data.quoteBalance, gb.data.breakEven * 1.05, gb.data.pairName)
}


```

### Example 4

* Trade USDT-BTC and USDT-XRP
* Assume you already have BTC and XRP
* Sell 1 BTC when news about BTC tagged "bearish" appears on cryptopanic.com, containing the keyword "Korea", then place a buy order 5% below the current price 
* Sell 10000 XRP whenever news about XRP tagged "bearish" appears with the keyword "sec"
* Do all of this in a single strategy running on a single pair in Gunbot

```javascript
// require the Cryptopanic module
// install https://www.npmjs.com/package/cryptopanic in an outside folder, 
// then copy the modules to the 'user_modules' folder inside the Gunbot root folder
gb.method.requirejs(['Cryptopanic'],
    function (Cryptopanic) {

        // make a connection to Cryptopanic
        const cp = new Cryptopanic({ auth_token: '<API_TOKEN>' })

        // creata variables to store status of detected FUD
        let koreaFud
        let secFud

        // get data from Cryptopanic and do a simple check if any of the BTC news includes 'Korea', and if there is XRP news that included 'SEC'
        // this is not a complete solution, for example it does not consider if news is old or has already been traded
        cp.currencies(['BTC', 'XRP'])
            .filter('bearish')
            .fetchPosts()
            .then((articles) => {

                // loop trough articles, search if specific strings exist and save result
                articles.forEach(article => {
                    // convert article object to string to easily search its contents
                    const articleString = JSON.stringify(article)

                    if (articleString.indexOf('Korea') > -1 && articleString.indexOf('BTC') > -1) {
                        koreaFud = true
                    }

                    if (articleString.indexOf('SEC') > -1 && articleString.indexOf('XRP') > -1) {
                        secFud = true
                    }
                });

                // fire BTC orders if Korea FUD detected
                if (koreaFud) {
                    // first sell 1 BTC in a market order
                    gb.method.sellMarket(1, 'USDT-BTC')
                        // wait for the promise to resolve, then check if the order filled before placing a limit buy 5% below current price
                        .then((result) => {
                            if (result.info.status === 'FILLED') {
                                gb.method.buyLimit(1, gb.data.bid * 0.95, 'USDT-BTC')
                            }
                        })
                }

                // sell XRP if SEC FUD detected
                if (secFud) {
                    gb.method.sellMarket(10000, 'USDT-XRP')
                }

            })
    })
```

### Example 5

* Trade USDT-BTC
* Invest 100 USDT per trade, allow max 1 buy order
* Strategy itself uses 3m candles, additionally fetch 60m candles
* Only collect data and allow trades in first minute of the hour
* Calculate TD Sequential using an external module, transform input data to the needed format
* Place buy order when bid price &gt; ema1, using 3m data AND TD Sequential on hourly chart has a buy setup index of 8 or higher
* Sell when TD sequential has a sell setup index of at least 11 and price is above break even
* Use the lodash module to simplify some code

```javascript
// don't do anything unless current minute is first of the hour
const currentMinute = new Date(Date.now()).getMinutes();
if (currentMinute != 0) {
    return
}

// get 300 60m candles, and persistently save them 
// to Gunbot pair ledger - for no real reason except showing that you can do this
gb.data.pairLedger.my60mCandles = await gb.method.getCandles(300, 60, gb.data.pairName)

// balance "settings": invest 100 USDT, convert into quote amount needed for calling the buy method
const baseAmount = 100
const buyAmount = baseAmount / gb.data.bid

// the rest of the code depends on two external modules: 'lodash' and 'tdsequential'
// install them with npm in an outside folder, and copy the modules to the 'user_modules' folder inside the Gunbot root folder
gb.method.requirejs(['lodash', 'tdsequential'],
    function (_, TDSequential) {

        // to calculate TD Sequential, first transform the collected candle data to the format required by this module
        let candles_60m_transformed = []
        gb.data.pairLedger.my60mCandles.close.forEach(function (item, index) {
            let temp = {}
            temp.time = item.timestamp
            temp.close = item.value
            temp.high = gb.data.pairLedger.my60mCandles.high[index].value
            temp.low = gb.data.pairLedger.my60mCandles.low[index].value
            temp.open = gb.data.pairLedger.my60mCandles.open[index].value
            temp.volume = gb.data.pairLedger.my60mCandles.volume[index].value
            candles_60m_transformed.push(temp)
        });

        // calculate TD Sequential and only store the most recent value
        const tdSequentialData = _.last(TDSequential(candles_60m_transformed))

        // define trading conditions
        const buyConditions = tdSequentialData.buySetupIndex >= 8 && gb.data.bid > gb.data.ema1 && !gb.data.gotBag
        const sellConditions = tdSequentialData.sellSetupIndex >= 11 && gb.data.bid > gb.data.breakEven && gb.data.gotBag

        // place orders if conditions are true
        if (buyConditions) {
            gb.method.buyMarket(buyAmount, gb.data.pairName)
        }
        else if (sellConditions) {
            gb.method.sellMarket(gb.data.quoteBalance, gb.data.pairName)
        }

    })
```

## How to create and run a custom strategy

To create a new custom strategy, use custom as buy and sell method:

![](../.gitbook/assets/image%20%28123%29.png)

Then specify the filename with your strategy logic and place that file with your strategy logic in your Gunbot root folder - next to the executable file

![](../.gitbook/assets/image%20%28122%29.png)

The data tab gives you control over how pre calculated indicator data is generated. These are indicators you can access in your strategy. You are not limited to using these indicators, this is just easily accessible data.

![](../.gitbook/assets/image%20%28121%29.png)

To run the strategy, simply assign it to a trading pair like you would with any other Gunbot strategy.

{% hint style="info" %}
It's recommended to disable the global cancel orders option when running a custom strategy.
{% endhint %}

## Trading methods

A strategy can use a number of methods to place or cancel orders, or fetch additional OHLCV data.

Trading methods can be called as function, without waiting for it's return value.

Alternatively, when calling a method you can wait for its [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) to resolve. This way you can create a promise chain using the [then\(\)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) method.

#### Built in modules

The requirejs module allow you to use additional javascript modules, like those freely available on NPM.

With [tulind](https://www.npmjs.com/package/tulind) you can easily calculate your own indicator values.



<table>
  <thead>
    <tr>
      <th style="text-align:left">Method</th>
      <th style="text-align:left">Returns</th>
      <th style="text-align:left">Extra info</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>gb.method.buyMarket(amount, pair)</code>
      </td>
      <td style="text-align:left"><code>Promise</code>
      </td>
      <td style="text-align:left">Places a market buy order, amount in quote</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.method.sellMarket(amount, pair)</code>
      </td>
      <td style="text-align:left"><code>Promise</code>
      </td>
      <td style="text-align:left">Places a market sell order, amount in quote</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.method.buyLimit(amount, price, pair)</code>
      </td>
      <td style="text-align:left"><code>Promise</code>
      </td>
      <td style="text-align:left">Places a limit buy order, amount in quote</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.method.sellLimit(amount, price, pair)</code>
      </td>
      <td style="text-align:left"><code>Promise</code>
      </td>
      <td style="text-align:left">Places a limit sell order, amount in quote</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.method.closeMarket(pair, amount)</code>
      </td>
      <td style="text-align:left"><code>Promise</code>
      </td>
      <td style="text-align:left">Places a market close order, for futures. Amount in quote.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.method.closeLimit(price, pair, amount)</code>
      </td>
      <td style="text-align:left"><code>Promise</code>
      </td>
      <td style="text-align:left">
        <p>Places a limit close order, for futures.</p>
        <p>Amount in quote.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.method.cancelOrder(orderId, pair)</code>
      </td>
      <td style="text-align:left"><code>Promise</code>
      </td>
      <td style="text-align:left">Cancels a specified open order.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.method.getCandles(interval, period, pair)</code>
      </td>
      <td style="text-align:left"><code>Promise</code>
      </td>
      <td style="text-align:left">
        <p>Get additional OHLCV data.</p>
        <p></p>
        <p>Easiest to call using <code>await</code>, see usage example in the example
          strategies.</p>
        <p></p>
        <p>Beware: frequent usage can quickly lead to exceeding exchange API rate
          limits.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.method.tulind [#parameters#]</code>
      </td>
      <td style="text-align:left"><code>array</code>
      </td>
      <td style="text-align:left">
        <p><a href="https://www.npmjs.com/package/tulind">Get </a>100+ different
          indicators. You can use the available OHLCV data, or use your own inputs.</p>
        <p></p>
        <p>See the example strategies for a usage example.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gunbot.method.requirejs([&apos;#module#&apos;], function (moduleName) {</code>
      </td>
      <td style="text-align:left"><code>function</code>
      </td>
      <td style="text-align:left">
        <p>Bring your own modules using <a href="https://www.npmjs.com/package/requirejs">requirejs</a>.</p>
        <p></p>
        <p>See the example strategies for a usage example.</p>
      </td>
    </tr>
  </tbody>
</table>



## Available data

A strategy can use all available pair data in Gunbot, easily accessible with unique property names.

Indicators mentioned in the list below are pre calculated using the indicator settings in your Gunbot strategy. Some indicators don't have configurable strategy parameters and just use the generally accepted standard inputs for the indicator.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property</th>
      <th style="text-align:left">Returns</th>
      <th style="text-align:left">Extra info</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>gb.data.pairLedger</code>
      </td>
      <td style="text-align:left"><code>object</code>
      </td>
      <td style="text-align:left">
        <p>Contains the complete pair ledger.</p>
        <p></p>
        <p>Can also be used to store and access your own persistent variables. Make
          sure to not overwrite existing properties.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.pairName</code>
      </td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">Current pair name, like <code>USDT-BTC</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.exchangeName</code>
      </td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">Current exchange name, like <code>binance</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.balances</code>
      </td>
      <td style="text-align:left"><code>object</code>
      </td>
      <td style="text-align:left">Object with all balances on same exchange</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.quoteBalance</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">
        <p>Pair quote balance, like BTC balance on an USDT-BTC pair</p>
        <p></p>
        <p>Specific to spot trading</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.baseBalance</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Base balance, like USDT balance on an USDT-BTC pair</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.onOrdersBalance</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Quote balance value of open orders for current pair</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.openOrders</code>
      </td>
      <td style="text-align:left"><code>array</code>
      </td>
      <td style="text-align:left">Open orders for current pair</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.orders</code>
      </td>
      <td style="text-align:left"><code>array</code>
      </td>
      <td style="text-align:left">History of filled orders for current pair</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.orderbook</code>
      </td>
      <td style="text-align:left"><code>object</code>
      </td>
      <td style="text-align:left">First 10 bid and ask entries in orderbook, price and volume</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.breakEven</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Break even price for current spot holdings. Includes trading fees as defined
        in Gunbot exchange settings</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.gotBag</code>
      </td>
      <td style="text-align:left"><code>boolean</code>
      </td>
      <td style="text-align:left">Indicates if value of <code>quoteBalance</code>for spot trading (including
        on orders volume) exceeds min volume to sell as defined in Gunbot strategy</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.leverage</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Leverage of current futures position</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.walletBalance</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Wallet balance. Specific to futures</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.availableMargin</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Available margin balance. Specific to futures</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.maintenanceMargin</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Maintenance margin requirement for futures account</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.maxNotionalValue</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Max notional value for futures position at current leverage</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.totalPositionInitialMargin</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Total initial position margin requirement for all current futures positions</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.totalOpenOrderInitialMargin</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Total initial margin requirement for open futures orders</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.currentQty</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Futures position size</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.currentSide</code>
      </td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>Indicates current futures position side
          <br />
          <br />Possible values: <code>long</code>, <code>short</code>, <code>none</code>
        </p>
        <p></p>
        <p></p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.liquidationPrice</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Futures position liquidation price</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.period</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">
        <p>Candle size as set in Gunbot strategy PERIOD setting.</p>
        <p></p>
        <p>This period is used for all pre calculated indicator values</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.bid</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Highest bid price</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.ask</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Lowest ask price</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.ema1</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.ema2</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.ema3</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.BTCUSDprice</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest BTC/USD price on same exchange (or equivalent spot pair, depending
        on availability)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.mfi</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.rsi</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.lowBB</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.highBB</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.tenkan</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.chikou</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.kijun</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.senkouSpanA</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.senkouSpanB</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.fastSma</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.slowSma</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.R1</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.S1</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.R2</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.S2</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.macd</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.macdSignal</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.macdHistogram</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.stochK</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.stochD</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.stochRsi</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.atr</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.fib</code>
      </td>
      <td style="text-align:left"><code>object</code>
      </td>
      <td style="text-align:left">Object with high and low fibonacci retracement levels</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.vwma</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.diPlus</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.diMinus</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.adx</code>
      </td>
      <td style="text-align:left"><code>float</code>
      </td>
      <td style="text-align:left">Latest indicator value</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.candlesOpen</code>
      </td>
      <td style="text-align:left"><code>array</code>
      </td>
      <td style="text-align:left">
        <p>Array of open values for OHLCV</p>
        <p></p>
        <p>Sorted old to new (newest last)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.candlesHigh</code>
      </td>
      <td style="text-align:left"><code>array</code>
      </td>
      <td style="text-align:left">
        <p>Array of high values for OHLCV</p>
        <p></p>
        <p>Sorted old to new (newest last)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.candlesLow</code>
      </td>
      <td style="text-align:left"><code>array</code>
      </td>
      <td style="text-align:left">
        <p>Array of low values for OHLCV</p>
        <p></p>
        <p>Sorted old to new (newest last)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.candlesClose</code>
      </td>
      <td style="text-align:left"><code>array</code>
      </td>
      <td style="text-align:left">
        <p>Array of close values for OHLCV</p>
        <p></p>
        <p>Sorted old to new (newest last)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.candlesVolume</code>
      </td>
      <td style="text-align:left"><code>array</code>
      </td>
      <td style="text-align:left">
        <p>Array of volume values for OHLCV</p>
        <p></p>
        <p>Sorted old to new (newest last)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.candlesTimestamp</code>
      </td>
      <td style="text-align:left"><code>array</code>
      </td>
      <td style="text-align:left">
        <p>Array of timestamp values for OHLCV</p>
        <p></p>
        <p>Sorted old to new (newest last)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>gb.data.candles</code>
      </td>
      <td style="text-align:left"><code>object</code>
      </td>
      <td style="text-align:left">Combined object with OHLCV data</td>
    </tr>
  </tbody>
</table>







