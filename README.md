# Final-Project-Analyzing-Stock-Performance-and-Building-a-Dashboard


!pip install yfinance
#!pip install pandas
#!pip install requests
!pip install lxml
#!pip install plotly 

import yfinance as yf
import pandas as pd
import requests
from bs4 import BeautifulSoup
import plotly.graph_objects as go
from plotly.subplots import make_subplots

def make_graph(stock_data, revenue_data, stock):
    fig = make_subplots(rows=2, cols=1, shared_xaxes=True, subplot_titles=("Historical Share Price", "Historical Revenue"), vertical_spacing = .3)
    fig.add_trace(go.Scatter(x=pd.to_datetime(stock_data.Date, infer_datetime_format=True), y=stock_data.Close.astype("float"), name="Share Price"), row=1, col=1)
    fig.add_trace(go.Scatter(x=pd.to_datetime(revenue_data.Date, infer_datetime_format=True), y=revenue_data.Revenue.astype("float"), name="Revenue"), row=2, col=1)
    fig.update_xaxes(title_text="Date", row=1, col=1)
    fig.update_xaxes(title_text="Date", row=2, col=1)
    fig.update_yaxes(title_text="Price ($US)", row=1, col=1)
    fig.update_yaxes(title_text="Revenue ($US Millions)", row=2, col=1)
    fig.update_layout(showlegend=False,
    height=900,
    title=stock,
    xaxis_rangeslider_visible=True)
    fig.show()
    
    Question-1
    
    #Question 1: Extracting Tesla Stock Data Using yfinance 


#1 Using the `Ticker` function enter the ticker symbol of the stock we want to extract data on to create a ticker object. 

tesla = yf.Ticker("TSLA")

#2 Using the ticker object and the function `history` extract stock information and save it in a dataframe named `tesla_data`. 

tesla_data = tesla.history(period="max")

#3 Reset the index** using the `reset_index(inplace=True)` function on the tesla_data DataFrame 

tesla_data.reset_index(inplace=True)
tesla_data.head()

Question-2

url = "https://www.macrotrends.net/stocks/charts/TSLA/tesla/revenue"
html_text = requests.get(url).text
soup = BeautifulSoup(html_text, 'lxml')
for index, table in enumerate(tables):
    if ('Tesla Quarterly Revenue'in str('table')):
        table_index = index
        for row in tables[table_index].tbody.find_all("tr"):
            col = row.find_all("td")
            if (col!=[]):
                date =col[0].text
                revenue = col[1].text.replace("$", "").replace(",", "")
                tesla_revenue = tesla_revenue.append({"Date" : date, "Revenue" : revenue}, ignore_index=True)
tesla_revenue = tesla_revenue[tesla_revenue['Revenue'] != ""]
tesla_revenue
tesla_revenue.tail()

Question-3
gamestop = yf.Ticker("GME")
gme_data = gamestop.history(period="max")
gme_data.reset_index(inplace=True)
gme_data.head()

Question-4
gme_url = "https://www.macrotrends.net/stocks/charts/GME/gamestop/revenue"
gme_html_data = requests.get(gme_url).text
gme_soup = BeautifulSoup(gme_html_data, 'lxml')
gme_tables = gme_soup.find_all('table')
for index,table in enumerate(gme_tables):
    if ("GameStop Quarterly Revenue" in str(table)):
        gme_table_index = index
        gme_revenue = pd.DataFrame(columns=["Date", "Revenue"])
        for row in gme_tables[gme_table_index].tbody.find_all("tr"):
    col = row.find_all("td")
    if (col !=[]):
        date = col[0].text
        revenue = col[1].text.replace("$", "").replace(",", "")
        gme_revenue = gme_revenue.append({"Date" : date, "Revenue" : revenue}, ignore_index=True)
        gme_revenue.tail()
        
 Question-5
 make_graph(tesla_data, tesla_revenue, 'Tesla')
 
 Question-6
 make_graph(gme_data, gme_revenue, 'GameStop')
