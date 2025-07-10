# 4. 研究团队详细流程

## 4.1 研究团队整体流程

```mermaid
flowchart TD
    A[研究团队开始] --> B[Bull Researcher]
    B --> C[Bear Researcher]
    C --> D{should_continue_debate}
    D -->|继续辩论| E[Bull Researcher]
    D -->|结束辩论| F[Research Manager]
    E --> C
    F --> G[生成investment_plan]
    G --> H[传递到交易员]
```

## 4.2 多头研究员详细流程

```mermaid
flowchart TD
    A[Bull Researcher开始] --> B[create_bull_researcher]
    B --> C[quick_thinking_llm初始化]
    C --> D[bull_memory加载]
    D --> E[分析分析师报告]
    E --> F[market_report分析]
    F --> G[sentiment_report分析]
    G --> H[news_report分析]
    H --> I[fundamentals_report分析]
    I --> J[历史记忆检索]
    J --> K[生成多头观点]
    K --> L[投资理由分析]
    L --> M[风险评估]
    M --> N[更新investment_debate_state]
    N --> O[bull_history更新]
    O --> P[传递到空头研究员]
```

## 4.3 空头研究员详细流程

```mermaid
flowchart TD
    A[Bear Researcher开始] --> B[create_bear_researcher]
    B --> C[quick_thinking_llm初始化]
    C --> D[bear_memory加载]
    D --> E[分析分析师报告]
    E --> F[market_report分析]
    F --> G[sentiment_report分析]
    G --> H[news_report分析]
    H --> I[fundamentals_report分析]
    I --> J[历史记忆检索]
    J --> K[生成空头观点]
    K --> L[风险因素分析]
    L --> M[市场担忧评估]
    M --> N[更新investment_debate_state]
    N --> O[bear_history更新]
    O --> P[传递到条件判断]
```

## 4.4 研究经理详细流程

```mermaid
flowchart TD
    A[Research Manager开始] --> B[create_research_manager]
    B --> C[deep_thinking_llm初始化]
    C --> D[invest_judge_memory加载]
    D --> E[分析辩论历史]
    E --> F[bull_history分析]
    F --> G[bear_history分析]
    G --> H[综合分析师报告]
    H --> I[市场分析总结]
    I --> J[基本面分析总结]
    J --> K[新闻影响评估]
    K --> L[社交媒体情绪总结]
    L --> M[生成投资建议]
    M --> N[投资理由说明]
    N --> O[风险评估]
    O --> P[更新investment_debate_state]
    P --> Q[judge_decision更新]
    Q --> R[传递到交易员]
```

## 4.5 辩论控制流程

```mermaid
flowchart TD
    A[辩论开始] --> B[investment_debate_state初始化]
    B --> C[count: 0]
    C --> D[history: ""]
    D --> E[current_response: ""]
    E --> F[Bull Researcher分析]
    F --> G[count++]
    G --> H[current_response更新]
    H --> I[should_continue_debate判断]
    I --> J{count >= 2 * max_debate_rounds?}
    J -->|否| K{current_response以"Bull"开头?}
    K -->|是| L[Bear Researcher]
    K -->|否| M[Bull Researcher]
    J -->|是| N[Research Manager]
    L --> O[count++]
    M --> O
    N --> P[辩论结束]
    O --> I
```

## 关键函数和类说明：

### 研究员创建函数：
- `create_bull_researcher(quick_thinking_llm, bull_memory)`: 创建多头研究员
- `create_bear_researcher(quick_thinking_llm, bear_memory)`: 创建空头研究员
- `create_research_manager(deep_thinking_llm, invest_judge_memory)`: 创建研究经理

### 条件逻辑函数：
- `should_continue_debate(state)`: 判断是否继续辩论
  - 检查 `investment_debate_state["count"] >= 2 * max_debate_rounds`
  - 检查 `current_response` 是否以 "Bull" 开头

### 状态管理：
- `investment_debate_state`: 投资辩论状态
  - `history`: 辩论历史
  - `current_response`: 当前回应
  - `count`: 辩论轮数
  - `bull_history`: 多头历史
  - `bear_history`: 空头历史
  - `judge_decision`: 判断决策

### 记忆系统：
- `bull_memory`: 多头研究员记忆
- `bear_memory`: 空头研究员记忆
- `invest_judge_memory`: 投资判断记忆

### 输出：
- `investment_plan`: 投资计划
- `trader_investment_plan`: 交易员投资计划 