# Disclaimer

<span style="color: red;">**This site does not take responsibility for any data that is scraped or used from external sources. Users are encouraged to respect and follow the terms of service and privacy policies of the data providers. Data scraping should be conducted responsibly and in compliance with all applicable laws and regulations. The following information is provided for educational purposes only, with the aim of explaining methods for accessing real-time data. This guidance is intended solely to demonstrate data access methods from publicly available sources on a well-known Webtrader platform. Users are responsible for complying with all applicable laws, terms of service, and the platform’s data usage policies. Unauthorized data scraping or misuse of this information is strictly discouraged.**</span>

# Building an Automated Scraper for Web Trader Platforms: A Step-by-Step Guide

When developing automated trading systems or gathering market data, scraping web trader platforms can be an essential technique. Web scraping allows users to extract useful information such as stock prices, market trends, and trade history directly from a trading platform’s interface. By automating this process, developers can create bots that make real-time decisions or gather data for analysis.

In this guide, you will learn how to scrape data from a web trading platform. The guide covers everything from identifying the target elements to ensuring compliance with the platform's terms of service. We will explore key tools such as Python's BeautifulSoup, Selenium, and handling dynamic content to build a robust and efficient scraper for trading data.

## Installation of pip packages

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install packages.

```bash
pip install selenium  
# Simulate user interactions with web applications, making it ideal for tasks like automated testing, web scraping, and browser-based automation.

pip install pyodbc
# Connect to and interact with databases using the ODBC protocol, enabling execution of SQL queries and database operations.
    
pip install beautifulsoup4
# Beautiful Soup is a library that makes it easy to scrape information from web pages. It sits atop an HTML or XML parser, providing Pythonic idioms for iterating, searching, and modifying the parse tree.

pip install dateparser
# Date parsing library designed to parse dates from HTML pages

pip install pandas
# pandas is a Python package that provides fast, flexible, and expressive data structures designed to make working with "relational" or "labeled" data both easy and intuitive. It aims to be the fundamental high-level building block for doing practical, real world data analysis in Python.
```


## Create a Firefox Profile for Web Scraping

To create a new Firefox profile specifically for web scraping—ensuring that cookies and session data are maintained—follow these steps:

1. Open the Windows Run Terminal by pressing `Windows + R`.
2. Type the command `Firefox -p` and press Enter to launch the Profile Manager. Click "OK."
3. In the Profile Manager, click on "Create Profile," and give your new profile a name (for example, `scraping_profile_2828`).
4. Ensure that each Firefox profile has its own directory.
5. Use a unique Marionette port number for each profile to avoid conflicts.
6. After creating the profile, visit `about:profiles` in Firefox to validate the newly created profiles.
7. Next, go to `about:config` and search for `marionette.port`. Change the port number to your desired value (for example, `2828`).

This new profile will be used in Selenium to preserve cookies and maintain logged-in states.

## Update the Download Path for Mozilla

We will update the download path in the Mozilla profile for the data obtained from our downloaded WebTrader file during the scraping cycle.
Update the download path in the Settings to: "C:\Projects\Finance\WebTrader\downloadCSV\".

## Create a project folder and a batch file to start the Firefox profile for web scraping

Open your code editor and enter the following commands, saving your file as `StartWebTraderScrapingProfile.bat` in your project folder, for example, `C:\Projects\Finance\WebTrader\`.

```shell
@echo off
CD "C:\Program Files\Mozilla Firefox\"
start firefox.exe -marionette -start-debugger-server -P 'scraping_profile_2828'
EXIT 0
```


## Open the WebTrader Application in Your Scraping Browser

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import subprocess
import time
import os

# Start the web trader browser using a batch file
os.system(r"C:\Projects\Finance\WebTrader\StartWebTraderScrapingProfile.bat")
time.sleep(3)  # Wait for a few seconds to ensure the browser is ready

from selenium.webdriver.firefox.service import Service

# Set up Firefox options
options = webdriver.FirefoxOptions()
options.binary_location = r'C:\Program Files\Mozilla Firefox\firefox.exe'

# Initialize the WebDriver service for Firefox
webdriver_service = Service(r'C:\geckodriver\geckodriver.exe', service_args=['--marionette-port', '2828', '--connect-existing'])

# Create a WebDriver instance for Firefox
driver = webdriver.Firefox(service=webdriver_service, options=options)

# Open the desired URL
driver.get("https://app.YOUR_PUPPET_MATRIX_WEBTRADER.com/tr/")
```



