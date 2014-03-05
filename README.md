# ANX Exchange API v2

![ANXBTC](https://anxpro.com/static/themes/_ANXPRO_COM/images/logo.png) 

The ANX Exchange API provides methods to access information from account enquiry, market data to placing orders. The primary objective of v2 is to provide transparent compatibility to existing mtgox client. At this moment, we support BTC only. Altcoins support will be added shortly.

## Group Migrate from MtGox
Version 2 of this API is compatible with MtGox v2 implementation. For general information on the MtGox v2 API see: https://bitbucket.org/nitrous/mtgox-api

Any MtGox v2 (polling) client should work, by simply changing the base url to https://anxpro.com

**Key differences between MtGox and ANX:**

* The ANX send_money service call does not return a blockchain TXID. We will expose this in a future enhancement.

## Group Quickstart Catalog

### Account

* [Account Enquiry](#post-%2Fapi%2F2%2Fmoney%2Finfo) - Your account balance
* [Bitcoin Deposit](#post-%2Fapi%2F2%2Fmoney%2Fbitcoin%2Faddress) - Get a bitcoin deposit address
* [Bitcoin Withdrawal](#post-%2Fapi%2F2%2Fmoney%2Fbitcoin%2Fsend_simple) - Send bitcoin to a bitcoin address.
* [Wallet History](#post-%2Fapi%2F2%2Fmoney%2Fwallet%2Fhistory) - A full history of transactions in each of your wallet

### Market Data
* [Ticker](#get-%2Fapi%2F2%2F%7Bcurrency_pair%7D%2Fmoney%2Fticker) - Get latest data (high, low, volume, buy, sell, vwap) of a currency pair
* [Order Book](#get-%2Fapi%2F2%2F%7Bcurrency_pair%7D%2Fmoney%2Fdepth%2Ffull) - Get the full order book

### Trading
* [Quote](#post-%2Fapi%2F2%2F%7Bcurrency_pair%7D%2Fmoney%2Forder%2Fquote) - Quote an estimated amount to buy/sell a currency pair
* [Place Order](#post-%2Fapi%2F2%2F%7Bcurrency_pair%7D%2Fmoney%2Forder%2Fadd) - Place a market/limit order
* [Cancel Order](#post-%2Fapi%2F2%2F%7Bcurrency_pair%7D%2Fmoney%2Forder%2Fcancel) - Cancel a market/limit order
* [Order Status](#post-%2Fapi%2F2%2F%7Bcurrency_pair%7D%2Fmoney%2Forders) - Get the status of all open orders
* [Order Result](#post-%2Fapi%2F2%2F%7Bcurrency_pair%7D%2Fmoney%2Forder%2Fresult) - Get the result of an executed order




## Group Authentication
### Getting the API key
In order to use the API, you must obtain an API key and secret. 
To do this, login to https://www.anxbtc.com, and navigate to the Profile section. By using the menu on the left, you can find a page called API. 

Click Create Key. Copy both the key and the secret to some place. The website will not show you it again after this point.

If you forget the API secret, click the reset secret button to re-generate the secret for you. Please note that any old secret will be disabled automatically and you cannot use the old secret to interact with the API server.

### Authenticate using Rest-Key and Rest-Sign
Your API key and secret will be used to authenticate with ANX servers. This is achieved via the use of two HTTP headers: Rest-Key and Rest-Sign

Rest-Key is your API key, Rest-Sign is an HMAC hash constructed from your API secret, your API method path, your post data, and uses the SHA-512 algorithm.

To test your Rest-Signs, use the following pseudocode:

```java
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
```    

# Group Account 
## POST /money/info
### Account Enquiry
This method returns the balance, available balance, daily withdrawal limit and maximum withdraw for every currency in your account.

Unsupported Mtgox fields

* data.Wallets.BTC.Operations
* data.Wallets.BTC.Monthly_Withdraw_Limit
* data.Wallets.BTC.Liabilities
* data.Wallets.BTC.Open_Orders


#### Request parameters
| Fields        | Type        | Description    |
| ------------- |-------------|----------------|
| nonce         | Integer     | unique nonce value. This value should be an integer and larger than any nonce sent previously. |


+ Request (application/x-www-form-urlencoded)
        + Headers
            Rest-Key: <Your-Rest-Key>
            Rest-Sign: <Your-Rest-Sign>
        + Body
            nonce=1390270268218
                    
         + Response 200 (application/json)
            + Body
                                {
                                    "result": "success",
                                    "data": {
                                        "Created": "2013-06-16 21:24:38",
                                        "Language": "en",
                                        "Last_Login": "2014-02-18 15:14:37",
                                        "Login": "test@anxpro.com",
                                        "Trade_Fee": "0.6000",
                                        "Rights": ["trade", "get_info"],
                                        "Wallets": {
                                            "USD": {
                                                "Balance": {
                                                    "currency": "USD",
                                                    "display": "340,279.15065 USD",
                                                    "display_short": "340,279.15 USD",
                                                    "value": "340279.15065",
                                                    "value_int": "34027915065"
                                                },
                                                "Available_Balance": {
                                                    "currency": "USD",
                                                    "display": "340,279.15065 USD",
                                                    "display_short": "340,279.15 USD",
                                                    "value": "340279.15065",
                                                    "value_int": "34027915065"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "USD",
                                                    "display": "10,000.00000 USD",
                                                    "display_short": "10,000.00 USD",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "USD",
                                                    "display": "10,000.00000 USD",
                                                    "display_short": "10,000.00 USD",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                }
                                            },
                                            "CNY": {
                                                "Balance": {
                                                    "currency": "CNY",
                                                    "display": "27,800.35700 CNY",
                                                    "display_short": "27,800.36 CNY",
                                                    "value": "27800.35700",
                                                    "value_int": "2780035700"
                                                },
                                                "Available_Balance": {
                                                    "currency": "CNY",
                                                    "display": "27,800.35700 CNY",
                                                    "display_short": "27,800.36 CNY",
                                                    "value": "27800.35700",
                                                    "value_int": "2780035700"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "CNY",
                                                    "display": "20,000.00000 CNY",
                                                    "display_short": "20,000.00 CNY",
                                                    "value": "20000.00000",
                                                    "value_int": "2000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "CNY",
                                                    "display": "20,000.00000 CNY",
                                                    "display_short": "20,000.00 CNY",
                                                    "value": "20000.00000",
                                                    "value_int": "2000000000"
                                                }
                                            },
                                            "LTC": {
                                                "Balance": {
                                                    "currency": "LTC",
                                                    "display": "29,999.97600000 LTC",
                                                    "display_short": "29,999.98 LTC",
                                                    "value": "29999.97600000",
                                                    "value_int": "2999997600000"
                                                },
                                                "Available_Balance": {
                                                    "currency": "LTC",
                                                    "display": "29,999.97600000 LTC",
                                                    "display_short": "29,999.98 LTC",
                                                    "value": "29999.97600000",
                                                    "value_int": "2999997600000"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "LTC",
                                                    "display": "100.00000000 LTC",
                                                    "display_short": "100.00 LTC",
                                                    "value": "100.00000000",
                                                    "value_int": "10000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "LTC",
                                                    "display": "100.00000000 LTC",
                                                    "display_short": "100.00 LTC",
                                                    "value": "100.00000000",
                                                    "value_int": "10000000000"
                                                }
                                            },
                                            "AUD": {
                                                "Balance": {
                                                    "currency": "AUD",
                                                    "display": "17.56644 AUD",
                                                    "display_short": "17.57 AUD",
                                                    "value": "17.56644",
                                                    "value_int": "1756644"
                                                },
                                                "Available_Balance": {
                                                    "currency": "AUD",
                                                    "display": "17.56644 AUD",
                                                    "display_short": "17.57 AUD",
                                                    "value": "17.56644",
                                                    "value_int": "1756644"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "AUD",
                                                    "display": "10,000.00000 AUD",
                                                    "display_short": "10,000.00 AUD",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "AUD",
                                                    "display": "10,000.00000 AUD",
                                                    "display_short": "10,000.00 AUD",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                }
                                            },
                                            "SBC": {
                                                "Balance": {
                                                    "currency": "SBC",
                                                    "display": "0.00000000 SBC",
                                                    "display_short": "0.00 SBC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "SBC",
                                                    "display": "0.00000000 SBC",
                                                    "display_short": "0.00 SBC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "SBC",
                                                    "display": "0.00000000 SBC",
                                                    "display_short": "0.00 SBC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "SBC",
                                                    "display": "0.00000000 SBC",
                                                    "display_short": "0.00 SBC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                }
                                            },
                                            "EUR": {
                                                "Balance": {
                                                    "currency": "EUR",
                                                    "display": "424.47592 EUR",
                                                    "display_short": "424.48 EUR",
                                                    "value": "424.47592",
                                                    "value_int": "42447592"
                                                },
                                                "Available_Balance": {
                                                    "currency": "EUR",
                                                    "display": "424.47592 EUR",
                                                    "display_short": "424.48 EUR",
                                                    "value": "424.47592",
                                                    "value_int": "42447592"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "EUR",
                                                    "display": "10,000.00000 EUR",
                                                    "display_short": "10,000.00 EUR",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "EUR",
                                                    "display": "10,000.00000 EUR",
                                                    "display_short": "10,000.00 EUR",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                }
                                            },
                                            "HKD": {
                                                "Balance": {
                                                    "currency": "HKD",
                                                    "display": "94,804.31000 HKD",
                                                    "display_short": "94,804.31 HKD",
                                                    "value": "94804.31000",
                                                    "value_int": "9480431000"
                                                },
                                                "Available_Balance": {
                                                    "currency": "HKD",
                                                    "display": "32,063.56980 HKD",
                                                    "display_short": "32,063.57 HKD",
                                                    "value": "32063.56980",
                                                    "value_int": "3206356980"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "HKD",
                                                    "display": "100,000.00000 HKD",
                                                    "display_short": "100,000.00 HKD",
                                                    "value": "100000.00000",
                                                    "value_int": "10000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "HKD",
                                                    "display": "100,000.00000 HKD",
                                                    "display_short": "100,000.00 HKD",
                                                    "value": "100000.00000",
                                                    "value_int": "10000000000"
                                                }
                                            },
                                            "BTC": {
                                                "Balance": {
                                                    "currency": "BTC",
                                                    "display": "40,186.45827083 BTC",
                                                    "display_short": "40,186.46 BTC",
                                                    "value": "40186.45827083",
                                                    "value_int": "4018645827083"
                                                },
                                                "Available_Balance": {
                                                    "currency": "BTC",
                                                    "display": "40,186.45827083 BTC",
                                                    "display_short": "40,186.46 BTC",
                                                    "value": "40186.45827083",
                                                    "value_int": "4018645827083"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "BTC",
                                                    "display": "100.00000000 BTC",
                                                    "display_short": "100.00 BTC",
                                                    "value": "100.00000000",
                                                    "value_int": "10000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "BTC",
                                                    "display": "100.00000000 BTC",
                                                    "display_short": "100.00 BTC",
                                                    "value": "100.00000000",
                                                    "value_int": "10000000000"
                                                }
                                            },
                                            "NMC": {
                                                "Balance": {
                                                    "currency": "NMC",
                                                    "display": "0.00000000 NMC",
                                                    "display_short": "0.00 NMC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "NMC",
                                                    "display": "0.00000000 NMC",
                                                    "display_short": "0.00 NMC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "NMC",
                                                    "display": "100.00000000 NMC",
                                                    "display_short": "100.00 NMC",
                                                    "value": "100.00000000",
                                                    "value_int": "10000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "NMC",
                                                    "display": "100.00000000 NMC",
                                                    "display_short": "100.00 NMC",
                                                    "value": "100.00000000",
                                                    "value_int": "10000000000"
                                                }
                                            },
                                            "NZD": {
                                                "Balance": {
                                                    "currency": "NZD",
                                                    "display": "0.00000 NZD",
                                                    "display_short": "0.00 NZD",
                                                    "value": "0.00000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "NZD",
                                                    "display": "0.00000 NZD",
                                                    "display_short": "0.00 NZD",
                                                    "value": "0.00000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "NZD",
                                                    "display": "10,000.00000 NZD",
                                                    "display_short": "10,000.00 NZD",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "NZD",
                                                    "display": "10,000.00000 NZD",
                                                    "display_short": "10,000.00 NZD",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                }
                                            },
                                            "GBP": {
                                                "Balance": {
                                                    "currency": "GBP",
                                                    "display": "0.00000 GBP",
                                                    "display_short": "0.00 GBP",
                                                    "value": "0.00000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "GBP",
                                                    "display": "0.00000 GBP",
                                                    "display_short": "0.00 GBP",
                                                    "value": "0.00000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "GBP",
                                                    "display": "10,000.00000 GBP",
                                                    "display_short": "10,000.00 GBP",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "GBP",
                                                    "display": "10,000.00000 GBP",
                                                    "display_short": "10,000.00 GBP",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                }
                                            },
                                            "DOGE": {
                                                "Balance": {
                                                    "currency": "DOGE",
                                                    "display": "0.00000000 DOGE",
                                                    "display_short": "0.00 DOGE",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "DOGE",
                                                    "display": "0.00000000 DOGE",
                                                    "display_short": "0.00 DOGE",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "DOGE",
                                                    "display": "100.00000000 DOGE",
                                                    "display_short": "100.00 DOGE",
                                                    "value": "100.00000000",
                                                    "value_int": "10000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "DOGE",
                                                    "display": "100.00000000 DOGE",
                                                    "display_short": "100.00 DOGE",
                                                    "value": "100.00000000",
                                                    "value_int": "10000000000"
                                                }
                                            },
                                            "MEC": {
                                                "Balance": {
                                                    "currency": "MEC",
                                                    "display": "0.00000000 MEC",
                                                    "display_short": "0.00 MEC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "MEC",
                                                    "display": "0.00000000 MEC",
                                                    "display_short": "0.00 MEC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "MEC",
                                                    "display": "0.00000000 MEC",
                                                    "display_short": "0.00 MEC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "MEC",
                                                    "display": "0.00000000 MEC",
                                                    "display_short": "0.00 MEC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                }
                                            },
                                            "SGD": {
                                                "Balance": {
                                                    "currency": "SGD",
                                                    "display": "0.00000 SGD",
                                                    "display_short": "0.00 SGD",
                                                    "value": "0.00000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "SGD",
                                                    "display": "0.00000 SGD",
                                                    "display_short": "0.00 SGD",
                                                    "value": "0.00000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "SGD",
                                                    "display": "10,000.00000 SGD",
                                                    "display_short": "10,000.00 SGD",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "SGD",
                                                    "display": "10,000.00000 SGD",
                                                    "display_short": "10,000.00 SGD",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                }
                                            },
                                            "CHF": {
                                                "Balance": {
                                                    "currency": "CHF",
                                                    "display": "6,480.50000 CHF",
                                                    "display_short": "6,480.50 CHF",
                                                    "value": "6480.50000",
                                                    "value_int": "648050000"
                                                },
                                                "Available_Balance": {
                                                    "currency": "CHF",
                                                    "display": "6,480.50000 CHF",
                                                    "display_short": "6,480.50 CHF",
                                                    "value": "6480.50000",
                                                    "value_int": "648050000"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "CHF",
                                                    "display": "10,000.00000 CHF",
                                                    "display_short": "10,000.00 CHF",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "CHF",
                                                    "display": "10,000.00000 CHF",
                                                    "display_short": "10,000.00 CHF",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                }
                                            },
                                            "JPY": {
                                                "Balance": {
                                                    "currency": "JPY",
                                                    "display": "0.00000 JPY",
                                                    "display_short": "0.00 JPY",
                                                    "value": "0.00000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "JPY",
                                                    "display": "0.00000 JPY",
                                                    "display_short": "0.00 JPY",
                                                    "value": "0.00000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "JPY",
                                                    "display": "1,000,000.00000 JPY",
                                                    "display_short": "1,000,000.00 JPY",
                                                    "value": "1000000.00000",
                                                    "value_int": "100000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "JPY",
                                                    "display": "1,000,000.00000 JPY",
                                                    "display_short": "1,000,000.00 JPY",
                                                    "value": "1000000.00000",
                                                    "value_int": "100000000000"
                                                }
                                            },
                                            "QRK": {
                                                "Balance": {
                                                    "currency": "QRK",
                                                    "display": "0.00000000 QRK",
                                                    "display_short": "0.00 QRK",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "QRK",
                                                    "display": "0.00000000 QRK",
                                                    "display_short": "0.00 QRK",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "QRK",
                                                    "display": "0.00000000 QRK",
                                                    "display_short": "0.00 QRK",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "QRK",
                                                    "display": "0.00000000 QRK",
                                                    "display_short": "0.00 QRK",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                }
                                            },
                                            "PPC": {
                                                "Balance": {
                                                    "currency": "PPC",
                                                    "display": "0.00000000 PPC",
                                                    "display_short": "0.00 PPC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "PPC",
                                                    "display": "0.00000000 PPC",
                                                    "display_short": "0.00 PPC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "PPC",
                                                    "display": "100.00000000 PPC",
                                                    "display_short": "100.00 PPC",
                                                    "value": "100.00000000",
                                                    "value_int": "10000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "PPC",
                                                    "display": "100.00000000 PPC",
                                                    "display_short": "100.00 PPC",
                                                    "value": "100.00000000",
                                                    "value_int": "10000000000"
                                                }
                                            },
                                            "CAD": {
                                                "Balance": {
                                                    "currency": "CAD",
                                                    "display": "0.00000 CAD",
                                                    "display_short": "0.00 CAD",
                                                    "value": "0.00000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "CAD",
                                                    "display": "0.00000 CAD",
                                                    "display_short": "0.00 CAD",
                                                    "value": "0.00000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "CAD",
                                                    "display": "10,000.00000 CAD",
                                                    "display_short": "10,000.00 CAD",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "CAD",
                                                    "display": "10,000.00000 CAD",
                                                    "display_short": "10,000.00 CAD",
                                                    "value": "10000.00000",
                                                    "value_int": "1000000000"
                                                }
                                            },
                                            "WDC": {
                                                "Balance": {
                                                    "currency": "WDC",
                                                    "display": "0.00000000 WDC",
                                                    "display_short": "0.00 WDC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Available_Balance": {
                                                    "currency": "WDC",
                                                    "display": "0.00000000 WDC",
                                                    "display_short": "0.00 WDC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Daily_Withdrawal_Limit": {
                                                    "currency": "WDC",
                                                    "display": "0.00000000 WDC",
                                                    "display_short": "0.00 WDC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                },
                                                "Max_Withdraw": {
                                                    "currency": "WDC",
                                                    "display": "0.00000000 WDC",
                                                    "display_short": "0.00 WDC",
                                                    "value": "0.00000000",
                                                    "value_int": "0"
                                                }
                                            }
                                        }
                                    }
                                }

## POST /money/bitcoin/address
### Bitcoin Deposit

Get a bitcoin address for deposit. Returns a unique bitcoin deposit address for your ANX account. Please note that, in contrary to Mtgox, the same address is returned for this function.

##### Request parameters

| Fields        | Type        | Description    |
| ------------- |-------------|----------------|
| nonce         | Integer     | unique nonce value. This value should be an integer and larger than any nonce sent previously. |


##### Response Definition

| Fields        | Description|
| ------------- |-------------|
| addr          | The bitcoin deposit address for your account |

+ Request (application/x-www-form-urlencoded)
    + Headers
        Rest-Key: <Your-Rest-Key>
        Rest-Sign: <Your-Rest-Sign>
    + Body
        nonce=1393225933584


+ Response 200 (application/json)
    + Body
        {
            "result": "success",
            "data": {
                "addr": "1GAUBau3nKQYJ1uvMWUfWCdEbMTJ1BXFjW"
            }
        }

## POST /money/bitcoin/send_simple
### Bitcoin withdrawal

Send bitcoin to a bitcoin address. Please note that a confirmation email would be sent to you to confirm this withdrawal. If you do not click the confirmation link on the email, the withdrawal will be reversed after 10 minutes. If you would like this feature disabled on your account, please contact service@anxpro.com.

In the example below, we send 1 BTC to a bitcoin address "1GAUBau3nKQYJ1uvMWUfWCdEbMTJ1BXFjW".

##### Request parameters

| Fields        | Type        | Description    |
| ------------- |-------------|----------------|
| address       | String      | The destination bitcoin address that will be receiving bitcoins |
| amount_int    | Integer     | the amount of bitcoin that you want to send. This should always be an integer. For example, if you want to send 1BTC, the value should be 100000000 |
| nonce         | Integer     | unique nonce value. This value should be an integer and larger than any nonce sent previously. |

##### Response Definition

| Fields        | Description   |
| ------------- |---------------|
| result        | success/error |

+ Request (application/x-www-form-urlencoded)
    + Headers
        Rest-Key: <Your-Rest-Key>
        Rest-Sign: <Your-Rest-Sign>
    + Body
        nonce=1393380799864&address=1GAUBau3nKQYJ1uvMWUfWCdEbMTJ1BXFjW&amount_int=100000000


+ Response 200 (application/json)
    + Body
        {
            "result": "success"
        }



## POST /money/wallet/history
### Wallet History
A full history of the transaction in each of your wallet. Transactions include buy, sell, deposit, withdrawal and fees.

Note that:

1. if there has been a reversal, for example from a BTC withdrawal timeout, the running balance will be difficult to reconcile however the total balance will remain correct.
2. The <a href="#wallet_history_trade">Trade object</a> always refer to the traded currency, regardless of which currency you specified. For example, in your BTC wallet history, the trade object will display BTC amount. And in a USD wallet history, the trade object will also display BTC amount.

#### Examples
In the following examples, the first one is a BTC wallet history and the second one is a USD wallet history.


#### Request parameters
| Fields        | Type        | Description    |
| ------------- |-------------|----------------|
| currency      | String      | The currency of the wallet. E.g. "BTC", "LTC", etc
| page          | Integer     | Requested page no  |
| nonce         | Integer     | unique nonce value. This value should be an integer and larger than any nonce sent previously. |

#### Response Definition

| Fields        | Description|
| ------------- |-------------|
| records       | No of records in this wallet |
| result        | A list of wallet history object, see below |
| current_page  | Current page number |
| max_page      | Maximum number of page available |
| max_result    | Maximum number of result per page, currently set at 50 |

##### Wallet History Object

| Fields        | Description|
| ------------- |-------------|
| Index         | Index no of this wallet history object |
| Date          | Unixtimestamp of this wallet history being created  |
| Type          | wallet history type, see below |
| Value         | Value of this transaction, in a [Currency Object](#currency_object) format|
| Balance       | Balance of the wallet after this transaction complete, in a [Currency Object](#currency_object) format|
| Info          | A description of this wallet history |
| Link          | (Optional) An array of String, TBC |
| <a name="wallet_history_trade">Trade</a>         | The corresponding Trade |
| Trade.oid     | Order id of the corresponding trade |
| Trade.app     | (Optional) Not implemented |
| Trade.tid     | Trade id in UUID format |
| Trade.Amount  | Amount of the traded currency (always), in a [Currency Object](#currency_object) format|
| Trade.Properties  | Type of the trade, either "market" or "limit"|

##### Wallet History Type

| Fields        | Description|
| ------------- |-------------|
| withdraw      | Money being withdrawn (eg to bank account) |
| deposit       | Incoming deposit of currency  |
| in            | BTC gained after a bid order |
| out           | BTC removed after an ask order |
| earned        | Auxiliary currency added after an ask order |
| spent         | Auxiliary currency removed after a bid order |
| fee           | Fee deducted from balance for the above transaction type |

+ Request (application/x-www-form-urlencoded)
    + Headers
        Rest-Key: <Your-Rest-Key>
        Rest-Sign: <Your-Rest-Sign>
    + Body
        nonce=1393225933584&page=1&currency=USD

+ Response 200 (application/json)

            + Body
                {
                    "result": "success",
                    "data": {
                        "records": "448",
                        "result": [{
                            "Index": "448",
                            "Date": 1393825095000,
                            "Type": "fee",
                            "Value": {
                                "currency": "BTC",
                                "display": "0.00006000 BTC",
                                "display_short": "0.00 BTC",
                                "value": "0.00006000",
                                "value_int": "6000"
                            },
                            "Balance": {
                                "currency": "BTC",
                                "display": "99,942.54148000 BTC",
                                "display_short": "99,942.54 BTC",
                                "value": "99942.54148000",
                                "value_int": "9994254148000"
                            },
                            "Info": "BTC bought: [tid:b4603be2-4f33-4307-bf4f-430f3410fd5c] 0.01000000 BTC at 6280.00000 HKD",
                            "Trade": {
                                "oid": "4acc6ed5-1ee6-46b6-820e-de0ce756aa5d",
                                "tid": "b4603be2-4f33-4307-bf4f-430f3410fd5c",
                                "Amount": {
                                    "currency": "BTC",
                                    "display": "0.01000000 BTC",
                                    "display_short": "0.01 BTC",
                                    "value": "0.01000000",
                                    "value_int": "1000000"
                                },
                                "Properties": "market"
                            }
                        }, {
                            "Index": "447",
                            "Date": 1393825095000,
                            "Type": "in",
                            "Value": {
                                "currency": "BTC",
                                "display": "0.01000000 BTC",
                                "display_short": "0.01 BTC",
                                "value": "0.01000000",
                                "value_int": "1000000"
                            },
                            "Balance": {
                                "currency": "BTC",
                                "display": "99,942.54154000 BTC",
                                "display_short": "99,942.54 BTC",
                                "value": "99942.54154000",
                                "value_int": "9994254154000"
                            },
                            "Info": "BTC bought: [tid:b4603be2-4f33-4307-bf4f-430f3410fd5c] 0.01000000 BTC at 6280.00000 HKD",
                            "Trade": {
                                "oid": "4acc6ed5-1ee6-46b6-820e-de0ce756aa5d",
                                "tid": "b4603be2-4f33-4307-bf4f-430f3410fd5c",
                                "Amount": {
                                    "currency": "BTC",
                                    "display": "0.01000000 BTC",
                                    "display_short": "0.01 BTC",
                                    "value": "0.01000000",
                                    "value_int": "1000000"
                                },
                                "Properties": "market"
                            }
                        }
                    }
                }
        
+ Response 200 (application/json)

            + Body
                {
                    "result": "success",
                    "data": {
                        "records": "26",
                        "result": [{
                            "Index": "26",
                            "Date": 1393225830,
                            "Type": "fee",
                            "Value": {
                                "currency": "USD",
                                "display": "1.14000 USD",
                                "display_short": "1.14 USD",
                                "value": "1.14000",
                                "value_int": "114000"
                            },
                            "Balance": {
                                "currency": "USD",
                                "display": "4,925.18000 USD",
                                "display_short": "4,925.18 USD",
                                "value": "4925.18000",
                                "value_int": "492518000"
                            },
                            "Info": "BTC sold: [tid:4225b9bc-fc50-4001-b65f-b291fc7823d0] 1.00000000 BTC at 380.00000 USD",
                            "Trade": {
                                "oid": "a42076b6-c359-436b-960e-e6f4a242d546",
                                "tid": "4225b9bc-fc50-4001-b65f-b291fc7823d0",
                                "Amount": {
                                    "currency": "BTC",
                                    "display": "1.00000000 BTC",
                                    "display_short": "1.00 BTC",
                                    "value": "1.00000000",
                                    "value_int": "100000000"
                                },
                                "Properties": "market"
                            }
                        }, {
                            "Index": "25",
                            "Date": 1393225830,
                            "Type": "earned",
                            "Value": {
                                "currency": "USD",
                                "display": "380.00000 USD",
                                "display_short": "380.00 USD",
                                "value": "380.00000",
                                "value_int": "38000000"
                            },
                            "Balance": {
                                "currency": "USD",
                                "display": "4,926.32000 USD",
                                "display_short": "4,926.32 USD",
                                "value": "4926.32000",
                                "value_int": "492632000"
                            },
                            "Info": "BTC sold: [tid:4225b9bc-fc50-4001-b65f-b291fc7823d0] 1.00000000 BTC at 380.00000 USD",
                            "Trade": {
                                "oid": "a42076b6-c359-436b-960e-e6f4a242d546",
                                "tid": "4225b9bc-fc50-4001-b65f-b291fc7823d0",
                                "Amount": {
                                    "currency": "BTC",
                                    "display": "1.00000000 BTC",
                                    "display_short": "1.00 BTC",
                                    "value": "1.00000000",
                                    "value_int": "100000000"
                                },
                                "Properties": "market"
                            }
                        }],
                        "current_page": 1,
                        "max_page": 1,
                        "max_results": 50
                    }
                }

                        

# Group Market Data
## GET /{currency_pair}/money/ticker
### Ticker

Get the most recent ticker for a [currency pair](#currency_pair)

Unsupported Mtgox fields

* data.last_lcoal
* data.last_orig
* data.last_all
* data.item

+ Response 200 (application/json)
    
            + Body
                    {
                        "result": "success",
                        "data": {
                            "high": {
                                "currency": "USD",
                                "display": "725.38123 USD",
                                "display_short": "725.38 USD",
                                "value": "725.38123",
                                "value_int": "72538123"
                            },
                            "low": {
                                "currency": "USD",
                                "display": "380.00000 USD",
                                "display_short": "380.00 USD",
                                "value": "380.00000",
                                "value_int": "38000000"
                            },
                            "avg": {
                                "currency": "USD",
                                "display": "429.34018 USD",
                                "display_short": "429.34 USD",
                                "value": "429.34018",
                                "value_int": "42934018"
                            },
                            "vwap": {
                                "currency": "USD",
                                "display": "429.34018 USD",
                                "display_short": "429.34 USD",
                                "value": "429.34018",
                                "value_int": "42934018"
                            },
                            "vol": {
                                "currency": "BTC",
                                "display": "7.00000000 BTC",
                                "display_short": "7.00 BTC",
                                "value": "7.00000000",
                                "value_int": "700000000"
                            },
                            "last": {
                                "currency": "USD",
                                "display": "725.38123 USD",
                                "display_short": "725.38 USD",
                                "value": "725.38123",
                                "value_int": "72538123"
                            },
                            "buy": {
                                "currency": "USD",
                                "display": "38.85148 USD",
                                "display_short": "38.85 USD",
                                "value": "38.85148",
                                "value_int": "3885148"
                            },
                            "sell": {
                                "currency": "USD",
                                "display": "897.25596 USD",
                                "display_short": "897.26 USD",
                                "value": "897.25596",
                                "value_int": "89725596"
                            },
                            "now": 1393388594814000
                        }
                    }

## GET /{currency_pair}/money/depth/full
### Order Book Full Depth

Get a complete copy of ANXâ€™s order book on a particular [currency pair](#currency_pair).

In the example below, we enquire the order book of BTCUSD. There are 4 bid items and 3 ask items. The highest bid is 2,000 USD for 3 BTC, the lowest ask is 3,260.4 USD for 16 BTC.

+ Response 200 (application/json)
                
            + Body
                    {
                        "result": "success",
                        "data": {
                            "now": "1392693096241000",
                            "asks": [{
                                "price": "3260.40000",
                                "price_int": "326040000",
                                "amount": "16.00000000",
                                "amount_int": "1600000000"
                            }, {
                                "price": "6280.79172",
                                "price_int": "628079172",
                                "amount": "8.84549083",
                                "amount_int": "884549083"
                            }, {
                                "price": "8580.00000",
                                "price_int": "858000000",
                                "amount": "0.02000000",
                                "amount_int": "2000000"
                            }],
                            "bids": [{
                                "price": "2000.00000",
                                "price_int": "200000000",
                                "amount": "3.00000000",
                                "amount_int": "300000000"
                            }, {
                                "price": "210.00000",
                                "price_int": "21000000",
                                "amount": "0.49000000",
                                "amount_int": "49000000"
                            }, {
                                "price": "205.00000",
                                "price_int": "20500000",
                                "amount": "1.00000000",
                                "amount_int": "100000000"
                            }, {
                                "price": "200.00000",
                                "price_int": "20000000",
                                "amount": "72.00000000",
                                "amount_int": "7200000000"
                            }]
                        }
                    }

 

# Group Trade

**Please note that you must enable trading permission in the API page before submitting a trade order**

## Glossary
### <a name="currency_pair">Currency pair</a>
Currency pair is a pair of currency that you intend to trade. BTCHKD, DODEBTC are some examples. For all trade operation, you should include a currency pair 

Supported currency pair:

```
BTCUSD,BTCHKD,BTCEUR,BTCCAD,BTCAUD,BTCSGD,BTCJPY,BTCCHF,BTCGBP,BTCNZD,
LTCBTC, LTCUSD, LTCHKD,LTCEUR,LTCCAD,LTCAUD,LTCSGD,LTCJPY,LTCCHF,LTCGBP,LTCNZD,
PPCBTC, PPCLTC,PPCUSD, PPCHKD,PPCEUR,PPCCAD,PPCAUD,PPCSGD,PPCJPY,PPCCHF,PPCGBP,PPCNZD,
NMCBTC, NMCLTC,NMCUSD, NMCHKD,NMCEUR,NMCCAD,NMCAUD,NMCSGD,NMCJPY,NMCCHF,NMCGBP,NMCNZD,
DOGEBTC, DOGEUSD, DOGEHKD,DOGEEUR,DOGECAD,DOGEAUD,DOGESGD,DOGEJPY,DOGECHF,DOGEGBP,DOGENZD,

```

### Trading currency
Trading currency is the first currency appeared in a currency pair. 

For example,  <br>
BTCUSD: BTC is the trading currency, USD is the settlement currency<br>
LTCBTC: LTC is the trading currency, BTC is the settlement currency

## POST /{currency_pair}/money/order/quote
### Quote 

Quote an estimated amount to buy/sell a currency pair

In the example below, we would like to quote the rate to buy 10 BTC in HKD. 
The server returned the rate 897.48774 USD per BTC and thte total amount to buy 10 BTC is 8974.87744919 HKD

**In contrast to MtGox, in addition to amount, this function also return a rate**

#### Request parameters
| Fields        | Type        | Description    |
| ------------- |-------------|----------------|
| type          | String      | "bid" / "ask". Bid is buying where ask is selling
| amount        | Integer     | the amount of bitcoin that you want to buy/sell. This should always be an integer. For example, if you want to buy 1BTC, the value should be 100000000 |
| nonce         | Integer     | unique nonce value. This value should be an integer and larger than any nonce sent previously. |


#### Response Definition

| Fields        | Description|
| ------------- |-------------|
| rate          | Estimated exchange rate to buy/sell| 
| amount        | The total amount to be earned/spent in settlement currency|
    
+ Request (application/x-www-form-urlencoded)
    + Headers
        Rest-Key: <Your-Rest-Key>
        Rest-Sign: <Your-Rest-Sign>
    + Body
        nonce=1392262618582&type=bid&amount=1000000000

+ Response 200 (application/json)
    {"result":"success","data":{"rate":89748774, "amount": 897487744919}}

+ Response 500
    {"result":"error","message":"Cannot quote temporarily"} 


## POST /{currency_pair}/money/order/add
### Place a limit/market order

To place a market order, do not specify the value of price_int

In the example below, we try to place an limit order to buy the 0.1 BTC at 100HKD (assuming the currency_pair is BTCHKD).

#### Request parameters
| Fields        | Type        | Description    |
| ------------- |-------------|----------------|
| type          | String      | "bid" / "ask". Bid is buying where ask is selling
| amount_int    | Integer     | the amount of bitcoin that you want to buy/sell. This should always be an integer. For example, if you want to buy 1BTC, the value should be 100000000 |
| price_int     | Integer     | the price of bitcoin that you want to buy/sell. This should always be an integer. For example, 5,000 HKD should read 500000000 |
| nonce         | Integer     | unique nonce value. This value should be an integer and larger than any nonce sent previously. |

#### Response Definition

| Fields        | Description|
| ------------- |-------------| 
| result        | success or error | 
| data          | order id in a UUID format|

+ Request (application/x-www-form-urlencoded)
    + Headers
        Rest-Key: <Your-Rest-Key>
        Rest-Sign: <Your-Rest-Sign>
    + Body
        nonce=1392258859762&type=bid&amount_int=10000000&price_int=10000000

+ Response 200 (application/json)

    {"result":"success","data":"4b263101-9a88-4d39-901c-aca098fa3020"}

+ Response 200 (application/json)

    {"result":"error", "message":"order too small - must be greater or equal to 0.01"}
    
+ Response 200 (application/json)
    
    {"message":"Insufficient Funds","result":"error"}

## POST /{currency_pair}/money/order/cancel
### Cancel an open order


#### Request parameters
| Fields        | Type        | Description    |
| ------------- |-------------|----------------|
| oid           | String      | Order Id (in UUID format) of the order to be deleted
| nonce         | Integer     | unique nonce value. This value should be an integer and larger than any nonce sent previously. |

#### Response Definition

| Fields        | Description|
| ------------- |-------------| 
| result        | success or error | 
| oid           | order id being deleted|
| qid           | This field returns empty at this moment |

+ Request (application/x-www-form-urlencoded)
    + Headers
        Rest-Key: <Your-Rest-Key>
        Rest-Sign: <Your-Rest-Sign>
    + Body
        nonce=1392693094105&oid=feeecbea-d032-4923-b842-13793230b28d

+ Response 200 (application/json)
    {
        "result": "success",
        "data": {
            "oid": "feeecbea-d032-4923-b842-13793230b28d",
            "qid": ""
        }
    }

+ Response 200 (application/json)
    {
        "message": "Order Not Found",
        "result": "error"
    }

## POST /{currency_pair}/money/orders
### Open orders status

Returns limit orders in pending state submission to the orderbook, or already active (they may be partially filled).

Note you could have market orders in state pending that would not be retrieved by this request.

Please note that regardsless of the currency_pair specified here, order in all currency pairs will be returned

Below examples show 2 open limit orders:

The first one is selling 10BTC at a rate of 412.34567 HKD per BTC. <br>
The second one is buying 10BTC at a rate of 212.34567 HKD per BTC.

#### Request parameters
| Fields        | Type        | Description    |
| ------------- |-------------|----------------|
| nonce         | Integer     | unique nonce value. This value should be an integer and larger than any nonce sent previously. |

+ Request (application/x-www-form-urlencoded)
    + Headers
        Rest-Key: <Your-Rest-Key>
        Rest-Sign: <Your-Rest-Sign>
    + Body
        nonce=1390270268218

+ Response 200 (application/json)
   
            + Body
                    {
                        "result": "success",
                        "data": [{
                            "oid": "e74305c7-c424-4fbc-a8a2-b41d8329deb0",
                            "currency": "HKD",
                            "item": "BTC",
                            "type": "offer",
                            "amount": {
                                "currency": "BTC",
                                "display": "10.00000000 BTC",
                                "display_short": "10.00 BTC",
                                "value": "10.00000000",
                                "value_int": "1000000000"
                            },
                            "effective_amount": {
                                "currency": "BTC",
                                "display": "10.00000000 BTC",
                                "display_short": "10.00 BTC",
                                "value": "10.00000000",
                                "value_int": "1000000000"
                            },
                            "price": {
                                "currency": "HKD",
                                "display": "412.34567 HKD",
                                "display_short": "412.35 HKD",
                                "value": "412.34567",
                                "value_int": "41234567"
                            },
                            "status": "open",
                            "date": 1393411075000,
                            "priority": 1393411075000000,
                            "actions": []
                        }, {
                            "oid": "7eecf4b2-5785-4500-a5d4-f3f8c924395c",
                            "currency": "HKD",
                            "item": "BTC",
                            "type": "bid",
                            "amount": {
                                "currency": "BTC",
                                "display": "10.00000000 BTC",
                                "display_short": "10.00 BTC",
                                "value": "10.00000000",
                                "value_int": "1000000000"
                            },
                            "effective_amount": {
                                "currency": "BTC",
                                "display": "10.00000000 BTC",
                                "display_short": "10.00 BTC",
                                "value": "10.00000000",
                                "value_int": "1000000000"
                            },
                            "price": {
                                "currency": "HKD",
                                "display": "212.34567 HKD",
                                "display_short": "212.35 HKD",
                                "value": "212.34567",
                                "value_int": "21234567"
                            },
                            "status": "open",
                            "date": 1393411073000,
                            "priority": 1393411073000000,
                            "actions": []
                        }]
                    }

## POST /{currency_pair}/money/order/result
### Enquire the result of a completed order

Get detail information such as average cost, total amount and of an executed order. 

  * This method only works for completed/executed orders. For open orders, see money/orders.
  * Cancelled orders are not returned by this method unless they have been partially filled before cancelling, in which case they do.


#### Response Definition

| Fields        | Description|
| ------------- |-------------| 
| result        | success/error | 
| avg_cost      | Average cost of this transaction |
| total_amount  | Total amount of bitcoin bought   |
| total_spent   | Total amount of cash spent       |
| trades        | The trades that involved in this order  |
| trades.amount | amount of this trade, in a [Currency Object](#currency_object) format|
| trades.price  | price of this trade, in a [Currency Object](#currency_object) format|
| trades.item   | Traded currency of the trade|
| trades.currency | Settlement currency of the trade |
| trades.date   | Date of the trade happened|
| trades.primary| always 'Y' |
| trades.timestamp| unix timestamp in millis, note in previous versions of this api this field was called trade_id |
| trades.trade_id | uuid of the trade |
| trades.type   | bid/ask |
| trades.properties   | limit/market |


#### Possible errors

| HTTP Code    | Error message |
| -------------|-------------| 
| 200          | No executed order with that identifer found |


+ Parameters
    + type (required, string) ... 
        Order Type. Bid is buying where ask is selling
        + Values
            + `bid`
            + `ask`
    + order (required, String) ... Order id in UUID format
    + nonce (required, number, `12345677`) ... Numeric `nonce` unique nonce value in the post data, and this value should be an integer and larger than any value sent previously. 
    

+ Request (application/x-www-form-urlencoded)
    + Headers
        Rest-Key: <Your-Rest-Key>
        Rest-Sign: <Your-Rest-Sign>
    + Body
        nonce=1392679609165&type=bid&order=8431d75e-a0fe-4cc6-8b9a-69af58515600

+ Response 200 (application/json)
    
            + Body
                    {
                        "result": "success",
                        "data": {
                            "avg_cost": {
                                "currency": "HKD",
                                "display": "999,999.00000 HKD",
                                "display_short": "999,999.00 HKD",
                                "value": "999999.00000",
                                "value_int": "99999900000"
                            },
                            "order_id": "7f986140-c49b-46f6-b927-5eb6ba75813f",
                            "total_amount": {
                                "currency": "BTC",
                                "display": "0.01000000 BTC",
                                "display_short": "0.01 BTC",
                                "value": "0.01000000",
                                "value_int": "1000000"
                            },
                            "total_spent": {
                                "currency": "HKD",
                                "display": "9,999.99000 HKD",
                                "display_short": "9,999.99 HKD",
                                "value": "9999.99000",
                                "value_int": "999999000"
                            },
                            "trades": [{
                                "amount": {
                                    "currency": "BTC",
                                    "display": "0.01000000 BTC",
                                    "display_short": "0.01 BTC",
                                    "value": "0.01000000",
                                    "value_int": "1000000"
                                },
                                "currency": "HKD",
                                "date": "2014-03-03 16:22",
                                "item": "BTC",
                                "price": {
                                    "currency": "HKD",
                                    "display": "999,999.00000 HKD",
                                    "display_short": "999,999.00 HKD",
                                    "value": "999999.00000",
                                    "value_int": "99999900000"
                                },
                                "primary": "Y",
                                "properties": "market",
                                "trade_id": "4f88c40d-d278-4173-b900-df42f6511041",
                                "timestamp": "1393834930000",
                                "type": "bid"
                            }]
                        }
                    }


+ Response 200 (application/json)
    
            + Body
                    {
                        "message": "No executed order with that identifer found",
                        "result": "error"
                    }

# Group API Usage Control

We have implement control on API usage to provide a stable environment for everybody to use. 

1. Currently all users and IP addresses are throttled to 1 request of any type per second. You will get an error result if you are throttled.
2. Additionally there is a max of 1000 requests per hour.
3. Information queries - such a get depth/results/ticker are throttled to 1 per second.
4. Finally there is a maximum of 50 open orders at any one time, with a total of 50 adds per hour.

# Group Changelog
## <a name="changelog_26022014">26 Feb 2014</a>

1. All fiat decimal place change from 2 to 5. This change affects: `money/ticker`, `money/info`, `money/orders` and `order/result`
2. Added [Wallet History](http://docs.anxv2.apiary.io/#post-%2Fapi%2F2%2Fmoney%2Fwallet%2Fhistory)
3. Added [Bitcoin Deposit](http://docs.anxv2.apiary.io/#post-%2Fapi%2F2%2Fmoney%2Fbitcoin%2Faddress)
4. Added [Bitcoin Withdrawal](http://docs.anxv2.apiary.io/#post-%2Fapi%2F2%2Fmoney%2Fbitcoin%2Fsend_simple)
5. Fixed [Order Quote](http://docs.anxv2.apiary.io/#post-%2Fapi%2F2%2F%7Bcurrency_pair%7D%2Fmoney%2Forder%2Fquote) -  Previously in the response of this function, we supplied one single field called "rate" only, which is the price of a currency pair. Now we have included an additional field called "amount" in the response.
6. Fixed [Open Orders](http://docs.anxv2.apiary.io/#post-%2Fapi%2F2%2F%7Bcurrency_pair%7D%2Fmoney%2Forders) - amount was previously displaying the wrong currency code
7. Enhanced error handling response
8. Fixed [Order Result](http://docs.anxv2.apiary.io/#post-%2Fapi%2F2%2F%7Bcurrency_pair%7D%2Fmoney%2Forder%2Fresult) - Changed traded_id from timestamp to uuid, added timestamp field with the old value

**Special note on item (1)**

Please note that this is a significant change. A simple example would illustrate the impact. <br/>
For example, the function money/ticker previously return a buy price as below:

```
"buy": {
    "currency": "USD",
    "display": "38.85 USD",
    "display_short": "38.85 USD",
    "value": "38.85",
    "value_int": "3885148"
},
```

will now change to 

```
"buy": {
    "currency": "USD",
    "display": "38.85148 USD",
    "display_short": "38.85 USD",
    "value": "38.85148",
    "value_int": "3885148"
},
```
This change affects: `money/ticker`, `money/info`, `money/orders` and `order/result`

# Group Data Object Definition
## Currency Object
<a name="currency_object"> </a>
Currency Object refer to a json block of the form

```
{
    "currency": "USD",
    "display": "4,925.18000 USD",
    "display_short": "4,925.18 USD",
    "value": "4925.18000",
    "value_int": "492518000"
}
```

| Fields        | Description|
| ------------- |-------------| 
| currency      | The currency code | 
| display       | The value in display format, might contains grouping separator(,) and ends with a currency code  |
| display_short  | The value in display short format, rounding to 2 decimal places   |
| value          | The value itself, does not contain any formatting text |
| value_int      | The value itself, multiplied by its corresponding multiplier (10^5, 10^8, etc), does not contain any formatting text |

### Multiplier

| Fields        | Multiplier|  Exmaples
| ------------- |-------------| ---------
| Cash/Fiat Amount     | 10^5 | HKD 1234.56 -> 123456000
| Crypto/Coins Amount  | 10^8 | BTC 1234.56789 -> 123456789000
| Currency pair rate - BTC, LTC| 10^5 |  BTCHKD 4,100.31234 -> 410031234
| Currency pair rate - all other crypto  | 10^8 | DOGEHKD 0.012345 -> 1234500



# Group Troubleshooting
1. I am getting HTTP Code 304 for all requests
Check nonce, must be incrementing, use unix timestamp is generally recommended
2. I am getting HTTP Code 403 for all requests
Check REST-SIGN. We return 403 when REST-SIGN is not ocrrect.
3. How do I reset my nonce?
Reset your secret key in our website
