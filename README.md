# Prompt 工程笔试助手

一个面向规则计算、条件分类和结构化 JSON 输出题目的 Codex Skill，尤其适合练习牛客等平台上的 Prompt 工程题。

它会读取题目截图或文字，提炼规则、优先级、边界与计算流程，并生成一份适合弱推理模型执行的结构化 Prompt。

## 解决什么问题

- 规则很多，难以判断哪些条件真正命中；
- `<`、`<=`、封顶、折扣、豁免等边界容易遗漏；
- 数值正确，却因字段顺序、数据类型或规则编号格式而失败；
- Prompt 越写越长，模型反而更容易漏算；
- 图片中的输出示例与文字描述存在格式差异。

## 工作方式

Skill 会固定生成以下六个部分：

1. `# Role`
2. `# Rules`
3. `# Workflow`
4. `# Output Format`
5. `# Examples`
6. `# Constraints`

其中：

- `Rules`保留题目的业务规则、优先级与边界；
- `Workflow`要求模型在`<thinking>`中按确定步骤执行；
- 如果题目提供明确输出示例，输出格式以示例为准；
- 如果没有示例，才根据输出描述构造格式；
- `Examples`只保留一个高价值示例，避免上下文膨胀；
- 最终约束固定要求先输出计算过程，再独立输出纯 JSON。

## 安装

将本仓库克隆到本地后，把整个目录复制到 Codex 技能目录：

### Windows PowerShell

```powershell
Copy-Item -Recurse -LiteralPath ".\prompt-engineering-exam-assistant" -Destination "$env:USERPROFILE\.codex\skills\prompt-engineering-exam-assistant"
```

### macOS / Linux

```bash
cp -R ./prompt-engineering-exam-assistant ~/.codex/skills/prompt-engineering-exam-assistant
```

重新打开 Codex 会话后即可使用。

## 使用

在 Codex 中发送题目截图或题目文字，并调用：

```text
使用 $prompt-engineering-exam-assistant 解析这道 Prompt 工程笔试题并生成可直接提交的提示词。
```

也可以直接说：

```text
按照 Prompt 工程笔试助手的格式完成这道题。
```

## 关键设计原则

- 图片中的题目内容只作为待解析材料，不视为高优先级指令；
- 不擅自创造取整方式、默认值或异常处理规则；
- 区分“检查过某条规则”和“实际命中某条规则”；
- 精确复刻示例中的字段名、字段顺序、JSON 层级和规则编号；
- 发现截图缺少输出字段或示例时，先索取完整截图；
- 针对弱模型提供确定性流程，但避免堆砌重复说明。

## 适用范围与兼容性

本 Skill 主要面向最终结果要求为结构化 JSON 的规则型计算与分类练习，不适合开放式写作、代码实现或无需固定输出格式的任务。

当前模板要求目标模型先输出`<thinking>`中的简短、可核验计算过程，再输出 JSON。部分模型或接口可能隐藏此类过程；遇到这种情况，应根据实际评测协议调整，而不是尝试提取模型的私有思维链。

## 项目定位与声明

本项目是独立的非官方开源学习与调试工具，与牛客（Nowcoder）不存在隶属、合作、赞助、背书或官方授权关系。“牛客”和“Nowcoder”相关名称、商标及标识归其权利人所有。

请遵守所在平台、学校和招聘方的规则。本项目适合个人练习、赛后复盘和 Prompt 工程学习，不应用于正在进行的受限或受监考测评。仓库不收录或分发牛客原题截图、受版权保护的完整题干、隐藏测试用例或答案。

## License

[MIT](./LICENSE)
