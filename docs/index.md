# Financial Services - Asset Portfolio Management

This notebook ingests Yahoo Finance data to S3 for processing by a Trifacta flow of the above name


```python
#pip install yfinance
import yfinance as yf
usa_top_10 = ['MSFT', 'AAPL', 'AMZN', 'GOOG', 'FB', 'WMT', 'JNJ', 'JPM', 'V', 'PG']
```


```python
def ingest_ticker(ticker, start='2019-01-01'):
    import yfinance as yf
    data = yf.download(ticker, start)
    data['Close'].to_csv(f'/tmp/stock_prices_{ticker}.csv')
    import boto3
    session = boto3.session.Session(profile_name='content')
    s3 = session.client('s3')
    s3.upload_file(f'/tmp/stock_prices_{ticker}.csv', 'trifacta-content-repository', f'ingest/stock_prices/{ticker}.csv')
```


```python
for t in usa_top_10:
    ingest_ticker(t)
```

    [*********************100%***********************]  1 of 1 completed
    [*********************100%***********************]  1 of 1 completed
    [*********************100%***********************]  1 of 1 completed
    [*********************100%***********************]  1 of 1 completed
    [*********************100%***********************]  1 of 1 completed
    [*********************100%***********************]  1 of 1 completed
    [*********************100%***********************]  1 of 1 completed
    [*********************100%***********************]  1 of 1 completed
    [*********************100%***********************]  1 of 1 completed
    [*********************100%***********************]  1 of 1 completed


### Trifacta Flow

![yahoo_finance_flow](media/yahoo_finance_flow.png)

### Output

![yahoo_finance_output](media/yahoo_finance_output.png)
