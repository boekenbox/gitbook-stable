# Critical errors - Критические ошибки

Когда есть проблема, которая может потребовать внимания, Gunbot обычно показывает уведомление об ошибке в графическом интерфейсе. Некоторые ошибки видны только в окне консоли с запущенным Gunbot.

В целях устранения неполадок последние журналы хранятся в папке / logs. После того, как Gunbot закрыт, файлы журналов можно читать с помощью текстового редактора, такого как Notepad ++.

На этой странице описаны общие проблемы и их возможные решения.

{% hint style="success" %}
**Советы по устранению неполадок**

Всегда полностью закрывайте и перезапускайте Gunbot, прежде чем пытаться решить критическую проблему.

Перезагрузите компьютер, если вы не уверены, полностью ли вы закрыли Gunbot.

При разрешении проблемы, которая может быть связана с ограничением скорости API, прекратите запуск Gunbot как минимум на 30 минут, прежде чем пытаться решить проблему.
{% endhint %}

## Ошибки при запуске Gunbot <a id="errors-when-starting-gunbot"></a>

Следующие ошибки являются **критическими** и будут препятствовать торговле, они указывают, что Gunbot не мог должным образом начать или соединиться с обменом.

**Причина**: главный ключ не зарегистрирован

**Возможные решения:**

* Дважды проверьте правильность вашего мастер-ключа и секрета и не начинайте и не заканчивайте пробелом. 
* Убедитесь, что вы используете правильную версию Gunbot. 
* Если проблема не устранена: обратитесь к продавцу.

### Невозможно подключиться к обмену

**Причина**: различная, чаще всего проблема с синхронизацией времени или именованием пар.

**Возможные решения:**

* Дважды проверьте правильность вашего мастер-ключа и секрета и не начинайте и не заканчивайте пробелом. 
* Проверьте, синхронизируются ли ваши системные часы с сервером времени в Интернете. [Инструмент для Windows](http://www.timesynctool.com/) / [Информация для Linux](https://www.howtogeek.com/tips/how-to-sync-your-linux-server-time-with-network-time-servers-ntp) 
* Убедитесь, что в вашей конфигурации присутствуют действительный главный ключ и ключ. 
* Убедитесь, что ваши пары действительно существуют на бирже и соответствуют синтаксису пар Gunbot. 
* Увеличьте задержку обмена, обмен может ограничить вас. Остановите Gunbot и подождите не менее 30 минут перед повторной попыткой. 
* Убедитесь, что все файлы Gunbot были правильно разархивированы. 
* Убедитесь, что прослушиватель IMAP отключен \(когда не используется надстройка TradingView\). 
* Попробуйте проводное подключение к интернету. 
* Перезагрузите маршрутизатор и / или очистите кэш DNS.

### Неверный ответ от сервера лицензий <a id="invalid-response-from-license-server"></a>

**Причина**: обычно проблема в сети

**Возможные решения:**

* Попробуйте проводное подключение к интернету. 
* Перезагрузите маршрутизатор и / или очистите кэш DNS.

### Ошибка: bind EADDRINUSE null: 5000 <a id="error-bind-eaddrinuse-null-5000"></a>

**Причина**: другой процесс уже использует указанный номер порта

**Возможные решения:**

* Перезагрузите компьютер. 
* Когда другому программному обеспечению требуется использовать любой из портов, необходимых для Gunbot, измените порты, которые Gunbot использует в файле config.js.

### Больше не могу получить доступ к GUI <a id="cant-access-gui-anymore"></a>

**Причина**: ошибка конфигурации, препятствующая правильной загрузке ядра

**Решение**:

* Установите `start`false в разделе GUI файла config.js с помощью текстового редактора, такого как Notepad ++. Сохраните файл, и вы сможете снова подключиться к графическому интерфейсу.

### SSL\_ERROR\_RX\_RECORD\_TOO\_LONG <a id="ssl_error_rx_record_too_long"></a>

**Reason:** https not enabled and the browser forces an https connection

**Possible solutions:**

* Enable `https` in the config.js file.
* Visit Gunbot via http, instead of https.

### Invalid signature <a id="invalid-signature"></a>

**Reason:** API issue

**Possible solutions:**

* Double check your API secret.
* Make sure your API key is activated on CEX.
* Make sure you filled in your ClienID, passphrase or password in the exchange connection settings.

## Errors after successfully starting Gunbot <a id="errors-after-successfully-starting-gunbot"></a>

The following orders can prevent trading when occurring very often. When any of these errors only occur intermittently, there is usually no issue that needs solving.

### Received broken open orders info: retrying again <a id="received-broken-open-orders-info-retrying-again"></a>

**Reason:** Exchange did not send requested order data

**Possible solutions:**

* Ensure proper internet connectivity.
* Increase exchange delay.

### Fetching OHLC again <a id="fetching-ohlc-again"></a>

**Reason:** There is not enough trading volume to receive the number of requested candles

**Possible solutions:**

* Try a different setting for `PERIOD`
* Try other trading pairs with more volume.

### WARNING: BUY order from more than 30 days ago. <a id="warning-buy-order-from-more-than-30-days-ago"></a>

**Reason:** Bought price for asset is unknown or it's impossible to sell an asset due to a wrongly configured `MIN_VOLUME_TO_SELL`

**Possible solutions:**

* Add `BOUGHT_PRICE` as an override for your pair.
* Correct the `MIN_VOLUME_TO_SELL` value for your pair.

### TypeError: Cannot read x of undefined <a id="typeerror-cannot-read-x-of-undefined"></a>

**Reason:** Problem with one or your pairs

**Possible solutions:**

* Make sure that you have not accidentally removed one of your exchanges and still have pairs connected to it.
* Check if all of the pairs in your setup actually exist on your exchange and conform to Gunbot pair syntax \(BASE-QUOTE\).
* Wait until the exchange finishes possible maintenance.

### Poloniex: 422 <a id="poloniex-422"></a>

**Reason:** Exceeding API rate limit

**Possible solution:**

* Increase the exchange delay for Poloniex.

​

### Stop trying to access exchanges you don't have a license to <a id="stop-trying-to-access-exchanges-you-dont-have-a-license-to"></a>

**Reason:** Gunbot core is not running on an already registered key

**Solution**:

1. Stop Gunbot core.
2. Check your exchanges/pairs and make sure to only have pairs set for exchanges that you already have a registered API key for.
3. Start Gunbot core, verify it starts without errors.
4. Refresh Gunbot in your browser.
5. Go to the "swap exchanges" page and make your desired changes.

