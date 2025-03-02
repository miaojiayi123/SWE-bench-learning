# SWE-bench

## What is SWE-bench
Software Engineering Benchmark 评估用LLM和AI代理在软件工程任务的解决上的表现的基准测试

# SWE-bench榜单前几名开源方法

## SWE-agent：an agent composed of an LM and ACI  基于论文总结
[SWE-Agent文档](https://swe-agent.com)。

## ACI: agent-comupter interface 代理-计算机接口

**ACI特点** 
1.在LM agent和计算机之间的抽象层
2.类似于IDE，专门为LM优提供一个接口以推动软件工程自动化
3.包括代码搜索，文件查看器，文件编辑器
4.LM无法直接操作基于GUI的应用程序，缺乏视觉理解能力。
ACI帮助LM代理理解应用程序在先前更改后的状态，管理历史记录，并提供模型能高效可靠使用的动作。

**ACI设计原则**
1.动作应简单易懂
2.动作应紧凑高效
3.环境反馈应信息丰富但简洁，编辑文件时更新修订内容对代理有帮助。
4.护栏减轻错误传播并加速恢复（代码语法检查器）
 
## How SWE-agent provides an ACI for LMs  技术组件

**搜索，导航**  
1.特殊命令find_file、search_file、search_dir
2.鼓励高效搜索，限制最多返回五十条结果

**文件查看器**
1.调用 open 命令使用交互式文件查看器，传入相关文件路径
2.可用 scroll_down 和 scroll_up 移动窗口，用 goto 命令跳转到特定行。

**文件编辑器**
1.edit 命令与文件查看器协作，允许代理替换打开文件中特定行范围
2.集成了linter（代码语法检查器） 
3.丢弃无效的编辑

**上下文管理**
提供ACI命令的正确使用示范

## 结果分析

**人类用户界面并不总是适合作为代理-计算机接口**
**紧凑高效的文件编辑对性能至关重要**
**护栏改善错误恢复**

**代理行为分析**
1.起步: 复现（create）或定位（find_file/search_dir）。
2.循环: “编辑（edit）后执行（python）。

