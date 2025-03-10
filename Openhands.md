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

## Agents' integration test 集成测试

**1.集成测试框架通过自动化任务执行和结果验证，测试代理的端到端功能**
人为定义任务和预期结果，当agent完成任务后将结果与gold file进行对比

**2.mocking 模拟LLM**
LLM的随机性：输入相同内容可能得到不同输出
在代理和LLM中设置一个模拟系统，提供事先定义好的相应，不需要一直频繁调用LLM

**3.重大变更时重新调用LLM**
添加新测试或修改现有提示时，脚本使用真实 LLM 生成新的提示-响应对。

## Workflow
**1.input 用户提供一个基础Docker image**
**2.OpenHands基于这个image构建OH runtime image**
**3.使用构建好的 OH 运行时镜像启动一个 Docker 容器**
**4.Communication 后端通过RESTful API 与容器内的运行时客户端通信**
**5.Action Execution 客户端接收后端发送的动作，在沙盒环境中执行**
**6.Observation Return 发送回 OpenHands 后端的event stream**
