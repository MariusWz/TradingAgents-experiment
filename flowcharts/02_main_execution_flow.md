 # 2. 主要运行流程

```mermaid
flowchart TD
    A[用户输入: 股票代码 + 交易日期] --> B[propagate company_name, trade_date]
    B --> C[Propagator.create_initial_state]
    C --> D[创建初始状态字典]
    D --> E[company_of_interest: company_name]
    D --> F[trade_date: trade_date]
    D --> G[investment_debate_state: InvestDebateState]
    D --> H[risk_debate_state: RiskDebateState]
    D --> I[market_report: ""]
    D --> J[fundamentals_report: ""]
    D --> K[sentiment_report: ""]
    D --> L[news_report: ""]
    
    G --> M[graph.invoke 或 graph.stream]
    H --> M
    I --> M
    J --> M
    K --> M
    L --> M
    
    M --> N[分析师团队阶段]
    N --> O[Market Analyst]
    O --> P[create_market_analyst]
    P --> Q{should_continue_market}
    Q -->|tool_calls存在| R[tools_market: ToolNode]
    Q -->|无tool_calls| S[Msg Clear Market]
    R --> T[get_YFin_data_online/get_stockstats_indicators_report_online]
    T --> O
    S --> U[Social Media Analyst]
    
    U --> V[create_social_media_analyst]
    V --> W{should_continue_social}
    W -->|tool_calls存在| X[tools_social: ToolNode]
    W -->|无tool_calls| Y[Msg Clear Social]
    X --> Z[get_stock_news_openai/get_reddit_stock_info]
    Z --> U
    Y --> AA[News Analyst]
    
    AA --> BB[create_news_analyst]
    BB --> CC{should_continue_news}
    CC -->|tool_calls存在| DD[tools_news: ToolNode]
    CC -->|无tool_calls| EE[Msg Clear News]
    DD --> FF[get_global_news_openai/get_google_news]
    FF --> AA
    EE --> GG[Fundamentals Analyst]
    
    GG --> HH[create_fundamentals_analyst]
    HH --> II{should_continue_fundamentals}
    II -->|tool_calls存在| JJ[tools_fundamentals: ToolNode]
    II -->|无tool_calls| KK[Msg Clear Fundamentals]
    JJ --> LL[get_fundamentals_openai/get_simfin_*]
    LL --> GG
    KK --> MM[研究团队阶段]
    
    MM --> NN[Bull Researcher]
    NN --> OO[create_bull_researcher]
    OO --> PP[Bull Researcher分析]
    PP --> QQ[Bear Researcher]
    QQ --> RR[create_bear_researcher]
    RR --> SS[Bear Researcher分析]
    SS --> TT{should_continue_debate}
    TT -->|count < max_rounds| UU[继续辩论]
    UU --> NN
    TT -->|count >= max_rounds| VV[Research Manager]
    
    VV --> WW[create_research_manager]
    WW --> XX[Research Manager总结]
    XX --> YY[Trader]
    YY --> ZZ[create_trader]
    ZZ --> AAA[Trader制定计划]
    AAA --> BBB[风险管理阶段]
    
    BBB --> CCC[Risky Analyst]
    CCC --> DDD[create_risky_debator]
    DDD --> EEE[Risky Analyst分析]
    EEE --> FFF[Safe Analyst]
    FFF --> GGG[create_safe_debator]
    GGG --> HHH[Safe Analyst分析]
    HHH --> III[Neutral Analyst]
    III --> JJJ[create_neutral_debator]
    JJJ --> KKK[Neutral Analyst分析]
    KKK --> LLL{should_continue_risk_analysis}
    LLL -->|count < max_rounds| MMM[继续风险分析]
    MMM --> CCC
    LLL -->|count >= max_rounds| NNN[Risk Manager]
    
    NNN --> OOO[create_risk_manager]
    OOO --> PPP[Risk Manager评估]
    PPP --> QQQ[最终决策]
    QQQ --> RRR[process_signal]
    RRR --> SSS[SignalProcessor.process_signal]
    SSS --> TTT[输出交易决策: BUY/SELL/HOLD]
```

## 关键函数和类说明：

### 主要函数：
- `TradingAgentsGraph.propagate()`: 主要传播函数
- `Propagator.create_initial_state()`: 创建初始状态
- `Propagator.get_graph_args()`: 获取图参数

### 分析师创建函数：
- `create_market_analyst()`: 创建市场分析师
- `create_social_media_analyst()`: 创建社交媒体分析师
- `create_news_analyst()`: 创建新闻分析师
- `create_fundamentals_analyst()`: 创建基本面分析师

### 研究员创建函数：
- `create_bull_researcher()`: 创建多头研究员
- `create_bear_researcher()`: 创建空头研究员
- `create_research_manager()`: 创建研究经理
- `create_trader()`: 创建交易员

### 风险分析师创建函数：
- `create_risky_debator()`: 创建激进分析师
- `create_safe_debator()`: 创建保守分析师
- `create_neutral_debator()`: 创建中性分析师
- `create_risk_manager()`: 创建风险经理

### 条件逻辑函数：
- `should_continue_market()`: 市场分析继续条件
- `should_continue_social()`: 社交媒体分析继续条件
- `should_continue_news()`: 新闻分析继续条件
- `should_continue_fundamentals()`: 基本面分析继续条件
- `should_continue_debate()`: 辩论继续条件
- `should_continue_risk_analysis()`: 风险分析继续条件

### 状态类：
- `InvestDebateState`: 投资辩论状态
- `RiskDebateState`: 风险辩论状态