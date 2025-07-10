 # 1. 程序初始化阶段流程图

```mermaid
flowchart TD
    A[main.py: 程序启动] --> B[加载 DEFAULT_CONFIG]
    B --> C[TradingAgentsGraph.__init__]
    C --> D[set_config config]
    D --> E[创建数据缓存目录]
    E --> F[初始化LLM模型]
    F --> G{llm_provider类型}
    G -->|openai/ollama/openrouter| H[ChatOpenAI]
    G -->|anthropic| I[ChatAnthropic]
    G -->|google| J[ChatGoogleGenerativeAI]
    
    H --> K[创建Toolkit]
    I --> K
    J --> K
    
    K --> L[初始化记忆系统]
    L --> M[FinancialSituationMemory: bull_memory]
    L --> N[FinancialSituationMemory: bear_memory]
    L --> O[FinancialSituationMemory: trader_memory]
    L --> P[FinancialSituationMemory: invest_judge_memory]
    L --> Q[FinancialSituationMemory: risk_manager_memory]
    
    M --> R[_create_tool_nodes]
    N --> R
    O --> R
    P --> R
    Q --> R
    
    R --> S[创建ToolNode]
    S --> T[market: YFin_data + stockstats]
    S --> U[social: reddit + news_openai]
    S --> V[news: google_news + finnhub]
    S --> W[fundamentals: fundamentals_openai + simfin]
    
    T --> X[初始化组件]
    U --> X
    V --> X
    W --> X
    
    X --> Y[ConditionalLogic]
    X --> Z[GraphSetup]
    X --> AA[Propagator]
    X --> BB[Reflector]
    X --> CC[SignalProcessor]
    
    Y --> DD[setup_graph]
    Z --> DD
    AA --> DD
    BB --> DD
    CC --> DD
    
    DD --> EE[StateGraph编译]
    EE --> FF[系统就绪]
```

## 关键函数和类说明：

### 主要类：
- **TradingAgentsGraph**: 主控制器类
- **GraphSetup**: 图构建类
- **Propagator**: 状态传播类
- **ConditionalLogic**: 条件逻辑类
- **Reflector**: 反思学习类
- **SignalProcessor**: 信号处理类

### 关键函数：
- `TradingAgentsGraph.__init__()`: 初始化主控制器
- `_create_tool_nodes()`: 创建工具节点
- `GraphSetup.setup_graph()`: 设置代理图
- `set_config()`: 设置配置

### 记忆系统：
- `FinancialSituationMemory`: 财务情况记忆类
- 包含5个记忆实例：bull_memory, bear_memory, trader_memory, invest_judge_memory, risk_manager_memory

### 工具节点：
- **market**: YFin数据 + StockStats指标
- **social**: Reddit数据 + OpenAI新闻
- **news**: Google News + Finnhub新闻
- **fundamentals**: OpenAI基本面 + SimFin财务报表