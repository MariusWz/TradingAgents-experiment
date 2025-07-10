
## TradingAgents 程序运行机制分析

### 📁 项目目录结构

```
TradingAgents/
├── main.py                    # 主入口文件（简单版本）
├── pyproject.toml            # 项目配置和依赖
├── requirements.txt           # 依赖列表
├── README.md                 # 项目说明
├── tradingagents/            # 核心模块
│   ├── __init__.py
│   ├── default_config.py     # 默认配置
│   ├── agents/               # 智能体模块
│   │   ├── analysts/         # 分析师团队
│   │   │   ├── market_analyst.py
│   │   │   ├── social_media_analyst.py
│   │   │   ├── news_analyst.py
│   │   │   └── fundamentals_analyst.py
│   │   ├── researchers/      # 研究团队
│   │   │   ├── bull_researcher.py
│   │   │   └── bear_researcher.py
│   │   ├── managers/         # 管理团队
│   │   │   ├── research_manager.py
│   │   │   └── risk_manager.py
│   │   ├── trader/           # 交易团队
│   │   │   └── trader.py
│   │   └── risk_mgmt/        # 风险管理团队
│   │       ├── aggresive_debator.py
│   │       ├── conservative_debator.py
│   │       └── neutral_debator.py
│   ├── graph/                # 图执行引擎
│   │   ├── trading_graph.py  # 主图类
│   │   ├── setup.py          # 图设置
│   │   ├── propagation.py    # 传播逻辑
│   │   ├── conditional_logic.py
│   │   ├── reflection.py     # 反思机制
│   │   └── signal_processing.py
│   └── dataflows/            # 数据流处理
├── cli/                      # 命令行界面
│   ├── main.py              # CLI主入口（你当前查看的文件）
│   ├── utils.py             # 用户交互工具
│   ├── models.py            # 数据模型
│   └── static/              # 静态资源
│       └── welcome.txt      # 欢迎信息
└── results/                 # 结果输出目录
```

### 程序运行流程

#### 1. **入口点**
- **CLI模式**: `python -m cli.main analyze` 或直接运行 `cli/main.py`
- **简单模式**: 运行根目录的 `main.py`

#### 2. **初始化阶段**
```python
# 1. 获取用户配置
selections = get_user_selections()  # 交互式获取用户选择

# 2. 创建配置
config = DEFAULT_CONFIG.copy()
config["max_debate_rounds"] = selections["research_depth"]
config["quick_think_llm"] = selections["shallow_thinker"]
config["deep_think_llm"] = selections["deep_thinker"]

# 3. 初始化图
graph = TradingAgentsGraph(
    [analyst.value for analyst in selections["analysts"]], 
    config=config, 
    debug=True
)
```

#### 3. **多智能体架构**

**团队结构**:
- **分析师团队** (Analyst Team): 市场、社交、新闻、基本面分析
- **研究团队** (Research Team): 多头研究员、空头研究员、研究经理
- **交易团队** (Trading Team): 交易员
- **风险管理团队** (Risk Management): 激进、保守、中性分析师
- **投资组合管理团队** (Portfolio Management): 投资组合经理

#### 4. **执行流程**

```python
# 1. 创建初始状态
init_agent_state = graph.propagator.create_initial_state(
    selections["ticker"], selections["analysis_date"]
)

# 2. 流式执行图
for chunk in graph.graph.stream(init_agent_state, **args):
    # 实时更新UI显示
    update_display(layout)
    
    # 处理消息和工具调用
    if len(chunk["messages"]) > 0:
        last_message = chunk["messages"][-1]
        message_buffer.add_message(msg_type, content)
        
        # 更新报告和状态
        if "market_report" in chunk:
            message_buffer.update_report_section("market_report", chunk["market_report"])
            message_buffer.update_agent_status("Market Analyst", "completed")
```

#### 5. **实时UI更新机制**

