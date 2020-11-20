# Binance to Google Sheets!

A lightweight **Google Spreadsheets** `add-on` to GET data _directly_ from **Binance API** _without_ any intermediaries!

This `add-on` is basically an **API client** specially hand-crafted to work between Google Spreadsheets and Binance.  
By using the `BINANCE()` formula in your spreadsheet, you can get data fetched from Binance API like:  
* Current crypto prices
* 24h stats
* Open and Done orders
* Last update time
* ..and many more to come!

At first glance, **NO Binance API key** is needed to call public endpoints like current crypto prices and 24h stats.  
It **only** requires a [Binance API key](#binance-api-key) for open/done orders lists, but a **READ-ONLY** API key is enough for everything to work.  
In deed, I _personally recommend_ to generate a **READ-ONLY** API key at Binance site [here](https://www.binance.com/en/usercenter/settings/api-management).  
It does **NOT need** write/trade access **in ANY way** to properly work with all its features, so don't give extra permissions if they aren't needed!

I think and hope that many of you will find it as useful as it is for myself.  
Enjoy, cheers! :beers:


## How can I use it in my Google Spreadsheets?

First of all, open your desired Google Spreadsheet and configure it properly:
1. Go to `File -> Spreadsheet settings`.
2. Under the `Calculation` tab set the `Recalculation` combobox to `On change and every minute`.
3. Hit `Save settings` button and that's it!

### The quick'n easy way for everybody

Just [download](https://github.com/diegomanuel/binance-to-google-sheets/releases/latest/download/BINANCE.gs) the latest released **all-in-one** `BINANCE.gs` file and **copy & paste** its contents following these steps:

1. With your desired `Google Spreadsheet` opened, go to `Tools -> Script editor`.
    * It should open a new page with a `Code.gs` file containing an empty function.
2. Remove any contents from `Code.gs` and paste the contents from the downloaded `BINANCE.gs` file.
3. Save the project at `File -> Save`. Give any name you want.
4. Refresh/reload your Google Spreadsheet (hit `F5`) and voila!

### If you are a developer  =]

You will need [node](https://nodejs.org) and [clasp](https://github.com/google/clasp) in order to apply the `add-on` to your Google Spreadsheets.  

1. Install `node` and `clasp` following their simple setup guides.
2. Clone the `repo` and login to your Google account with `clasp` by running: `clasp login`.
3. Get the `Script ID` for your desired Google Spreadsheet.
    1. With your `Google Spreadsheet` open, go to `Tools -> Script editor`.
    2. At the `Google Script` screen, go to `File -> Project properties`.
    3. The needed ID is the one under the `Script ID` label!
4. Just for the first time, run the target `make setup SCRIPT_ID=my-script-id` replacing `my-script-id` with the ID obtained at point `3`.
    * It should create the file `.clasp.json` with your `scriptId` inside for future use.
    * **NOTE:** You only need to re-run this step if you want to change the configured `scriptId`.
5. Now you can run `make push` (or just `make` alone) to upload/apply local code to your desired Google Spreadsheet!
    * From now on, you can just run `make` to keep applying changes to the same configured spreadsheet.
    * **TIP:** You can run `make update` to pull latest changes from this repo and push them to your configured spreadsheet.
6. Refresh/reload your Google Spreadsheet (hit `F5`) and voila!

**Windows users:** You can download and use `make` from [here](https://sourceforge.net/projects/gnuwin32/files/make/3.81/) or even the entire [GnuWin](https://sourceforge.net/projects/gnuwin32/) toolset.


### Binance API Key

Only needed if you **also want** to have open/pending and done/finished orders listing working in your spreadsheet.  
To get your keys, go to [Binance API panel](https://www.binance.com/en/usercenter/settings/api-management) and create a new one:

1. Enter a label like `Binance to Google Sheets` and click the `Create` button.
2. Take note for both `API Key` and `Secret Key` values.
3. Click the `Edit restrictions` button.
    1. Under `API restrictions` **ONLY** `Can Read` checkbox should be checked.  
    2. Under `IP access restrictions` select `Unrestricted`.
4. Click the `Save` button and now you have to configure them on your spreadsheet!

Once you have the `add-on` already installed/enabled on your desired Google Spreadsheet, the main menu item `Binance` should have appeared at the rightmost position.

1. At spreadsheet's main menu, go to `Binance -> Setup API Keys`.
2. Set your `API Key` and click `OK`. Do the same for `API Secret Key`.
3. Voila, you are ready to go!

**NOTE:** You can remove or re-configure them at any time from the `Binance` main menu item in your spreadsheet.


## OK, I have it installed! How do I use it at my spreadsheet?

**NOTE:** Check the `Examples` sheet in the [live DEMO](https://docs.google.com/spreadsheets/d/1AcOcPFsncrDB_ve3wWMHwfiFql6A4hmG1sFc01LLTDg/edit#gid=1522299933)  spreadsheet for more details.

You just need to call the `=BINANCE()` formula in a cell.  
So far, these are the available operations:

* `=BINANCE("last_update")` will return the timestamp of the last request to Binance API (**public**, no API keys needed).
* `=BINANCE("prices")` will return a list with the latest ticker prices from Binance (**public**, no API keys needed).
    * `=BINANCE("prices", "BTC")` Optionally you can give a symbol to just return its price (against `USDT` by default).
    * `=BINANCE("prices", "BNB", "BTC")` Optionally you can give a ticker to compare against and to just return its price.
    * `=BINANCE("prices", A1:A3)` Optionally you can give a ticker range to return a list of symbols and prices.
        * Values must be simple symbols like `A1="BTC"`, `A2="ETH"` and `A3="LTC"`.
    * `=BINANCE("prices", A1:A3, "headers: false")` Optionally you can give more options like not returning table headers.
    * `=BINANCE("prices", A1:A3, "ticker: BNB, prices: true")` Optionally you can return only the prices (and give a ticker in the meantime).
* `=BINANCE("stats/24h", A1:A3)` will return a list with the 24hs stats for given symbols from Binance (**public**, no API keys needed).
    * A single value or a range of values is required. Values must be simple symbols like `A1="BTC"`, `A2="ETH"` and `A3="LTC"`.
    * `=BINANCE("stats/24h", A1:A3, "BTC")` Optionally you can give a ticker to match against (defaults to `USDT`).
    * `=BINANCE("stats/24h", A1:A3, "ticker: BTC, headers: false")` Optionally you can give more options like not returning table headers.
* `=BINANCE("orders/done", A1:A3)` will return a list with your latest finished BUY/SELL orders for given symbols from Binance (**private**, API keys required).
    * A single value or a range of values is required. Values must be simple symbols like `A1="BTC"`, `A2="ETH"` and `A3="LTC"`.
    * `=BINANCE("orders/done", A1:A3, "BTC")` Optionally you can give a ticker to match against (defaults to `USDT`).
    * `=BINANCE("orders/done", A1:A3, "ticker: BTC, headers: false")` Optionally you can give more options like not returning table headers.
* `=BINANCE("orders/open")` will return a list with all your pending BUY/SELL orders from Binance (**private**, API keys required).
    * `=BINANCE("orders/open", "BTCUSDT")` Optionally you can give a full ticker to filter the results.
    * `=BINANCE("orders/open", "BTCUSDT", "headers: false")` Optionally you can give more options like not returning table headers.
* `=BINANCE("version")` will return the current `Binance to Google Sheets` version you are running.


## See it working live!

A spreadsheet example using the `BINANCE()` formula:  
https://docs.google.com/spreadsheets/d/1AcOcPFsncrDB_ve3wWMHwfiFql6A4hmG1sFc01LLTDg

<img src="img/screenshot-crypto-prices-list.png" alt="Binance to Google Sheets DEMO - Prices list" title="Binance to Google Sheets DEMO - Prices list" width="400"/>
<img src="img/screenshot-crypto-24h-stats.png" alt="Binance to Google Sheets DEMO - 24h stats" title="Binance to Google Sheets DEMO - 24h stats" width="800"/>


## Some background: Why this tool had ever to come alive?!

I needed a way to have Binance data directly available at my Google Spreadsheet.  
First, I've looked for several existing solutions, but none provided me the _freedom_ and _confidence_ that I wanted for this kind of _delicate_ stuff (you know what I mean, right? =)  
So I decided to write my own code, all from scratch, with only my will and my javascript knownledge aboard..  
..and I was sooo happy with the results that I simply decided to share it to the world! :tada:


## Disclaimer

I'm just a guy that uses both services and wanted to have **Binance** data available at my personal **Google Spreadsheet**.  
I did it for myself and I liked it so much, that I decided to share it so anyone can use it!

The script only needs **READ** access keys to **Binance API**, so there is **no security concerns** about what the script is able to do at Binance in your behalf.  
It will just **retrieve** useful Binance data for your enjoyment in your spreadsheets.  =]

I'm not responsible for your private usage of this tool, although it will never cause you any problems!  
Therefore, you will agree upon your own fully responsibility at the very moment you start using this tool.

**NOTE:** If you have any concerns, please feel free to open a ticket in the [issues](https://github.com/diegomanuel/binance-to-google-sheets/issues) section or email me.


### Privacy Policy

No personal data collect and/or usage is done in any way, that's why this `add-on` doesn't require any _"controversial"_ permission from your side.  
The only _sensitive scopes_ according to **Google** are:
* `script.external_request` :: Needed to **fetch data from Binance API** into the spreadsheet (GET requests only).
* `script.scriptapp` :: Needed to **install and run 2 triggers** to keep data updated in the spreadsheet (every 1 and 5 minutes).

The script, and in the end, myself, don't need/want/wish any personal information like name, email and so on.  
**I am firmly committed to honor this pact from now and forever!**

**NOTE:** This is an _open-source_ project, so you will always be available to keep and eye to the code and audit it.  
If you have any concerns, please feel free to open a ticket in the [issues](https://github.com/diegomanuel/binance-to-google-sheets/issues) section or email me.


### Terms of Service

**This is not a service.** There is no contract nor obligations between the code/myself and you.  
The only commitment on my behalf is regarding to no personal data usage in any way.

Only **you** decide when and how to use this tool. You can remove the `add-on` anytime like any other add-on.  
You may also remove your Binance API key anytime and just use the public endpoints.

Requests to Binance API from your spreadsheets are made from your Google account on your behalf.  
No other service acts as an intermediary between your Google spreadsheet and Binance!

**NOTE:** If you have any concerns, please feel free to open a ticket in the [issues](https://github.com/diegomanuel/binance-to-google-sheets/issues) section or email me.


## Binance Account

Don't you have a **Binance** account yet?  
Just register there and start trading with its fantastic platform!  
[https://www.binance.com/en/register?ref=45140860](https://www.binance.com/en/register?ref=45140860)  
<a href="https://www.binance.com/en/register?ref=45140860" target="_blank"><img src="img/binance-join.png" alt="Join to Binance!" title="Join to Binance!"/></a>


## Enjoy - Donate - Buy me a beer!  =]

Thank you for using Binance to Google Sheets add-on!  
I really hope you enjoyed and loved it as much as I love to use it everyday.

If your love is strong enough, feel free to share it with me!  =D  
I will much appreciate any contribution and support to keep working on it.

I have plenty of ideas for new features and improvements.  
So far, this is just the first acceptable release, but much more could come..!

---

**[BTC] Bitcoin donate address** | **[ETH] Ethereum donate address** | **[LTC] Litecoin donate address** | **[BNB] Binance Coin donate address**
:---:|:---:|:---:|:---:
![Bitcoin donate address](img/BTC-donate-address.png "Bitcoin donate address") | ![Ethereum donate address](img/ETH-donate-address.png "Ethereum donate address") | ![Litecoin donate address](img/LTC-donate-address.png "Litecoin donate address") | ![Binance Coin donate address](img/BNB-donate-address.png "Binance Coin donate address")
1FsN54WNibhhPhRt4vnAPRGgzaVeeFvEnM | 0x1d047bc3e46ce0351fd0c44fc2a2029512e87a97 | LZ8URuChzyuuy272isMCrts7R7UKtwnj6a | 0x1d047bc3e46ce0351fd0c44fc2a2029512e87a97

---

This software was published and released under the **GPL-3.0 License**.

Use it wisely, happy trading!  
Diego.
