# 3. 分析师团队详细流程

## 3.1 分析师团队整体流程

```mermaid
flowchart TD
    A[分析师团队开始] --> B[Market Analyst]
    B --> C[Social Media Analyst]
    C --> D[News Analyst]
    D --> E[Fundamentals Analyst]
    E --> F[分析师团队完成]
    
    B --> G[market_report生成]
    C --> H[sentiment_report生成]
    D --> I[news_report生成]
    E --> J[fundamentals_report生成]
    
    G --> K[状态更新]
    H --> K
    I --> K
    J --> K
    K --> L[传递到研究团队]
```

## 3.2 市场分析师详细流程

```mermaid
flowchart TD
    A[Market Analyst开始] --> B[create_market_analyst]
    B --> C[quick_thinking_llm初始化]
    C --> D[分析当前状态]
    D --> E[生成市场分析请求]
    E --> F[调用市场数据工具]
    F --> G{工具选择}
    G -->|在线模式| H[get_YFin_data_online]
    G -->|在线模式| I[get_stockstats_indicators_report_online]
    G -->|离线模式| J[get_YFin_data]
    G -->|离线模式| K[get_stockstats_indicators_report]
    
    H --> L[获取YFin数据]
    I --> M[获取StockStats指标]
    J --> L
    K --> M
    
    L --> N[数据处理]
    M --> N
    N --> O[技术指标分析]
    O --> P[价格趋势分析]
    P --> Q[市场情绪分析]
    Q --> R[生成market_report]
    R --> S[更新状态]
    S --> T[传递到下一个分析师]
```

## 3.3 社交媒体分析师详细流程

```mermaid
flowchart TD
    A[Social Media Analyst开始] --> B[create_social_media_analyst]
    B --> C[quick_thinking_llm初始化]
    C --> D[分析当前状态]
    D --> E[生成社交媒体分析请求]
    E --> F[调用社交媒体工具]
    F --> G{工具选择}
    G -->|在线模式| H[get_stock_news_openai]
    G -->|离线模式| I[get_reddit_stock_info]
    
    H --> J[获取OpenAI新闻分析]
    I --> K[获取Reddit股票信息]
    
    J --> L[社交媒体数据处理]
    K --> L
    L --> M[情感分析]
    M --> N[用户情绪评估]
    N --> O[社区讨论分析]
    O --> P[生成sentiment_report]
    P --> Q[更新状态]
    Q --> R[传递到下一个分析师]
```

## 3.4 新闻分析师详细流程

```mermaid
flowchart TD
    A[News Analyst开始] --> B[create_news_analyst]
    B --> C[quick_thinking_llm初始化]
    C --> D[分析当前状态]
    D --> E[生成新闻分析请求]
    E --> F[调用新闻工具]
    F --> G{工具选择}
    G -->|在线模式| H[get_global_news_openai]
    G -->|在线模式| I[get_google_news]
    G -->|离线模式| J[get_finnhub_news]
    G -->|离线模式| K[get_reddit_news]
    
    H --> L[获取OpenAI全球新闻]
    I --> M[获取Google新闻]
    J --> N[获取Finnhub新闻]
    K --> O[获取Reddit新闻]
    
    L --> P[新闻数据处理]
    M --> P
    N --> P
    O --> P
    P --> Q[新闻重要性评估]
    Q --> R[市场影响分析]
    R --> S[新闻情感分析]
    S --> T[生成news_report]
    T --> U[更新状态]
    U --> V[传递到下一个分析师]
```

## 3.5 基本面分析师详细流程

```mermaid
flowchart TD
    A[Fundamentals Analyst开始] --> B[create_fundamentals_analyst]
    B --> C[quick_thinking_llm初始化]
    C --> D[分析当前状态]
    D --> E[生成基本面分析请求]
    E --> F[调用基本面工具]
    F --> G{工具选择}
    G -->|在线模式| H[get_fundamentals_openai]
    G -->|离线模式| I[get_finnhub_company_insider_sentiment]
    G -->|离线模式| J[get_finnhub_company_insider_transactions]
    G -->|离线模式| K[get_simfin_balance_sheet]
    G -->|离线模式| L[get_simfin_cashflow]
    G -->|离线模式| M[get_simfin_income_stmt]
    
    H --> N[获取OpenAI基本面分析]
    I --> O[获取内幕交易情感]
    J --> P[获取内幕交易数据]
    K --> Q[获取资产负债表]
    L --> R[获取现金流量表]
    M --> S[获取利润表]
    
    N --> T[基本面数据处理]
    O --> T
    P --> T
    Q --> T
    R --> T
    S --> T
    T --> U[财务比率分析]
    U --> V[盈利能力评估]
    V --> W[偿债能力分析]
    W --> X[成长性评估]
    X --> Y[生成fundamentals_report]
    Y --> Z[更新状态]
    Z --> AA[传递到研究团队]
```

## 关键函数和类说明：

### 分析师创建函数：
- `create_market_analyst(quick_thinking_llm, toolkit)`: 创建市场分析师
- `create_social_media_analyst(quick_thinking_llm, toolkit)`: 创建社交媒体分析师
- `create_news_analyst(quick_thinking_llm, toolkit)`: 创建新闻分析师
- `create_fundamentals_analyst(quick_thinking_llm, toolkit)`: 创建基本面分析师

### 工具函数：
- `get_YFin_data_online()`: 在线获取YFin数据
- `get_stockstats_indicators_report_online()`: 在线获取StockStats指标
- `get_stock_news_openai()`: 获取OpenAI股票新闻
- `get_reddit_stock_info()`: 获取Reddit股票信息
- `get_global_news_openai()`: 获取OpenAI全球新闻
- `get_google_news()`: 获取Google新闻
- `get_fundamentals_openai()`: 获取OpenAI基本面分析
- `get_simfin_*()`: 获取SimFin财务报表数据

### 状态更新：
- `market_report`: 市场分析报告
- `sentiment_report`: 社交媒体情感报告
- `news_report`: 新闻分析报告
- `fundamentals_report`: 基本面分析报告 