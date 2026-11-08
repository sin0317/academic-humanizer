---
name: academic-humanizer
version: 1.0.0
display_name: 学术论文去AI味（academic-humanizer）
display_name_en: "Academic Humanizer: papers, theses, and grant proposals"
description_zh: |
  学术论文专用的去 AI 味与文风校准技能：中文学位论文/期刊稿按知网、万方、维普、朱雀实测敏感维度清理 AI 腔（摘要八股、理论排比、统计分组对称、拔高尾巴），英文稿按六层学术结构清理 AI tells 并做 claim↔evidence 核对，另含 NSF/NIH 基金申请书模式。全程守住数字、公式、引文不改的学术底线。
description_en: |
  An academic-only humanizer for Chinese theses/journal manuscripts (calibrated against CNKI, Wanfang, VIP, and Zhuque detectors) and English papers/conference submissions (six layers: general tells, academic tells, preserve conventions, claim-evidence discipline, voice/venue, NSF-NIH proposal mode). Never alters a number, equation, or citation.
description: |
  学术论文方向的去 AI 味技能（中英双语）。整合两套方法论：① 英文学术六层法（通用 AI-tell 目录 → 学术专项 tells → 保留学术惯例 → claim↔evidence 纪律 → 声音与期刊/会议匹配 → NSF/NIH 基金申请模式），守住"每个主张都要挂证据、动词不得强于证据"；② 中文实测校准（知网/万方/维普/朱雀/BestPaper/论文狗 四档敏感维度），针对中文论文特有的摘要八股、理论排比、年表平铺、分类对称、统计分组对称、拔高尾巴、政策建议"并…并…"排比逐条给改法。
  用于：中文学位论文（本/硕/博）、中文期刊投稿、英文期刊/会议投稿、基金申请书（NSF/NIH/国自然）、rebuttal 与 response。不做公文、月报、公众号与营销文案（那些走 humanizer-zh-pro / human-writing）。
  触发词：论文去AI、学位论文降AI、期刊投稿去AI味、摘要八股、论文润色去AI腔、投稿前检查AI率、academic humanizer、grant proposal 润色、claim evidence 检查。
examples_zh:
  - "这篇硕士论文的摘要和结论去AI味，学校用万方检测，报告里结论段标红62%"
  - "英文论文 related work 去AI味，投ICLR，引文一个别动"
  - "润色NSF Project Summary，愿景别删，但每个大胆主张都要有可行性落脚"
examples_en:
  - "Humanize this thesis abstract and conclusion; our school uses Wanfang, the conclusion scored 62%"
  - "Humanize the related work for an ICLR submission; keep every citation"
  - "Polish this NSF Project Summary: keep the vision, but back every bold claim with feasibility"
license: MIT
author: sin0317
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
metadata:
  version: "1.0.0"
  trigger: 编辑或审阅学术论文、学位论文、期刊投稿、基金申请书，去除 AI 写作痕迹并校正主张与证据的强度匹配
  scope: 中文学位论文/期刊稿、英文期刊会议稿、基金申请书、rebuttal
  sources:
    - "AIScientists-Dev/academic-humanizer v0.3.3 (MIT)"
    - "blader/humanizer (MIT), 经 academic-humanizer Layer 1 引入"
    - "humanizer-zh-pro v1.4.10 (MIT, 尹洁平/杰平)"
  references:
    - references/detector-calibration-zh.md
    - references/academic-tells-bilingual.md
    - examples/before-after-zh.md
    - examples/before-after-en.md
---

# Academic Humanizer：学术论文去 AI 味

把 AI 辅助写出来的论文，改回**能通过同行评审的样子**：句子清楚了，声音是作者自己的，每个主张都挂得住证据，而数字、公式、引文一个没动。

这个技能只干两件事，顺序不能反：

1. **拆 AI 腔**（见 §4 中文层、§6 英文层）。AI 腔不是"用词高级"，而是**空泛 + 整齐**：空泛的拔高词，配整齐的对称结构。
2. **守学术纪律**（见 §6.4 claim↔evidence、§8 保留清单）。通用 humanizer 会把"the results suggest X"改成"the results prove X"，那是**制造**过度主张。学术稿的底线是：动词不得强于证据。

## 0. 边界：什么时候不用本技能

| 文本类型 | 去哪 |
|---|---|
| 中文学位论文、中文期刊稿、开题/中期报告 | **本技能** §3–§5 |
| 英文期刊/会议论文、arXiv 稿、rebuttal | **本技能** §6（Layer 1–5） |
| 基金申请书（NSF/NIH/国自然/省基金） | **本技能** §6（Layer 6） |
| 公文、红头文件、工作月报、汇报材料 | `humanizer-zh-pro` |
| 公众号、知乎、博客、科普、演讲稿 | `human-writing` |
| 英文博客、营销文案、百科条目 | `humanizer` |

本技能**不承诺"绕过 AI 检测"**。它做的是特征清理——把机器味的句式密度降下来，这在中文学术送审里是现实需求。但作者对 AI 使用的披露义务不因此消失，投稿前按目标期刊/学校的政策声明。

---

## 1. 五条铁律（保真契约，任何情况下不破）

1. **数字、公式、图表编号、引文与参考文献，一字不改、一条不删、一条不加。** 缩写在原文出现几次就还是几次。
2. **结构对等。** 原文五段，改写后五段；原文三个贡献，改写后仍三个；原文有表 3 的引用，改写后还有。
3. **不补造事实。** 可以把"大幅提升"换成"提升 3–6 个百分点（表 2）"，可以把无证据的主张降级，但**不许替作者编数据、编实验、编文献、编基金号、编合作单位**。数据来源只有三处：原稿已有、作者另给的素材、可核实的公开来源——并且必须标明是哪一种。
4. **中性即人味。** 学术稿不注入幽默、感叹、网络语、第一人称情绪。作者的"声音"体现在**取舍、节奏、谨慎的程度**上，不体现在俏皮话。只有学位论文的致谢、以及引言里"我为什么做这个题"的部分，才允许一点个人语气。
5. **先审计后动笔。** 没有逐段列出命中清单之前，不改任何一句（见 §3 Step 2）。

---

## 2. 冲突消解：两套方法论对不上的地方，这样办

整合两套规则时有三处直接冲突，已按下表定档，不必再纠结：

