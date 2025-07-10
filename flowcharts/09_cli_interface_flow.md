# 9. CLI界面流程

## 9.1 CLI界面整体流程

```mermaid
flowchart TD
    A[CLI启动] --> B[typer.Typer初始化]
    B --> C[显示欢迎界面]
    C --> D[获取用户选择]
    D --> E[选择分析师类型]
    E --> F[输入股票代码]
    F --> G[输入分析日期]
    G --> H[开始分析]
    H --> I[实时显示进度]
    I --> J[显示最终报告]
    J --> K[保存结果]
```

## 9.2 CLI初始化流程

```mermaid
flowchart TD
    A[cli/main.py] --> B[导入依赖模块]
    B --> C[typer.Typer初始化]
    C --> D[rich.Console初始化]
    D --> E[MessageBuffer初始化]
    E --> F[create_layout初始化]
    F --> G[CLI就绪]
    
    E --> H[消息缓冲区设置]
    H --> I[max_length: 100]
    I --> J[agent_status初始化]
    J --> K[report_sections初始化]
```

## 9.3 用户选择流程

```mermaid
flowchart TD
    A[get_user_selections] --> B[create_question_box]
    B --> C[选择分析师类型]
    C --> D[AnalystType枚举]
    D --> E[market: 市场分析师]
    D --> F[social: 社交媒体分析师]
    D --> G[news: 新闻分析师]
    D --> H[fundamentals: 基本面分析师]
    
    E --> I[确认选择]
    F --> I
    G --> I
    H --> I
    I --> J[返回selected_analysts]
```

## 9.4 股票代码输入流程

```mermaid
flowchart TD
    A[get_ticker] --> B[typer.prompt]
    B --> C[输入股票代码]
    C --> D[验证股票代码格式]
    D --> E{格式正确?}
    E -->|是| F[返回股票代码]
    E -->|否| G[显示错误信息]
    G --> H[重新输入]
    H --> D
```

## 9.5 分析日期输入流程

```mermaid
flowchart TD
    A[get_analysis_date] --> B[typer.prompt]
    B --> C[输入分析日期]
    C --> D[验证日期格式]
    D --> E{格式正确?}
    E -->|是| F[返回分析日期]
    E -->|否| G[显示错误信息]
    G --> H[重新输入]
    H --> D
```

## 9.6 分析执行流程

```mermaid
flowchart TD
    A[run_analysis] --> B[获取用户选择]
    B --> C[selected_analysts]
    C --> D[ticker]
    D --> E[analysis_date]
    E --> F[创建TradingAgentsGraph]
    F --> G[配置代理图]
    G --> H[开始传播]
    H --> I[实时显示进度]
    I --> J[显示最终结果]
```

## 9.7 实时显示流程

```mermaid
flowchart TD
    A[实时显示] --> B[update_display]
    B --> C[create_layout]
    C --> D[Header面板]
    D --> E[Progress面板]
    E --> F[Messages面板]
    F --> G[Analysis面板]
    G --> H[Footer面板]
    
    H --> I[Live更新]
    I --> J[Spinner动画]
    J --> K[状态更新]
    K --> L[消息显示]
```

## 9.8 消息缓冲区流程

```mermaid
flowchart TD
    A[MessageBuffer] --> B[add_message]
    B --> C[add_tool_call]
    C --> D[update_agent_status]
    D --> E[update_report_section]
    E --> F[_update_current_report]
    F --> G[_update_final_report]
    
    G --> H[report_sections]
    H --> I[market_report]
    H --> J[sentiment_report]
    H --> K[news_report]
    H --> L[fundamentals_report]
    H --> M[investment_plan]
    H --> N[trader_investment_plan]
    H --> O[final_trade_decision]
```

## 9.9 代理状态更新流程

```mermaid
flowchart TD
    A[代理状态更新] --> B[update_agent_status]
    B --> C[Market Analyst: pending]
    C --> D[Social Analyst: pending]
    D --> E[News Analyst: pending]
    E --> F[Fundamentals Analyst: pending]
    F --> G[Bull Researcher: pending]
    G --> H[Bear Researcher: pending]
    H --> I[Research Manager: pending]
    I --> J[Trader: pending]
    J --> K[Risky Analyst: pending]
    K --> L[Neutral Analyst: pending]
    L --> M[Safe Analyst: pending]
    M --> N[Risk Manager: pending]
    
    N --> O[状态变化: pending -> active -> completed]
```

## 9.10 报告显示流程

```mermaid
flowchart TD
    A[display_complete_report] --> B[提取最终状态]
    B --> C[market_report]
    C --> D[sentiment_report]
    D --> E[news_report]
    E --> F[fundamentals_report]
    F --> G[investment_plan]
    G --> H[trader_investment_plan]
    H --> I[final_trade_decision]
    
    I --> J[格式化报告]
    J --> K[Markdown渲染]
    K --> L[Panel显示]
    L --> M[保存到文件]
```

## 关键函数和类说明：

### CLI主类：
- `typer.Typer`: CLI框架
- `rich.Console`: 富文本控制台
- `MessageBuffer`: 消息缓冲区类

### 用户交互函数：
- `get_user_selections()`: 获取用户选择
- `get_ticker()`: 获取股票代码
- `get_analysis_date()`: 获取分析日期
- `create_question_box()`: 创建问题框

### 显示函数：
- `create_layout()`: 创建布局
- `update_display()`: 更新显示
- `display_complete_report()`: 显示完整报告
- `update_agent_status()`: 更新代理状态

### 分析执行函数：
- `run_analysis()`: 运行分析
- `save_message_decorator()`: 消息保存装饰器
- `save_tool_call_decorator()`: 工具调用保存装饰器
- `save_report_section_decorator()`: 报告部分保存装饰器

### 枚举类型：
- `AnalystType`: 分析师类型枚举
  - `market`: 市场分析师
  - `social`: 社交媒体分析师
  - `news`: 新闻分析师
  - `fundamentals`: 基本面分析师

### 状态管理：
- `agent_status`: 代理状态字典
- `report_sections`: 报告部分字典
- `messages`: 消息队列
- `tool_calls`: 工具调用队列

### 布局组件：
- `Header`: 标题面板
- `Progress`: 进度面板
- `Messages`: 消息面板
- `Analysis`: 分析面板
- `Footer`: 底部面板 