---
title: "python用yfinance抓取数据"
layout: blog
excerpt: "yfinance是利用雅虎财经的数据模块，可以作为一个用python分析美股的数据来源。"
read_time: true
comments: true
share: true
# author_profile: true
classes: wide
categories:
  - 欧耶之AI
tags:
  - yfinance
---

yfinance是利用雅虎财经的数据模块，可以作为一个用python分析美股的数据来源。

这是一篇[python使用yfinance最新教程2024](https://analyzingalpha.com/yfinance-python)，熟悉一下yfinance的基本功能。下面主体内容是摘自这篇文章。

## 使用需要的库：
* ython >= 2.7, 3.4+
* Pandas (tested to work with >=0.23.1)
* Numpy >= 1.11.1
* requests >= 2.14.2
* lxml >= 4.5.1
* pandas_datareader >= 0.4.0

## yfinance模块介绍
### yfinance的几大功能块:
![yfinance的几大功能块](https://image.olim.cc/ai.olim.ca/2024/yfinance-classes-and-methods.jpg)

### Ticker
yfinance下ticker获取资料方式：
![ticker获取资料方式](https://image.olim.cc/ai.olim.ca/2024/yfinance-ticker-methods.jpg)

#### 获取单只股票历史数据：
```
def history(self, period="1mo", interval="1d",
            start=None, end=None, prepost=False, actions=True,
            auto_adjust=True, back_adjust=False,
            proxy=None, rounding=False, tz=None, timeout=None, **kwargs):
    """
    :Parameters:
        period : str
            Valid periods: 1d,5d,1mo,3mo,6mo,1y,2y,5y,10y,ytd,max
            Either Use period parameter or use start and end
        interval : str
            Valid intervals: 1m,2m,5m,15m,30m,60m,90m,1h,1d,5d,1wk,1mo,3mo
            Intraday data cannot extend last 60 days
        start: str
            Download start date string (YYYY-MM-DD) or _datetime.
            Default is 1900-01-01
        end: str
            Download end date string (YYYY-MM-DD) or _datetime.
            Default is now
        prepost : bool
            Include Pre and Post market data in results?
            Default is False
        auto_adjust: bool
            Adjust all OHLC automatically? Default is True
        back_adjust: bool
            Back-adjusted data to mimic true historical prices
        proxy: str
            Optional. Proxy server URL scheme. Default is None
        rounding: bool
            Round values to 2 decimal places?
            Optional. Default is False = precision suggested by Yahoo!
        tz: str
            Optional timezone locale for dates.
            (default data is returned as non-localized dates)
        timeout: None or float
            If not None stops waiting for a response after given number of
            seconds. (Can also be a fraction of a second e.g. 0.01)
            Default is None.
        **kwargs: dict
            debug: bool
                Optional. If passed as False, will suppress
                error message printing to console.
    """
```

举例：
```
data = goog.history(interval='1m', start='2022-01-03', end='2022-01-10')
data.head()
```

#### 下载多只股票历史数据：
```
def download(tickers, start=None, end=None, actions=False, threads=True,
             group_by='column', auto_adjust=False, back_adjust=False,
             progress=True, period="max", show_errors=True, interval="1d", prepost=False,
             proxy=None, rounding=False, timeout=None, **kwargs):
    """Download yahoo tickers
    :Parameters:
        tickers : str, list
            List of tickers to download
        period : str
            Valid periods: 1d,5d,1mo,3mo,6mo,1y,2y,5y,10y,ytd,max
            Either Use period parameter or use start and end
        interval : str
            Valid intervals: 1m,2m,5m,15m,30m,60m,90m,1h,1d,5d,1wk,1mo,3mo
            Intraday data cannot extend last 60 days
        start: str
            Download start date string (YYYY-MM-DD) or _datetime.
            Default is 1900-01-01
        end: str
            Download end date string (YYYY-MM-DD) or _datetime.
            Default is now
        group_by : str
            Group by 'ticker' or 'column' (default)
        prepost : bool
            Include Pre and Post market data in results?
            Default is False
        auto_adjust: bool
            Adjust all OHLC automatically? Default is False
        actions: bool
            Download dividend + stock splits data. Default is False
        threads: bool / int
            How many threads to use for mass downloading. Default is True
        proxy: str
            Optional. Proxy server URL scheme. Default is None
        rounding: bool
            Optional. Round values to 2 decimal places?
        show_errors: bool
            Optional. Doesn't print errors if True
        timeout: None or float
            If not None stops waiting for a response after given number of
            seconds. (Can also be a fraction of a second e.g. 0.01)
    """
```

举例：
```
data = yf.download(['GOOG','META'], period='1mo')
data.head()
```

把上面下载数据group一下：
```
data = yf.download(['GOOG','META'], start='2021-12-10', end='2021-12-30', group_by='ticker')
data.head()
```

#### 获取基本面信息：
基本面信息范畴有：
![基本面信息](https://image.olim.cc/ai.olim.ca/2024/ticker-fundamental-methods.webp)

举例：
```
dhr = yf.Ticker('DHR')
info = dhr.info
info.keys()
```
```
info['sector']
'Healthcare'
```
```
dhr.earnings
```
```
dhr.get_financials()
```
```
pnl = dhr.financials
bs = dhr.balancesheet
cf = dhr.cashflow
fs = pd.concat([pnl,bs,cf])
print(fs)
```
```
fs.T
```

由于没有一次下载多个股票基本面信息的方式，所以要用loop去获取。
```
tickers = ['FB','AMZN','NFLX','GOOG']
tickers
```
```
tickers = [yf.Ticker(ticker) for ticker in fang]
```
```
[yfinance.Ticker object <FB>,
 yfinance.Ticker object <AMZN>,
 yfinance.Ticker object <NFLX>,
 yfinance.Ticker object <GOOG>]
```
```
dfs = [] # list for each ticker's dataframe
for ticker in tickers:
    # get each financial statement
    pnl = ticker.financials
    bs = ticker.balancesheet
    cf = ticker.cashflow

    # concatenate into one dataframe
    fs = pd.concat([pnl, bs, cf])

    # make dataframe format nicer
    # Swap dates and columns
    data = fs.T
    # reset index (date) into a column
    data = data.reset_index()
    # Rename old index from '' to Date
    data.columns = ['Date', *data.columns[1:]]
    # Add ticker to dataframe
    data['Ticker'] = ticker.ticker
    dfs.append(data)
data.iloc[:,:3]# for display purposes
```
整理数据：
```
parser = pd.io.parsers.base_parser.ParserBase({'usecols': None})

for df in dfs:
     df.columns = parser._maybe_dedup_names(df.columns)
df = pd.concat(dfs, ignore_index=True)
df = df.set_index(['Ticker','Date'])
df.iloc[:,:5] # for display purposes
```

#### 获取期权数据：
使用Ticker.options和Ticker.option_chain方式：
![获取期权方式](https://image.olim.cc/ai.olim.ca/2024/yfinance-options-methods.jpg)
分别给出下面信息：
* options 返回到期日信息。
* option_chain 返回期权链信息。

期权链包括：
![期权链模块](https://image.olim.cc/ai.olim.ca/2024/yfinance-options-object-methods.jpg)

举例（获得call/put信息）：
```
calls = options.calls
calls
```
```
puts = options.puts
puts
```
#### 获取机构持股信息
```
aapl.insitutional_holders
```

## 数据提供商
[Nasdaq](https://www.nasdaq.com/solutions/data-link-api)数据提供商。。。。。这里有一篇[Nsadaq DATA Link API使用教程](https://analyzingalpha.com/nasdaq-data-link-quandl-python-api)。

[polygon.io](https://polygon.io/)数据的免费版提供5个查询。提供将顶级REST和流式股票市场API的强大功能无缝数据集成。。。。。这里有一篇[Polygon API使用教程](https://analyzingalpha.com/polygon-api-python)。

[IEX](https://www.iexcloud.io/)数据提供商。。。。。。这里有一篇[IEX API教程](https://analyzingalpha.com/iex-cloud-python-tutorial)

[Alpaca Markets](https://alpaca.markets/)数据提供商，也有免费版，15分钟延迟。。。。。。这里有一篇[Alpaca Market API使用教程](https://analyzingalpha.com/alpaca-markets-api-python-tutorial)。

## 额外信息
* [yfinance Python数据可视化](https://analyzingalpha.com/python-data-visualization)

* [用yfinance数据计算option greeks的一个pdf文档](/assets/pdf/Option_Greeks.pdf)取自[github仓库-期权希腊值计算](https://github.com/AmirDehkordi/OptionGreeks)。

* [一个金融数据分析博客](https://www.codearmo.com/)。。。找到这博客是因为搜索option greek找出来[期权交易](https://www.codearmo.com/python/options-trading)这篇文章。

**注：会继续更新**