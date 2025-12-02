# EarnMore Platform

> Automated Equity Research & Quant Trading Assistant for Indian Markets

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-green.svg)](https://fastapi.tiangolo.com)
[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](LICENSE)

## 🎯 What is EarnMore?

EarnMore is a dual-platform system designed to monetize financial research through automation:

### 1. **Equity Research Platform**
Automated generation of professional equity research reports for Indian small-cap stocks.
- Daily screening of 100+ stocks from Screener.in
- AI-powered deep research using Perplexity API
- Standardized reports: Business overview, 4-6 quarter analysis, risk assessment, valuation
- Professional PDF generation with charts and tables
- Subscription-based delivery to HNIs and RIAs

### 2. **Quant Research Assistant**
Real-time trading research assistant with AI-powered analysis.
- SEC filing monitoring and summarization
- News aggregation with sentiment analysis
- Peer company comparison engine
- Hypothesis generation for backtesting strategies
- API access for algo traders
- Discord/Telegram bot integration

## 🏗️ Architecture

```
Data Sources → Processing Engines → Output & Delivery
    ↓              ↓                    ↓
Screener.in → Financial Analyzer → PDF Reports
Perplexity  → Risk Assessor     → API Endpoints  
SEC EDGAR   → Valuation Engine  → Email Delivery
News APIs   → Report Builder    → Subscriptions
```

## 📁 Project Structure

```
earnmore/
├── equity_research/              # Equity Research Platform
│   ├── config/
│   │   ├── settings.py          # Environment configuration
│   │   └── logging_config.py    # Structured logging
│   ├── data_sources/
│   │   ├── screener_scraper.py  # Screener.in integration
│   │   ├── perplexity_client.py # Perplexity AI research
│   │   ├── edgar_client.py      # SEC EDGAR filings
│   │   └── news_aggregator.py   # News collection
│   ├── analysis/
│   │   ├── financial_analyzer.py # Metrics calculation
│   │   ├── risk_assessor.py     # Risk scoring
│   │   ├── valuation_engine.py  # DCF & multiples
│   │   └── sector_comparator.py # Peer analysis
│   ├── report_generation/
│   │   ├── templates/           # Jinja2 HTML templates
│   │   ├── report_builder.py    # Report assembly
│   │   ├── pdf_generator.py     # PDF creation
│   │   └── email_sender.py      # Delivery system
│   └── api/
│       ├── main.py              # FastAPI app
│       └── routes/              # API endpoints
├── quant_assistant/              # Quant Research Assistant
│   ├── api/
│   │   ├── main.py              # FastAPI application
│   │   └── routes/              # Endpoints
│   ├── engines/
│   │   ├── filing_analyzer.py   # SEC filing analysis
│   │   ├── news_summarizer.py   # News processing
│   │   ├── peer_comparator.py   # Company comparison
│   │   └── hypothesis_engine.py # Strategy generation
│   └── integrations/
│       ├── discord_bot.py       # Discord integration
│       └── telegram_bot.py      # Telegram bot
├── database/
│   ├── models.py                # SQLAlchemy models
│   ├── crud.py                  # Database operations
│   └── migrations/              # Alembic migrations
├── frontend/                     # React/Next.js UI (optional)
├── docker-compose.yml
├── requirements.txt
└── .env.example
```

## 🚀 Quick Start

### Prerequisites
- Python 3.10+
- PostgreSQL 14+
- Redis 7+
- Git

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/ash4529/earnmore.git
cd earnmore
```

2. **Create virtual environment**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Configure environment**
```bash
cp .env.example .env
# Edit .env with your API keys and credentials
```

5. **Setup database**
```bash
# Start PostgreSQL and Redis (using Docker)
docker-compose up -d postgres redis

# Run migrations
alembic upgrade head
```

6. **Run the platforms**

**Equity Research Platform:**
```bash
cd equity_research
python -m api.main
# Access at http://localhost:8000
```

**Quant Assistant:**
```bash
cd quant_assistant
uvicorn api.main:app --reload --port 8001
# Access at http://localhost:8001
```

## ⚙️ Configuration (MANUAL SETUP REQUIRED)

### 1. Perplexity API Key
1. Visit: https://www.perplexity.ai/settings/api
2. Generate API key
3. Add to `.env`: `PERPLEXITY_API_KEY=pplx-xxxxx`

### 2. Screener.in Credentials
1. Create account at https://www.screener.in
2. Add to `.env`:
   ```
   SCREENER_USERNAME=your_email
   SCREENER_PASSWORD=your_password
   ```

### 3. Database Connection
```env
DATABASE_URL=postgresql://earnmore:password@localhost:5432/earnmore
```

### 4. Redis Cache
```env
REDIS_URL=redis://localhost:6379/0
```

### 5. Email Configuration (for report delivery)
```env
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your_email@gmail.com
EMAIL_PASSWORD=app_password
```

## 💡 Usage Examples

### Equity Research - Screen Small-Cap Stocks
```python
from equity_research.data_sources.screener_scraper import ScreenerClient

client = ScreenerClient()
stocks = client.screen_stocks(
    market_cap_min=1000,  # Crores
    market_cap_max=5000,
    revenue_growth_min=15,  # %
    roe_min=15
)

print(f"Found {len(stocks)} qualifying stocks")
```

### Equity Research - Generate Report
```python
from equity_research.report_generation.report_builder import ReportBuilder

builder = ReportBuilder()
report = builder.generate(
    symbol="NSE:TCS",
    quarters=6,
    include_valuation=True
)

report.save_pdf("TCS_Research_Report.pdf")
```

### Quant Assistant - Analyze Filing
```bash
curl -X POST http://localhost:8001/api/analyze/filing \
  -H "Content-Type: application/json" \
  -d '{"company": "NVIDIA", "filing_type": "10-K"}'
```

### Quant Assistant - Generate Hypotheses
```python
import requests

response = requests.post(
    "http://localhost:8001/api/generate/hypotheses",
    json={
        "market_conditions": "high_volatility",
        "sector": "technology",
        "timeframe": "intraday"
    }
)

print(response.json()["hypotheses"])
```

## 💰 Monetization Strategy

### Equity Research Platform
| Tier | Reports/Month | Features | Price |
|------|--------------|----------|-------|
| Basic | 10 | PDF reports only | ₹5,000/mo |
| Professional | 30 | PDF + Email delivery | ₹12,000/mo |
| Premium | 100 | All + Custom requests | ₹30,000/mo |
| Enterprise | Unlimited | All + API access + White-label | ₹75,000/mo |

### Quant Research Assistant
| Tier | Features | Price |
|------|----------|-------|
| Starter | Basic filing alerts, News summaries | ₹3,000/mo |
| Professional | All features + Discord access | ₹8,000/mo |
| API Access | Programmatic access for algo traders | ₹15,000/mo |
| Enterprise | White-label + Custom integrations | ₹50,000/mo |

## 📊 Key Features

### Equity Research Platform
- ✅ Automated daily screening of 100+ stocks
- ✅ AI-powered deep research using Perplexity
- ✅ 4-6 quarter financial analysis
- ✅ Risk assessment framework
- ✅ DCF and relative valuation models
- ✅ Professional PDF report generation
- ✅ Automated email delivery
- ✅ Subscription management

### Quant Research Assistant
- ✅ Real-time SEC filing monitoring
- ✅ AI summarization of filings and news
- ✅ Peer company comparisons
- ✅ Trading hypothesis generation
- ✅ Backtesting integration
- ✅ Discord/Telegram bots
- ✅ RESTful API for developers

## 🛠️ Technology Stack

- **Backend**: FastAPI, Python 3.10+
- **Database**: PostgreSQL with SQLAlchemy ORM
- **Cache**: Redis
- **Task Queue**: Celery with Redis broker
- **AI Research**: Perplexity API
- **Data Sources**: Screener.in, SEC EDGAR, News APIs
- **Report Generation**: Jinja2, WeasyPrint, Plotly
- **Frontend** (Optional): React, Next.js, TailwindCSS
- **Deployment**: Docker, Docker Compose

## 📈 Roadmap

- [x] Core infrastructure setup
- [x] GitHub repository creation
- [ ] Phase 1: Data source integrations (Week 1-2)
- [ ] Phase 2: Analysis engines (Week 3-4)
- [ ] Phase 3: Report generation (Week 5-6)
- [ ] Phase 4: API development (Week 7-8)
- [ ] Phase 5: Subscription management (Week 9-10)
- [ ] Phase 6: Frontend dashboard (Week 11-12)
- [ ] Phase 7: Production deployment (Week 13-14)
- [ ] Phase 8: Marketing & customer acquisition (Week 15+)

## 🤝 Contributing

This is a proprietary commercial project. For collaboration inquiries, please contact the repository owner.

## 📄 License

Proprietary - All Rights Reserved

## 📧 Contact

For inquiries: [Create an issue](https://github.com/ash4529/earnmore/issues)

---

**Built with GitHub Copilot** | **Powered by Perplexity AI** | **Data from Screener.in**