| 冲突点 | 通用中文 humanizer 的立场 | 学术 humanizer 的立场 | 本技能采用 |
|---|---|---|---|
| 要不要"注入灵魂/个性" | 适当用"我"、允许一点乱、要有观点 | 绝不注入观点与个性，中性精确本身即人味 | **学术稿采用后者**；仅致谢/问题缘起允许前者的弱化版 |
| 破折号 | 过度使用要改，但可保留 | **英文稿彻底删除 em dash** | 英文稿删净；中文稿每千字 ≤2 处，且不得用来做"肤浅分析拖尾" |
| 补数据降 AI 率 | 规则 P：抽象处补年份、地名、数字 | 绝不改数字、绝不编造 | **只能补原稿已有或作者提供的真实数据**；无数据可补时改为"降级主张 + 指出缺什么证据"，不许编 |

---

## 3. 流程（Step 0 → 7）

**Step 0 预检（不许跳过）。** 五件事先问清楚，缺哪项就按括号里的默认值走，并在交付里明写假设：

1. 文档类型：中文学位论文 / 中文期刊稿 / 英文期刊会议稿 / 基金申请书 / rebuttal（默认：中文期刊稿）
2. 目标机构与**它用的那款检测器**：学校用知网还是万方/维普？期刊指不指定？（默认：中文→知网口径；英文→不做检测器校准）
3. 是否已有检测报告？（有则**以报告标注片段为唯一改写依据**，见下）
4. 有没有作者自己的已发表稿/已录用稿作为声音样本？（有则先读一段，摸句长、连接词习惯、hedging 力度）
5. 目标期刊/会议的文风（ICLR/NeurIPS 简练直接、结果前置；Nature/PNAS 更铺陈；中文社科类学报多铺陈）

**Step 1 三平台 AI 率预估（中文稿强制，先告知用户再动手）。** 按 §5.2 的口径给出区间预估表，明确标注"特征预估，非送检实测"。用户知情后才决定改到什么程度。

**Step 2 审计（只列不改）。** 逐段输出：

| 位置 | 命中模式（规则号） | 原句摘录 | 拟改法 | 证据状态 |
|---|---|---|---|---|

"证据状态"一列是学术稿专属：这句主张有没有数字/表/图/文献支撑？动词是否过强？没有证据的先标出来，不要靠改文笔糊过去。

**Step 3 改写。** 按 §4（中文）或 §6（英文）逐条落地。同一段里**先拆结构对称，再换词**——顺序反了等于白改，因为检测器盯的是句式密度，不是词表。

**Step 4 证据核对（claim↔evidence pass）。** 对每一句实证主张问两件事：有没有证据指针？动词强度配不配得上证据？见 §6.4。

**Step 5 自检。** 过 §9 清单。

**Step 6 交付。** 按 §10 的格式：改后全文 + 变更摘要 + 质量评分表。

**Step 7 复测闭环。** 用**同一款**检测器再扫一遍，确认原标注片段降下来了。换检测器复测得说明"两次结论不可比"。

---

## 4. 中文层：按章节功能组织的 AI 腔清单

不按字母表走，按**论文的段落功能**走——作者改稿时手里拿的是一节一节的内容，不是一个词表。括注里的字母是原 `humanizer-zh` 规则号，便于和检测器校准档案对照。

### 4.1 摘要与引言

**Q. 摘要八股（万方头号敏感信号）。** 固定套路：`本文综合运用[方法A]、[方法B]与[方法C]，系统识别[对象X]、[Y]与[Z]，检验[机制]，进而构建[模型]，并据此提出[方案]，可为[宏大目标]提供参照。`
"综合运用—系统识别—检验—构建—提出"这条串行动词链，万方几乎必标（实测可到 79%）。

> 改前：本文综合运用历史考证、GIS 空间分析与分层问卷实证，系统识别流域农耕遗产的演进脉络、五大核心基因与多层级价值，检验文化价值感知对多元主体保护意愿的作用机理，进而构建"基底刚性保护、文化活化、溢价反哺"闭环发展模式，并据此提出法治化管控、田长制考核优化等政策方案。文中实证呼应了省内两大典型农耕景观区，可为统筹耕地刚性保护与农耕文明活态传承提供参照。
>
> 改后：某省建立"数量、质量、生态、文化"四位一体耕地保护体系后，文化维度怎么转成社会护地动力，仍是短板。本文以某梯田、某湖区圩田为样本，用历史考证、空间分析与问卷实证，追踪这套体系的演进脉络，检验"文化认同→保护意愿"这条路径，据此提出法治化管控与田长制考核优化两类对策。两处样点的问卷共 N=412 份（表 1）。

改法：拆成"背景与缺口 → 做了什么 → 得出什么 → 所以呢"四拍；结尾删掉"可为…提供参照"。

**2.5 式程式化开场（英：Formulaic openers）。** `近年来，X 受到广泛关注`、`随着…的快速发展`、`在…背景下`。中文检测器（知网、万方、维普）全部敏感。
> 改法：直接给这个领域的结构性缺陷，或一个具体数字。"近年来深度学习在故障诊断中受到广泛关注" → "轴承故障样本少、工况漂移大，现有诊断模型在跨工况下准确率掉 20 个百分点以上（表 1）。"

**F. 意义拔高。** `具有重要意义`、`重大而深远`、`为…贡献了智慧`、`提供了…样板/范式`。
> 改法：换成可验证的结果。"对全省耕地保护具有重要意义" → "该做法使该县违法占用耕地同比下降 37%（2023—2025）。"

### 4.2 文献综述（知网、万方、维普三家共同的误判重灾区）

**"XX 等人提出了……"链式罗列（Y）。** 万方对这类句式最敏感，实测标红集中在此。
> 改前：张三等提出了 A 方法；李四等提出了 B 方法；王五等提出了 C 方法。
> 改后：早期工作把该问题当作纯回归处理[3,7]，直到 A 方法引入自适应权重[9]才把误差压到 5% 以内；但这类方法都假设标签干净，一旦标注噪声超过 10% 就退化——这正是本文要处理的缺口。

改法：**每引 2–3 篇插一句自己的判断**（它解决了什么、漏了什么、和本文什么关系）。综述不是文献目录，是论证。

**K. 年表平铺。** 从远古到现代按时间顺序罗列事件、人物、遗址，没有取舍。
> 改法：按**论点**组织而非按时间。明确说"我只取哪几个节点，因为什么"。