## Log In to Your WebTrader Account Only Once

To access real-time or 15-minute delayed data, log in using your WebTrader username and password. You can set your WebTrader account to never expire; this is a one-time manual process. Our Mozilla browser will keep you logged in even if you close it. If you prefer to automate the login process, you can use Selenium to automatically input your username and password.


## Retrieve Real-Time Stock Data from Your WebTrader and Create New Orders

Create a script named `RetrieveStockData.py`.

### Import the necessary libraries:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver import ActionChains
from bs4 import BeautifulSoup

from datetime import datetime
from zoneinfo import ZoneInfo
import time
import dateparser

import os
import socket

import csv
import pandas as pd

import http.client
import urllib
import shutil
import pyodbc

#Helper Python script to update the database with fulfilled orders
from StockActivity import main as updateDB  
```

### Set up your iOS push notifications

```python
import http.client
import urllib

#Send push notifications when needed
def push_notification(message, html):
    #To enable HTML formatting, include an html parameter set to 1.
    try:
        conn = http.client.HTTPSConnection("api.pushover.net:443", timeout=10)
        conn.request("POST", "/1/messages.json",
          urllib.parse.urlencode({
            "token": "YOUR_TOKEN",
            "user": "YOUR_USER",
            "message": message,
            "html": html
          }), {"Content-type": "application/x-www-form-urlencoded"})
        return conn.getresponse().status
    except Exception as e:
        print(str(e))
        return 1
```


### Setting up Mozilla Firefox WebDriver to connect to an existing scraper browser

```python
from selenium.webdriver.firefox.service import Service

options = webdriver.FirefoxOptions()
options.binary_location = r'C:\Program Files\Mozilla Firefox\firefox.exe'

#Download the geckodriver.exe file and place it in the designated folder.
webdriver_service = Service(r'c:\geckodriver\geckodriver.exe', service_args=['--marionette-port', '2828', '--connect-existing'])
options.add_argument("start-maximized")
options.add_argument("disable-infobars")
options.add_argument("--disable-extensions")
options.add_argument('--no-sandbox')
options.add_argument('--disable-application-cache')
options.add_argument('--disable-gpu')
options.add_argument("--disable-dev-shm-usage")
```


### Define Database Tables and Source Files

```python
hostname = socket.gethostname()

outfile = r"C:\Projects\Finance\WebTrader\sourceCSV\liveprices.csv"
source = r"C:\Projects\Finance\WebTrader\output.csv"
cwd = os.getcwd() + "\\"
```


### Capture the Timestamp at the Start of the Scraping Cycle

```python
curr_dt = datetime.now(tz=ZoneInfo("Europe/Istanbul"))
curr_dt_iso = curr_dt.replace(tzinfo=None).replace(microsecond=0).isoformat()
curr_dt_str = str(curr_dt_iso)
nowtime = (curr_dt.hour * 60) + curr_dt.minute
weekno = curr_dt.weekday()
bizday = True

#Automatically generated bizday.txt file using Google Calendar API
with open(r"C:\Projects\Finance\scriptDB\maintenance\bizday.txt") as f:
    bizday = f.read() != "False"
```


### Update the Configuration for Test Mode

```python
TestMode = False
writeDB = True
loop = True
```


### Main Loop to Verify Whether the Market is Open

We will run `RetrieveStockData.py` as a Windows Service in an infinite loop.

```python
while not (nowtime>=595 and nowtime<=1095 and weekno<5 and bizday) and TestMode==False:
    
    with open(r"C:\Projects\Finance\scriptDB\maintenance\bizday.txt") as f:
        bizday = f.read() != "False"    
    dt = datetime.now(tz=ZoneInfo("Europe/Istanbul"))
    nowtime = (dt.hour*60) + dt.minute
    weekno = dt.weekday()
    print(f'\r Market is closed: {dt}', end='', flush=True)
    dt_str = dt.strftime('The date is %d/%m/%Y %H:%M:%S') + "\n"
    f = open("WebTraderOffline.txt", "w")
    f.write(dt_str)
    f.close()
    time.sleep(10)
