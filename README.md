# Extract-ALL-Stock-Data-SP500 ; Stoxx600 ; Daxx40 ; FTSE100 ; Japan 180 ; Brazil MSCI 25/50 ; France 40 ; Mexico 25/50 ; Portugal 20 ;  -All 1000 Tickers 50 Years Data Daily Interval

S&P500 USA ; Stoxx600 ; Dax40 Germany ; FTSE100 ; Japan 180 ;  MSCI Spain 57; Brazil MSCI 25/50 ; France 40 ; Mexico 25/50 ; Portugal 20 has been completed the procedure, all other ETF's have now been started as project. 

Extract ALL Stock Data S&amp;P500 All 500 Ticker 50 Years Data Daily Interval. Can be configured easily to do more than 50 year data on daily interval
50 Years * 365 days * 500 Companies = 9.125.000 Excel Data Cells and yes due to not every company being 50 years old it will be >4,5 million cells total. -did not count yet - 

Stoxx600 its format has been completed, the list should contain 600 stocks compared to the current 161 , to be completed. 

the code to always be up-to-date without you manually changing the date, you can use:

Python
from datetime import datetime
end = datetime.today().strftime('%Y-%m-%d')

To be implemented. The current code in main file works. thus till now did not test this upgrade "end = datetime.today().strftime('%Y-%m-%d')".

Main Code 

import pandas as pd
import yfinance as yf
import os
import time

# Your Ticker List
tickers = ["NVDA", "AAPL", "GOOGL", "GOOG", "MSFT", "AMZN", "META", "AVGO", "TSLA", "BRK-B", "WMT", "LLY", "JPM", "XOM", "V", "JNJ", "MU", "MA", "COST", "ORCL", "ABBV", "BAC", "HD", "PG", "CVX", "GE", "CAT", "KO", "NFLX", "AMD", "PLTR", "CSCO", "LRCX", "MRK", "AMAT", "GS", "PM", "MS", "RTX", "WFC", "UNH", "IBM", "AXP", "TMUS", "MCD", "LIN", "PEP", "GEV", "INTC", "VZ", "C", "AMGN", "TXN", "KLAC", "T", "ABT", "NEE", "TMO", "GILD", "DIS", "APH", "BA", "DE", "ISRG", "BLK", "TJX", "CRM", "ADI", "SCHW", "ANET", "UNP", "LOW", "HON", "QCOM", "UBER", "PFE", "LMT", "BX", "DHR", "SYK", "WELL", "ETN", "APP", "COP", "PLD", "NEM", "ACN", "CB", "COF", "BKNG", "PH", "SPGI", "MDT", "BMY", "PANW", "VRTX", "GLW", "PGR", "HCA", "MCK", "MO", "CMCSA", "CME", "SBUX", "BSX", "NOW", "CEG", "ADBE", "INTU", "SO", "HWM", "TT", "NOC", "UPS", "DUK", "CRWD", "CVS", "NKE", "WDC", "SNDK", "GD", "PNC", "WM", "FCX", "MAR", "STX", "FDX", "EQIX", "USB", "KKR", "WMB", "SHW", "JCI", "MMM", "AMT", "ICE", "MRSH", "ADP", "ECL", "RCL", "ITW", "SNPS", "EMR", "CRH", "PWR", "CMI", "MNST", "BK", "DELL", "CDNS", "REGN", "CTAS", "MCO", "ORLY", "CSX", "SPG", "ABNB", "MSI", "CL", "DASH", "SLB", "ELV", "TDG", "MDLZ", "CI", "CVNA", "GM", "KMI", "HLT", "WBD", "NSC", "COR", "AEP", "AON", "APO", "TEL", "HOOD", "RSG", "PCAR", "EOG", "LHX", "TFC", "TRV", "ROST", "APD", "PSX", "AZO", "BKR", "DLR", "VLO", "SRE", "O", "FTNT", "AFL", "NXPI", "MPWR", "VST", "MPC", "URI", "D", "F", "AJG", "OKE", "ZTS", "PSA", "CARR", "ALL", "AME", "GWW", "FAST", "TGT", "CAH", "BDX", "MET", "FIX", "CTVA", "OXY", "TER", "IDXX", "FANG", "EA", "TRGP", "CMG", "EXC", "XEL", "ADSK", "GRMN", "DHI", "CIEN", "ETR", "NDAQ", "EW", "COIN", "WAB", "YUM", "DAL", "HSY", "ROK", "CCL", "SYY", "AIG", "AMP", "CBRE", "PEG", "ODFL", "MCHP", "KR", "KEYS", "MLM", "EL", "NUE", "TKO", "VTR", "DDOG", "PCG", "ARES", "KDP", "MSCI", "VMC", "ED", "EBAY", "HIG", "LVS", "NRG", "GEHC", "PYPL", "CCI", "LYV", "EQT", "RMD", "IR", "WEC", "TTWO", "UAL", "HBAN", "EME", "WDAY", "KMB", "OTIS", "PRU", "KVUE", "ROP", "STT", "FITB", "CPRT", "ACGL", "A", "MTB", "AXON", "TPL", "EXR", "DG", "IBKR", "FISV", "PAYX", "CHTR", "WAT", "ADM", "IRM", "VICI", "FICO", "XYZ", "TPR", "DOV", "XYL", "RJF", "CTSH", "TDY", "AEE", "ULTA", "CBOE", "DTE", "ATO", "ROL", "HAL", "FE", "KHC", "LEN", "WTW", "JBL", "HPE", "EIX", "PPG", "STLD", "BIIB", "DXCM", "IQV", "CNP", "HUBB", "MTD", "TSCO", "CFG", "PPL", "ES", "DVN", "ON", "STZ", "NTRS", "PHM", "WRB", "DLTR", "RF", "EXE", "FSLR", "OMC", "WSM", "LUV", "SYF", "FIS", "SW", "CINF", "AWK", "VRSK", "DRI", "AVB", "EXPE", "IP", "CPAY", "STE", "KEY", "CHD", "FOXA", "EQR", "GIS", "Q", "EFX", "CTRA", "BRO", "BG", "AMCR", "RL", "CMS", "LH", "FOX", "GPN", "VLTO", "HUM", "L", "CHRW", "TSN", "DGX", "NI", "LULU", "LDOS", "DOW", "JBHT", "CNC", "SBAC", "PKG", "NVR", "CSGP", "EXPD", "IFF", "TROW", "PFG", "BR", "DD", "NTAP", "INCY", "SNA", "ALB", "VRSN", "MRNA", "LII", "SMCI", "ZBH", "EVRG", "PTC", "MKC", "VTRS", "FTV", "LNT", "LYB", "WY", "BALL", "TXT", "WST", "HII", "HPQ", "PODD", "ESS", "APTV", "DECK", "HOLX", "PNR", "COO", "GPC", "J", "NDSN", "TRMB", "CDW", "MAA", "FFIV", "KIM", "INVH", "IEX", "MAS", "AVY", "CF", "CLX", "BEN", "UHS", "REG", "ERIE", "SWK", "HAS", "HST", "ALLE", "EG", "BF-B", "HRL", "AKAM", "ALGN", "NWS", "TYL", "GEN", "BBY", "GNRC", "UDR", "NWSA", "DPZ", "SOLV", "ZBRA", "GDDY", "BLDR", "TTD", "DOC", "WYNN", "SJM", "PNW", "AES", "PSKY", "IVZ", "GL", "JKHY", "CPT", "RVTY", "AIZ", "BAX", "NCLH", "IT", "BXP", "AOS", "APA", "DVA", "MGM", "TAP", "HSIC", "MOS", "FRT", "ARE", "SWKS", "TECH", "CAG", "CRL", "POOL", "CPB", "MOH", "EPAM", "MTCH", "FDS", "LW", "PAYC"]

