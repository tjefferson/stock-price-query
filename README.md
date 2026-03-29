# 📈 stock-price-query

An [OpenClaw](https://github.com/openclaw/openclaw) skill for querying real-time stock prices across multiple markets.

## Features

- **Multi-market support**: A-shares (Shanghai & Shenzhen), Hong Kong stocks, and US stocks
- **Auto market detection**: Automatically identifies the market based on stock code format
- **Batch query**: Query up to 20 stocks in a single call with comma-separated codes
- **Extended data**: PE ratio and market cap included when available from data source
- **Zero dependencies**: Pure Python 3 standard library, no external packages required
- **Structured output**: Returns JSON with price, change, volume, PE ratio, market cap and more

## Supported Markets

| Market | Code Format | Example |
|--------|------------|---------|
| Shanghai (SH) | 6-digit starting with 6 | `600519` (Kweichow Moutai) |
| Shenzhen (SZ) | 6-digit starting with 0/3 | `300750` (CATL) |
| Hong Kong (HK) | Up to 5 digits, or index code | `00700` (Tencent), `HSI` (Hang Seng Index) |
| US | Alphabetic ticker, or `.`-prefixed index | `AAPL` (Apple), `.IXIC` (NASDAQ) |

### Market Indices

| Index | Code | Market |
|-------|------|--------|
| SSE Composite (上证指数) | `000001` | sh |
| SZSE Component (深证成指) | `399001` | sz |
| ChiNext (创业板指) | `399006` | sz |
| Hang Seng Index (恒生指数) | `HSI` | hk |
| HSCEI (国企指数) | `HSCEI` | hk |
| NASDAQ Composite (纳斯达克) | `.IXIC` | us |
| Dow Jones (道琼斯) | `.DJI` | us |
| S&P 500 (标普500) | `.INX` | us |

## Usage

### As an OpenClaw Skill

Copy this repository to your OpenClaw skills directory:

```bash
# User-level
cp -r stock-price-query ~/.openclaw/skills/

# Or project-level
cp -r stock-price-query /path/to/project/skills/
```

Then ask naturally in OpenClaw:

- "What's the current price of AAPL?"
- "查一下贵州茅台的股价"
- "How is Tencent doing today?"

### Standalone

```bash
python3 scripts/stock_query.py <stock_code> [market]

# Examples — Individual stocks
python3 scripts/stock_query.py 600519        # A-share (auto-detect)
python3 scripts/stock_query.py AAPL          # US stock
python3 scripts/stock_query.py 00700 hk      # Hong Kong stock

# Examples — Market indices
python3 scripts/stock_query.py 000001 sh     # SSE Composite (上证指数)
python3 scripts/stock_query.py HSI           # Hang Seng Index (恒生指数)
python3 scripts/stock_query.py .IXIC         # NASDAQ Composite (纳斯达克)
python3 scripts/stock_query.py .DJI          # Dow Jones (道琼斯)

# Examples — Batch query (mixed stocks & indices)
python3 scripts/stock_query.py .IXIC,HSI,600519,AAPL
```

### Output Example

```json
{
  "code": "600519",
  "name": "贵州茅台",
  "market": "sh",
  "current_price": 1466.80,
  "change": -18.50,
  "change_percent": -1.25,
  "open": 1521.00,
  "high": 1524.40,
  "low": 1463.60,
  "prev_close": 1485.30,
  "volume": 4191300,
  "amount": 6198840000.00,
  "time": "20260224161416",
  "status": "success"
}
```

### Display Example (via OpenClaw + Feishu)

When queried through OpenClaw in Feishu, the result is displayed in a compact format:

```
📈 贵州茅台（600519.sh）

💰 当前价格：1466.80 元 | 📊 涨跌幅：-18.50 (-1.25%) ↓
📅 行情时间：2026/02/24 16:14:16
📊 今开 1521.00 | 最高 1524.40 | 最低 1463.60 | 昨收 1485.30
📦 成交量：4,191,300 | 成交额：61.99亿
```

## Project Structure

```
stock-price-query/
├── SKILL.md              # OpenClaw skill definition
├── CHANGELOG.md          # Version history
├── README.md             # This file
├── scripts/
│   └── stock_query.py    # Query script (Python 3)
└── references/
    └── api-docs.md       # API documentation
```

## Data Source

Uses Tencent Finance API (`qt.gtimg.cn`) — free, no API key required, no special headers needed.

## License

MIT