**连接词密集（知网、维普双敏感）。** `首先/其次/再次/最后`、`一方面/另一方面`、`此外/与此同时`、`综上所述/由此可见`。连续两句以上用连接词开头，就是在给检测器发信号。
> 改法：让逻辑自己走。真需要过渡时用具体指代（"这一点在…上更明显"）而不是抽象的"此外"。

### 4.3 理论框架

**J. 理论排比腔。** `XX 理论揭示……；YY 理论强调……；ZZ 理论主张……` 平行罗列，像在抄词条。朱雀高敏感（≥0.98），万方通常不标。
> 改前：人地关系理论揭示梯田是长期人地磨合的结晶；活态文化理论强调农耕民俗须依托耕作方能存续；多功能价值理论主张将隐性非使用价值纳入管护范畴。
> 改后：本文把耕地价值从单一生产功能拓展到生态、文化与认同三个层面，主要借多功能价值理论；人地互动与乡土认同则作为解释保护动力的辅助视角。

改法：只留真正用到的核心理论，其余删；用因果/转折替代分号排比；每引入一个理论，必须回答"本文拿它解释什么"。

**L. 分类标签的机械展开。** `四维`、`五位一体`、`三大机制`、`四大基因`，然后每一类用同一套句式解释一遍——这是 AI 最爱的整齐结构。
> 改法：分类可以留，但要打破绝对对称。允许各类长短不一，允许某类只给一句，允许用具体例子代替抽象定义。别每类都用同一个谓语（"是/以/涵盖/贯通"）。

### 4.4 方法与模型

**模板化定义与算法描述（万方最凶，实测同篇 56% vs 知网 21%）。** 标准化的"定义—假设—步骤"三件套，用词高度同质，是万方统计特征模型的靶心。
> 改法：把教科书式定义换成"本文这里怎么用"的写法；补具体操作细节（参数怎么定、数据怎么清洗、异常怎么处理）；"XX 等人提出了…"改成"沿用 [9] 的框架，但把岭参数改为按折自适应"。

**M. 统计术语与机制词堆砌。** `显著正相关（P<0.01）`、`完全中介作用`、`关键纽带`、`协同机制`——概念循环定义，读完不知道发生了什么。
> 改法：统计结论后面必须补一句"这意味着什么"，落到"谁、在什么情境下、做了什么"。
> 改前：文化价值感知与护地意愿显著正相关（P<0.01），且隐性价值的作用强于显性经济价值。
> 改后：问卷显示，受访者对历史遗产与乡土认同的认同度越高，表达的保护意愿越强（β=0.42，P<0.01）；相比之下，经济收益的解释力弱得多（β=0.09，不显著）。换句话说，讲"这地方是你的根"比讲"这地能多打粮"更管用。

**公式、伪代码、参数表**：一般不触发任何检测器，原样保留，不要"为其降 AI 率"而改写。

### 4.5 结果与讨论

**W. 统计分组对称（统计型论文的高频触发器，朱雀/BestPaper/论文狗全中）。** 典型形态：

- **四并列 + 百分比**：优等/高等/中等/低等地各带百分比，分号机械并列
- **三对称**：重点防控区/关注预警区/一般管控区；制度完善/分区施策/技术支撑
- **①②③ 枚举**：三项及以上，每项句式等长
- **区域四分区**：四个区各写一句等长描述
- **四字对仗式概括**：`优等集中、高等突出、整体偏弱、中等为主`

实测证据很硬：同一篇统计稿，BestPaper 78.15%（42 段 High）、朱雀 82.51%、论文狗正文 71.08%，其中案例剖析与治理响应段落**逐段 100%**——每一个 100% 都精确对应一个对称结构。而同一篇里数据更密、但展开成叙述的"关键制约"段，朱雀只给 0.2560、论文狗 76.89%。

> **触发的是对称，不是数据。** 数据再密，只要被对称并列包裹，激进检测器照样标红。

> 改前：质量等别呈优等 12.3%、高等 28.6%、中等 41.2%、低等 17.9% 的分布；重点防控区 3 个、关注预警区 5 个、一般管控区 12 个；建议从制度完善、分区施策、技术支撑三方面推进。
>
> 改后：质量等别以中等为主（41.2%），优等地只占 12.3%，且集中在北部三县；低等地 17.9%，主要落在坡度大于 15° 的地带（表 4）。风险分区上，真正需要盯的是 3 个重点防控区，其余 17 个区可按常规频次巡查。治理上最见效的一步是把分区结果接进现有的田长考核，技术支撑反而是次要的。

改法：保留全部数字、百分比、地名、年份（这是学术精确性的命根），只把"对称并列的呈现方式"改成"有主次、长短交错"的叙述。用 `其中/尤其/相比之下/真正需要盯的是` 替代并列分号。

**P. 数字与事实是压分最有效的手段——但有前提。** 抽象论述里嵌具体年份、地名、数字、政策文件名，比改句式更能降 AI 率。**前提见铁律 3：只能嵌原稿已有或作者提供的真实数据。** 没有就空着，改为把主张降级。

### 4.6 结论与政策建议

**R. 拔高尾巴（万方二号敏感信号）。** 实证段或政策段末尾强行上价值：`实证结果印证了上级关于…的工作要求`、`为制度落地提供了现实支撑`、`为…提供参照`。
> 改法：整句删。直接陈述发现或政策效果，用具体数据收尾。

**S. 政策建议的多重"并…并…"排比。** `政策上应完善…，并选取…，并探索…`——结构整齐、主语缺失，万方易判 AI（实测 5.3 段 79.62%）。
> 改法：拆成有主语的短句，按动作主体或时间排序。真要并列，用自然衔接，别连续三个"并"。

**T. 机制词空泛结论。** `构成多元共治格局`、`形成正向循环`、`构建可持续的…体系`——把已经说过的话重新打包成口号。
> 改法：一句话说清"谁、在什么条件下、产生了什么具体效果"。"把田长制考核与村规民约、乡贤议事结合起来，外部约束和内部认同互补，比单靠执法更可持续。"

**N. "构建/形成/打造"模式包装 + 四字标语。** `构建…闭环模式`、`形成…长效机制`，以及`刚性守安全、文化促长效`这类四字对仗。
> 改法：先讲实际做法，再点出它是什么模式；对仗标语拆成完整句，补主语宾语。

### 4.7 全篇通用

