# 6. 工具调用流程

## 6.1 工具调用整体流程

```mermaid
flowchart TD
    A[工具调用请求] --> B{工具类型}
    B -->|市场数据| C[Market Tools]
    B -->|社交媒体| D[Social Tools]
    B -->|新闻| E[News Tools]
    B -->|基本面| F[Fundamentals Tools]
    
    C --> G[在线/离线模式]
    D --> G
    E --> G
    F --> G
    
    G --> H[获取数据]
    H --> I[处理数据]
    I --> J[返回结果]
    J --> K[更新状态]
```

## 6.2 市场数据工具流程

```mermaid
flowchart TD
    A[Market Tools] --> B{在线模式?}
    B -->|是| C[get_YFin_data_online]
    B -->|是| D[get_stockstats_indicators_report_online]
    B -->|否| E[get_YFin_data]
    B -->|否| F[get_stockstats_indicators_report]
    
    C --> G[调用YFin API]
    D --> H[调用StockStats API]
    E --> I[读取本地YFin数据]
    F --> J[读取本地StockStats数据]
    
    G --> K[获取股票价格数据]
    H --> L[获取技术指标]
    I --> K
    J --> L
    
    K --> M[数据处理]
    L --> M
    M --> N[技术分析]
    N --> O[趋势分析]
    O --> P[生成市场报告]
```

## 6.3 社交媒体工具流程

```mermaid
flowchart TD
    A[Social Tools] --> B{在线模式?}
    B -->|是| C[get_stock_news_openai]
    B -->|否| D[get_reddit_stock_info]
    
    C --> E[调用OpenAI API]
    D --> F[读取Reddit数据]
    
    E --> G[获取股票相关新闻]
    F --> H[获取Reddit讨论]
    
    G --> I[情感分析]
    H --> I
    I --> J[用户情绪评估]
    J --> K[社区讨论分析]
    K --> L[生成社交媒体报告]
```

## 6.4 新闻工具流程

```mermaid
flowchart TD
    A[News Tools] --> B{在线模式?}
    B -->|是| C[get_global_news_openai]
    B -->|是| D[get_google_news]
    B -->|否| E[get_finnhub_news]
    B -->|否| F[get_reddit_news]
    
    C --> G[调用OpenAI全球新闻API]
    D --> H[调用Google News API]
    E --> I[读取Finnhub新闻数据]
    F --> J[读取Reddit新闻数据]
    
    G --> K[获取全球新闻]
    H --> L[获取Google新闻]
    I --> M[获取Finnhub新闻]
    J --> N[获取Reddit新闻]
    
    K --> O[新闻数据处理]
    L --> O
    M --> O
    N --> O
    O --> P[新闻重要性评估]
    P --> Q[市场影响分析]
    Q --> R[生成新闻报告]
```

## 6.5 基本面工具流程

```mermaid
flowchart TD
    A[Fundamentals Tools] --> B{在线模式?}
    B -->|是| C[get_fundamentals_openai]
    B -->|否| D[get_finnhub_company_insider_sentiment]
    B -->|否| E[get_finnhub_company_insider_transactions]
    B -->|否| F[get_simfin_balance_sheet]
    B -->|否| G[get_simfin_cashflow]
    B -->|否| H[get_simfin_income_stmt]
    
    C --> I[调用OpenAI基本面API]
    D --> J[读取内幕交易情感数据]
    E --> K[读取内幕交易数据]
    F --> L[读取资产负债表]
    G --> M[读取现金流量表]
    H --> N[读取利润表]
    
    I --> O[获取基本面分析]
    J --> P[获取内幕交易情感]
    K --> Q[获取内幕交易数据]
    L --> R[获取财务数据]
    M --> R
    N --> R
    
    O --> S[基本面数据处理]
    P --> S
    Q --> S
    R --> S
    S --> T[财务比率分析]
    T --> U[盈利能力评估]
    U --> V[生成基本面报告]
```

## 6.6 工具节点创建流程

```mermaid
flowchart TD
    A[_create_tool_nodes] --> B[创建ToolNode字典]
    B --> C[market: ToolNode]
    C --> D[YFin数据工具]
    C --> E[StockStats指标工具]
    
    B --> F[social: ToolNode]
    F --> G[Reddit数据工具]
    F --> H[OpenAI新闻工具]
    
    B --> I[news: ToolNode]
    I --> J[Google News工具]
    I --> K[Finnhub新闻工具]
    I --> L[Reddit新闻工具]
    
    B --> M[fundamentals: ToolNode]
    M --> N[OpenAI基本面工具]
    M --> O[SimFin财务报表工具]
    M --> P[Finnhub内幕交易工具]
```

## 关键函数和类说明：

### 工具节点类：
- `ToolNode`: LangGraph工具节点类
- `_create_tool_nodes()`: 创建工具节点的方法

### 市场数据工具：
- `get_YFin_data_online()`: 在线获取YFin数据
- `get_stockstats_indicators_report_online()`: 在线获取StockStats指标
- `get_YFin_data()`: 离线获取YFin数据
- `get_stockstats_indicators_report()`: 离线获取StockStats指标

### 社交媒体工具：
- `get_stock_news_openai()`: 获取OpenAI股票新闻
- `get_reddit_stock_info()`: 获取Reddit股票信息

### 新闻工具：
- `get_global_news_openai()`: 获取OpenAI全球新闻
- `get_google_news()`: 获取Google新闻
- `get_finnhub_news()`: 获取Finnhub新闻
- `get_reddit_news()`: 获取Reddit新闻

### 基本面工具：
- `get_fundamentals_openai()`: 获取OpenAI基本面分析
- `get_finnhub_company_insider_sentiment()`: 获取内幕交易情感
- `get_finnhub_company_insider_transactions()`: 获取内幕交易数据
- `get_simfin_balance_sheet()`: 获取资产负债表
- `get_simfin_cashflow()`: 获取现金流量表
- `get_simfin_income_stmt()`: 获取利润表

### 数据源：
- **YFin**: Yahoo Finance数据
- **StockStats**: 技术指标数据
- **Reddit**: 社交媒体讨论
- **Google News**: 新闻数据
- **Finnhub**: 金融数据API
- **SimFin**: 财务报表数据
- **OpenAI**: AI分析服务 