**MessageBuffer类**:
- 存储最近的消息和工具调用
- 管理代理状态（pending/in_progress/completed/error）
- 更新报告章节
- 维护最终报告

**Rich UI布局**:
```python
layout = Layout()
layout.split_column(
    Layout(name="header", size=3),      # 欢迎信息
    Layout(name="main"),                # 主要内容
    Layout(name="footer", size=3),      # 统计信息
)
layout["main"].split_column(
    Layout(name="upper", ratio=3),      # 进度和消息
    Layout(name="analysis", ratio=5),   # 分析报告
)
```

#### 6. **数据流和工具调用**

**工具节点**:
- `market`: YFinance数据、技术指标
- `social`: 股票新闻、Reddit信息
- `news`: 全球新闻、Google新闻
- `fundamentals`: 基本面数据、财务报表

**LLM提供商支持**:
- OpenAI (GPT-4o, o4-mini等)
- Anthropic (Claude系列)
- Google (Gemini系列)
- OpenRouter
- Ollama (本地模型)

#### 7. **决策流程**

1. **分析师团队** → 收集市场数据、情感、新闻、基本面
2. **研究团队** → 多头/空头辩论 → 研究经理决策
3. **交易团队** → 制定交易计划
4. **风险管理团队** → 风险评估辩论 → 投资组合经理最终决策

#### 8. **输出和存储**

- **实时显示**: Rich库提供的富文本终端界面
- **日志文件**: `results/{ticker}/{date}/message_tool.log`
- **报告文件**: `results/{ticker}/{date}/reports/`
- **状态记录**: JSON格式的完整状态记录

### 🎯 核心特点

1. **多智能体协作**: 5个专业团队协同工作
2. **实时可视化**: Rich库提供美观的终端界面
3. **流式处理**: 实时显示分析进度和结果
4. **模块化设计**: 每个智能体独立可配置
5. **多LLM支持**: 支持多种大语言模型提供商
6. **记忆机制**: 智能体具有记忆和学习能力

这个系统本质上是一个基于LangGraph的多智能体金融分析框架，通过模拟专业投资团队的工作流程来进行股票分析和交易决策。



## Introduction:
TradingAgents 是一个多智能体交易框架，模拟了真实世界交易公司的运作方式。通过部署由大语言模型（LLM）驱动的专业智能体——包括基本面分析师、情绪分析师、技术分析师、交易员和风险管理团队——平台能够协作评估市场状况并辅助交易决策。此外，这些智能体还会进行动态讨论，以确定最优策略。

<p align="center">
  <img src="assets/schema.png" style="width: 100%; height: auto;">
</p>

