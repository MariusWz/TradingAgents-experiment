# 10. 信号处理流程

## 10.1 信号处理整体流程

```mermaid
flowchart TD
    A[最终交易决策] --> B[SignalProcessor.process_signal]
    B --> C[LLM分析]
    C --> D[提取核心决策]
    D --> E[BUY/SELL/HOLD]
    E --> F[返回处理结果]
```

## 10.2 信号处理器初始化流程

```mermaid
flowchart TD
    A[SignalProcessor.__init__] --> B[quick_thinking_llm初始化]
    B --> C[设置系统提示词]
    C --> D[信号处理器就绪]
    
    C --> E[system: 提取投资决策]
    E --> F[human: 分析段落或财务报告]
    F --> G[输出: SELL/BUY/HOLD]
```

## 10.3 信号处理详细流程

```mermaid
flowchart TD
    A[process_signal] --> B[构建消息]
    B --> C[system: 系统提示词]
    C --> D[human: full_signal]
    D --> E[LLM分析]
    E --> F[提取决策关键词]
    F --> G{决策类型}
    G -->|BUY| H[买入决策]
    G -->|SELL| I[卖出决策]
    G -->|HOLD| J[持有决策]
    
    H --> K[返回BUY]
    I --> L[返回SELL]
    J --> M[返回HOLD]
```

## 10.4 最终决策生成流程

```mermaid
flowchart TD
    A[Risk Manager完成] --> B[生成final_trade_decision]
    B --> C[综合所有分析]
    C --> D[市场分析总结]
    D --> E[基本面分析总结]
    E --> F[新闻影响评估]
    F --> G[社交媒体情绪]
    G --> H[投资建议]
    H --> I[风险评估]
    I --> J[最终交易决策]
    J --> K[process_signal处理]
```

## 10.5 决策提取流程

```mermaid
flowchart TD
    A[LLM分析] --> B[读取完整信号]
    B --> C[分析决策内容]
    C --> D[识别关键决策词]
    D --> E[BUY关键词识别]
    E --> F[SELL关键词识别]
    F --> G[HOLD关键词识别]
    
    G --> H[选择最匹配的决策]
    H --> I[验证决策合理性]
    I --> J[返回最终决策]
```

## 10.6 信号处理系统提示词

```mermaid
flowchart TD
    A[系统提示词] --> B[角色定义]
    B --> C[高效助手]
    C --> D[分析段落或财务报告]
    D --> E[提取投资决策]
    E --> F[SELL/BUY/HOLD]
    F --> G[仅输出决策]
    G --> H[不添加额外信息]
```

## 10.7 决策验证流程

```mermaid
flowchart TD
    A[决策验证] --> B[检查决策格式]
    B --> C{格式正确?}
    C -->|是| D[验证决策逻辑]
    C -->|否| E[重新分析]
    D --> F{逻辑合理?}
    F -->|是| G[返回决策]
    F -->|否| H[调整决策]
    E --> I[重新LLM分析]
    H --> I
```

## 10.8 信号处理错误处理

```mermaid
flowchart TD
    A[信号处理] --> B{LLM响应正常?}
    B -->|是| C[提取决策]
    B -->|否| D[错误处理]
    D --> E[重试机制]
    E --> F[备用分析]
    F --> G[默认决策]
    
    C --> H[决策验证]
    H --> I{验证通过?}
    I -->|是| J[返回决策]
    I -->|否| K[重新处理]
    K --> B
```

## 关键函数和类说明：

### 信号处理器类：
- `SignalProcessor`: 信号处理类
  - `__init__(quick_thinking_llm)`: 初始化信号处理器
  - `process_signal(full_signal)`: 处理完整信号

### 核心函数：
- `process_signal(full_signal: str) -> str`: 处理交易信号
  - 输入: 完整的交易信号文本
  - 输出: 提取的决策 (BUY, SELL, 或 HOLD)

### 系统提示词：
```
You are an efficient assistant designed to analyze paragraphs or financial reports provided by a group of analysts. Your task is to extract the investment decision: SELL, BUY, or HOLD. Provide only the extracted decision (SELL, BUY, or HOLD) as your output, without adding any additional text or information.
```

### 处理流程：
1. **输入**: 完整的交易决策文本
2. **分析**: LLM分析决策内容
3. **提取**: 识别关键决策词
4. **验证**: 确保决策格式正确
5. **输出**: 返回标准化的决策

### 决策类型：
- **BUY**: 买入决策
- **SELL**: 卖出决策
- **HOLD**: 持有决策

### 错误处理：
- LLM响应异常处理
- 决策格式验证
- 重试机制
- 备用分析策略

### 集成点：
- 与 `TradingAgentsGraph.propagate()` 集成
- 与 `Risk Manager` 最终决策集成
- 与 CLI 显示系统集成 