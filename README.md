Bitcoin dealer
==============

Bitcoin dealer is simple Django program (trading bot) for trading bitcoins currently only on MtGox exchange. 

Bitcoin dealer supports MtGox API version 1 (https://en.bitcoin.it/wiki/MtGox/API) with following currencies:
USD, EUR, GBP, PLN, CAD, AUD, CHF, CNY, NZD, RUB, DKK, HKD, SGD, THB, JPY and SEK.

Although program is really simple, it supports [stop orders](https://en.wikipedia.org/wiki/Order_%28exchange%29#Stop_orders) and maybe something more, but I am not really familiar with trading techniques and jargon.

Program is most useful for those who do not have time to watch price of bitcoin all the time, have a PC running all the time and know how to write few "Hello worlds" with two if cases.

Installation
------------
* Register to MtGox - https://mtgox.com/
* Install Django - https://www.djangoproject.com/
* Run: git clone git@github.com:rokj/bitcoin_dealer.git
* Install and configure database you will use.
* Change database settings in settings.py.
* Run: python manage.py syncdb This will create database schema, tables and load intial_data.json to database.
* Change following variables in settings.py (under EXCHANGES):
 - mtgox -> key = 'you get this in mtgox admin page'
 - mtgox -> secret = 'you get this also in mtgox admin page'
 - check_interval = 7
 - bd_debug = True
* Set DEBUG to True in settings.py and set TEMPLATE_DEBUG to DEBUG.
* Start trading script with in root bitcoin dealer folder like:
  python -u scripts/dealing.py
* Start webserver in bitcoin_dealer's folder like:
  python manage.py runserver 8000
* Goto http://127.0.0.1:8000/admin/ and trade.

Troubleshooting
---------------
If paths for running script cannot be found and you get errors, set following
variables:
 export DJANGO_SETTINGS_MODULE=bitcoin_dealer.settings

 export PYTHONPATH=$PYTHONPATH:/your_path_to_one_path_less_than_bitcoin_dealer/ 

bitcoin_dealer is the name of the folder you have cloned this project. If you have bitcoin_dealer in **/programs/bitcoin_dealer/** then you should export **PYTHONPATH=$PYTHONPATH:/programs/**.

Info
----
Right now you should not change status of trades, since API v1 still does not support that (but it will have effect on other related trades if you change status from **selling** to **sold** for example). Just leave it as it is. If you would like cancel trade, go to your MtGox account and cancel trades/orders; but be careful to deactivate/cancel related first.

Check https://en.bitcoin.it/wiki/MtGox/API what different verions of API support.

WARNING
-------
Watch out, settings.py should be accessible only to you, your account and with your permissions.

Program works correctly only if you have enough funds (bitcoins, $, €, ...) on your MtGox account. There is no checking if you do not have enough coins or money on your account.

Every time you do "python manage.py syncdb" tables exchange_currency, exchange_exchange and exchange_exchange_currencies will be overwritten by data from initial_data.json.

There is no south (http://south.aeracode.org/) or similar support implemented. There are database differences between revisions. At worst case, backup old database, do "python manage.py syncdb" and "restore by hand".

You like these few lines of code, you find it useful or want new feature? 
----------------------------------------
You can donate a bitcoin or few:
1MC1BSkwD45gAuQ8mvXtD1RZWhNtxGV1ho

Help
----
See AUTHORS.

Developers
----------
With little effort, support for other exchanges can be added also. Database models and program itself is designed with that in mind. However, script which is responsible for trading (scripts/dealing.py), by the time of this writing supports only MtGox exchange.