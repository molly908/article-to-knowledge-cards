# make-knowledge-cards

一个 [Agent Skills](https://github.com/anthropics/skills) 格式的开源 Skill：把一篇文章自动转换成一套可复习、可自测的知识卡片。

## 1. 解决什么问题

- 读完一篇好文章，合上就忘——缺少一个能**主动回忆**的复习材料
- 手工整理卡片费时费力，还容易把一整段塞成一张"大杂烩"卡片
- 网上摘要工具只做"缩写"，不产出**能拿来考自己的学习材料**

make-knowledge-cards 让 Agent 替你完成这件事：读完全文，提取真正重要的知识点，生成 5-8 张标准化的复习卡片，每张都能用来自测。

## 2. 主要功能

- **标准五字段卡片**：每张卡片固定包含 `标题、核心知识、简明解释、例子、自测`，每张只讲 **1 个知识点**
- **只提取重要的**：优先选文章重点展开、反复出现、其他内容依赖的概念；路过式细节不入选
- **自动去重**：多段讲同一件事只保留一张卡片；结尾总结段视为复述，不单独成卡
- **不编造**：所有内容严格出自原文；原文没有示例时用原文概念做最小演绎并标注（演绎示例）
- **不凑数量**：文章只支撑 3 个点就输出 3 张，并说明原因，绝不注水到 5-8 张
- **语言跟随**：中文文章出中文卡片，英文文章出英文卡片
- **自动保存**：卡片在回复中展示的同时写入 Markdown 文件

## 3. 安装方法

把 `skills/make-knowledge-cards/` 整个目录复制到你所用 Agent 的 skills 目录：

```
# Claude Code
cp -r skills/make-knowledge-cards ~/.claude/skills/

# Codex CLI
cp -r skills/make-knowledge-cards ~/.codex/skills/
```

- 无需安装任何依赖：本 Skill 是纯指令（`SKILL.md`），不含脚本
- `agents/openai.yaml` 是 OpenAI Agent Skills 的界面元数据，其他平台会自动忽略
- 可选：复制后用官方校验脚本检查格式（脚本来自 [openai/skills](https://github.com/openai/skills) 的 skill-creator，不在本仓库内，需 Python 3 与 PyYAML）：

```
python quick_validate.py skills/make-knowledge-cards
# Skill is valid!
```

## 4. 使用方法

对装好该 Skill 的 Agent 说一句话即可。

**方式一：本地文件**（支持 `.md` / `.txt`）

```
把 C:\notes\http-cache.md 转成知识卡片
```

**方式二：直接粘贴文章**

```
把下面这篇文章做成知识卡片：
<在此粘贴文章全文>
```

Agent 的执行行为：

1. 通读全文，列出候选知识点，合并重复内容
2. 按重要性选出 5-8 个点（不足则更少并说明）
3. 在回复中输出全部卡片，**并自动保存**：
   - 文件输入 → 源文件旁的 `原文件名-cards.md`
   - 粘贴文本 → 当前目录下的 `文章标题-cards.md`（kebab-case）

**支持输入**：粘贴的文本、本地 `.md` / `.txt` 文件
**暂不支持**：网页抓取（URL）、PDF、Anki、图形界面

## 5. 输入输出示例

**输入**（`浏览器缓存详解.md` 节选）：

> 强缓存的含义是：浏览器直接使用本地副本，完全不询问服务器。它由响应头中的 `Cache-Control` 控制…… 早期的 `Expires` 头也可以实现强缓存，但它依赖客户端本地时钟，可能因时钟不准而失效……

**输出**（真实产出节选，原文自带例子）：

```markdown
## 1. 强缓存：不问服务器直接用

- **核心知识** — 强缓存由 Cache-Control 控制，生效期间浏览器完全不发请求，直接使用本地副本。
- **简明解释** — 服务器响应带上 Cache-Control: max-age=31536000，意味着一年内浏览器都信任本地文件。
  早期用 Expires 实现，但它依赖客户端时钟，时钟不准就失效，现代开发优先用 Cache-Control。
- **例子** — 文中做法：静态资源命名成 app.3f2a1c.js（文件名带哈希），内容一变文件名就变，缓存自然失效。
- **自测** — ① max-age 生效期间，浏览器会向服务器发请求吗？② 为什么 Expires 被认为不可靠？
```

**原文没有例子时**，会用原文概念做最小演绎并标注（真实产出节选）：

```markdown
- **例子**（演绎示例） — 像一杯同时混有铁钉和木屑的水，晃动后铁钉沉底、木屑浮起——熔融地球里的元素在重力下做了同样的"分选"。
```

## 目录结构

```
skills/make-knowledge-cards/
├── SKILL.md            # Skill 主指令（触发条件、工作流、卡片模板、质量规则）
└── agents/
    └── openai.yaml     # OpenAI Agent Skills 界面元数据
```