```


### Connect to the Database and Launch the WebTrader Application

```python
try:
    #Establish a connection to the database.
    cnxn = pyodbc.connect("Driver={SQL Server};"f"Server={hostname};""Database=DB_NAME;""UID=DB_USER;""PWD=DB_PASSWORD;")
    
    #Establish a connection to the WebTrader application. If the connection cannot be established, raise an error.
    driver = webdriver.Firefox(service = webdriver_service)
    driver.get("https://app.YOUR_PUPPET_MATRIX_WEBTRADER.com/tr/main")
    
    main_page = ""
    for handle in driver.window_handles:
        print(handle)
        driver.switch_to.window(handle)
        if "app.YOUR_PUPPET_MATRIX_WEBTRADER.com" in driver.current_url:
            main_page = driver.current_window_handle
            print(driver.title,main_page)
    
    if main_page:
        driver.switch_to.window(main_page)
        time.sleep(5)
    else:
        raise Exception("Web application is not ready")
    
except Exception as err:
    errLog = f"Exception Occured {curr_dt_str} \n {str(err)}\n"
    print(errLog)
    f = open(f"{sys.argv[0]}.Error.txt", "a")
    f.write(errLog)
    f.close()
    time.sleep(30)
    loop = False
    pass
```

### Function Definition for Processing Stock Buy and Sell Orders

```python
def emir(sCode,amount,price,order,confirm):

    #sCode: StockCode as String
    #amount: Lot as Integer
    #price: Stock price as Decimal
    #order: String values "AL" as Buy or "SAT" as Sell
    #confirm: Boolean True/False

    #Place an order to buy or sell a stock.
    print("Starting process for: ",sCode,amount,price,order,confirm)
    curr_dt = datetime.now(tz=ZoneInfo("Europe/Istanbul"))
    curr_dt_iso = curr_dt.replace(tzinfo=None).replace(microsecond=0).isoformat()
    curr_dt_str = str(curr_dt_iso)
    nowtime = (curr_dt.hour*60) + curr_dt.minute
    weekno = curr_dt.weekday()
    #print(f"Session {curr_dt_str} PID {driver.service.process.pid}")
    
    
    #Define the XPath for the elements where data will be input.
    SembolID = '//app-equity-market-order-entry//app-symbol-selection/div/kendo-autocomplete/kendo-searchbar/input'
    FiyatID = "//app-equity-market-order-entry//span[contains(text(), 'Fiyat')]/following::input"
    MiktarID = "//app-equity-market-order-entry//span[contains(text(), 'Miktar')]/following::input"
    
    alcopyXPATH = "//app-equity-market-order-entry//div[@class='action-buttons ng-star-inserted']//span[contains(text(), 'AL')]"
    satcopyXPATH = "//app-equity-market-order-entry//div[@class='action-buttons ng-star-inserted']//span[contains(text(), 'SAT')]"
    
    confirmXPATH = '/html/body/app-root/app-main/app-window-container/div[2]/kendo-dialog/div[2]/kendo-dialog-actions/div/button[2]'
    rejectXPATH = '/html/body/app-root/app-main/app-window-container/div[2]/kendo-dialog/div[2]/kendo-dialog-actions/div/button[1]'
    successXPATH = '/html/body/app-root/app-main/app-window-container/div[2]/kendo-dialog/div[2]/kendo-dialog-actions/button'
    
    emir_sCode = driver.find_element(By.XPATH,SembolID)
    emir_sCode.clear()
    emir_sCode.send_keys(sCode)
    
    emir_price = driver.find_element(By.XPATH,FiyatID)
    emir_price.clear()
    emir_price.send_keys(price)
    
    emir_miktar = driver.find_element(By.XPATH,MiktarID)
    emir_miktar.clear()
    emir_miktar.send_keys(amount)
    
    
    if order == 'AL':
        al = driver.find_element(By.XPATH,alcopyXPATH)
        al.click()
        
        time.sleep(2)
        confirmbutton = driver.find_element(By.XPATH,confirmXPATH)
        rejectbutton = driver.find_element(By.XPATH,rejectXPATH)
        
        if confirm:
            confirmbutton.click()
            time.sleep(2)
            successbutton = driver.find_element(By.XPATH,successXPATH)
            successbutton.click()
            return f"{curr_dt_str}|{order}|{sCode}|{amount}|{price}|Confirmed\n"
        else:
            rejectbutton.click()
            return f"{curr_dt_str}|{order}|{sCode}|{amount}|{price}|Rejected\n"

    
    if order == 'SAT':
        sat = driver.find_element(By.XPATH,satcopyXPATH)
        sat.click()
        
        time.sleep(2)
        confirmbutton = driver.find_element(By.XPATH,confirmXPATH)
        rejectbutton = driver.find_element(By.XPATH,rejectXPATH)
        
        if confirm:
            confirmbutton.click()
            time.sleep(2)
            successbutton = driver.find_element(By.XPATH,successXPATH)
            successbutton.click()
            return f"{curr_dt_str}|{order}|{sCode}|{amount}|{price}|Confirmed\n"
        else:
            rejectbutton.click()
            return f"{curr_dt_str}|{order}|{sCode}|{amount}|{price}|Rejected\n"
