---
description: Instructions to run the Gunbot browser interface over a secure connection.
---

# How to enable SSL/TLS

## Run the GUI on https with a self-signed certificate

To run the GUI on https, you'll need a certificate. Below are instructions to generate a self-signed certificate on Windows, Mac and Linux.

{% hint style="info" %}
Certificates from common SSL providers work too. Just make sure to rename the files and change the file extensions to .crt and .key, then place the files in the Gunbot folder.
{% endhint %}

Make sure to set the `https` parameter in `config.js` to `true` to actually enable https after creating a certificate.

When connecting to the GUI for the first time, you'll probably encounter a browser warning due to the use of a self-signed certificate. Create a permanent exception to not get warned again on the same browser.

### **Windows**

1. [Download](https://slproweb.com/products/Win32OpenSSL.html) and install OpenSSL for windows
2. Go to the following folder: C:\Program Files\OpenSSL-Win64\bin
3. Right click "openssl" and run as administrator, a cmd window opens
4. Run the following command: `req -newkey rsa:2048 -nodes -keyout localhost.key -x509 -days 365 -out localhost.crt` \(you'll be asked to fill in some details, you can do this or leave the fields blank by hitting enter several times\)
5. Copy the `localhost.key` and `localhost.crt` from C:\OpenSSL-Win64\bin to your Gunbot folder

### **Mac**

1. Open a terminal window and navigate to your Gunbot folder
2. Run the following command `openssl req -newkey rsa:2048 -nodes -keyout localhost.key -x509 -days 365 -out localhost.crt` and make sure to enter the country code field. The rest can be left blank

### **Linux**

1. Open a terminal window and navigate to your Gunbot folder. Possibly you'll need to install openssl through your package manager first.
2. Run the following command `openssl req -newkey rsa:2048 -nodes -keyout localhost.key -x509 -days 365 -out localhost.crt` and make sure to enter the country code field. The rest can be left blank.