> TradingAgents 框架主要用于学术研究。交易表现会受到多种因素影响，包括所选用的基础语言模型、模型温度、交易周期、数据质量以及其他非确定性因素。[本框架不构成任何金融、投资或交易建议。](https://tauric.ai/disclaimer/)

我们的框架将复杂的交易任务分解为多个专业角色，从而实现了对市场分析和决策的高效、可扩展处理。

### 分析师团队
- 基本面分析师：评估公司财务和业绩指标，识别内在价值和潜在风险信号。
- 情绪分析师：利用情绪评分算法分析社交媒体和公众情绪，判断短期市场情绪。
- 新闻分析师：监控全球新闻和宏观经济指标，解读事件对市场的影响。
- 技术分析师：利用技术指标（如 MACD 和 RSI）识别交易模式并预测价格走势。

<p align="center">
  <img src="assets/analyst.png" width="100%" style="display: inline-block; margin: 0 2%;">
</p>

### 研究员团队
- 由多头和空头研究员组成，批判性地评估分析师团队提供的见解。通过结构化辩论，平衡潜在收益与内在风险。

<p align="center">
  <img src="assets/researcher.png" width="70%" style="display: inline-block; margin: 0 2%;">
</p>

### 交易智能体
- 汇总分析师和研究员的报告，做出明智的交易决策。根据全面的市场洞察，决定交易的时机和规模。

<p align="center">
  <img src="assets/trader.png" width="70%" style="display: inline-block; margin: 0 2%;">
</p>

### 风险管理与投资组合经理
- 持续评估投资组合风险，包括市场波动性、流动性等因素。风险管理团队会评估并调整交易策略，向投资组合经理提供评估报告以供最终决策。
- 投资组合经理负责批准或拒绝交易提案。若获批准，订单将被发送至模拟交易所并执行。

<p align="center">
  <img src="assets/risk.png" width="70%" style="display: inline-block; margin: 0 2%;">
</p>

## Installation and CLI

### Installation

Clone TradingAgents:
```bash
git clone https://github.com/TauricResearch/TradingAgents.git
cd TradingAgents
```

Create a virtual environment in any of your favorite environment managers:
```bash
conda create -n tradingagents python=3.13
conda activate tradingagents
```

Install dependencies:
```bash
pip install -r requirements.txt
```

### Required APIs

You will also need the FinnHub API for financial data. All of our code is implemented with the free tier.
```bash
export FINNHUB_API_KEY=$YOUR_FINNHUB_API_KEY
```

You will need the OpenAI API for all the agents.
```bash
export OPENAI_API_KEY=$YOUR_OPENAI_API_KEY
```

### CLI Usage

You can also try out the CLI directly by running:
```bash
python -m cli.main
```
You will see a screen where you can select your desired tickers, date, LLMs, research depth, etc.

<p align="center">
  <img src="assets/cli/cli_init.png" width="100%" style="display: inline-block; margin: 0 2%;">
</p>

An interface will appear showing results as they load, letting you track the agent's progress as it runs.

<p align="center">
  <img src="assets/cli/cli_news.png" width="100%" style="display: inline-block; margin: 0 2%;">
</p>

<p align="center">
  <img src="assets/cli/cli_transaction.png" width="100%" style="display: inline-block; margin: 0 2%;">
</p>

## TradingAgents Package

### Implementation Details

We built TradingAgents with LangGraph to ensure flexibility and modularity. We utilize `o1-preview` and `gpt-4o` as our deep thinking and fast thinking LLMs for our experiments. However, for testing purposes, we recommend you use `o4-mini` and `gpt-4.1-mini` to save on costs as our framework makes **lots of** API calls.

### Python Usage

To use TradingAgents inside your code, you can import the `tradingagents` module and initialize a `TradingAgentsGraph()` object. The `.propagate()` function will return a decision. You can run `main.py`, here's also a quick example:

```python
from tradingagents.graph.trading_graph import TradingAgentsGraph
from tradingagents.default_config import DEFAULT_CONFIG

ta = TradingAgentsGraph(debug=True, config=DEFAULT_CONFIG.copy())

# forward propagate
_, decision = ta.propagate("NVDA", "2024-05-10")
print(decision)
```

You can also adjust the default configuration to set your own choice of LLMs, debate rounds, etc.

```python
from tradingagents.graph.trading_graph import TradingAgentsGraph
from tradingagents.default_config import DEFAULT_CONFIG

# Create a custom config
config = DEFAULT_CONFIG.copy()
config["deep_think_llm"] = "gpt-4.1-nano"  # Use a different model
config["quick_think_llm"] = "gpt-4.1-nano"  # Use a different model
config["max_debate_rounds"] = 1  # Increase debate rounds
config["online_tools"] = True # Use online tools or cached data

# Initialize with custom config
ta = TradingAgentsGraph(debug=True, config=config)

# forward propagate
_, decision = ta.propagate("NVDA", "2024-05-10")
print(decision)
```

> For `online_tools`, we recommend enabling them for experimentation, as they provide access to real-time data. The agents' offline tools rely on cached data from our **Tauric TradingDB**, a curated dataset we use for backtesting. We're currently in the process of refining this dataset, and we plan to release it soon alongside our upcoming projects. Stay tuned!

You can view the full list of configurations in `tradingagents/default_config.py`.

