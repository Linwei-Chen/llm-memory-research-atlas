# 大模型记忆技术检索与证据审计

> 截止日期：2026-08-11；当前覆盖等级：L2。完整逐查询记录见 `planning/search_ledger.jsonl`，逐候选纳排见 `planning/screening.csv`，核心主张见 `planning/claim_ledger.csv`。

## 1. 范围与操作定义

- 研究问题：大模型记忆技术如何写入、保存、寻址、读取、更新、压缩或删除跨时间状态；不同机制的直接证据、失败边界和部署约束是什么？
- 对象：大语言模型、对话系统、检索增强系统、自主智能体，以及直接评测这些对象记忆能力的基准。
- 场景：单轮提示、长文档和长任务、跨会话对话、开放世界知识更新、持续部署与模型生命周期。
- 时间：奠基工作至 2026-08-11。
- 语言：英文一手文献为主；中文用于检索辅助和交付叙事；题名保留原文。
- 成果类型：正式同行评议论文优先；高相关预印本、官方报告、数据/基准、作者项目和代码单列。
- “全面”的操作定义：七类查询家族均执行；六条机制路线均有代表、前沿和评价/反证或明确缺口；完成引用链补漏、两轮异源边际收敛、核心深读和主张审计。
- 分类说明：前五条路线按存储或管理机制划分，“评测、安全与治理”按主要贡献类型划分；评测条目的持续时间层级指主要被测记忆的持续时间。负面证据通过辅助路线连接到被评测的正面机制，但主要路线仍只计一次。
- 不可覆盖长尾：未公开商业实现、无一手入口成果、截止日后版本、只有普通 RAG/缓存/微调应用而无记忆贡献的工作。

## 2. 来源层级与核验原则

### T1：结论证据

正式会议、期刊与 DOI 入口，包括 ACL Anthology、PMLR、NeurIPS Proceedings、OpenReview 明确正式状态页面、ACM Digital Library、AAAI Proceedings、USENIX、期刊官方页和 arXiv 原始记录。核心机制、比较和数字只由 T1 直接支持。

### T2：实现与材料边界

作者或机构官方项目页、代码仓库、数据集页面与技术材料。T2 用于核验代码、数据、版本变化和复现实用信息，不单独把营销性表述升级为论文结论。

### T3：线索发现

搜索结果、综述、聚合页与博客只用于发现术语、候选或引用链。所有最终条目必须回到 T1/T2；无法回到一手入口的候选排除。

OpenReview 特别按页面状态处理：`Published as a conference paper`、正式 poster/oral 等状态可记为同行评议；`Submitted`、`Under review`、`Withdrawn` 和 workshop 不得误标为主会正式论文。预印本与正式版属于同一版本族时，以元数据最完整的正式版作为主入口。

## 3. 查询矩阵

完整精确查询、日期、来源、筛选数和纳入数逐行保存于 JSONL。以下列出七类查询家族及代表式；命中总量无法由站点可靠返回时使用 `null`，不伪造为 0。

| 查询家族 | 目标 | 代表精确查询式 |
|---|---|---|
| 核心对象 | 操作定义和术语 | `site:arxiv.org LLM memory survey taxonomy large language model memory 2024 2025` |
| 历史与奠基 | 参数、循环、外部存储起点 | `site:aclanthology.org "Language Models as Knowledge Bases?"`；`site:aclanthology.org "Transformer-XL: Attentive Language Models Beyond a Fixed-Length Context"` |
| 方法路线 | 编辑、压缩、检索、智能体与个性化 | `site:openreview.net "Mass-Editing Memory in a Transformer"`；`site:proceedings.mlr.press "Memory-Based Model Editing at Scale"` |
| 评测与应用 | 长上下文、长对话、工具行动和用户画像 | `site:aclanthology.org large language model memory benchmark long-term dialogue agent memory` |
| 综述与谱系 | 同义词、分类和引用链 | `site:arxiv.org LLM memory survey taxonomy large language model memory 2024 2025` |
| 风险与反证 | 编辑副作用、输入长度、污染、隐私、遗忘 | `site:aclanthology.org knowledge editing side effects locality generalization critique language models`；`site:usenix.org large language model memory poisoning retrieval augmented generation training data extraction` |
| 前沿与更新 | 2025–2026 正式版本与新基准 | `site:aclanthology.org/2026 "memory" "LLM agents" ACL 2026 long-term`；`site:proceedings.neurips.cc/paper_files/paper/2025 LLM memory agent long-term` |

