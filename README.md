# ShoonyaApi-Amibroker

## Instructions ( for 64 bit Amibroker): 

1. Copy the ShoonyaAPI_x64.dll to Amibroker/Plugins folder
2. Copy cpprest141_2_10.dll to Amibroker folder

## Instructions ( for 32 bit Amibroker): 

1. Copy the ShoonyaAPI.dll to Amibroker/Plugins folder
2. Copy cpprest141_2_10.dll to Amibroker folder

===============================================================================
## First Login:
Enter Login Credentials provided in the Credentials window. 
NorenAmicache.dat is created on successful login and restarting Amibroker will read credentials from the same.

## AFL strategy:
One of most important aspects of AFL is that it is an array processing language. It operates on arrays (or rows/vectors) of data. 
As such you will need to build guards into your AFL so as not signal for the same data repeatedly.
Read More here.https://www.amibroker.com/guide/h_understandafl.html

ShoonyAPI.afl provides a basic framework for Entry and Exit Signals on the last candle. 
Setup the Parameters of the AFL below

stg      = ParamStr("Strategy Name","Systematic Trading");
flag     = ParamList("STATE","STOP|START");
lasttime = StrFormat("%0.f",LastValue(BarIndex()));
exch     = ParamList("Exchange","NSE|NFO|BSE|CDS");
tsym     = ParamStr("TradingSymbol","XYZ-EQ");    <=Note set this up correctly for each symbol, we donot pickup symbol being plotted.
qty      = Param("Quantity",1,1,1000,1);
prd      = ParamList("Product","I|C|M");

The following method collects the signals for each data record as per your AFL and evaluates the last record for an order signal. Example RSI_Crossover.afl
#### ShoonyaFireSignal(Buy,Sell,Short,Cover);
===============================================================================
# Place Order:
In your afl (or ShoonyaAPI.afl ) call the method when you want to send the signal to Shoonya

##NorenPlaceOrder(exch, tsym, qty, prc, trgprc, dscqty, prd, trantype, prctyp, ret, remarks);       

DONOT call this function directly in your AFL without a signal guard. This may result in multiple 

Logs:
AMIAPI_RequestLog.txt is created in Amibroker directory. Check the same for confirmations. 

===============================================================================
Note: 
Amibroker runs the AFL code a couple of times every second for entire dataset. Make sure to build in checks to fire at the correct signal. 
An example of this is provided ShoonyaAPI.afl

****
## Example Buy Limit Order
````
exch = "NSE";
tsym = "ACC-EQ";
qty = "1";
prc = Close;
trgprc = "0";
dscqty = "0";
trantype = "B";
prctyp = "LMT";
ret = "DAY";
remarks = "1234";
NorenPlaceOrder(exch, tsym, qty, prc, trgprc, dscqty, prd, trantype, prctyp, ret, remarks);       
````
## Example Sell Limit Order
````

exch = "NSE";
tsym = "ACC-EQ";
qty = "1";
prc = Close;
trgprc = "0";
dscqty = "0";
trantype = "S";
prctyp = "LMT";
ret = "DAY";
remarks = "1234";
NorenPlaceOrder(exch, tsym, qty, prc, trgprc, dscqty, prd, trantype, prctyp, ret, remarks);  
````
## Example Market Order
````

exch = "NSE";
tsym = "ACC-EQ";
qty = "1";
prc = 0;
trgprc = "0";
dscqty = "0";
trantype = "B";
prctyp = "MKT";
ret = "DAY";
remarks = "1234";
NorenPlaceOrder(exch, tsym, qty, prc, trgprc, dscqty, prd, trantype, prctyp, ret, remarks);       
````
## Example StopLoss Limit Order
````
exch = "NSE";
tsym = "ACC-EQ";
qty = "1";
prc = Close;
trgprc = "100.5";
dscqty = "0";
trantype = "B";
prctyp = "SL-LMT";
ret = "DAY";
remarks = "1234";
NorenPlaceOrder(exch, tsym, qty, prc, trgprc, dscqty, prd, trantype, prctyp, ret, remarks);       
````
