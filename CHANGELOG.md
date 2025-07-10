# 更新日志 (Changelog)

## [未发布] - 07-10-2025

### 修复 (Fixed)
- **CLI界面类型错误修复**: 修复了 `cli/main.py` 中 `graph.stream()` 方法的类型错误
  - 添加了 `AgentState` 导入: `from tradingagents.agents.utils.agent_states import AgentState`
  - 将 `init_agent_state` 字典转换为 `AgentState` 对象: `AgentState(**init_agent_state)`
  - 解决了 "Dict[str, Any] cannot be assigned to parameter 'input' of type 'AgentState'" 错误

- **表格显示功能修复**: 修复了 Rich Table 组件的 footer 属性错误
  - 移除了不存在的 `messages_table.footer` 属性赋值
  - 改用 `messages_table.add_row()` 方法添加信息行
  - 解决了 "Cannot assign to attribute 'footer' for class 'Table'" 错误

- **日志文件编码错误修复**: 修复了日志写入时的 Unicode 编码错误
  - 将日志文件编码从默认 `cp936` 改为 `utf-8`
  - 添加了编码错误处理机制，使用 `errors="replace"` 作为后备方案
  - 解决了 "UnicodeEncodeError: 'cp936' codec can't encode character" 错误

### 技术细节
- **文件**: `cli/main.py`
- **修复者**: MariusWz
- **影响范围**: CLI界面的类型安全和表格显示功能
- **兼容性**: 向后兼容，不影响现有功能

---

## 版本说明
- 此更新日志仅记录非原作者的修改
- 原始项目功能保持不变
- 修复主要针对类型安全和界面显示问题 