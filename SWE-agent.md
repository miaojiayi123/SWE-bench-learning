
# SWE-agent：an agent composed of an LM and ACI  基于论文总结
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
4.护栏减轻错误传播并加速恢复（LM的编辑应用前检查代码，防止出现编辑错误）
 
## How SWE-agent provides an ACI for LMs  技术组件

### **搜索，导航**  

**利用问题描述信号快速定位问题**
1.特殊命令find_file、search_file、search_dir
2.鼓励高效搜索，限制最多返回五十条结果

| **搜索** | `search_file <search_term> [<file>]`     | 在指定文件中搜索词。若未提供文件，则在当前打开文件中搜索。                                   |
|            | `search_dir <search_term> [<dir>]`       | 在指定目录的所有文件中搜索词。若未提供目录，则在当前目录搜索。                               |
|            | `find_file <file_name> [<dir>]`          | 在指定目录中查找给定文件名的所有文件。若未提供目录，则在当前目录搜索。                       |


### **文件查看器  File viewer**
1.调用 open 命令使用交互式文件查看器，传入相关文件路径
2.可用 scroll_down 和 scroll_up 移动窗口，用 goto 命令跳转到特定行。

| **文件查看器** | `open <path> [<line_number>]`            | 打开指定路径的文件。若提供行号，窗口将移动到包含该行。                                       |
|            | `goto <line_number>`                     | 将窗口移动到显示指定行号。                                                                   |
|            | `scroll_down`                            | 将窗口向上移动100行。                                                                        |
|            | `scroll_up`                              | 将窗口向下移动100行。                                                                        |


### **文件编辑器**

**edit命令解决Shell-only编辑的复杂性，提升LM效率**

1.edit 命令与文件查看器协作，允许代理替换打开文件中特定行范围
2.集成了linter（护栏） 
3.丢弃无效的编辑

| **文件编辑** | `edit <n>:<m> <replacement_text> end_of_edit` | 将打开文件中第n至m行（含）替换为给定文本。Python文件编辑后检查语法错误，若有则不执行。 |
|            | `create <filename>`                      | 创建并打开指定名称的新文件。                                                                 |



### **上下文管理**
提供ACI命令的正确使用示范

## 结果分析

**人类用户界面并不总是适合作为代理-计算机接口**
**紧凑高效的文件编辑对性能至关重要**
**护栏改善错误恢复**

**代理行为分析**
1.起步: 复现（create）或定位（find_file/search_dir）。
2.循环: “编辑（edit）后执行（python）。
---