| 规则 | 病灶 | 改法 |
|---|---|---|
| **O** | 判断动词单一：`表现为/呈现出/体现出/构成了/印证了` 通篇连用 | 换具体动词：发现、显示、导致、反映、说明、使、让；并补主语 |
| **A** | 空洞引领词：`以…为引领/抓手/牵引/支撑` 每句都套 | 直接说动作与对象；每千字 ≤1 处 |
| **B** | `一是…二是…三是…` 强行凑三条 | 能合并就合并；必须列举时改自然衔接；相近内容并一段 |
| **D** | 万能动词：`赋能/助力/夯实底座/锚定目标/提质增效` | 补主语与宾语，说清"谁对什么做了什么" |
| **E** | `综上所述/总而言之/由此可见` 收束 | 直接给结论，或换"本研究认为""据此"；无实质结论的整句删 |
| **G** | 模糊量词：`多项/若干/一大批/全方位/近年来`（无年份） | 补具体年份、数字、地名 |
| **H** | 主语缺失：`得到了…/进行了…/被…` 通篇 | 补主语。但**actor 无关的被动是合法的**，见 §8 |
| **I** | 中英标点混用：半角逗号、英文括号、多余空格 | 统一全角；缩写保留半角并加中文括号，如"GDP（国内生产总值）" |

### 4.8 通用 AI 模式里，论文也会踩的 12 条

原 `humanizer-zh`「通用内容模式 24 类」中，有 12 类会出现在学术稿里。其中 3 类（知名度强调、挑战与展望提纲、虚假范围）在中文论文的引言与结论里尤其常见，且**会被检测器算作空泛信号**。

| # | 模式 | 论文里的样子 | 改法 |
|---|---|---|---|
| 1 | 强调知名度与媒体报道 | 引起了广泛关注、受到学界高度重视、被多家媒体报道 | 换成学术定位：谁在哪个问题上没解决 |
| 2 | 句尾 "-ing" 式肤浅分析拖尾 | ……，**彰显了**…意义 / **体现了**…价值 / **确保了**…效果 / **反映出**…趋势 | 删尾巴；或把尾巴提成主句，补主语 |
| 3 | 宣传广告式语言 | 开创性、领先、卓越、丰硕成果、亮点纷呈、成效显著 | 换事实：指标、幅度、对照 |
| 4 | 模糊归因（无引文） | 有研究认为、大量研究表明、学界普遍认为 | **学术稿里这条比公文严重得多**：无引文归因是学术不端风险，必须给文献或删 |
| 5 | "挑战与未来展望"提纲腔 | 尽管取得进展，仍面临**数据不足、方法局限、应用受限**三重挑战。未来可从以下方向展开…… | 只留你**真遇到**的那一个限制，并说明它如何影响你的结论 |
| 6 | 回避系词"是" | 作为…的重要组成部分、扮演着…角色、构成了…的基础 | 直接"是 / 占 / 贡献"。学术稿里"X 是 Y"往往最准确 |
| 7 | 刻意换词（同义词循环） | 相邻句里"耕地—土地—农地—地块"来回换 | 术语必须一以贯之。**学术稿换词会破坏概念一致性**，这不是文采问题 |
| 8 | 虚假范围 | 从微观到宏观、从理论到实践、涵盖从 A 到 B 的全链条（两端不在同一尺度） | 具体列出你实际覆盖的层级 |
| 9 | 过度限定叠加 | 在一定程度上可能或许有助于 | 留一个最贴切的限定词 |
| 10 | 通用积极结论 | 取得了显著成效、前景广阔、具有广阔应用前景 | 换可验证效果，或删 |
| 11 | 装饰性排版 | 正文 **加粗**、项目符号清单、✅ ★ 等符号 | 学术正文改连续行文（表格、图注、贡献列表除外） |
| 12 | 标点与引号 | 英文 curly quotes（“ ”）、半角逗号、中英混排多余空格 | 统一全角；缩写与数值保留半角（GDP、P<0.01） |

另有三类属于**对话产物残留**，出现在论文里一律删净（多见于把 AI 对话直接粘进文档时）：

- **协作交流痕迹**：希望本文对您有帮助、如需进一步修改请告知、以下是修改后的版本
- **知识截止免责**：截至我的知识截止日期、根据我所掌握的信息
- **谄媚语气**：这是一个非常好的问题、您的观察非常敏锐

---

## 5. 中文送审：检测器校准（决策版）

> 完整证据（每档的样本、数值、标红机制、2026 阈值）见 `references/detector-calibration-zh.md`。这里只给"改稿时怎么用"。

### 5.1 四档敏感维度（首要规则：结论不可混用、不可互证）

同一篇文章，不同检测器能差出数倍，标注片段几乎不重叠。实测跨平台反证：同一篇 **知网 3% vs 维普 67%**；同一篇算法定义密集硕论 **万方 56% vs 知网 21%**。

| 档位 | 检测器 | 盯什么 | 典型正文 AI 率 |
|---|---|---|---|
| 激进档 | 朱雀 / BestPaper / 论文狗 | **密集空泛 + 结构对称**的句式密度 | 71–83%（同稿三档互证） |
| 中等档·模板凶 | 万方 | 模板化 / 定义类 / 算法描述 / 摘要八股 / 标准学术用语 | AI 辅助 45–69%，重度模板 85% |
| 中严档·规整凶 | 知网 | 写作逻辑高度规整的人类原创综述 / 推导 / 定义 | 纯 AI 37–78%，AI 辅助 21–45%，手写规整 29–47%，纯人工 2–3% |
| 中等档·句式凶 | 维普 | 均匀长句 / 连接词密集 / 段落对称 / 整齐排比 | 纯 AI 75–95%，AI 辅助 47–72%，波动极大 |

**2026 通用阈值**：本科 ≤30% / 硕士 ≤20% / 博士 ≤10% / 期刊 ≤25%（超 50% 多直接退稿）。

### 5.2 按目标检测器决定"清什么、留什么"

| 目标 | 必须清 | 放心留 |
|---|---|---|
| **知网** | Q/R/S/T + **J/K/L/M/N 在综述、定义、推导、实验步骤等"太规范"章节的密集堆叠**（知网对规整的人类写作更敏感，手写零 AI 也可能 29%，其中推导 47%、综述 41%） | 统计分组对称（W）通常不触发；数据表、公式、带具体数字的实证段一般不标红；参考文献通常不计入 |
| **万方** | Q/R/S/T + **模板化/定义类/算法描述/标准学术用语**（万方对这几类比知网凶，同篇 56% vs 21%） | 带具体年份/地名/数字的排比、年表、分类标签；数据表、伪代码、公式一般不触发 |
| **维普** | **句式工整本身**：把 20–30 字均匀长句拆短、句式错落、删密集连接词、破段落对称；再叠加 Q–T | 长短交错且带具体事实的段落；数据表、公式、伪代码一般不标红 |
| **朱雀/BestPaper/论文狗** | **J/L/M/N/O/W 的句式架构**：降空泛、破对称、补主语、以具体事例或数据落脚 | 数据一个不删（删数据是最常见的错误自救）；参考文献即使被标也按规范排版，不为降 AI 率改写 |