同义词分组包括：`LLM memory`、`long-term memory`、`agent memory`、`episodic/semantic/procedural memory`、`parametric memory`、`knowledge editing`、`model unlearning`、`long context`、`recurrent/compressive memory`、`non-parametric memory`、`retrieval-augmented generation`、`personalized conversational memory`。邻近但不可偷换的概念单独记录：通用缓存、单纯位置编码扩展、普通 PEFT、测试时适应、通用持续学习、与记忆无关的检索应用。

## 4. 检索阶段与增量轮

### 发现轮

用术语综述、官方会刊搜索和奠基论文引用发现参数、上下文、外部、智能体、个性化和治理六条候选路线。发现轮不直接支撑结论；其作用是建立作者、基准、同义词和历史转折候选池。

### 核验轮

逐条打开正式页面，核验题名、作者、年份、venue、DOI/稳定 ID、出版状态、摘要与可用项目材料。核心条目至少达到摘要核验；涉及定量、比较、安全、首创或失败机制的主张继续读取正文相关章节、表格或限制部分。

### 反证轮

主动检索 `failure`、`limitation`、`side effect`、`negative result`、`benchmark`、`privacy`、`poisoning`、`forgetting`、`distribution shift` 等。已确认的关键反证包括：

- 参数知识访问受提示与数据集偏差影响：[Knowledgeable or Educated Guess?](https://aclanthology.org/2021.acl-long.146/)。
- 知识编辑可能损害通用能力或只形成表面编辑：[Model Editing Harms General Abilities](https://aclanthology.org/2024.emnlp-main.934/)、[Revealing the Deceptiveness of Knowledge Editing](https://aclanthology.org/2025.acl-long.868/)。
- 输入长度本身可在完美检索条件下造成退化：[Context Length Alone Hurts LLM Performance Despite Perfect Retrieval](https://aclanthology.org/2025.findings-emnlp.1264/)。
- 智能体会把错误经验传给后续任务：[How Memory Management Impacts LLM Agents](https://aclanthology.org/2026.acl-long.27/)。
- 外部库和长期记忆形成污染面：[AgentPoison](https://proceedings.neurips.cc/paper_files/paper/2024/hash/eb113910e9c3f6242541c1652e30dfd6-Abstract-Conference.html)、[PoisonedRAG](https://www.usenix.org/conference/usenixsecurity25/presentation/zou-poisonedrag)。
- 反记忆算法需同时检查效用、隐私和连续请求：[MUSE](https://openreview.net/forum?id=TArmA033BU)。
- PII 重构若未控制表面线索，可能把模式补全误判为记忆化：[Cue-Resistant Memorization](https://aclanthology.org/2026.acl-long.1560/)。

### 补漏轮

按每条路线检查“奠基/代表、前沿、评价/反证、实际系统”四类位置；沿核心论文参考文献做后向追踪，沿正式会刊和 2025–2026 引用线索做前向追踪。应用过窄、对象迁移过远或只在 workshop/撤稿状态出现的候选不会为凑数量纳入。

### 增量轮次与撤销记录

所有分母都是该轮在 DOI、稳定 ID、正式 URL、规范化题名和作品族去重后的唯一候选数；不同轮次可能检查同一作品，所以不能把各轮分母相加当作唯一文献总量。纳入率使用最终全文审计决定，不使用初筛建议数。

| 轮次 | 查询家族 | 唯一筛选 | 最终纳入 | 边际率 | 判定 |
|---|---|---:|---:|---:|---|
| 已撤销 ACL 轮 | 宽泛 ACL 站点查询 | 16 | 0 | 0% | 独立审计发现正式目录漏项，整轮不计入收敛证据 |
| 早期正式目录轮 | NeurIPS／PMLR／OpenReview／ICLR | 17 | 0 | 0% | 只支持该查询局部无新增，不能抵消后续漏检 |
| ACL 2026 官方补漏 | 逐项检查正式长文目录 | 18 | 3 | 16.67% | 纳入图/非图比较、记忆依赖控制与摘要式轨迹压缩 |
| 收敛轮 3 | PMLR／NeurIPS／ACM／AAAI 异源目录 | 26 | 8 | 30.77% | 独立全文复核后净增 7；不收敛 |
| 收敛轮 4 | 新增代表工作的前后向引用链 | 27 | 10 | 37.04% | 原 16 项初筛压缩为 6 core／4 bridge；不收敛 |
| 收敛轮 5 | 11 类正式程序与论文集刷新 | 58 | 1 | 1.72% | 首轮低于 5%，无新一级路线 |
| 收敛轮 6 | 新增工作的前后向引用与失败链 | 35 | 6 | 17.14% | 仍有历史前身、删除、产品审计与部署增量；不收敛 |
| 收敛轮 7 | 10 份近期综述参考文献交叉 | 40 | 0 | 0% | 无新一级或二级分支；轮 6 后第一轮低边际证据 |
| 收敛轮 8 | OpenAlex／Crossref 结构化索引等额抽样 | 72 | 5 | 6.94% | 新增 5 个二级分支；不收敛 |
| 收敛轮 9 | ACM／USENIX／PoPETs／IEEE／NDSS 治理正式会刊 | 38 | 6 | 15.79% | 新增 5 个治理二级分支；不收敛 |
| 收敛轮 10 | 轮 8–9 新增工作的正式前后向引用链 | 42 | 1 | 2.38% | 无新一级路线；轮 9 后第一轮低边际证据 |
| 收敛轮 11 | 2025—2026 前沿版本刷新 | 72 | 4 | 5.56% | 新增 4 个二级分支；不收敛，低边际序列重置 |
| 收敛轮 12 | 生命周期基准、数据集与失败分析 | 66 | 13 | 19.70% | 新增 13 个二级分支；不收敛 |
| 收敛轮 13 | 系统实现、效率与运行治理 | 71 | 8 | 11.27% | 后置剔除 1 个重复；新增 8 个二级分支，不收敛 |
| 收敛轮 14 | 最新新增工作的引用与依赖链 | 72 | 0 | 0% | 无新一级或二级分支；轮 13 后第一轮低边际 |
| 收敛轮 15 | 新综述、教程与领域回顾参考文献交叉 | 72 | 2 | 2.78% | 无新一级路线；与轮 14 共同满足连续两轮低边际条件 |

这组结果表明，六条一级路线从 ACL 补漏后没有增加，顶层本体已经稳定；代表工作覆盖曾在轮 3、4、6、8、9、11、12、13 出现高边际增量，因此“路线稳定”和“文献饱和”必须分开。轮 13 是最后一次高边际扩展，其后轮 14 的引用依赖链为 0/72，轮 15 的新综述交叉为 2/72；二者来源与查询方式不同、均低于 5% 且没有新一级路线，满足本研究预先冻结的边际收敛合同。

## 5. 纳入、排除与边界案例

### 纳入标准

1. 直接定义一条记忆机制、关键操作或评测范式。
2. 有可核验的正式页、DOI、arXiv 或作者官方入口。
3. 对全貌、路线比较、负面边界、真实应用或治理贡献不可替代信息。
4. 与大模型/大模型智能体及截止日期一致。

### 排除标准与代码

- `范围外`：对象不是 LLM/LLM 智能体，且迁移距离过大。
- `词面重合`：memory 指运行内存占用、缓存效率或一般心理学概念，却不处理跨时间状态。
- `重复记录`：同一成果的预印本、会议版或重复 URL。
- `无法核验一手来源`：只有博客、聚合页或搜索摘要。
- `证据不足`：只有愿景或营销声明，没有机制与评测。
- `非目标成果类型`：普通应用或工具介绍，没有记忆研究贡献。
- `被更新版本取代`：已有正式版本，旧预印本不再作为主记录。

### 边界案例

- 长上下文只有在直接研究状态保持、利用或失效时纳入；单纯位置编码和注意力提速排除。
- RAG 奠基和检索/存储机制创新纳入；仅把现成 RAG 用于某行业问答的应用排除。
- 知识编辑与反记忆分别作为参数记忆的写入和删除操作纳入；普通 PEFT 不自动纳入。
- 训练数据记忆化主要作为参数记忆的隐私与测量边界纳入，不把所有版权/抽取研究扩张为本地图核心。
- 智能体工作只有在经验写入、组织、检索、反思或删除构成主要贡献时纳入；只有工具调用或规划者排除。

## 6. 去重与版本族

规范键按 DOI → arXiv/OpenReview/ACL ID → 正式论文 URL → 规范化题名与第一作者建立。预印本、正式会议、期刊扩展和项目页放入同一 `work_family_id`；默认保留正式且元数据完整的版本。若版本改变结论、实验或出版状态，则在 `version_note` 和 `source_note` 明示，不静默合并。

重点版本风险包括：MemGPT 等仍以预印本/系统项目为主；OpenReview 上撤稿或 workshop 与正式主会记录并存；LongMemEval 等项目页、arXiv 和 ICLR 正式版并存；同名基准后续清洗数据不能倒推原始论文结果。所有最终状态以截止日前可核验的一手入口为准。

## 7. 证据与主张审计

出版状态、阅读优先级和证据等级独立记录：`peer-reviewed/preprint/official-report`；`core/bridge/background`；`A/B/C`。核心条目保留 V/D/P/Q 四轴：核验深度、支持直接性、出版状态、研究质量。

数字、比较、因果、安全、首创和“最优”主张只有在直接相关摘要、正文、表格或图注可定位时进入叙事。作者报告与本文推断分开：`evidence` 记录作者直接实验或正式页面事实；`limitations` 记录适用边界；综合报告的跨论文判断使用“本地图归纳”措辞。

截至当前冻结点，地图含 134 项，其中 98 个核心条目全部为 `full-text-checked` 且 V 为 V3/V4；`claim_ledger.csv` 含 116 条已核验主张。五个早期核心条目没有单列高风险数字或比较，其不登记理由已在 `planning/claim_ledger_gap_audit.md` 逐条说明；此后每个新增核心必须至少有一条主张审计，否则跨文件门禁失败。

## 8. 覆盖饱和与五道闸门

候选、结构、证据、最终编译与真实浏览器门禁均已通过：

1. **范围与来源闸门：通过。** 范围合同、七类查询家族、T1/T2/T3 来源层级和排除边界已经冻结；检索词、日期、来源与失效入口保留在 JSONL 和轮次审计中。
2. **分支代表性闸门：通过。** 六条路线均有奠基、代表、前沿和评价/反证；压缩域流式记忆、个人行为—认知双层状态、语义事务恢复、工业 RAG 生命周期、序列遗忘与可验证遗忘均已有不可替代代表或明确缺口。
3. **边际收敛闸门：通过。** 最后一次高边际轮 13 之后，轮 14 为 0/72=0%，轮 15 为 2/72=2.78%；两轮查询家族异质、分母冻结、均低于 5%且无新一级路线。
4. **深读闸门：通过。** 98 个核心条目全部为正文核验，机制、证据位置、限制和 V/D/P/Q 完整。
5. **证据审计闸门：通过。** 116 条高风险主张均为 `VERIFIED`，论文 ID 与证据向量和地图一致。

最终候选冻结后，134 条记录已重新编译，严格数据验证、离线依赖约束和跨账本审计均为零错误。早期运行环境无法提供有效的 `file://` 浏览器证据，因此没有用静态检查冒充验收；2026-08-15 在具备本机 Chrome 的受控环境从最终位置复跑，134 条详情与 13 个非空聚合组在桌面、键盘、375 像素手机和 `file://` 下全部通过。按质量合同，本审计升级为 L2。

## 9. 失败入口与覆盖限制

- 部分 OpenReview 页面触发浏览器验证，需以可访问 PDF、正式状态摘要、会刊页或作者项目交叉核验。
- 搜索引擎不能稳定返回数据库总命中量，因此全部 `hits` 和一部分历史逐查询 `screened` 为 `null`；完整轮次的唯一筛选数、纳入数和集合另保存在各轮 JSON 与纳排账本，不能从 JSONL 各行简单求和。
- 部分 2026 前沿只有 workshop、投稿或撤稿状态；这些不会被误标为主会同行评议。
- 商业产品的内部写入策略、数据保留和删除实现通常不公开，本地图不推测其机制。
- 真实跨月用户研究和端到端可验证删除证据相对稀缺；报告将此作为领域空白而非用模拟数据填补。
- 机制图均由条目文字证据重绘为解释性示意，不嵌入来源/许可不明的论文原图，也不把示意图当作实验结果。