```

### Collect Real-Time Data While the Market is Open


#### Download CSV File from Webtrader Real-Time Prices Table

```python
errCount = 0       #Identify errors in order to take action and improve the script.

while (nowtime>=595 and nowtime<=1095 and weekno<5 and loop and bizday) or TestMode == True:
        
    try:
        
        checkDownload = False
        
        # Ensure that a timestamp is recorded in each scraping cycle.
        curr_dt_loop = datetime.now(tz=ZoneInfo("Europe/Istanbul"))
        curr_dt_iso_loop = curr_dt_loop.replace(tzinfo=None).replace(microsecond=0).isoformat()
        curr_dt_str_loop = str(curr_dt_iso_loop)
        nowtime = (curr_dt_loop.hour*60) + curr_dt_loop.minute
        timestamp = curr_dt_loop.strftime("%Y%m%d%H%M%S")
        
        
        # There is no need to download data in real-time after 6 PM.
        if nowtime >1080 and TestMode == False:
            time.sleep(180)
        
        # Verify the Selenium driver session and its associated process ID (PID).        
        print(f"Session Loop {curr_dt_str_loop} PID {driver.service.process.pid}")
        
        
        try:
            
            # Delete any old downloaded files if they exist.
            if os.path.exists(r"C:\Projects\Finance\WebTrader\downloadCSV\export.csv"):
                os.remove(r"C:\Projects\Finance\WebTrader\downloadCSV\export.csv")  
            
            
            # Click on the required elements to download the CSV file.
            priceXPATH = f"//li[@title='Fiyatlar']//span[contains(text(), 'Fiyatlar')]"
            price = driver.find_element(By.XPATH,priceXPATH)
            if price:
                print("Click Fiyatlar")
                price.click()
                time.sleep(2)
            
            
            appMarketDataSheet = driver.find_element(By.XPATH,"//app-market-data-sheet")
            prices = appMarketDataSheet.find_element(By.CSS_SELECTOR, 'div[role="row"][row-index="0"]>div[role="gridcell"][col-id="last"]')
            
            if prices:
                prices.click()
                print("Right Click Symbol")
                action = ActionChains(driver)
                action.move_to_element(prices).context_click(prices).perform()
                time.sleep(2)
                pencereislemleriXPATH = "//*[contains(text(), 'Pencere İşlemleri')]"
                pencereislemleri = prices.find_element(By.XPATH,pencereislemleriXPATH)
                pencereislemleri.click()
                time.sleep(2)
                aktarXPATH = "//*[contains(text(), 'Aktar')]"
                aktar = pencereislemleri.find_element(By.XPATH,aktarXPATH)
                aktar.click()
                time.sleep(2)
                CSVCiktisiXPATH = "//*[contains(text(), 'CSV Çıktısı')]"
                CSVCiktisi = aktar.find_element(By.XPATH,CSVCiktisiXPATH)
                print("Downloading CSV...")
                CSVCiktisi.click()
                time.sleep(2)
            

            # Verify that the prices file has been downloaded successfully.
            shutil.move(r"C:\Projects\Finance\WebTrader\downloadCSV\export.csv", outfile)
            
            if os.path.exists(outfile) and (datetime.now(tz=ZoneInfo("Europe/Istanbul")).timestamp() - os.path.getmtime(outfile) <= 60):
                print("Download File successful")
                checkDownload = True
                
            else:
                raise Exception("Download failed")
                
            
        except Exception as e:
            
            errCount += 1
            exMsg = "Exception in CSV Aktar cycle: " + type(e).__name__
            print(exMsg)
            pushNotification(exMsg,0)
            
            if errCount > 8:
                break
            else:
                
                driver.switch_to.window(main_page)
                time.sleep(15)
                continue
        
        # Verify the downloaded file and convert it to a compatible format for bulk insertion into an MS SQL database.
        if checkDownload:
            
            print("Reading downloaded csv")
            
            rawFile = pd.read_csv(outfile)
            rawFile.dropna(subset=["Güncelleme"], inplace=True)
            rawFile.dropna(subset=["Son"], inplace=True)
            rawFile = rawFile.head(1)
            countCommaDecimal = sum(rawFile['Son'].str.contains(",", na=False))
            
            #print("Raw file sample")
            #print(rawFile)
            rawFile = None
            if countCommaDecimal == 0:
                print("Format: US")
                csv_input = pd.read_csv(outfile,decimal=".",thousands=",")
                
            else:
                print("Format: TR")
                csv_input = pd.read_csv(outfile,decimal=",",thousands=".")
            
            
            dtypesText = csv_input.dtypes.to_string() 
            text_file = open("C:\Projects\Finance\WebTrader\sourceCSV\dtypes.txt", "w",encoding = 'utf-8')
            text_file.write(dtypesText)
            text_file.close()
            
            csv_input.dropna(subset=["Güncelleme"], inplace=True)
            csv_input.dropna(subset=["Son"], inplace=True)
            
            if countCommaDecimal == 0:
                csv_input['Güncelleme'] = csv_input['Güncelleme'].map(lambda x: str(x)[:-21])
                csv_input['Güncelleme'] = pd.to_datetime(csv_input['Güncelleme'], format='%a %b %d %Y %H:%M:%S')
            else:
                #todayStr = datetime.today().strftime('%Y-%m-%d')
                #csv_input['Güncelleme'] = todayStr + "T" + csv_input['Güncelleme'].str[-8:]
                #csv_input['Güncelleme'] = pd.to_datetime(csv_input['Güncelleme'], format='%Y-%m-%dT%H:%M:%S')
                csv_input['Güncelleme'] = csv_input['Güncelleme'].map(lambda x: dateparser.parse(str(x),languages=['tr']))
            
            csv = csv_input[['Sembol', 'Son', 'Alış', 'Satış', 'Açılış', 'Yüksek', 'Düşük', 'Ö.Gün.K.', 'Miktar', 'Hacim', 'Güncelleme','Ağ.Ort.','Alış Miktar','Satış Miktar','Adet']]
            csv = csv[csv['Son'] > 0]
            csv_input = None
            
            #print(csv.head(1))
            print(len(csv.index))
            csv.to_csv('output.csv', index=False,header=True)
