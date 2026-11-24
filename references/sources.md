# 来源与抓取

## 1. 来源分工

| 来源 | URL 模板 | 强项 | 注意 |
| --- | --- | --- | --- |
| Wikisłownik (pl) | `https://pl.wiktionary.org/wiki/<词>` | 七格变格表、完整变位表、释义、例句、近义词 | 一页含多语言，只取 `język polski` 段；词形变化表最可靠 |
| WSJP PAN | 搜索 `site:wsjp.pl "<词>"` | 权威释义、义项划分、支配格（składnia） | 词条 URL 带数字 ID，不能直接拼；用搜索定位 |
| Glosbe | `https://glosbe.com/pl/en/<词>`、`/en/pl/`、`/zh/pl/<中文>`、`/pl/zh/` | 三语互译、双语例句对 | 机器翻译噪声大，只取高频义项并复核 |
| SJP PWN | `https://sjp.pwn.pl/slowniki/<词>.html` | 简明释义、拼写、部分变形 | 变形只给片段，完整表仍看 Wikisłownik |
| Tatoeba | `https://tatoeba.org/en/api_v0/search?from=pol&to=eng&query=<词>` | 英–波例句对（JSON） | 句子质量参差，选短句、直接翻译 |
| Wikipedia (pl) | `https://pl.wikipedia.org/wiki/<条目>` | 百科段 | 不是词典，不拿它当释义来源 |
| Diki | `https://www.diki.pl/slownik-angielskiego?q=<词>` | 英→波搭配、习语 | 页面广告多，例句部分为机器生成 |
| Wiktionary (en) | `https://en.wiktionary.org/wiki/<词>#Polish` | 英文视角的波兰语条目 | 波兰语表格不如 pl 版全 |

## 2. 常用 URL 实例

- 波兰语名词：`https://pl.wiktionary.org/wiki/kot`
- 波兰语动词：`https://pl.wiktionary.org/wiki/pisa%C4%87`（`pisać`）
- 中文查波兰语：`https://glosbe.com/zh/pl/%E7%8C%AB`（`猫`）
- 英文查波兰语：`https://glosbe.com/en/pl/cat`
- 波兰语查英文：`https://glosbe.com/pl/en/kot`
- PWN：`https://sjp.pwn.pl/slowniki/kot.html`
- 拼写/变形片段：`https://sjp.pwn.pl/so/kot.html`
- Tatoeba：`https://tatoeba.org/en/api_v0/search?from=pol&to=eng&query=kot&page=1`
- 百科：`https://pl.wikipedia.org/wiki/Kot_domowy`

## 3. 编码

先直接传 Unicode；返回 404 或空页时再百分号编码（UTF-8）：

| 字符 | 编码 | 字符 | 编码 |
| --- | --- | --- | --- |
| ą | `%C4%85` | ć | `%C4%87` |
| ę | `%C4%99` | ł | `%C5%82` |
| ń | `%C5%84` | ó | `%C3%B3` |
| ś | `%C5%9B` | ź | `%C5%BA` |
| ż | `%C5%BC` | 空格 | `%20` 或 `_` |

中文词按 UTF-8 百分号编码（`猫` → `%E7%8C%AB`）。

## 4. 抓取顺序

1. 先开 `pl.wiktionary.org/wiki/<词>`，拿释义 + 变形表 + 例句。
2. 用 WSJP 或 PWN 核对义项划分和支配格；义项多时以 WSJP 为准。
3. 用 Glosbe 补三语对应词和例句；只保留人工、通顺的句子。
4. 需要百科时开 pl.wikipedia。
5. 变形表仍缺格时，用搜索补：`"<词>" odmiana`、`"<词>" koniugacja`、`"<词>" dopełniacz`、`"<词>" biernik`。

## 5. Tatoeba JSON 读法

返回体里：

- `results[].text`：波兰语句子
- `results[].lang`：应为 `pol`
- `results[].translations[0][0].text`：英文对应句
- `results[].translations[0][0].isDirect`：`true` 表示直接对译，优先选
- `paging.nextPage`：还有下一页时继续翻

挑选标准：≤ 12 词、目标词以原形或常见变形出现、英文是自然翻译而不是逐词硬译。不要整段照搬，必要时轻改成更短的句子。

## 6. 失败兜底

| 情况 | 处理 |
| --- | --- |
| 页面 403/Cloudflare | 换 Glosbe / PWN，或用搜索摘要 |
| URL 404 | 检查变音符号编码；换 `en.wiktionary.org` 的 `#Polish` 段 |
| 查不到词条 | 说明“词典未收录”，给最接近的 2–3 个候选；不造词形 |
| 只有 shell、没有网页工具 | `curl -L "<url>"` 或 `Invoke-WebRequest -UseBasicParsing "<url>"`；仍不需要 Python |
| 完全无网络 | 明确标注“未联网核对”，只给高置信度内容，变形表标 `*` |

## 7. 引用与版权

- 输出末尾列出实际使用的 2–3 个链接。
- 释义用自己的话概括，不整段翻译词典定义。
- 例句若直接取自 Tatoeba，注明来源（CC BY 2.0 FR）；改写过的句子不必逐句署名，但仍列来源页。
- Wikisłownik 内容为 CC BY-SA；Glosbe 的例句来自其合作词典，转载时保留链接即可。
