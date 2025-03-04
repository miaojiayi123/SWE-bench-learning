# OpenHands: 提供AI agent处理复杂软件系统
 
## Introduction

**AI 技术正在从简单的问答工具演变为能够主动与世界交互的代理**

### **开发、测试这些agents的核心组件**
1.交互接口：功能调用、代码执行
2.运行环境
3.通信机制：人与代理、代理之间

### AI agent有效开发软件亟待解决的问题
1.修改复杂代码
2.收集实时信息
3.安全性

## OpenHands架构

### 定义、实现AI agent
#### AI agent感知环境状态，根据用户指定任务生成动作

**1.State，Event Stream，auxiliary information**
State是一个数据结构，包含代理执行所需的所有信息。它是代理决策的基础。

**2.Actions**
**强调通过代码连接代理与环境**
1.IPythonRunCellAction：允许代理执行任意 Python 代码，适用于编程任务。
2.CmdRunAction：支持执行 bash 命令，适合操作文件系统或运行脚本。
3.BrowserInteractiveAction：允许代理与浏览器交互（如点击、导航）。

**3.Observations**
代理感知环境的变化，根据反馈体哦阿正行为

**实现新的agent**
定制化

## Agent Runtime 动作执行如何产生观察

**代理 -> 事件流中的动作 -> REST API -> 执行（bash/IPython/浏览器） -> 观察-> 返回代理**

## Agent Skills
**SWE-agent提出的ACI 帮助代理与计算机环境高效交互，AgentSkills是对ACI的实现**
定义python函数即可创建新工具，通用性，
| **工具名称**      | **功能描述**                              | **来源/备注**             |
|--------------------|-------------------------------------------|---------------------------|
| **文件编辑工具**  |                                           |                           |
| `edit_file`       | 从指定行修改文件                          | 改编自 SWE-Agent 和 Aider |
| `scroll_up`       | 向上滚动查看文件内容                      | 无特定来源                |
| `scroll_down`     | 向下滚动查看文件内容                      | 无特定来源                |
| **多模态文档处理** |                                           |                           |
| `parse_image`     | 使用视觉-语言模型（如 GPT-4V）从图像中提取信息 | 无特定来源                |
| `parse_pdf`       | 从 PDF 文件中提取文本                     | 无特定来源                |


## Agent Delegation委托
**多个代理之间的交互**
任务分工，通用代理与专业代理配合