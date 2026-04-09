# Content Digestion Skill

> **把任何长内容（文章、播客、视频字幕）消化成结构化笔记。两种模式：沉淀 / 发布。**

这是一个 [Claude Skill](https://www.anthropic.com/news/skills)，零依赖，单文件即可使用。

[English](./README_EN.md) · 中文

---

## 这个 Skill 解决什么问题

你每天都会接触到一些值得消化的长内容：一集 2 小时的播客、一篇 8000 字的长文、一个 3 小时的访谈视频。你大概率会做下面三件事之一：

1. **存进 Notion / Obsidian / 收藏夹**，告诉自己「以后再看」——然后再也没看
2. **强迫自己花两小时看完**，然后什么都没记住
3. **想做个笔记**，但不知道笔记该长什么样、做完放哪里

这个 Skill 把这件事变得简单：丢一份内容给 Claude，告诉它你想**沉淀**还是**发出去**，10 秒钟拿到一份结构化的笔记。

## 两种模式

### 1. 沉淀模式（Knowledge Mode）

**用途**：把内容存进你的知识库，以后随时调用。

**产物**：一份带 YAML frontmatter（含 `summary_for_index`、`concepts_to_create` 等对接 Karpathy LLM Wiki 范式的字段）的 markdown 笔记，正文用 `[[双链]]` 标注关键实体和概念，末尾自带「知识库整合建议」、建议文件名、以及一段直接可用于 Karpathy 工作流的 LLM agent prompt。

**直接可用于**：Obsidian、Notion、Apple Notes、本地 markdown 文件夹、Karpathy LLM Wiki 工作流的 `raw/` 目录、或者直接喂给 AI 当上下文。

→ 看完整样例（中文）：[`examples/knowledge-mode-example.md`](./examples/knowledge-mode-example.md) · 英文版：[`examples/knowledge-mode-example-en.md`](./examples/knowledge-mode-example-en.md)

### 2. 发布模式（Publishing Mode）

**用途**：把一份长内容（2-3 小时的播客、长文章）整理成有框架、有逻辑、有人文温度的长文笔记。

**两种用法**：
- **自用**：你想 10 分钟读完一个原本 2-3 小时的长内容
- **对外**：你想直接发到社交平台或分享给朋友

两种用法共享同一种产物形态：杂志长文风格的笔记，按主题板块组织（不是时间线流水账），保留有冲击力的引语和有温度的细节。**输出语言根据你和 Claude 对话的语言自动判断**——你用中文聊天就输出中文，用英文聊天就输出英文。

→ 看完整样例（中文）：[`examples/publishing-mode-example.md`](./examples/publishing-mode-example.md) · 英文版：[`examples/publishing-mode-example-en.md`](./examples/publishing-mode-example-en.md)

## 怎么用

### 1. 装上这个 Skill

#### 如果你用 Claude.ai (网页版 / 桌面 App)

> ⚠️ 注意：Skill 在 Claude.ai 里**不是**上传到 Project 的 Files 或 Instructions 里。它是一个**全局功能**，装一次之后所有对话和 Project 都能用。

**详细步骤：**

1. **先确认 Code Execution 已开启**
   - 进入 `Settings → Capabilities`
   - 找到 "Code execution and file creation" 这一项，确保是开启状态
   - 这是 Skills 功能的前置条件，没开 Skills 不会工作

2. **打包 Skill 文件夹**
   - 你下载的 `content-digestion/` 文件夹**整体**打包成一个 ZIP 文件（不是只把里面的 SKILL.md 拿出来打包）
   - Mac 用户：右键文件夹 → 压缩
   - Windows 用户：右键文件夹 → 发送到 → 压缩文件夹
   - 重要：ZIP 解压后的最外层应该是 `content-digestion/` 文件夹本身，里面才是 SKILL.md。如果你看到的是直接展开的文件，就打包错了

3. **上传到 Claude.ai**
   - 进入 `Settings → Customize → Skills`（或者 `Customize → Skills`，取决于你的 plan）
   - 点击 "+" 按钮 → 选择 "Create skill" 或 "Upload skill"
   - 上传你打包好的 ZIP 文件
   - 上传成功后，Skill 会出现在你的 Skills 列表里，确保它处于**开启状态**（toggle on）

4. **验证安装**
   - 在任意对话里粘贴一段文字稿，说"帮我消化一下这份内容"
   - 如果 Claude 反问你要"沉淀模式"还是"发布模式/读完"，说明 Skill 装好了
   - 如果 Claude 直接开始用通用方式总结，说明 Skill 没生效，回去检查上面三步

#### 如果你用 Claude Code CLI

```bash
# 把整个文件夹复制到全局 skills 目录
mkdir -p ~/.claude/skills/content-digestion
cp SKILL.md ~/.claude/skills/content-digestion/
```

Claude Code 会自动发现 `~/.claude/skills/` 下的所有 skill，无需重启。

### 2. 给 Claude 一份内容

任何方式都可以：
- 粘贴一段文字稿
- 上传 PDF / TXT / SRT 文件
- 发一个文章 URL（Claude 会自动抓取）
- 发一个 YouTube 链接（Claude 会尝试搜索第三方文字稿）

### 3. 告诉它你想要哪种模式

如果你不说，Claude 会简短问一句你想要哪种模式。如果你已经知道，下面任何一种说法都可以触发：

**触发沉淀模式**（存进知识库以后用）：

> - "帮我用沉淀模式消化这份内容"
> - "帮我消化一下，存进我的知识库"
> - "整理成笔记入库"
> - "把这个沉淀下来"
> - "做成结构化笔记，以后我要查"

**触发发布模式**（现在就读完，或对外发出去）：

> - "帮我整理成一篇可以发出去的长文笔记"
> - "我想 10 分钟读完这份内容"
> - "总结成一篇能发公众号的文章"
> - "整理成可以发小红书 / 推特 / 朋友圈的笔记"
> - "做成一篇能让我快速读完的长文"

**两种都要**：

> - "两种模式都做一份"
> - "先沉淀，再帮我整理一份能发出去的"

完成。

## 一个真实示例

下面是同一份内容（David Sinclair 在 The Diary of a CEO 播客上关于衰老逆转的 2 小时访谈），用两种模式产出的差异：

| | 沉淀模式 | 发布模式 |
|---|---|---|
| 形态 | YAML frontmatter + 主体笔记 + 知识库整合建议 | 杂志长文 |
| 长度 | ~3500 字 | ~5000 字 |
| 重点 | 结构化、可检索、关键数据精度 | 叙事流畅、人文温度、引语张力 |
| 给谁看 | 未来的你 / AI / 你的知识库系统 | 现在的你 / 你的读者 |
| 产物去处 | Obsidian / Notion / Apple Notes / 喂 AI | 发到社交平台或自己读完 |

完整样例（中文 + 英文,同一份 Sinclair 内容）：

- 沉淀模式：[`examples/knowledge-mode-example.md`](./examples/knowledge-mode-example.md) · [English](./examples/knowledge-mode-example-en.md)
- 发布模式：[`examples/publishing-mode-example.md`](./examples/publishing-mode-example.md) · [English](./examples/publishing-mode-example-en.md)

## 设计哲学

这个 Skill 有几个和市面上 AI 总结工具不一样的选择，会**直接影响你拿到的笔记长什么样**。

### 1. 默认保留，不是默认精简

**对你的影响**：你拿到的笔记会比你预想的「长」。一份 2 小时的播客不会得到 200 字总结，而是一份 3000-5000 字的笔记。

**为什么**：大多数 AI 总结工具默认在做「压缩」——把 2 小时的访谈压成 5 个 bullet points。信息留下了，但**让这个内容值得听的所有东西都没了**：那个让你停下来想了 10 秒的故事、那句你想截图发朋友的金句、那个让你恍然大悟的具体数字。这些细节才是你愿意花 2 小时听这份内容的真正原因。

> **一份你愿意三个月后回头再读的笔记，比一份只能扫一眼的摘要更值钱。**

### 2. 引语用「三明治」包起来，不是直接扔出来

**对你的影响**：发布模式里每一段嘉宾原话都不会孤零零地出现。前面有一句「编者引入」（告诉你接下来要看什么），中间是引语本身，后面有一句「编者解读」（告诉你这段话为什么重要）。

**为什么**：你读过那种「抄了一堆别人的话拼起来」的文章吗？读起来像看字幕——一句接一句，没人在引导你。那种文章读完就忘，因为**没有一个有判断力的人在帮你理解**。

真正好的长文笔记读起来像有一个朋友坐在你旁边，先告诉你「这段要注意」，然后让你看原话，最后告诉你「这就是关键」。这就是「三明治」——引语夹在编者的两句话中间。

这也是为什么这个 Skill 特别适合处理对话/访谈类内容（播客、视频访谈）。

### 3. 沉淀模式的笔记从一开始就为「以后能被找到」而设计

**对你的影响**：沉淀模式给你的笔记最上面会有一段 `---` 包围的元数据，正文里有些词被 `[[双链]]` 框起来。**这些不是装饰，是为了让你三个月后能找到这份笔记。**

**为什么**：大多数人存笔记的痛点不是「存的时候」，而是「想用的时候找不到」。你存了 100 篇 Notion 文档，三个月后想找「那篇讲衰老逆转的笔记」，只能凭模糊的关键词搜索。

frontmatter 给笔记自动打上「作者、日期、主题、相关概念」的标签，让 Notion / Obsidian 能像图书馆目录一样组织你的所有笔记。`[[双链]]` 把关键词变成可点击的链接——下一份笔记也提到同一个概念时，**两份笔记会自动连起来**。久而久之你得到的是一张知识网，不是 100 个孤岛文件。

> **沉淀模式不是给你一份漂亮的笔记，而是给你一份能和你未来笔记长在一起的笔记。** 用一份看起来像浪费功能；用到第 10 份你会感谢这个设计。

## Karpathy LLM Wiki 范式的「raw → markdown」一步

> **如果你不知道 Andrej Karpathy 是谁，或者从来没听过 LLM Wiki，可以直接跳过这一段——这个 Skill 不依赖你懂这些。这一段是写给那些已经在用或想用 LLM 维护个人知识库的人。**

如果你看过 Andrej Karpathy 关于 [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 的那篇 gist——他描述了一个用 LLM 增量"编译"个人知识库的完整工作流：

1. **Data ingest** — 把原始素材索引进 `raw/`，让 LLM 编译成 wiki
2. **IDE** — 用 Obsidian 作为前端
3. **Q&A** — 对 wiki 提问，LLM 自动检索和综合
4. **Output** — 渲染成 markdown / 幻灯片 / 图表，再 file 回 wiki
5. **Linting** — LLM 健康检查、补缺、连接发现
6. **Extra tools** — 搜索引擎等辅助工具

这是一个完整的、会自我成长的知识库系统。Karpathy 自己说，"there is room here for an incredible new product instead of a hacky collection of scripts."

**这个 Skill 解决的是其中最日常的一步：把单份原始内容（一篇文章、一集播客、一段视频字幕）初步处理成一份合格的 markdown，让它能被丢进 Karpathy 工作流中的 `raw/` 目录、被 LLM agent 立即理解和编译。**

它不维护 wiki，不做反向链接，不做编译——这些是更大的系统该做的事（或者你的 LLM agent 在 Obsidian / Claude Code 里直接做）。但是它确保：

- 你扔进 `raw/` 的每一份内容，**不再是一个原始 PDF / SRT 文件**，而是一份结构化的、有 frontmatter 元数据、有概念候选列表的 markdown
- frontmatter 里的 `summary_for_index` 字段直接对接 Karpathy 工作流中的 wiki 索引文件
- frontmatter 里的 `concepts_to_create` 字段直接对接 Karpathy 工作流中的"writes articles for them"步骤
- 沉淀模式输出末尾的「🧠 如果你在用 Karpathy LLM Wiki 工作流」段落里有**一段直接可用的 prompt**，告诉你的 LLM agent 如何把这份产物编译进 wiki

简单说：**Karpathy 描述了 6 个组件，这个 Skill 把第 1 个组件中"原始素材 → 可编译 markdown"那个最容易被忽略但最日常的转换做对了。其他 5 个组件不在它的范围内——它们是你自己的 LLM agent（Claude Code / Cursor / 等）在 Obsidian 里直接做的事。**

如果你不在用 Karpathy 范式：这个 Skill 对你也完全成立——你可以把它当成"产出能被任何 markdown 系统加载的高质量笔记"的工具。

如果你在用或想用 Karpathy 范式：这个 Skill 把你工作流的入口摩擦降到了最低。

## Roadmap

**v1.0.2（当前版本）**

- 沉淀模式 + 发布模式
- frontmatter（含 `summary_for_index` / `concepts_to_create` 等对接 Karpathy LLM Wiki 范式的字段）
- 双链 / 知识库整合建议 / 建议文件名 / 直接可用于 Karpathy 工作流的 LLM agent prompt
- SRT / PDF / 网页 / YouTube（通过第三方文字稿）输入
- 完整双语支持（中文 / 英文输出，根据用户对话语言自动判断；明确指定语言时按指定执行）
- 中英文双 README + 中英文双 examples

**v1.1（计划中）**

- 基于早期用户反馈优化引导话术和错误处理
- 增加更多输入源支持（如直接处理音频文件 → 转写 → 消化的端到端流程）
- 沉淀模式支持「用户提供已有 wiki 目录」的精确整合建议

## 这个 Skill 适合做什么

遇到一份有价值的长内容，想沉淀下来、积累素材、或者整理成笔记发出去——这就是这个 Skill 服务的核心场景。

具体来说：

- **行业大牛的演讲、长课程视频** —— 90 分钟的硬核内容，沉淀进知识库，或整理成长文发出去
- **有价值的播客** —— 2-3 小时的访谈，做成可检索的素材笔记，或可发布的长文
- **投资 / 行业研究报告** —— 上万字的研报，10 分钟读完精华，关键数据一个不漏
- **长文章 / 深度报道 / 论文** —— 值得读三遍的长文，转成你以后能反复回来翻的笔记
- **英文内容，中文笔记** —— 读英文比读中文慢 3-5 倍，有价值的英文长内容很容易被拖延。让 Skill 帮你消化成中文，1/3 的时间拿到精华

> 帮你做对「消化」这一步——节省时间，但信息准确度、质量、密度都不让步。

## 这个 Skill 不适合做什么

为了避免错误期待，明确说明边界：

- **不是 200 字的极简摘要工具**。如果你只需要一段话的总结，请用别的工具
- **不是逐字翻译工具**。如果你需要完整的中英对照翻译，请用专门的翻译工具
- **不替你判断内容值不值得读**。这个 Skill 假设你已经选好了值得消化的内容
- **不会自动维护你的知识库**。沉淀模式输出的整合建议是建议，不是自动化的 wiki 维护——真正的自动维护需要更大的系统（比如 Karpathy 范式里的完整 wiki 编译流程），这不在这个 Skill 的范围内

## License

[MIT](./LICENSE)。你可以自由使用、修改、分发，包括商用，只需保留版权声明。

## 关于作者

由 [Vinson Liang](https://github.com/VinsonLiangyc) 创建。

如果这个 Skill 对你有帮助，欢迎 star 这个 repo，或者把你用它产出的笔记分享出来——我很想看到这个 Skill 被用在我没想过的内容上。

如果你发现 bug 或者想提建议，欢迎开 issue。