**操作顺序**：先拿目标机构的检测报告 → 按报告标注片段优先改写 → 报告**未标注**的段落，即使句式整齐也不强改（保护信息量）→ 改完同款复测。

**参考文献假阳性**：BestPaper 这类激进档会把参考文献标到 88–98%（短、格式统一、无语义变化），但朱雀判 0.0001、论文狗不标、知网/万方通常不计入。**不要为降 AI 率改写或删减参考文献格式**——改了也继续被标，还伤可追溯性。

### 5.3 三平台预估表（Step 1 强制输出 / Step 6 强制对照）

| 平台 | 敏感维度 | 主要命中信号 | 修订前预计 AI 率 |
|---|---|---|---|
| 知网（中严档） | 语义规整 / 太规范章节 | J/K/L/M/N（综述·定义·推导） | 30–47% |
| 万方（中等档·模板凶） | 模板化 / 定义类 / 八股 | Q/R/S/T/Y | 45–69% |
| 维普（中等档·句式凶） | 句式工整 / 连接词 / 段落对称 | 均匀长句 + 首先/其次 + 段落等长 | 47–72% |

预估口径：按文档实际呈现的信号类型映射到各平台敏感维度，给**区间**。文档事实具体、长短交错、无模板 → 各档相应下调（知网 2–20%、万方 5–20%、维普 8–20%）。表下必须注明"基于校准档案的特征预估，非送检实测"。

---

## 6. 英文层：六层结构

英文稿按 Layer 1 → 6 逐层过。基金申请书要额外走 Layer 6，且 Layer 6 **覆盖**前面各层的"删野心"倾向。

### 6.1 Layer 1：通用 AI-tell 目录

扫描并修掉下列通用模式（学术例外见 Layer 3）：

拔高意义（"marking a pivotal moment"）；伪装深度的 -ing 尾巴（"..., highlighting..."）；宣传与比喻语言（"rich"、"vibrant"、"groundbreaking"）；无引文的模糊归因（"experts argue"）；AI 高频词（*delve, underscore, intricate, tapestry, testament, landscape*（抽象义）*, pivotal, showcase, foster, leverage*（填充义）*, realm, seamless*）；回避系词（"serves as" → "is"）；否定式排比（"not just X, but Y"）；凑三件套（rule-of-three padding）；刻意换词（同一个东西轮换同义词）；填充（"it is worth noting that"、"in order to"）；**堆砌从句的超长句（拆开，见 2.11）**；**em dash（全部删除，改用逗号、冒号、括号或断句）**。

> **Before:** *Additionally, an enduring testament to the method's value is its ability to delve into intricate dependencies, showcasing a seamless integration that underscores its pivotal role.*
> **After:** *The method also captures higher-order dependencies, which the baselines miss (Table 2).*

### 6.2 Layer 2：学术专项 tells（2.1–2.11）

**2.1 过强动词。** 实证工作 *shows* 和 *provides evidence*，不 *prove*、不 *demonstrate universal truths*。
**Watch:** demonstrate, prove, establish, confirm, guarantee；以及没有检验/数字的 "significantly"。
> **Before:** *We prove that our method significantly outperforms all prior approaches.*
> **After:** *Our method improves held-out accuracy by 4--7 points over the strongest prior approach (Table 3); the gain is significant at p < 0.01 by a paired test.*

**2.2 意义炒作。** **Watch:** paves the way for, a crucial/pivotal step toward, has the potential to revolutionize, opens new avenues, sheds light on, of paramount importance, bridges the gap.
> **Before:** *This work paves the way for a new paradigm and sheds light on a problem of paramount importance.*
> **After:** *This work addresses one failure mode of prior methods: error accumulation under long-horizon rollout (Section 4).*

**2.3 空强化词。** **Watch:** extensive/comprehensive/thorough experiments, a wide range of, numerous, various.
> **Before:** *We conduct extensive experiments on a wide range of datasets.*
> **After:** *We evaluate on three datasets (ImageNet, CIFAR-100, and iNaturalist).*

**2.4 新意堆料。** **Watch:** 一节里 "novel" 出现超过一次；"to the best of our knowledge"；"for the first time"。
> **Before:** *We propose a novel framework and, to the best of our knowledge, are the first to study this.*
> **After:** *We study online calibration under delayed labels, which prior calibration work (offline) does not address.*

**2.5 程式化开场。** **Watch:** "In recent years, X has attracted increasing attention"；"With the rapid development of..."；"Despite recent advances,..."。
> **Before:** *In recent years, tabular deep learning has attracted increasing attention.*
> **After:** *Tabular deep learning has a structural limitation: most models discard feature-type metadata and must relearn it from data.*

**2.6 连接词滥用。** 不要连续用 Moreover/Furthermore/Additionally/In particular 开头，让逻辑自己承载。
> **Before:** *Moreover, the method is fast. Furthermore, it is simple. Additionally, it scales.*
> **After:** *The method is fast and simple, and it scales to one million rows (Section 5).*

**2.7 贡献清单套话。** 每条贡献要**具体到结果**，不是把摘要再抄一遍。
> **Before:** *Our contributions are: (1) a novel method; (2) extensive experiments; (3) strong results.*
> **After:** *We (1) introduce a metadata-aware encoder that reaches 0.91 AUROC vs 0.86 for the strongest baseline; (2) show it stays within 2 points under 20% label noise where the baseline drops 9; (3) release the benchmark.*

**2.8 文献堆砌。** 引一两篇真正相关的并说清为什么，不要方括号列表。
> **Before:** *Many methods exist [3, 7, 9, 12, 15].*
> **After:** *The closest prior method is TabNet [7], which encodes all features jointly; we instead condition on feature-type metadata.*

**2.9 含糊对冲。** **Watch:** somewhat, relatively, fairly, to some extent, quite。量化或删。
> **Before:** *Performance is somewhat better and relatively robust.*
> **After:** *Accuracy is 3 points higher and varies by less than 1 point across five seeds.*

