# zh-en-pl-dictionary

![Agent Skills](https://img.shields.io/badge/Agent%20Skills-compatible-4B32C3)
![No Python](https://img.shields.io/badge/python-not%20required-2ea44f)
![License](https://img.shields.io/badge/license-MIT-blue)

给本地 AI agent 用的**中英波三语词典技能**：输入一个中文、英语或波兰语单词，联网查证后给出释义；动词附完整变位表，名词附七格变格表，并固定输出 **3 组英–波例句**（不附中文翻译）。

> A Chinese–English–Polish dictionary skill for Agent Skills-compatible tools (DSH, Codex, Claude Code). Look up a word, get definitions, full conjugation or declension tables, and exactly three English–Polish example sentences — no Python, no scripts, no dependencies.

## 为什么是“技能”而不是脚本

波兰语的变格、变位不规则点多，自己写规则引擎容易出错。这个技能把“查哪本词典、怎么交叉核对、表格怎么排、例句给几句”固化成指令，让 agent 直接读 [Wikisłownik](https://pl.wiktionary.org)、[WSJP PAN](https://wsjp.pl)、[Glosbe](https://glosbe.com)、[SJP PWN](https://sjp.pwn.pl) 的现成表格，再按统一模板输出。

因此它**不需要安装 Python、Node 或任何依赖**，只要能联网搜索/抓网页就能用。

## 特性

- **三语互查**：中文 → 英/波，英语 → 中/波，波兰语 → 中/英
- **自动识别变形词**：`poszedłem`、`kotu`、`książki`、`napisałem` 会先还原原形，再出表
- **动词**：现在时、过去时（3 性 × 6 人称）、将来时、命令式、条件式、主动/被动分词、副动词、动名词、无人称形式，并标体（impf./pf.）与对应体
- **名词**：七格 × 单复数，标性别与动物性，注明仅单/仅复数
- **形容词**：额外给三性 + 复数的变格表
- **百科段**：专名、术语、物种、地名按需补 1–3 句；普通词不硬凑
- **例句**：固定 3 组 `EN:` / `PL:` 对照，目标词加粗，**不加中文译文**
- **来源可查**：末尾附实际打开过的词典链接
- **纯 Markdown**：没有 `scripts/`，没有运行时，没有第三方库

## 演示

输入：

```text
wydawać
```

输出（节选）：

```markdown
# wydawać  [vɨˈdavat͡ɕ] · 动词，未完成体（impf.），及物 · 花费；发放；出版；发出

体：wydawać (impf.) ↔ wydać (pf.)

## 释义
1. 花（钱），支出 — spend — wydatkować, przeznaczać
2. 发放，交给；找零 — give out, hand over; give change — oddawać, przekazywać
3. 出版，发行 — publish — publikować
4. 发出（声音、气味、光）— emit, give off — wydzielać, emitować
5. 作出，发布（命令、判决、决定）— issue, hand down — nakazywać, ogłaszać

## 变位
| 人称 | 现在时 |
| --- | --- |
| ja | wydaję |
| ty | wydajesz |
| on / ona / ono | wydaje |
| my | wydajemy |
| wy | wydajecie |
| oni / one | wydają |

（过去时、将来时、命令式、条件式、分词等见完整示例）

## 例句（EN–PL）
1. **EN:** I'd rather not spend so much on clothes.
   **PL:** Wolałbym nie **wydawać** tyle na ubrania.

2. **EN:** This publishing house publishes books for children.
   **PL:** To wydawnictwo **wydaje** książki dla dzieci.

3. **EN:** The engine is making a strange noise.
   **PL:** Silnik **wydaje** dziwny dźwięk.

## 来源
- Wikisłownik · Glosbe · Tatoeba
```

完整的 `kot`（名词）和 `pisać`（动词）成品见 [`references/worked-examples.md`](references/worked-examples.md)。

## 安装

技能目录就是本仓库根目录，目录名建议与技能名一致：`zh-en-pl-dictionary`。

### DSH

```powershell
git clone https://github.com/HansWang2222/zh-en-pl-dictionary.git "$env:USERPROFILE\.dsh\skills\zh-en-pl-dictionary"
```

macOS / Linux：

```bash
git clone https://github.com/HansWang2222/zh-en-pl-dictionary.git ~/.dsh/skills/zh-en-pl-dictionary
```

### Codex

```powershell
git clone https://github.com/HansWang2222/zh-en-pl-dictionary.git "$env:USERPROFILE\.codex\skills\zh-en-pl-dictionary"
```

### Claude Code

```bash
git clone https://github.com/HansWang2222/zh-en-pl-dictionary.git ~/.claude/skills/zh-en-pl-dictionary
```

### 没有 git

下载仓库 ZIP → 解压 → 把 `zh-en-pl-dictionary` 整个文件夹复制到对应 skills 目录：

| 工具 | skills 目录 |
| --- | --- |
| DSH | `~/.dsh/skills/`（Windows：`%USERPROFILE%\.dsh\skills\`） |
| Codex | `~/.codex/skills/` |
| Claude Code | `~/.claude/skills/` |
| 其他 | 以其文档为准，支持 `SKILL.md` 即可 |

安装后新开一个会话，让 agent 重新扫描技能目录。

> 上面的 `HansWang2222` 是仓库地址示例，换成你自己的 GitHub 用户名即可。

## 上传到 GitHub

仓库已经按“根目录即技能目录”整理好，README、LICENSE、`.gitignore` 都在位，可以直接上传。

### 网页上传

1. GitHub → **New repository**，名称填 `zh-en-pl-dictionary`，Public。
2. **不要**勾选 “Add a README file / .gitignore / license”，因为仓库里已经有了。
3. 进入新仓库 → **Add file → Upload files**。
4. 把本目录里的 `SKILL.md`、`README.md`、`LICENSE`、`.gitignore`、`.gitattributes` 和 `references/` 文件夹一起拖进去。
5. Commit。

### 命令行

```bash
git init
git add .
git commit -m "feat: initial release"
git branch -M main
git remote add origin https://github.com/HansWang2222/zh-en-pl-dictionary.git
git push -u origin main
```

打版本标签：

```bash
git tag v1.0.0
git push origin v1.0.0
```

### 建议的仓库信息

- **Description**：中英波三语词典技能：查词、动词变位、名词变格，附 3 组英–波例句
- **Topics**：`agent-skills` `ai-agents` `dictionary` `polish` `translation` `codex` `claude-code` `dsh`

## 使用

装好后直接对 agent 说：

- `książka`
- `pisać 的变位表`
- `猫 用波兰语怎么说`
- `napisałem 的原形和变位`
- `wydawać się 是什么意思`
- `这个波兰语句子里的 biernik 为什么用这个形式`

技能会自动判断该给变位表还是变格表；名词不会硬套动词表，功能词也不会硬造变形。

## 输出格式

固定顺序，细节见 [`references/output-template.md`](references/output-template.md)：

1. 标题行：词 + IPA + 词性/性别/体 + 中文核心义
2. 释义：最多 5 条，每条「中文 — English — Polski」
3. 百科（可选）：1–3 句
4. 变位表 / 变格表
5. 例句（EN–PL）：恰好 3 组
6. 来源：实际使用的词典链接
7. 常见搭配（可选）：≤5 条

## 工作原理

```text
输入单词
  → 识别语言 / 还原原形 / 判断词性与体
  → 查 Wikisłownik（表）+ WSJP 或 PWN（义项）+ Glosbe（互译、例句）
  → 两个以上来源交叉核对
  → 按模板输出表格、释义、3 组英波例句、来源
```

查证顺序、URL 模板、编码方式和失败兜底写在 [`references/sources.md`](references/sources.md)；七格、性数、变位模式和自检清单写在 [`references/grammar-tables.md`](references/grammar-tables.md)。

## 目录结构

```text
zh-en-pl-dictionary/
├── SKILL.md                  # 技能入口：流程、规则、输出结构
├── README.md                 # 本文件
├── LICENSE                   # MIT
├── .gitignore
├── .gitattributes
└── references/
    ├── sources.md            # 词典 URL、抓取顺序、Tatoeba API、兜底
    ├── output-template.md    # 逐段骨架与占位符
    ├── grammar-tables.md     # 七格、变格/变位模式、自检清单
    └── worked-examples.md    # kot 与 pisać 的完整成品
```

## 数据来源

| 来源 | 用途 | 许可 / 说明 |
| --- | --- | --- |
| [Wikisłownik](https://pl.wiktionary.org) | 波兰语释义、完整变格/变位表、例句 | CC BY-SA |
| [WSJP PAN](https://wsjp.pl) | 权威释义、义项划分、支配格 | 学术词典，引用时保留链接 |
| [Glosbe](https://glosbe.com) | 中/英/波互译、双语例句 | 机器翻译噪声大，只取常用义项并复核 |
| [SJP PWN](https://sjp.pwn.pl) | 简明释义、拼写、变形片段 | 引用时保留链接 |
| [Tatoeba](https://tatoeba.org) | 英–波例句对 | CC BY 2.0 FR |
| [Wikipedia (pl)](https://pl.wikipedia.org) | 百科段 | CC BY-SA |

## 质量约定

- 变格/变位**逐格抄**词典表，不凭语感生成；不确定的形式标 `*`。
- 至少两个来源交叉；冲突时以 Wikisłownik / WSJP 为准，并在来源行注明。
- 查不到就说明查不到，给最接近的候选，不编词形。
- 例句只给英–波对照，**不加中文翻译**、不做逐词注释。
- 完成体没有现在时；其现在时形式表示将来，不与未完成体混表。

## 限制

- 依赖上述词典的收录范围，生僻词、新词可能查不到。
- 释义默认中文；需要英文释义为主时，改 `references/output-template.md` 的释义行即可。
- 例句固定 3 组，不按用户要求增减（除非用户明确要求）。
- 需要联网；完全离线时会标注“未联网核对”，变形表置信度下降。

## 贡献

欢迎 Issue / PR。请保持两条底线：

1. **零依赖**：不引入 Python、Node 脚本或第三方包。
2. **不编造词形**：新增规则必须能指回词典来源。

## License

[MIT](LICENSE)。词典内容的版权归各来源所有，转载释义/例句时请保留对应链接与署名。

## 致谢

- [Wikisłownik](https://pl.wiktionary.org) 与 [Wikimedia](https://www.wikimedia.org) 社区
- [Wielki słownik języka polskiego PAN](https://wsjp.pl)
- [Glosbe](https://glosbe.com)、[SJP PWN](https://sjp.pwn.pl)、[Tatoeba](https://tatoeba.org)