print("Phase 1: Ranking Tickers by Market Cap Weight...")

# Create a list to store market caps
cap_list = []

# Using yf.Tickers (plural) is faster for bulk info
bulk_tickers = yf.Tickers(" ".join(tickers))

for t in tickers:
    try:
        # Get market cap for the specific ticker
        m_cap = bulk_tickers.tickers[t].info.get('marketCap', 0)
        cap_list.append({'Ticker': t, 'MarketCap': m_cap})
    except Exception:
        cap_list.append({'Ticker': t, 'MarketCap': 0})

# Sort list by MarketCap descending
sorted_df = pd.DataFrame(cap_list).sort_values(by='MarketCap', ascending=False)
ordered_tickers = sorted_df['Ticker'].tolist()

print(f"Sorting complete. Top stock is {ordered_tickers[0]}. Bottom stock is {ordered_tickers[-1]}.")

# Phase 2: Download Daily Data
start = "1976-01-01"
end = "2026-02-21"

print("\nPhase 2: Downloading 50 years of daily data (Ordered by Weight)...")
data = yf.download(ordered_tickers, start=start, end=end, auto_adjust=True)["Close"]

# Reindex columns to match our ordered_tickers exactly
# This ensures the CSV order matches our weight order
data = data[ordered_tickers]

# Transpose so Tickers are rows and Dates are columns
df_wide = data.transpose()

# Clean Date headers (YYYY-MM-DD)
df_wide.columns = df_wide.columns.strftime('%Y-%m-%d')

# Save to CSV
file_name = "sp500_WEIGHTED_DAILY_wide.csv"
df_wide.to_csv(file_name)

print("-" * 30)
print(f"Success! Weighted file saved at: {os.getcwd()}/{file_name}")
print(f"Total Columns: {len(df_wide.columns)}")