**2.10 样板强调。** **Watch:** "It is worth noting that"、"It should be emphasized that"、"Notably,"、"Importantly,"。真重要的话，句子本身会显示出来。
> **Before:** *It is worth noting that, importantly, the gain holds across scenarios.*
> **After:** *The gain holds across all three scenarios (Table 4).*

**2.11 超长堆砌句。** AI 偏爱用逗号和 which/that/while/with 串三四个从句的长句。拆开：一句一个意思，删掉不承重的从句。**Watch:** 超过约 30 词，或含 3 个以上从句的句子。
> **Before:** *Existing methods, though promising, are largely empirical, with unclear principles underpinning their behavior, which limits their reliability and further progress.*
> **After:** *Existing methods stay empirical. Their principles are unclear, which limits reliability and progress.*

### 6.3 Layer 3：这些必须保留（别过度修正）

通用 humanizer 会把合法的学术构造一起铲平。以下保留：

- **有证据支撑的对冲是正确且必需的。** 主张真的不确定时，"suggests"、"is consistent with"、"we hypothesize that"、"may indicate"、"appears to" 要留。
  *错误的改法*：把 *"the results suggest X"* 改成 *"the results prove X"*——那是在制造过度主张。动词按证据强度原位保留。
- **被动语态**在施动者无关时完全合适：*"Samples were normalized to total protein."*
- **第一人称复数 "we"** 是标准写法，不要绕着说。
- **分号、偶尔的三项并列**适度使用没问题。破折号是例外：英文稿**全部删除**，改用逗号、冒号、括号或断句。
- **形式化定义、命名方法/指标、术语、公式、符号**原样保留。
- **绝不新增、删除或改动任何数字、公式、引文。** 内容不变，所有 cite key 保留。

### 6.4 Layer 4：claim↔evidence 纪律

对每条实证主张问两件事：(a) 文中有数字、图、表或文献支撑吗？(b) 动词强度配得上这份证据吗？

- **无证据 → 补证据指针或降级。**
  *Before:* *Our method is more robust.*
  *After:* *Our method's accuracy drops by 2 points under distribution shift, versus 11 points for the baseline (Figure 3).*
- **动词过强 → 降级。**
  *Before:* *This demonstrates that our method is universally superior.*
  *After:* *On these three datasets, our method matches or exceeds the strongest baseline (Table 2).*
- **模糊量级 → 数字或区间，并注明出处。**
  *Before:* *a large improvement.*
  *After:* *a 2--6% improvement in balanced accuracy over the strongest baseline.*

优先用区间（如 "2--6%"）而非单一平均值，除非说明了平均方式；每个数字都要归到具体方法、指标与基线。做对比时，**先跟最强的对手比**，不要只跟最弱的基线比。

### 6.5 Layer 5：声音与期刊/会议匹配

作者给了旧稿就先读一段，记下：句长节奏、连接词习惯、对冲的力度与位置、各节怎么开头、记号习惯、反复出现的措辞，然后照着写。同时匹配目标场合的 register（ICLR/NeurIPS：简练、直接、结果前置；Nature/PNAS：更铺陈、更解释性）。

没有样本时，默认写**干净、精确、符合场合**的散文，不要退回通用 humanizer 那种随意、带主观意见的口吻。

### 6.6 Layer 6：基金申请书模式（NSF / NIH / 国自然）

申请书不是论文。它卖的是**愿景加可行性**，评审是打分的。register 的差别很关键：论文层要删的野心词（"long-term goal"、"pioneer"、"transformative"、"establish a foundation"），在这里**恰当且被期待**——前提是背后有可信的计划和证据。所以申请书模式**不要把愿景铲平**，换成另一套纪律：**claim ↔ feasibility**。

**6.6.1 结构：分数在前几页就已经定了。** 评审从开头形成印象，后面只是扫读确认。把力气花在前几页。
- **NSF**：一页 **Project Summary**，三个评审口径写清楚且各自自足：**Overview**、**Intellectual Merit**、**Broader Impacts**。Project Description 的开头顺序是 **长期愿景 → 本项目的目标 → 缺口 → 具体 thrust/aim → 回报**，最好在 1--2 页内讲完，配一张总览图。Broader Impacts 要有实质内容并融入研究，不能是事后补的。
- **NIH (R01)**：**Specific Aims 一页就是整个申请**，是读得最多、最决定成败的一页。标准弧线：(1) 问题、已知什么、**缺口/关键需求**；(2) **长期目标**与**中心假设**及其依据；(3) "**The objective of this application is...**" 加上假设是怎么形成的；(4) **2--3 个 Aim**，每个一句目标 + 一句方法 + 预期结果；(5) **回报段**：成功了会改变什么。之后是 **Significance、Innovation、Approach** 三个独立打分节。

**6.6.2 前三页优先（这几页改得最狠）。** 读到第 1 页末（NIH Aims）或第 2--3 页（NSF），读者必须已经拿到：**hook**（为什么重要，要具体）、**gap**（缺什么，缺的代价是什么）、**中心思路**（一句话说清你怎么做）、**aims/thrusts**（清晰且平行）、**payoff**。缺哪个就先补哪个，别去动后面的章节。第 3 页还没说服评审，第 10 页救不回来。

**6.6.3 申请书特有的坏招，这样修：**
- **重要性含糊。** *Watch:* "this is an important/timely problem"、"X has many applications"。
  *Before:* *Understanding this problem is critically important.*
  *After:* *Without bounds on how measurement noise propagates to diagnosis, clinical models are tuned by trial and error, the inefficiency this proposal removes.*
- **方法当 aim**（aim 里写的是技术，而不是问题或结果）。
  *Before:* *Aim 2: Apply transfer learning to the dataset.*
  *After:* *Aim 2: Determine whether fusing wearable and lab signals improves early detection, and for which patient subgroups it helps or hurts.*
- **多米诺 aim**（Aim 1 失败则 2/3 崩，评审会判为脆弱）。*修法*：把 aims 写成**平行、各自独立有价值**；确有依赖的，把 fallback 写出来。
- **有野心无可行性。** 每个大胆主张都要有落脚点：预实验数据、已有结果/发表、所依托的经典定理、合作者或支持信。*修法*：把证据放在主张旁边（"our preliminary result in Fig. X shows..."、"building on a classical minimax lower bound..."）。
- **样板化 Broader Impacts / 培养计划。** *Watch:* "we will mentor students and disseminate via talks and papers." *修法*：写具体、可枚举、与研究挂钩：具体项目、具名课程或工具、可度量的 outreach。
- **对冲过的中心假设。** Aims 页的假设是可证伪的承诺，不是 "we will explore whether possibly..."。校准过的对冲属于 Approach 的解释部分，不属于中心主张。