```

#### Check for Outstanding Buy/Sell Orders Generated by Excel VBA and Execute the Order
```python

            # Check if there is any pending Order           
            fileTransaction = open("TransactionLog.csv", "a")
            
            # Function to execute pending buy or sell orders for stocks.
            def BuySellOrder(sourcefilename,ordertype,orderprice):
                sourcefile = f"C:\Stocks\{sourcefilename}"
                
                if os.path.exists(sourcefile):
                    print(sourcefile)
                    data = pd.read_csv(sourcefile, encoding = 'utf8')
                    df = pd.DataFrame(data, columns=["symbol","orderLot","confirm"])
                    df = df.dropna(subset = ['symbol','orderLot','confirm'])
                    
                    
                    print(df)
                    
                    if not df.empty:
                        dt = datetime.now(tz=ZoneInfo("Europe/Istanbul"))
                        dt_iso = dt.replace(tzinfo=None).replace(microsecond=0).isoformat()
                        timefetch = str(dt_iso)
                        
                        for ind in df.index:
                            
                            if int(df['confirm'][ind]) == 1:
                                Confirm = True
                            else:
                                Confirm = False
                            Order = ordertype
                            Stock = df['symbol'][ind]
                            Amount = str(int(df['orderLot'][ind]))
                            StockIndex = csv.index[csv['Sembol']==Stock].tolist()
                            
                            PriceNum = csv.loc[StockIndex[0],orderprice]
                            bazMulti = 1
                            
                            
                            if PriceNum == 0:
                                PriceNum = csv.loc[StockIndex[0],'Son']
                                bazMulti = 2
                            
                            baz = 0
                            if PriceNum <= 19.99:
                                baz = 0.01
                                
                            elif PriceNum <= 49.99:
                                baz = 0.02

                            elif PriceNum <= 99.99:
                                baz = 0.05

                            elif PriceNum <= 249.99:
                                baz = 0.10

                            elif PriceNum <= 499.99:
                                baz = 0.25

                            elif PriceNum <= 999.99:
                                baz = 0.50
                            
                            elif PriceNum <= 2499.99:
                                baz = 1.00
                            
                            elif PriceNum >= 2500:
                                baz = 2.50
                            
                            else:
                                baz = 0
                            
                            baz = bazMulti * baz
                            
                            if ordertype == "SAT":
                                baz = baz * -1
                            
                            Price = "%.2f" % (PriceNum + baz)
                            Price = str(Price).replace(".",",")
                            
                            try:
                                print(f"Executing emir {Stock} {Amount} {Price} {Order} {Confirm}")
                                execute = emir(Stock,Amount,Price,Order,Confirm)
                            except Exception as e:
                                print(str(e))
                                return False
                            
                            fileTransaction.write(execute)
                            pushNotification(execute,0)
                            
                            
                    if os.path.exists(sourcefile):
                        os.remove(sourcefile)  
                    if os.path.exists(sourcefile):
                        os.remove(sourcefile)  
                    
                    return True
                else: 
                    return False
            
            sat = BuySellOrder("SellOrder.csv","SAT",'Alış')
            al = BuySellOrder("BuyOrder.csv","AL",'Satış')
            fileTransaction.close()
            csv = None
