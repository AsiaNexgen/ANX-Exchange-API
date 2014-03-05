ANX-Exchange-API
================

The ANX Exchange API provides methods to access information from account enquiry, market data to placing orders. The primary objective of v2 is to provide transparent compatibility to existing mtgox client. At this moment, we support BTC only. Altcoins support will be added shortly.


Quickstart Catalog
================
Account

    Account Enquiry - Your account balance
    Bitcoin Deposit - Get a bitcoin deposit address
    Bitcoin Withdrawal - Send bitcoin to a bitcoin address.
    Wallet History - A full history of transactions in each of your wallet

Market Data

    Ticker - Get latest data (high, low, volume, buy, sell, vwap) of a currency pair
    Order Book - Get the full order book

Trading

    Quote - Quote an estimated amount to buy/sell a currency pair
    Place Order - Place a market/limit order
    Cancel Order - Cancel a market/limit order
    Order Status - Get the status of all open orders
    Order Result - Get the result of an executed order


Authentication
================
In order to use the API, you must obtain an API key and secret. To do this, login to https://www.anxbtc.com, and navigate to the Profile section. By using the menu on the left, you can find a page called API.

Click Create Key. Copy both the key and the secret to some place. The website will not show you it again after this point.

If you forget the API secret, click the reset secret button to re-generate the secret for you. Please note that any old secret will be disabled automatically and you cannot use the old secret to interact with the API server.
Authenticate using Rest-Key and Rest-Sign

Your API key and secret will be used to authenticate with ANX servers. This is achieved via the use of two HTTP headers: Rest-Key and Rest-Sign

Rest-Key is your API key, Rest-Sign is an HMAC hash constructed from your API secret, your API method path, your post data, and uses the SHA-512 algorithm.

To test your Rest-Signs, use the following pseudocode:

        function hmac_512(msg, sec) {
            sec = Base64Decode(sec);
            result = hmac(msg, sec, sha512);
            return Base64Encode(result);
        }
        
        
        secret = "7pgj8Dm6";
        message = "Test\0Message";
        
        result = hmac_512(message, secret);
        if (result == "69H45OZkKcmR9LOszbajUUPGkGT8IqasGPAWqW/1stGC2Mex2qhIB6aDbuoy7eGfMsaZiU8Y0lO3mQxlsWNPrw==")
            print("Success!");
        else
            printf("Error: %s", result);

Please visit http://docs.anxv2.apiary.io/ for the full documentation.