**6.6.4 这些要保留并强化（获批申请书的craft）：**
- **愿景/野心框架**：开头一个大胆的长期目标，本项目是通往它的一步。
- **可扫读的 run-in 小标题**：**Goal:**、**Motivation:**、**Innovation:**、*Thrust/Aim N (one-line mission):*。评审在扫读，可见的结构能换到时间。
- **一个贯穿全文的具体例子或"主角"**：让抽象方法变具体，并在各个 aim 里保持一致。
- **把挑战/aim 写成问句**：一个清晰的开放问题读起来就是一个well-posed 的问题（每个 aim 配一个加框的问句很有效）。
- **把新工作锚在深的、具名的经典结果上**：一个已知不等式、容量概念或检验，说明新方法推广了它——这是 rigor 与学脉的信号。
- **把团队的既有战绩前置，作为可行性证据**：已获资助、预实验、发表、合作者、示范伙伴，放在**前面**，作为"这个计划能执行"的证明。真实的履历是证据，不是吹牛；放在能为 aims 降风险的位置。*（只能用 PI 本人真实、已提供的记录，绝不编造资助、结果、伙伴或支持信。）*

**6.6.5 claim ↔ feasibility**（Layer 4 在申请书的对应物）。对每个 aim 和承诺的结果检查：这份野心配得上可信的手段吗——预实验、已有方法、经典基础、合作者、分阶段降风险？配得上，保留大胆动词；配不上，补上缺失的证据，或把主张缩到计划撑得住的范围内。**绝不编造预实验、既有资助、伙伴或支持信**；支持不存在时，向作者指出这个缺口，而不是糊过去。

---

## 7. 中英对照：同一批 AI 痕迹的两种面孔

双语稿最容易翻车的地方是：中文按中文词表清理，英文按英文词表清理，结果两边都留着同一种毛病。下表按"病灶"对齐，一行就是一对。

| 病灶 | 英文形态 | 中文形态 | 两侧统一的改法 |
|---|---|---|---|
| 程式化开场 | In recent years, X has attracted increasing attention | 近年来，X 受到广泛关注 / 随着…的快速发展 | 直接给结构性缺陷或一个具体数字 |
| 拔高作用 | plays a crucial / pivotal role | 发挥着至关重要的作用 / 至关重要 | 说清具体机制，或删 |
| 铺路式结尾 | paves the way for | 为…奠定基础 / 为…提供参照 | 说明解决了哪个具体失败模式 |
| 伪深度动词 | delve into / underscore / showcase | 深入探讨 / 深入剖析 / 凸显 / 彰显 | 换成分析、检验、比较、显示；或直接给事实 |
| 抽象名词 | intricate, tapestry, landscape, realm | 格局 / 图景 / 图景式表述 / 错综复杂 | 具体说复杂在哪（几个变量、什么耦合） |
| 填充义动词 | leverage, foster, facilitate | 赋能 / 助力 / 依托 / 抓手 | 说清"用 X 做 Y" |
| 样板强调 | It is worth noting that / Notably, | 值得注意的是 / 需要指出的是 | 删 |
| 连接词连击 | Moreover / Furthermore / Additionally 连续开头 | 此外 / 与此同时 / 首先…其次…再次 | 删，让逻辑承载 |
| 空强化 | extensive / comprehensive experiments | 大量实验 / 充分的实验 / 全面系统 | 给数据集名、数量、指标 |
| 新意堆料 | to the best of our knowledge / for the first time | 据我们所知 / 首次 / 填补空白 | 说明与最近工作的具体差别 |
| 无检验的显著 | significantly outperforms | 显著优于 / 明显提升（无检验） | 给数值差 + 检验方法 + p 值 |
| 动词过强 | prove / demonstrate | 证明 / 充分表明 / 验证了 | 降级：shows / 表明 / 结果显示 |
| 模糊量级 | a large improvement | 大幅提升 / 显著改善 | 给区间 + 对比基线 |
| 含糊对冲 | somewhat / relatively / fairly | 一定程度上 / 相对较为 | 量化或删（有证据的对冲保留，见 §8） |
| 否定排比 | not just X, but Y | 不仅是…更是… / 不只是…而是… | 拆成两句 |
| 三件套 | rule of three | 一是…二是…三是 / 三维 / 四位一体 | 改两项，或打破各类对称 |
| 破折号 | em dash | 破折号 —— | 英文删净；中文每千字 ≤2，且不做「肤浅分析拖尾」 |
| 内联加粗标题 | **Bold label:** body | **加粗标题：** 正文 | 改连续行文 |

---

## 8. 保留清单（中英共用，别过度修正）

- **有证据的对冲**：suggests / may indicate / appears to ↔ 可能 / 似乎 / 倾向于。主张真不确定就留着，删掉等于造假。
- **施动者无关的被动**：Samples were normalized ↔ 样本按总蛋白归一。合法。
- **第一人称复数 / "本文"**：we ↔ 本文/本研究。标准写法，不用绕。
- **形式化定义、命名方法、指标名、符号、公式**：原样保留，一个下标都不改。
- **分号、偶尔的三项并列**：适度可用。
- **具体数字、百分比、年份、地名、政策文件名、图表编号、引文编号**：全部保留，一个不删。
- **参考文献格式**：按目标期刊/学校规范排版，**不为降 AI 率改写或删减**（见 §5.2 假阳性）。
- **学科内的固定表达**：如 "P<0.01"、"β=0.42"、"95% CI"、"per-protocol"、"intention-to-treat"，这些不是 AI 腔。

---

## 9. 交付前检查清单