```

#### Write Real-Time CSV Data to Database

```python          
            dbTbl = "WebTraderLiveStocks"
            dbTblSnap = "WebTraderLiveStocksSnap"
                       
            if writeDB:
                cursor = cnxn.cursor()
                sql = f"""
                BULK INSERT {dbTbl}
                FROM '{source}'
                WITH
                (
                FIRSTROW = 2,
                FIELDTERMINATOR = ',',
                ROWTERMINATOR = '\n'
                )
                """
                cursor.execute(sql)
                print(cursor.rowcount," rows affected")
                cnxn.commit()
                
                sqlsnap = f"""
                TRUNCATE TABLE {dbTblSnap}
                BULK INSERT {dbTblSnap}
                FROM '{source}'
                WITH
                (
                FIRSTROW = 2,
                FIELDTERMINATOR = ',',
                ROWTERMINATOR = '\n'
                )
                """
                cursor.execute(sqlsnap)
                print(cursor.rowcount," rows affected")
                cnxn.commit()
                cursor.close()
```

#### Check Filled Orders and Update Database   
```python         
            
            #Check for filled orders every n minutes if the order was placed manually from a different device.
            if nowtime <= 600:
                timer = curr_dt_loop.minute % 2
            else:
                timer = curr_dt_loop.minute % 5
            
            #Verify if there were any buy or sell orders processed during this cycle.
            print(f"SAT:{sat} AL:{al}") 
            
            #Check if a buy or sell transaction occurred, but the filled orders CSV has not been downloaded from webtrader application yet.
            filetransaction_ti_m = os.path.getmtime("TransactionLog.csv")
            filefilledorders_ti_m = os.path.getmtime(r"C:\Projects\Finance\WebTrader\sourceCSV\filledorders.csv")
            
            print(f"Compare time delta between Transaction and FilledOrders Files: {filefilledorders_ti_m-filetransaction_ti_m} seconds")
            
            # Verify if there are any filled orders, including both transacted orders and manually processed orders.
            if sat or al or (timer == 0) or (filetransaction_ti_m > filefilledorders_ti_m) :
                
                time.sleep(5)
                fileFilledOrders = r"C:\Projects\Finance\WebTrader\sourceCSV\filledorders.csv"
                ti_m = os.path.getmtime(fileFilledOrders)
                ti_today = curr_dt_loop.strftime("%Y-%m-%d 09:50")
                ti_today_dt = datetime.strptime(ti_today, "%Y-%m-%d %H:%M") 
                
                print(ti_today_dt)
                countLastFileFilledOrders = 0
                countCurrentFileFilledOrders = 0
                

                # Count the number of last filled orders from today
                if os.path.exists(fileFilledOrders) and ti_m > ti_today_dt.timestamp():
                    dfFileFilledOrders = pd.read_csv(fileFilledOrders)
                    countLastFileFilledOrders = len(dfFileFilledOrders)
                
                countErr = 0
                downloadFilledOrders = False   #Set the flag to false for download, and check the status at the end of the while loop. 
                while countErr < 3:
                    try:
                        try:
                            FooterGoster = driver.find_element(By.XPATH,"//app-footer//button[@title='Göster']")
                            if FooterGoster:
                                FooterGoster.click()
                                time.sleep(2)
                        except:
                            pass
                        
                        appEquityMarketPortfolio = driver.find_element(By.XPATH,"//app-equity-market-portfolio")
                        
                        hesapozeti = appEquityMarketPortfolio.find_element(By.XPATH,f"//*[contains(text(), 'Hesap Özeti')]")
                        if hesapozeti:
                            print("Click Hesap Ozeti")
                            hesapozeti.click()
                            time.sleep(2)
                        else:
                            raise Exception("Hesap Ozeti is not present")
                        
                        accountinfo = driver.find_element(By.XPATH,"//app-equity-market-account-info")
                        if accountinfo:                            
                            T2nakit = accountinfo.find_element(By.XPATH,f"//*[contains(text(), 'T2 nakit değeri')]/following::div")
                            if T2nakit:
                                T2nakitVal = T2nakit.text.replace(",","")
                                T2file = open("T2Nakit.csv","w")
                                T2file.write(T2nakitVal)
                                T2file.close()
                            else:
                                raise Exception("T2 Nakit is not present")
                        
                        emirlerim = appEquityMarketPortfolio.find_element(By.XPATH,f"//*[contains(text(), 'Emirlerim')]")
                        if emirlerim:
                            print("Emirlerim Tab is present")
                            emirlerim.click()
                            time.sleep(2)
                        else:
                            raise Exception("Emirlerim is not present")
                        
                        
                        bekleyenler = appEquityMarketPortfolio.find_element(By.XPATH,"//*[contains(text(), 'Bekleyenler')]")
                        if bekleyenler:
                            print("Click Bekleyenler")
                            bekleyenler.click()
                            time.sleep(2)
                        else:
                            raise Exception("Bekleyenler element is not found")
                        
                        
                        gerceklesenler = appEquityMarketPortfolio.find_element(By.XPATH,"//*[contains(text(), 'Gerçekleşenler')]")
                        if gerceklesenler:
                            print("Gerceklesen Tab is present")
                            gerceklesenler.click()
                            time.sleep(2)
                        else:
                            raise Exception("Gerceklesen Tab is not present")
                        
                        
                        # Scrape the filled orders table and parse it using BeautifulSoup.
                        try:
                            filledorders = appEquityMarketPortfolio.find_element(By.XPATH,"//app-equity-filled-orders//div[@data-ref='eBody']//div[@data-ref='eContainer'][@role='rowgroup'][@class='ag-center-cols-container']")
                            filledordersContainer = filledorders.get_attribute('outerHTML')
                            
                            #print(filledordersContainer)
                            soup = BeautifulSoup(filledordersContainer,'html.parser')
                            gerceklesenRows = soup.find_all("div",attrs={"role": "row"})
                            for i in gerceklesenRows:
                                print(i.get_text("|", strip=True))
                            print(f"Count rows in Gerceklesenler: {len(gerceklesenRows)}")
                        except:
                            gerceklesenRows = []
                            pass
                        
                        print(f"FilledOrdersLastDownload:{countLastFileFilledOrders} and GerceklesenRows: {len(gerceklesenRows)}")
                        
                        # If there are any new filled orders, download them as a CSV file.
                        if len(gerceklesenRows)>countLastFileFilledOrders or al or sat:
                            downloadfileFilledOrders = r"C:\Projects\Finance\WebTrader\downloadCSV\export.csv"
                            
                            if os.path.exists(downloadfileFilledOrders):
                                os.remove(downloadfileFilledOrders)
                                
                            print("Downloading filledorders...")
                            
                            filledordersROW = appEquityMarketPortfolio.find_element(By.XPATH,"//app-equity-filled-orders//div[@data-ref='eBody']//div[@data-ref='eContainer'][@role='rowgroup']//div[@row-index='0']")
                            filledordersROW.click()
                            action = ActionChains(driver)
                            
                            action.context_click(filledordersROW).perform()
                            time.sleep(1)
                            
                            aktarXPATH = "//*[contains(text(), 'Aktar')]"
                            aktar = filledordersROW.find_element(By.XPATH,aktarXPATH)
                            aktar.click()
                            print("Clicked Aktar")
                            time.sleep(1)
                            
                            CSVCiktisiXPATH = "//*[contains(text(), 'CSV Çıktısı')]"
                            CSVCiktisi = aktar.find_element(By.XPATH,CSVCiktisiXPATH)
                            CSVCiktisi.click()
                            print("Clicked CSV")
                            time.sleep(3)
                            
                            if os.path.exists(downloadfileFilledOrders):
                                downloadFilledOrders = True
                        
                        
                    except:
                        print("Exception occurred during checking filled orders")
                        downloadfile = r"C:\Projects\Finance\WebTrader\downloadCSV\export.csv"
                        if os.path.exists(downloadfile):
                            os.remove(downloadfile)  
                        countErr += 1
                        driver.get("https://app.YOUR_PUPPET_MATRIX_WEBTRADER.com/tr/main")
                        time.sleep(5)
                        continue
                    
                    try:
                        FooterGizle = driver.find_element(By.XPATH,"//app-footer//button[@title='Gizle']")
                        if FooterGizle:
                            FooterGizle.click()
                            
                    except:
                        pass
                    
                    break  #Exit the while loop if the iteration completes successfully.
                    
                # Update the database if new filled orders have been downloaded successfully.
                if downloadFilledOrders:
                    shutil.move(r"C:\Projects\Finance\WebTrader\downloadCSV\export.csv", fileFilledOrders)
                    if countCommaDecimal == 0:
                        dfFileFilledOrders = pd.read_csv(fileFilledOrders,decimal=".",thousands=",")
                    else:
                        dfFileFilledOrders = pd.read_csv(fileFilledOrders,decimal=",",thousands=".")
                    countCurrentFileFilledOrders = len(dfFileFilledOrders)
                    deltaFilledOrders = countCurrentFileFilledOrders-countLastFileFilledOrders

                    if deltaFilledOrders>0:
                        dfNewFilledOrders = dfFileFilledOrders.head(deltaFilledOrders)
                        fnameNewFilledOrders = f"C:\\Projects\\Finance\\WebTrader\\sourceCSV\\newfilledorders_{timestamp}.csv"
                        dfNewFilledOrders.to_csv(fnameNewFilledOrders,index=False,header=True)
                        updateDB()
```
### Notify if the scraping cycle encounters an error.
```python    
    except Exception as err:
        
        errCount += 1
        errLog = f"Exception Occured in Loop Count: {errCount} {curr_dt_str_loop} \n {str(err)}\n"
        print(errLog)
        f = open(f"{sys.argv[0]}.Error.txt", "w")
        f.write(errLog)
        f.close()
        continue
```

<style type="text/css">
.markdown-body {
  max-width: 80% !important;
  margin: auto;
}


</style>