**中文稿**
- ✓ 摘要还在走"综合运用—系统识别—检验—构建—提出—可为…提供参照"？拆成背景/做法/结论/含义四拍。
- ✓ 文献综述是"XX 等人提出了…"链式罗列？每 2–3 篇插一句自己的判断。
- ✓ 有"四维/五位/三大机制"且每类句式对称？打破对称，允许长短不一。
- ✓ 统计结论只写"显著正相关/完全中介/关键纽带"？补一句"这意味着什么"，落到谁做了什么。
- ✓ 分组结果写成"优/高/中/低四并列 + %""①②③ 枚举""三对称"？改叙述化，**数字全留**（规则 W）。
- ✓ 实证/政策段末尾有"印证了…要求/为…提供了现实支撑/提供参照"？删。
- ✓ 政策建议连续"并…并…并"？拆成有主语的短句。
- ✓ 结尾是"构成…格局/形成…循环"？换成具体效果句。
- ✓ 段落长度是否都在 150–200 字、句子 20–30 字？拆短、错落。
- ✓ 连续两句以连接词开头？删。
- ✓ 全篇被动、读不出谁在做？补主语（但施动者无关的被动保留）。
- ✓ 带数据的段落被激进档标红？先看数据是不是被空泛/对称句包住了——**拆包裹，不删数据**。
- ✓ 参考文献被标红？按规范排版，不动。
- ✓ 目标检测器是哪款？报告标注片段改了没有？未标注的段落动过没有（不该动）。
- ✓ 结论里有"仍面临三重挑战 + 未来可从以下方向展开"的提纲腔？只留真正遇到的那一个限制。
- ✓ 有"有研究认为 / 大量研究表明"却没引文？补文献或删（无引文归因是学术不端风险）。
- ✓ 同一概念在相邻句里换了三种叫法（耕地/土地/农地）？术语统一。
- ✓ 正文有加粗小标题、项目符号清单、✅★ 符号？改连续行文。
- ✓ 句尾挂着一串"彰显了…/体现了…/确保了…"？删尾巴。

**英文稿**
- ✓ 还有 em dash 吗？一个都不能剩。
- ✓ 还有 "novel" 一节出现两次以上、"to the best of our knowledge"、"for the first time"？删或改写。
- ✓ 动词比证据强？prove/demonstrate/establish 逐个降级。
- ✓ 有超过 30 词的堆砌句？拆。
- ✓ 连续 Moreover/Furthermore/Additionally 开头？删。
- ✓ 每句"It is worth noting that"？删。
- ✓ 每条贡献都具体到一个结果？不是"a novel method"这种空壳。
- ✓ 每个数字都有归因（哪个方法、哪个指标、哪个基线）？比较是不是先跟最强基线？
- ✓ 有证据的对冲被误删了吗？"suggests" 是不是被改成了 "proves"？
- ✓ 数字、公式、引文一条没动？
- ✓ 申请书：前三页有 hook/gap/中心思路/平行 aims/payoff 吗？aim 里写的是问题还是技术？每个野心都配了可行性证据吗？

---

## 10. 输出格式

交付三件东西：

1. **改后全文**（保持原结构、数据、结论；docx 优先用本地 Office 编辑通道就地改写以保留字体版式；纯文本直接给修订版）。
2. **变更摘要**：逐条列 位置 / 原句摘录 / 改后 / 命中规则 / 是否对应检测报告标注。作者要能照着复核。
3. **质量评分表**：
   - 中文稿：三平台（知网/万方/维普）修订前 → 修订后 预估对照，附"质量分：AI 率 X%（检测器：XX；送检版本：XX）"；有实测报告以实测为准，并注明三平台数值为特征预估。
   - 英文稿：不用 AI 率，用条目化的 audit 前后统计（命中模式数按类计、过度主张降级数、未能补证据而标出的主张数）。

**评级对照（中文，按 AI 率 %，越低越好）**

| AI 率 | 评级 | 含义与处置 |
|---|---|---|
| ≤20% | 达标/优秀 | 硕士线达标；激进档下此值已极低，可交付 |
| 20–40% | 良好（偏高） | 仍有明显痕迹，按报告标注片段再改 Q–T / J–O |
| 40–60% | 不合格 | 空泛对称包裹密集，须系统性重写（见 `examples/before-after-zh.md`） |
| >60% | 严重不合格 | 近乎未处理，整体重做后复测 |

三平台对照表（强制附于交付）：

| 平台 | 敏感维度 | 修订前预计 | 修订后预计 | 变化 | 评级 |
|---|---|---|---|---|---|
| 知网（中严档） | 语义规整 / 太规范章节 | XX% | XX% | ↓ / → | |
| 万方（中等档·模板凶） | 模板化 / 定义类 / 八股 | XX% | XX% | ↓ / → | |
| 维普（中等档·句式凶） | 均匀长句 / 连接词 / 段落对称 | XX% | XX% | ↓ / → | |

---

## 11. 参考与许可

本技能是整合产物，三条来源都是 MIT：

- **[AIScientists-Dev/academic-humanizer](https://github.com/AIScientists-Dev/academic-humanizer)**（MIT, Copyright © 2026 AIScientists-Dev, v0.3.3）——提供 §6 六层结构（Layer 1–6）、claim↔evidence 纪律、NSF/NIH 申请书模式，以及 `examples/before-after-en.md` 中的英文范例。其 Layer 1 又沿用 **[blader/humanizer](https://github.com/blader/humanizer)**（MIT）的通用 AI-tell 目录。
- **[humanizer-zh](https://github.com/blader/humanizer) 中文扩充版 / humanizer-zh-pro v1.4.10**（MIT, 著作权人 尹洁平 · 杰平）——提供 §4 中文 A–Z 规则、§5 四档检测器校准档案、三平台 AI 率预估的强制流程与评分惯例。

整合改动（相对两个上游）：
1. 中文规则从字母表重排为**按论文段落功能**组织（摘要/综述/理论/方法/结果/结论/通用），规则号保留可追溯。
2. 新增 §2 冲突消解表（个性化、破折号、补数据三处立场冲突的裁决）与 §7 中英对照表。
3. 给原中文规则 P（补数据降 AI 率）加了**禁止编造**的硬约束，与学术保真契约对齐。
4. 检测器校准的完整证据移入 `references/`，主文件只留决策表，避免每次调用都吞掉几十 KB。

许可要求：任何复制、修改、再分发必须保留上述版权声明与各自的 MIT 许可文本，不得删除作者署名。本技能输出不构成官方结论或审批意见；涉及政策法规、技术标准、投稿要求的具体内容，以现行有效文本和主管部门/期刊官方发布为准。

**相关技能**：公文与月报去 AI 腔 → `humanizer-zh-pro`；中文创意写作 → `human-writing`；英文非学术文本 → `humanizer`；开题报告撰写与返修 → `thesis-proposal-cn`；知网检索与文献下载 → `cnki-search` / `cnki-paper-downloader`。
