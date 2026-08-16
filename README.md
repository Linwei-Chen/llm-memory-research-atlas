# 大模型（LLM）的记忆（Memory）技术研究地图

<!-- generated-complete-readme:v1 -->
> 按存储机制与持续时间梳理写入、读取、更新、遗忘和治理证据

**完整文字版综述：** 本 README 已直接收录领域全貌、综合报告、检索与证据审计，以及全部 134 项工作的逐篇解读。无需打开网页即可阅读全部研究内容；[在线地图](https://linwei-chen.github.io/llm-memory-research-atlas/)仅提供可选的可视化筛选与机制图。

[在线地图](https://linwei-chen.github.io/llm-memory-research-atlas/) · [结构化真源](https://github.com/Linwei-Chen/llm-memory-research-atlas/blob/main/atlas.json) · [原始综合报告](docs/REPORT.md) · [原始检索审计](docs/SEARCH_AUDIT.md)

## 阅读导航

1. [研究全貌](#研究全貌)
2. [路线与层级](#路线与层级)
3. [综合报告](#综合报告)
4. [全部研究工作](#全部研究工作)
5. [检索与证据审计](#检索与证据审计)
6. [复现与使用边界](#复现与使用边界)

<a id="研究全貌"></a>
## 研究全貌

**领域概览：** 大模型记忆不是单一组件，而是分布在权重、激活与上下文、外部索引、智能体管理层和用户状态层的一组可寻址状态。地图首先按主要技术贡献归入唯一机制路线，再按状态预期持续时间定位；评测、安全与治理作为主贡献路线单列，用于显式呈现失败、污染、隐私和删除证据。

**研究领域：** 大模型（LLM）的记忆（Memory）技术

**核心判断：** 决定记忆系统价值的不是容量本身，而是写入选择、检索寻址、冲突更新、受控遗忘和证据可审计性之间的联合设计。

**阅读建议：** 先读每条路线的核心条目，再沿持续时间比较同类机制；部署决策必须同时查看评测与治理路线。

**范围与纳排边界：** 覆盖从奠基工作到 2026-08-11 的大模型参数、上下文、外部、智能体与用户记忆，以及直接评测其能力与风险的工作；正式同行评议优先，高相关预印本单列。排除只有词面重合、无状态管理贡献的普通 RAG 应用、缓存或通用微调。‘全面’表示声明范围内的多源检索与证据饱和，不表示穷尽未公开系统和全部长尾论文。

**覆盖说明：** 范围、代表性、连续两轮边际收敛、98个核心全文深读与116条主张审计均已通过；134项数据及离线约束通过自动验证。当前运行环境禁止对最终file://页面执行真实浏览器回归，因此按质量合同保持L1，待桌面与375像素手机验收后再升级L2。

**目标读者：** 研究人员与具备基础背景的专业读者

**来源材料：** 正式论文页、DOI、ACL Anthology、PMLR、OpenReview 正式状态、NeurIPS/USENIX 正式页、arXiv 与作者官方项目/代码。

**地图中心标签：** LLM Memory

**内容语言：** zh-CN

| 维度 | 数值 |
|---|---|
| 纳入成果 | 134 项 |
| 年份范围 | 2019—2026 |
| 研究路线 | 6 条 |
| 分析层级 | 4 个 |
| 覆盖等级 | L2 |
| 资料截止 | 2026-08-11 |
| 第二轴 | 记忆持续时间 |

<a id="路线与层级"></a>
## 路线与层级

| 路线（ID） | 收录 | 判定问题 | 说明 |
|---|---:|---|---|
| 参数记忆与知识修改 / 参数记忆（`parametric`） | 18 | 主要贡献是否直接改变或定位模型参数中的可持久知识？ | 知识或经验进入模型权重、适配器或可路由参数模块，并通过编辑、持续学习或反学习改变长期行为。 |
| 上下文与隐状态记忆 / 上下文记忆（`contextual`） | 13 | 记忆是否主要存在于一次推理过程可见的激活、上下文或循环状态中？ | 用提示上下文、注意力/KV、循环隐状态、摘要或可学习压缩保存当前会话与长序列状态。 |
| 外部检索与非参数记忆 / 外部检索（`retrieval`） | 8 | 主要创新是否在模型外存储的建立、寻址或与生成器融合？ | 把事实、文本或历史状态放在模型外部，以稠密/稀疏检索、近邻或结构化索引在生成时读取。 |
| 智能体记忆管理 / 智能体记忆（`agentic`） | 25 | 主要贡献是否是由智能体策略管理记忆生命周期，而非一次静态检索？ | 智能体主动选择经历，执行写入、链接、反思、巩固、检索和删除，以支持长程任务与经验复用。 |
| 个性化与用户长期记忆 / 用户记忆（`personal`） | 17 | 记忆的主要对象是否是特定用户及其随时间变化的个人状态？ | 跨会话维护用户事实、偏好、关系与变化历史，强调一致性、更新、适度遗忘和用户控制。 |
| 评测、安全与治理 / 评测治理（`evaluation`） | 53 | 工作是否主要回答记忆能力如何测、何时失效、如何受攻击或如何受控？ | 主要贡献是定义测量范式、揭示失效、攻击或隐私风险，或验证更新、删除和遗忘是否真正发生。 |

| 层级 | 说明 |
|---|---|
| 即时或单轮 / 单轮（`0`） | 状态只要求在当前提示、当前生成或极短步骤内可用，结束后不承诺保留。 |
| 会话或任务期 / 会话内（`1`） | 状态跨越一段对话、文档或任务轨迹，但不以跨会话持久化为主要目标。 |
| 跨会话长期 / 跨会话（`2`） | 状态跨独立交互或任务长期保留，并需处理检索、冲突、演化与遗忘。 |
| 模型生命周期 / 生命周期（`3`） | 状态随预训练模型、全局知识库或部署版本长期存在，更新和删除影响广泛用户与后续版本。 |

<a id="综合报告"></a>
## 综合报告

### 大模型记忆技术：领域全貌与路线地图

> 检索截止：2026-08-11；目标读者：研究人员与具备大模型基础背景的专业读者；当前覆盖等级：L1（只有检索、主张审计和浏览器验收全部通过后才升级）。

#### 执行摘要

大模型记忆不是一个单独模块，而是一组跨时间状态管理机制。它们把信息写入模型权重、当前上下文与隐状态、外部索引、智能体经验库或用户档案，并在未来执行读取、更新、压缩、巩固和删除。真正的技术分界不是“有没有一个叫 memory 的组件”，而是四个问题：状态存在哪里、能持续多久、由谁决定写入和读取、错误与过期信息如何被修正或遗忘。

本地图的核心判断是：**容量扩展只是必要条件，可靠寻址、冲突更新、受控遗忘和来源审计共同决定记忆是否可用。** 参数记忆密度高但难以局部修改；上下文记忆即时且可见，却受窗口成本和长输入退化影响；外部检索易更新和追踪来源，但把检索错误与知识库污染带入生成；智能体记忆增加自主写入、反思和组织，也会把错误经验持续放大；用户长期记忆带来个性化价值，同时把时效、隐私、删除和用户控制变成一等约束。

因此，不应以“能在一次问答中复述过去信息”作为完成标准。部署级记忆至少应分别测量：写入选择、证据召回、正确利用、跨会话推理、时间与冲突更新、拒答、删除有效性、污染鲁棒性、成本和用户可控性。

#### 范围合同

本研究覆盖从奠基工作到 2026-08-11 的大语言模型、对话系统、检索增强系统和自主智能体。纳入对象包括：参数知识与知识编辑；上下文、KV/隐状态、循环和压缩记忆；外部非参数存储与检索；智能体情景、语义与程序记忆；用户个性化长期记忆；直接评测上述能力或揭示隐私、安全、污染和遗忘边界的工作。

正式同行评议论文优先。高相关预印本、官方技术报告、项目与代码只在没有等价正式版本或代表重要前沿时单列，并明确出版状态。核心结论只依赖论文正式页、DOI、会议/期刊入口和作者官方材料；综述、聚合页与搜索摘要只用于发现线索。

以下对象被明确排除：只有词面使用“memory”但没有状态管理机制或评测的工作；没有记忆贡献的普通 RAG 应用、向量数据库产品和缓存工程；只扩展位置编码或提高注意力效率、但没有长程状态证据的工作；与 LLM 记忆无直接关系的通用微调、测试时适应或持续学习；只有营销声明或无法核验一手入口的候选。

“全面”在本研究中表示声明范围内的多源覆盖、分支代表性、反证检索、引用链补漏与边际收敛，不表示穷尽全部论文、未公开产品实现或截止日后的长尾工作。

#### 操作定义与双轴地图

一项机制被视为“记忆”，需要存在可辨认的状态及跨时间影响：输入或经验被写入状态；状态在某一时间尺度上保留；后续查询能够寻址；读取结果稳定影响输出或行动。可将完整生命周期概括为六个操作：写入、组织/索引、读取、更新/巩固、压缩、遗忘/删除。

地图第一轴是六条互斥的主要路线。每项工作按主要技术贡献只计入一条路线，辅助关系仅用于检索，防止聚合重复计数。前五条按存储或管理机制划分，第六条“评测、安全与治理”按主要贡献类型划分；因此它不是与前五条完全同构的存储位置。第二轴是记忆持续时间：即时或单轮、会话或任务期、跨会话长期、模型生命周期。持续时间指设计目标，不等于一次实验实际运行的墙钟时间；对评测治理条目，它表示主要被测记忆的持续时间。

##### 路线一：参数记忆与知识修改

参数路线把知识存入模型权重、适配器或可路由参数模块。早期工作用填空探针表明预训练模型能召回关系事实，但也揭示不同事实的可学性不均。[Language Models as Knowledge Bases?](https://aclanthology.org/D19-1250/) 建立了“模型即知识库”的重要实验入口；随后 [Knowledgeable or Educated Guess?](https://aclanthology.org/2021.acl-long.146/) 表明提示和数据集偏差可能夸大可靠知识访问。

知识编辑将“写入”从完整再训练缩小为局部行为更新。[ROME](https://proceedings.neurips.cc/paper_files/paper/2022/hash/6f1d43d5a82a37e89b0665b33bf3a182-Abstract-Conference.html) 通过因果定位和秩一权重更新修改事实关联；[SERAC](https://proceedings.mlr.press/v162/mitchell22a.html) 则把编辑存入显式记忆并用检索控制何时覆盖基础模型。这两个方向也说明“参数式”和“外部式”不是绝对二分，而是不同写入成本、作用范围和可回滚性的组合。

[Larimar](https://proceedings.mlr.press/v235/das24a.html) 更直接地展示这种混合：冻结解码器旁的固定大小潜在情景记忆支持单次写入与负写入，兼顾连续编辑和选择性遗忘；但写入量超过设计容量后保留率下降，输入改写仍可恢复部分目标。负写入因此是可测的删除机制，不是密码学意义的清除保证。

参数记忆也可以通过稀疏寻址扩容。2019 年的 [Product-Key Memory](https://papers.neurips.cc/paper_files/paper/2019/hash/9d8df73a3cfbf3c5b47bc9b50f214aff-Abstract.html) 用两个子键码本的组合降低大键空间寻址成本，是这一支的历史前身；[Memory Layers at Scale](https://proceedings.mlr.press/v267/berges25a.html) 随后将可训练键值层扩展到最高 128B 记忆参数，并把容量增长与每次前向计算量部分解耦。相近前向 FLOPs 不等于零存储、零带宽或零工程成本；这类层在训练期形成全局参数容量，也不应误写成用户交互后可直接更新的个人记忆。

关键边界是编辑成功不等于知识真正一致地改变。[Model Editing Harms General Abilities of Large Language Models](https://aclanthology.org/2024.emnlp-main.934/) 在多模型、多任务上检查了通用能力副作用；[Revealing the Deceptiveness of Knowledge Editing](https://aclanthology.org/2025.acl-long.868/) 指出常规指标近乎满分时仍可能残留原知识。参数记忆因此必须联合评估可靠性、泛化、局部性、关联事实传播、通用能力、顺序编辑和回滚。

规模化实证进一步收紧了这一判断。[WikiBigEdit](https://proceedings.mlr.press/v267/thede25a.html) 用 Wikidata 时间快照构造超过 50 万问答更新，在五个约 7B 模型上比较局部编辑、RAG、持续 LoRA 与模型合并；局部编辑随顺序更新快速退化，而 RAG 的事实更新优势仍受多跳取回瓶颈约束。[Everything is Editable](https://proceedings.iclr.cc/paper_files/paper/2025/hash/02763667a5761ff92bb15d8751bcd223-Abstract-Conference.html) 则把编辑对象扩展到非结构化文本，但其结果仍应作为特定编辑协议的桥接证据，而不是“任意文本都可无副作用修改”的保证。

持续编辑还暴露了路由和写入之间的干扰。[DKME](https://aclanthology.org/2026.findings-acl.792/) 先学习独立的事实感知寻址空间，再只更新被选中的参数记忆分区；消融显示寻址与分区都影响综合表现，但外部编码器、聚类敏感性和分区增长仍是部署成本。[GLAME](https://aclanthology.org/2024.emnlp-main.1261/) 则从外部知识图采样关联子图，把关系编码合并进一次秩一参数更新。它连接图式外存与参数写入，却依赖知识图覆盖，也没有证明连续编辑流中的长期稳定性。

[AnyEdit](https://proceedings.mlr.press/v267/jiang25b.html) 将长形式目标分成顺序知识块，逐块定位末端关键标记并优化下一块生成，再把多个目标状态写回参数。它把编辑对象扩展到代码、数学表达和长文本等格式；论文报告效果提升的同时也记录约 24.7% 的平均编辑时间增幅，且尚未为终身连续编辑或多模态一致更新优化。

[StableEdit](https://icml.cc/virtual/2026/poster/64339) 从更新几何解释大规模连续编辑：它维护编辑梯度的运行均值与协方差，经过中心化和完整白化后再求解参数更新。正文消融直接显示持续归一化对所测 WikiBigEdit 设置重要，但理论依赖可跟踪漂移、非退化协方差等假设；这是一条参数编辑稳定机制，不是外部记忆或真实在线治理的通用保证。

[SSU](https://aclanthology.org/2025.findings-naacl.288/) 把遗忘从单次请求扩展到连续到达的版权删除流：每轮学习并减去目标任务向量，以随机标签损失强化忘却，再用梯度显著性限制被更新权重。论文同时观察到历史目标知识可能在后续时间步重新显现，且主要指标是词法重合；因此它提供序列遗忘机制与直接失败证据，不构成语义、法律或基础设施层面的不可逆删除保证。

##### 路线二：上下文与隐状态记忆

这条路线在一次推理过程中保留状态。[Transformer-XL](https://aclanthology.org/P19-1285/) 用段级循环跨越固定片段边界；[Recurrent Memory Transformer](https://proceedings.neurips.cc/paper_files/paper/2022/hash/47e288629a6996a17ce50b90a056a0e1-Abstract-Conference.html) 用专门记忆令牌在片段间传递压缩状态；[EM-LLM](https://proceedings.iclr.cc/paper_files/paper/2025/hash/c05144b635df16ac9bbf8246bbbd55ca-Abstract-Conference.html) 按事件边界分段并将情景表示放入外部缓冲；[Titans](https://papers.neurips.cc/paper_files/paper/2025/hash/a4ca07aa108036f80cbb5b82285fd4b1-Abstract-Conference.html) 将短期注意力与测试时更新的神经长期记忆结合；[SUPO](https://aclanthology.org/2026.acl-long.966/) 则在工具轨迹接近窗口预算时写入任务摘要，并用终局奖励联合优化压缩与后续行动。

这类方法的优势是状态与当前计算紧密耦合，无需单独检索系统；代价是状态压缩可能不可解释、早期证据可能被覆盖，训练分布外的更长序列不保证稳定。尤其要区分“宣称支持的上下文长度”和“任务上有效利用的长度”。本地图纳入的直接反证 [Context Length Alone Hurts LLM Performance Despite Perfect Retrieval](https://aclanthology.org/2025.findings-emnlp.1264/) 表明，在所测五个模型和三类任务中，即使相关证据被完美定位，长度增长本身仍可能损害表现。

[M+](https://proceedings.mlr.press/v267/wang25au.html) 展示了潜空间记忆在规模化时重新引入检索与分层存储：短期隐藏向量保留在快速存储，长期向量卸载到 CPU，并由共同训练的检索器取回。它在所测保持任务中把有效范围推进到 160K 以上，但 128K 设置查询更慢、隐藏状态难解释且最旧记忆仍会淘汰，因此“容量增加”不能替代延迟、可解释性和遗忘审计。

##### 路线三：外部检索与非参数记忆

外部路线将信息放在模型外的文档、键值或向量索引中。[kNN-LM](https://iclr.cc/virtual_2020/poster_HklBjCEKvH.html) 用隐藏表示近邻与基础语言模型概率插值；[REALM](https://proceedings.mlr.press/v119/guu20a.html) 在预训练、微调和推理中学习检索文档；[RAG](https://papers.nips.cc/paper_files/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html) 明确组合参数记忆与 Wikipedia 非参数记忆；[RETRO](https://proceedings.mlr.press/v162/borgeaud22a.html) 把检索规模扩展到万亿级词元语料。

外部记忆便于新增、删除、审计来源和共享，但完整链路至少包含索引、查询形成、召回、重排、上下文装配和阅读。最终失败可能来自任一环节，单独报告召回率或最终答案都不足以诊断。外部库还形成独立安全边界：[PoisonedRAG](https://www.usenix.org/conference/usenixsecurity25/presentation/zou-poisonedrag) 证明少量恶意文档可以操纵目标回答；这不意味着 RAG 不可用，而是意味着写库授权、来源签名、冲突检测和异常监控必须进入记忆设计。

图式外部记忆提供了一条清晰的方法—纠错链。[HippoRAG](https://proceedings.neurips.cc/paper_files/paper/2024/hash/6ddc001d07ca4f319af96a3024f6dbd1-Abstract-Conference.html) 用开放信息抽取构建知识图，再以个性化 PageRank 做多跳检索；其在线效率比较不包含离线抽取和建图成本。[HippoRAG 2](https://proceedings.mlr.press/v267/gutierrez25a.html) 增加段落节点与识别过滤，扩展到事实、意义建构和关联记忆，同时报告过滤可能删去相关三元组、时间与内存仍高于标准稠密 RAG。二者是独立论文，不是同一工作的预印本与正式版。

[RAGVA](https://www.sciencedirect.com/science/article/pii/S0164121225001049) 从 Transurban 收费公路客服系统给出数据摄取、元数据、检索生成、回退、测试与维护的工业生命周期，并通过九名工程人员的两次焦点组归纳八类工程挑战。它不提出新检索算法，也不是独立因果效果评估；价值在于补足真实 RAG 外部记忆的工程与治理桥接证据，且单一公司、单一场景不能代表全部部署。

##### 路线四：智能体记忆管理

智能体路线把记忆从“被动提供上下文”推进到“主动选择经验并管理生命周期”。[Generative Agents](https://doi.org/10.1145/3586183.3606763) 将观察写入记忆流，按相关性、近期性和重要性检索，并用反思形成更高层摘要；[Reflexion](https://papers.nips.cc/paper_files/paper/2023/hash/1b44b878bb782e6954cd888628510e90-Abstract-Conference.html) 把任务反馈转成自然语言反思，存入情景缓冲以改进后续试验；[MemGPT](https://arxiv.org/abs/2310.08560) 用类似虚拟内存的分层上下文管理在快慢记忆之间移动信息。

后续工作开始研究结构组织和记忆演化。[A-Mem](https://papers.nips.cc/paper_files/paper/2025/hash/19909c36f51abc4856b4560aff3d36d6-Abstract-Conference.html) 让新记忆生成结构化属性、链接旧记忆并触发已有表示更新；[RecMem](https://aclanthology.org/2026.findings-acl.1619/) 则把原始交互、情景摘要和语义事实分层，并只在相似内容持续复现时触发较昂贵的巩固。

[AriGraph](https://www.ijcai.org/proceedings/2025/2) 将动态环境中的语义关系与原始情景观察连成可更新世界图，先扩展语义子图，再回取覆盖这些关系的经历供规划使用。它在文本游戏中提供了图式世界模型的直接证据，但汇总采用五次运行中最佳三次的平均，图抽取与冲突检测错误也会累积。结构化索引曾把它误挂到 IJCAI 2024 的 DOI；本地图已按正式论文集更正为 `10.24963/ijcai.2025/2`。

自主性也扩大错误影响范围。[How Memory Management Impacts LLM Agents](https://aclanthology.org/2026.acl-long.27/) 在受控实验中观察到“经验跟随”：相似任务会诱发相似输出，从而带来错误传播和不匹配经验回放。智能体记忆的评价单位因此应从“单次回答”提升到轨迹级收益、错误恢复、写入质量、跨任务迁移和删除后的行为。

当记忆主体从单个智能体扩展到团队，组织层也需要状态。[G-Memory](https://proceedings.neurips.cc/paper_files/paper/2025/hash/136a45cd9b841bf785625709a19c6508-Abstract-Conference.html) 用洞见、查询与交互三层图保存跨试次协作轨迹，并按新任务双向检索与更新。这补足了多智能体组织记忆，但受控基准结果不能外推为真实团队的长期收益，模型生成的洞见还可能把错误经验传播给多个成员。

[Text2Mem](https://aclanthology.org/2026.findings-acl.100/) 把抽取、合并、删除与检索等生命周期操作表达为统一文本操作语言，使系统可以直接评测“选择了哪种记忆操作”而不只看最终回答。它推进了记忆操作的可执行表示，但一个删除指令被正确生成并不等于所有原文、摘要、向量、缓存和派生状态已经完成可验证清除。

权限本身也可成为需要长期维护的状态。[Towards Automating Data Access Permissions in AI Agents](https://doi.org/10.1109/SP63933.2026.00018) 用个人历史做上下文学习，并以相似用户的权限模式辅助新请求预测；高置信度预测更准确，却只覆盖部分请求。它尚未实现可信执行、沙箱或端到端数据流执法，因此可部署设计必须同时报告覆盖率、错误授权和人工回退，而不是只看平均准确率。

[MRAgent](https://icml.cc/virtual/2026/poster/60697) 把对话记忆组织为线索、标签和内容异质图，并依据当前中间证据反复扩展、筛选和剪枝路径。它在论文的 LoCoMo 与 LongMemEval 设置中优于所列基线，但遍历深度增加成本，图随交互增长，当前静态构图也没有更新、合并或遗忘操作，因此“主动重构”仍不等于完整生命周期管理。

[ChronoMem](https://arxiv.org/abs/2607.27773) 为每次长期记忆写入保存全局快照，再把自然语言撤销请求解析到历史版本并执行原子回滚，补入了语义版本控制这一治理原语。不过它截至截止日仍是预印本，PDF 含出版模板占位符，且同页正文与表格的部分数值冲突；本地图将其降为 C 级桥接证据，不用冲突数字支撑效果结论。

生命周期操作也开始从固定启发式走向显式协作与程序记忆。[AMA](https://aclanthology.org/2026.findings-acl.152/) 用 Constructor、Retriever、Judge 与 Refresher 把多粒度写入、相关性判断、冲突检测、目标更新和删除连成闭环；[Memp](https://aclanthology.org/2026.findings-acl.866/) 则把程序记忆拆为构建、检索和更新，比较追加、验证淘汰与失败后原位修改，并测试跨模型迁移。两者都仍依赖给定任务奖励或多代理调用；一次正确的删除决策也不能证明所有派生状态已经清除。

系统层开始给语义状态增加压缩与事务原语。[Hippocampus](https://proceedings.mlsys.org/paper_files/paper/2026/hash/a1d04870cf83a0f29819d66f1dfdbfcb-Abstract-Conference.html) 用动态小波矩阵在压缩域内搜索语义签名，并保留可无损重建的词元流；[SagaLLM](https://www.vldb.org/pvldb/vol18/p4874-chang.pdf) 把目标、理由、约束、状态转移和补偿计划写成不可变事务记录与检查点，支持多智能体恢复和补偿回滚。前者不自动解决冲突、权限或删除，后者的事务一致性也不保证模型计划本身正确。

[AgenticCache](https://proceedings.mlsys.org/paper_files/paper/2026/hash/c66a9db149261435664284a20b6f1d42-Abstract-Conference.html) 把跨轨迹计划转移写入在线缓存，并由后台模型异步验证与修订；它在所测具身多智能体任务上改善行动成功率与延迟，但依赖计划局部性，故只作程序记忆桥接。[PromptX](https://dl.acm.org/doi/10.1145/3774905.3793108) 的 Engram 与激活扩散提供跨会话平台演示；四页展示论文缺少受控基准，企业采用和业务影响主要为作者或合作方自报，按 C 级/Q1 处理。

##### 路线五：个性化与用户长期记忆

用户记忆以特定人的事实、偏好、关系和变化历史为对象，核心不是无限保存，而是形成适度、及时且可撤销的个性化。[Long Time No See!](https://aclanthology.org/2022.findings-acl.207/) 探索抽取与持续更新长期人物记忆；[MemoryBank](https://ojs.aaai.org/index.php/AAAI/article/view/29946) 结合检索、用户画像和随时间衰减的选择性保留；[RMM](https://aclanthology.org/2025.acl-long.413/) 用前瞻反思写出多粒度记忆，再用回答中的记忆引用形成回顾反馈闭环，提供不依赖图结构的强基线。[SteeM](https://aclanthology.org/2026.acl-long.670/) 更进一步把生成对历史的依赖程度设为用户可调控制轴，显式处理“忠实继承”和“重新开始”之间的权衡。

这一分支最容易把“记住”误当成“正确服务用户”。用户状态会变化，旧偏好可能被撤销，相同信息在不同场景的敏感性也不同。评价应同时考察事实召回、隐式偏好应用、冲突更新、过期信息失效、拒答、推荐质量、删除请求和用户知情控制，而不能只看历史问答准确率。

[Associa](https://aclanthology.org/2025.findings-acl.901/) 把话语、事件、实体、时间和用户构成事件中心图，先取关联子图，再由审议模型发现证据缺口并迭代补充。它在 LongMemEval 与 LoCoMo 的所测设置上改善检索和问答，但只覆盖英语基准，没有真实用户长期部署，也没有充分解决时间冲突。其价值是给出“快速关联加慢速审议”的可检验组合，而不是证明个人记忆默认应采用图结构。

[PersonalAlign](https://aclanthology.org/2026.acl-long.1669/) 从长期图形界面操作记录中抽取并流式维护用户偏好与惯例，再分别用于模糊指令执行和前摄建议。它把个人记忆从“记住对话事实”推进到“从行为记录推断隐式意图”，同时也放大了推断越界、旧惯例复用和跨设备撤销问题；AndroidIntent 的受控结果不能直接外推到真实手机长期部署。

[Ontology-Guided Long-Term Agent Memory](https://proceedings.mlsys.org/paper_files/paper/2026/hash/2fb4be70fc9668e9ec2c71b34fb127d4-Abstract-Conference.html) 并行维护关系图、向量片段和用户档案，并用时间、来源和反馈控制晋升、查询路由与回滚；[MemWeaver](https://dl.acm.org/doi/10.1145/3774904.3792732) 把交互行为写成时间—语义图，再汇总为演化偏好的认知叙事，并比较增量更新、全量重建和不更新。两者都说明个人记忆需要来源与版本，但模型生成摘要也可能把错误或越界推断固化为长期画像。

[ARIEL](https://petsymposium.org/popets/2026/popets-2026-0087.php) 将过去的数据分享判断保存为可追踪前例，用字段本体和方向性蕴含规则处理新请求，没有充分前例或出现冲突时交还用户。它比把历史直接塞进提示更容易审计，但评测把一次性情境数据拆成“历史”和“新请求”，尚未覆盖真实偏好随时间变化、撤销和跨场景冲突。

真实使用证据开始出现，但设计强度并不相同。[CareCall](https://doi.org/10.1145/3613904.3642420) 在公共卫生重复通话中把健康与日常摘要更新为后续输入，观察到披露和互动差异，也记录敏感追问的隐私张力；两组来自不同日历时期且不是同期随机分组，只能支持关联判断。[RECALLbot](https://dl.acm.org/doi/10.1145/3772318.3790714) 结合可见记忆管理与 14 天实验，但基线同时改变记忆、披露策略和控制界面，界面删除也没有验证后端数据是否彻底清除。[Relational Gains, Privacy Strains](https://dl.acm.org/doi/10.1145/3772318.3791635) 则让 20 名用户在访谈中检查本人产品记忆，直接暴露画像、情境身份与透明度问题；这是用户感知研究，不是客观准确率或删除审计。

[Knoll](https://doi.org/10.1145/3746059.3747711) 把外部长期知识做成可创建、启停、刷新、共享和显示来源的模块，并报告两个月真实部署；它说明“用户治理”也可以作用于知识模块，而不只作用于个人画像。源文档权限并不自动覆盖发送给第三方模型后的完整数据路径，相关但不必要的模块还可能诱发过度依赖。

##### 路线六：评测、安全与治理

这一分支把记忆系统当作需要测量和约束的对象。[LoCoMo](https://aclanthology.org/2024.acl-long.747/) 构造跨多会话的长对话，测试问答、事件总结和多模态生成，并显示长上下文与 RAG 仍明显落后于人类；[LongMemEval](https://proceedings.iclr.cc/paper_files/paper/2025/hash/d813d324dbf0598bbdc9c8e79740ed01-Abstract-Conference.html) 将能力拆为信息抽取、跨会话推理、时间推理、知识更新和拒答，同时把系统拆成索引、检索与阅读阶段；[Mem2ActBench](https://aclanthology.org/2026.acl-long.370/) 则把评价从被动问答推进到利用记忆执行工具任务。[Does Memory Need Graphs?](https://aclanthology.org/2026.acl-long.1232/) 在统一抽取器、预算和底座后拆分比较图式与平面记忆，显示更好的检索指标并不自动带来更好的端到端回答。

[Beyond Prompts](https://proceedings.neurips.cc/paper_files/paper/2024/hash/4aedf0cba303537fcb6cf948bb41b2df-Abstract-Datasets_and_Benchmarks_Track.html) 进一步把多个任务交错到同一长对话中，反复制造中断与上下文切换；多数受测配置在交错和长历史下退化，但并非每个模型与配置都下降。该基准每个场景只重复三次、依赖自动评分，且外部记忆系统的检索内容会膨胀上下文，因此它支持“静态单任务评测不够”，不支持“某一种记忆架构普遍优于长上下文”。

新的增量协议更接近系统真实运行方式。[MemoryAgentBench](https://iclr.cc/virtual/2026/poster/10010781) 在交互逐轮到达时分别检查准确检索、测试时学习、长期理解与选择性遗忘；[LifeDialBench](https://aclanthology.org/2026.findings-acl.351/) 用真实生活日志事件锚定合成对话，测试时间、更新和持续检索。后者不是直接收集的真实对话日志；“真实事件锚定”和“真实用户纵向交互”必须分开报告。

[Mem-Gallery](https://aclanthology.org/2026.acl-long.1892/) 把视觉与文本共同放入跨会话长轨迹，分别测试抽取与测试时适应、记忆推理、知识管理和成本；[StreamBench](https://proceedings.neurips.cc/paper_files/paper/2024/hash/c189915371c4474fe9789be3728113fc-Abstract-Datasets_and_Benchmarks_Track.html) 则用逐步输入、预测、二元反馈和状态更新协议测量持续改进。前者仍是受控生成对话，后者由静态数据随机串行化且不专属于记忆，因此 StreamBench 只作为桥接协议。

冲突评测开始区分“看到冲突、理解冲突和采取行动”。[ConflictBank](https://proceedings.neurips.cc/paper_files/paper/2024/hash/baf4b960d118f838ad0b2c08247a9ebe-Abstract-Datasets_and_Benchmarks_Track.html) 用错误信息、时间差异和语义分歧分别测外部证据、参数知识及二者交互；[When Facts Change](https://aclanthology.org/2026.findings-acl.103/) 显示模型可能在推理中识别事实具有可变性，却不据此接受最新证据。后者没有执行持久写回或版本替换，所以是更新失败的桥接反证，不是更新机制。

记忆治理还包括污染、隐私和可验证删除。[AgentPoison](https://proceedings.neurips.cc/paper_files/paper/2024/hash/eb113910e9c3f6242541c1652e30dfd6-Abstract-Conference.html) 说明长期记忆和知识库可能成为后门入口；[FragFuse](https://www.usenix.org/conference/usenixsecurity26/presentation/rao) 进一步表明逐轮访问控制可能遗漏长期记忆跨交互组合形成的有状态风险，防御重点应是跨轮来源、权限域和执行前再授权，而不是复现具体绕过步骤。[MUSE](https://proceedings.iclr.cc/paper_files/paper/2025/hash/4556f5398bd2c61bd7500e306b4e560a-Abstract-Conference.html) 将反记忆评测分为逐字记忆、知识记忆、隐私泄漏、效用保持、规模和连续请求六个维度，显示近似删除往往牺牲效用或无法持续处理请求。与此同时，[Do LLMs Really Memorize Personally Identifiable Information?](https://aclanthology.org/2026.acl-long.1560/) 提醒研究者控制表面提示线索，否则可能把模式补全误判为真实记忆化。风险结论因此既不能淡化，也不能用失控的测量方法夸大。

[The Good and The Bad](https://aclanthology.org/2024.findings-acl.267/) 揭示 RAG 的双重隐私效应：外部检索库本身可能被生成模型复现，而在指定模型和数据上，加入检索上下文又降低了基础模型训练文本的复现次数。两种现象作用于不同状态，不能合并成“RAG 更安全”或“RAG 更危险”。重排、摘要和阈值只能作为经验性缓解，无法替代访问控制、删除传播或形式化隐私保证。

[Reproducing LightMem](https://arxiv.org/abs/2607.29104) 提供了少见的直接独立反证：在对齐检索器、检索深度和回答预算后，朴素原始轮次检索通常可以与复杂记忆构造竞争；金标原始证据还优于对应构造记忆，指向压缩写入的信息损失。该研究只使用 LongMemEval-S、一个回答生成模型且尚未同行评议，因此它足以否定“复杂构造必然更好”的强断言，却不足以判定所有任务上的最终优劣。

反记忆评价也从单一“忘记率”扩展为恢复与指标审计。[OpenUnlearning](https://proceedings.neurips.cc/paper_files/paper/2025/hash/3e4a38f228427ab819ba7899003a44b1-Abstract-Datasets_and_Benchmarks_Track.html) 统一多种算法、基准和指标，并对指标本身做元评测；[FaithUn](https://aclanthology.org/2025.emnlp-main.657/) 同时检查改写、多跳关联知识和同答案无关知识；[FUMA](https://aclanthology.org/2025.emnlp-main.551/) 说明直接知识难以提取后，忘却集身份仍可能被识别；[Catastrophic Failure via Quantization](https://openreview.net/forum?id=lHSeDYamnz) 则显示部署量化可能恢复被压制知识。这四类失败作用于不同攻击面，不能合并成一个分数。

[RAGuard](https://proceedings.neurips.cc/paper_files/paper/2025/hash/ed25c00ff6900989116d3ba5d607d33d-Abstract-Datasets_and_Benchmarks_Track.html) 使用自然发生的支持、误导和无关证据，并在所测事实核查设置中观察到误导检索使所有被测 RAG 系统低于无检索基线。其领域集中于政治事实核查；它与同名研讨会防御框架是不同作品族，本地图只采用正式数据集与基准轨工作。

[Rethinking KV Cache Compression](https://proceedings.mlsys.org/paper_files/paper/2025/hash/26289c647c6828e862e271ca3c490486-Abstract-Conference.html) 直接显示状态压缩不仅改变吞吐和延迟，也会改变大量回答的长度与语义质量。它只因答案层反证进入评测路线，不代表普通缓存压缩属于长期语义记忆；工程比较必须同时报告端到端行为，而不能只报告节省的显存。

[ZK-APEX](https://proceedings.mlsys.org/paper_files/paper/2026/hash/148865706acbd18627d3fc15cc3f3b93-Abstract-Conference.html) 为个性化客户端执行约定的近似遗忘变换生成零知识证明，使提供方能验证操作合规而不读取私有参数。该证明绑定的是既定变换是否执行，不是目标知识在任意提示、量化或隐藏状态攻击下都不可恢复；可验证执行和可验证遗忘必须分开。

用户可见的控制还存在对象和语义错位。[Privacy Control in Conversational LLM Platforms](https://doi.org/10.1145/3772318.3791054) 对六个平台做政策与界面穿行，区分聊天历史、派生记忆片段和定制对象；删除原对话与删除派生记忆可能彼此独立，界面结果也不能证明后端副本已经清除。[The Impact of Security and Privacy Controls](https://www.usenix.org/conference/usenixsecurity26/presentation/kwesi) 在情感支持情境中比较记忆开关、删除、训练退出与其他控制，显示连续性效用、保护感和可验证保证之间存在张力。

两项用户研究进一步说明“设置存在”不等于“治理有效”。[Hoovered up as a data point](https://petsymposium.org/popets/2025/popets-2025-0160.php) 发现英国月度用户很少采用训练退出，且常把未来训练退出、聊天删除与既有模型影响的移除混为一谈；[Understanding Users’ Security and Privacy Concerns](https://doi.org/10.1109/SP61157.2025.00241) 用两年自然发生的社区讨论观察关切如何随功能、政策和事故变化。前者是横截面自报与仿真界面，后者只来自单一公开社区，都不能替代实际产品遥测或同一用户的长期行为轨迹。

产品审计揭示了另一层边界。[Big Help or Big Brother?](https://www.usenix.org/conference/usenixsecurity25/presentation/vekaria) 跨多类生成式助手检查跟踪、画像与个性化，说明部署态数据流不能从模型论文推断；它与针对单一记忆功能的访谈互补，但因产品配置、账户状态和测量时间会变化，只作为治理桥接证据。

[The Algorithmic Self-Portrait](https://doi.org/10.1145/3774904.3792671) 直接审计 80 名用户导出的 2,050 条产品记忆，分析写入能动性、个人数据、心理属性和来源忠实度。它弥补了访谈与跨产品黑盒审计之间的空白，但仍是单一、持续变化产品上的有限数据捐赠样本，部分标注依赖模型判断。

#### 发展阶段

1. **2019–2020：状态位置分化。** Transformer-XL 表明循环状态可跨片段；LAMA 把权重视为事实存储；kNN-LM、REALM 和 RAG 建立参数与非参数混合记忆范式。
2. **2021–2023：可编辑与可管理。** 研究从“是否记住”转向“能否定位和改写”，形成 MEND、ROME、SERAC、MEMIT 等编辑路线；同时 Generative Agents、Reflexion、MemGPT 把写入、反思和分层管理引入智能体。
3. **2024–2025：长程评测与生命周期暴露。** LoCoMo、LongMemEval 与输入长度反证揭示标称容量和有效使用之间的差距；WISE、A-Mem、Titans 等开始直接优化持续编辑、组织演化和测试时记忆；安全工作揭示外部库和智能体记忆的污染面。
4. **2026：从召回转向更新、行动和治理。** 新基准开始要求处理过期信息、隐式约束和工具行动；实证工作直接测量错误传播、经验删除、产品记忆控制、主动图重构、语义回滚和检索后利用。这标志着评价目标从静态问答准确率转向持续系统行为。

#### 路线对比

| 路线 | 典型写入 | 典型读取 | 优势 | 主要失败 | 适合场景 |
|---|---|---|---|---|---|
| 参数记忆 | 预训练、微调、编辑、适配器 | 前向计算隐式召回 | 高密度、低额外检索延迟 | 难定位、难回滚、关联不一致、通用能力受损 | 稳定通用知识、低延迟基础能力 |
| 上下文/隐状态 | 提示拼接、循环、压缩、测试时更新 | 注意力或状态递归 | 即时、与当前任务耦合 | 长度成本、覆盖、位置偏差、状态不可解释 | 文档理解、单次长任务、流式会话 |
| 外部检索 | 文档摄取、嵌入、索引更新 | 查询、召回、重排、阅读 | 易更新、可溯源、可删除 | 召回失败、冲突、注入、阅读错误 | 动态知识、企业资料、证据型问答 |
| 智能体记忆 | 轨迹筛选、反思、摘要、图组织 | 策略触发检索和经验复用 | 可积累经验、支持长程任务 | 错误传播、记忆污染、成本增长 | 自主任务、重复环境、多智能体协作 |
| 用户长期记忆 | 用户事实抽取、画像更新、偏好合并 | 跨会话个性化检索 | 连续体验与个性化 | 过时、越界推断、隐私、删除不彻底 | 助手、推荐、长期对话 |
| 评测治理 | 基准构造、审计记录、攻击/删除测试 | 指标与行为检查 | 暴露不可见边界 | 合成偏差、指标代理失真、缺少长期真人数据 | 方法选择、上线门禁、持续监控 |

#### 关键争议与负面证据

##### 长上下文是否能替代记忆系统

不能简单等同。更长窗口可以减少显式检索，但输入长度会增加计算成本，并不保证模型能在所有位置同等利用证据。检索则能缩小阅读范围，却可能漏召回或丢失跨片段关系。实践上应把长上下文视为工作记忆，把外部索引视为长期候选库，并用任务证据决定是否需要摘要、结构化索引或反复回看。

##### 知识编辑是否等于可靠更新

常规编辑指标往往只测目标提示、改写提示和少量邻近事实，不能证明所有关联问题都一致更新，也不能证明原知识已经不可恢复。顺序编辑还会累积干扰。可部署更新需要版本化、影响范围测试、通用能力回归和回滚，而不是只报告编辑成功率。

##### 反思与经验复用是否一定改善智能体

否。反思能够把反馈转成可读经验，但错误反馈、错误总结或不匹配检索也会变成高置信度先例。记忆写入需要质量门控，后续任务结果应反向更新经验可信度；删除不是简单清空最近几条，而要处理由旧经验派生出的摘要和图关系。

##### 图结构是否天然优于平面记忆

不能这样概括。图可以显式编码关系并支持多跳扩展，但收益同时受抽取器、键值粒度、更新操作、返回预算和重排影响。统一控制这些变量后，图方法可在部分检索指标上占优，平面强基线却可能在端到端问答上更好。本地图因此把图视为一种可检验的组织选择，而不是长期记忆的默认升级路径。

##### “忘记”是否可由拒答或错误回答证明

不能。输出层抑制可能掩盖仍可被其他提示恢复的知识；相反，过度破坏模型会降低目标信息输出，却同时损害一般效用。[Can Sensitive Information Be Deleted From LLMs?](https://proceedings.iclr.cc/paper_files/paper/2024/hash/c652e5f62fd1f5acbbf0d6413a1113e7-Abstract-Conference.html) 用隐藏状态、概率差和输入改写检查目标是否仍可提取，说明直接拒答不是充分证据。严格反记忆应接近“未用被删除数据训练的模型”行为，并同时检查逐字恢复、语义知识、成员推断、效用、规模和连续请求。

[Fast Exact Unlearning](https://proceedings.mlr.press/v267/muresanu25a.html) 提供另一种设计思路：把高撤回风险的任务适配数据留在可重建的上下文示例结构中，而不是写入权重，从而实现定义明确的精确删除。代价会转移到长期推理上下文，而且该保证不覆盖预训练数据；精确删除、输出不可提取性和下游效用仍是三个不同验收层。

##### 现有基准是否代表真实长期使用

部分代表。合成长对话允许控制证据位置、时间和冲突，也便于复现；但其语言分布、用户目标和交互后果与真实跨月使用仍有距离。LLM 评分器还可能引入偏差。更可靠的路线是合成压力测试、人工核验样本、真实用户纵向数据和线上故障日志的组合。

#### 工程决策框架

选择记忆架构时，先回答以下问题，而不是先选某个框架：

1. 状态是全局事实、任务经历还是用户私有信息？三者需要不同命名空间和权限。
2. 写入由谁批准？原始交互、模型抽取和人工确认应有不同可信度。
3. 需要保留多久？会话状态、跨会话档案和模型级知识不应共享同一过期策略。
4. 冲突如何解决？至少保留时间戳、来源、版本、置信度和失效标记。
5. 读取失败如何诊断？分别记录召回、重排、阅读和行动阶段，而不是只有最终答案。
6. 删除的传播范围是什么？原始记录、摘要、嵌入、派生画像、缓存和训练产物都要进入删除清单。
7. 哪些风险必须上线前验证？记忆注入、错误传播、越权跨用户检索、旧偏好复用和删除后恢复应是最低门禁。

#### 研究空白

- **真实长期纵向证据有限。** CareCall、RECALLbot 与 Knoll 已提供特定场景的重复使用或部署证据，社区研究也追踪了关切随事件变化；但跨月个体行为、真实任务后果、跨文化差异和因果设计仍少。
- **写入质量缺少统一测量。** 当前基准更多测“能否找到”，较少测“是否应该写、写成何种粒度、错误何时应被撤销”。
- **检索与利用没有充分解耦。** 相同最终错误可能来自漏召回、证据冲突、阅读失败或行动映射错误，需统一分阶段诊断。
- **派生记忆删除尚不成熟。** 删除原文不等于删除摘要、画像、图边和由其产生的参数更新；缺少端到端可验证删除协议。
- **有状态访问控制不足。** 单轮输入安全检查不能覆盖跨轮记忆组合后的新语义；权限、来源和执行授权需要贯穿写入、检索、合并与行动。
- **多用户与多智能体治理薄弱。** 共享经验能提高复用，但权限、来源、污染隔离和责任追踪仍未形成稳定范式。
- **成本—价值曲线缺少报告。** 存储增长、嵌入重建、摘要更新和重复阅读成本常被隐藏，难以比较长期系统效益。

#### 推荐阅读路径

##### 快速建立全貌

依次阅读 LAMA、Transformer-XL、RAG、ROME、Generative Agents、LoCoMo 与 LongMemEval。它们分别代表参数、上下文、外部、编辑、智能体和评测视角，可在较短路径内建立共同术语。

##### 面向架构设计

从 kNN-LM、REALM、RETRO 理解外部存储，再用 HippoRAG 与 HippoRAG 2 阅读图式检索的方法—纠错链；从 Recurrent Memory Transformer、Titans、M+ 理解压缩与测试时状态；从 MemGPT、A-Mem、G-Memory 理解单体到组织级管理；最后阅读 2026 年经验传播研究，避免只看到正面方法结果。

##### 面向安全与治理

先读模型编辑副作用、MUSE 和 PII 线索控制，再读 AgentPoison、PoisonedRAG 与记忆管理实证。阅读目标不是复现攻击，而是识别写库权限、来源认证、异常监控、删除和回滚的系统要求。

#### 覆盖与限制

结构化 `atlas.json` 是本报告的证据真源；每条记录都提供一手入口、出版状态、机制步骤、直接证据、局限和来源边界。检索日志、纳排记录和主张审计分别保存在 `planning/search_ledger.jsonl`、`planning/screening.csv` 与 `planning/claim_ledger.csv`。

本报告在声明范围内已通过五道研究闸门：范围与来源、分支代表性、最后一次高边际后的两轮异源边际收敛、98 个核心条目全文深读与 116 条主张审计。134 项结构化数据和离线依赖也通过自动验证。当前运行环境无法对最终 `file://` 页面执行真实桌面与 375px 手机浏览器回归，因此覆盖等级仍为 L1；这是一项交付界面验收限制，不把它误写成研究证据未收敛，也不据此宣称绝对穷尽全部论文或未公开系统。

<a id="全部研究工作"></a>
## 全部研究工作（134 项）

以下条目严格保持 `atlas.json` 的策展顺序，并在 README 内直接列出问题、机制、证据、局限、启示、核验边界与全部可用来源。

<a id="paper-agent-generative-agents-2023"></a>
**1. 生成式智能体：人类行为的交互式拟真｜Generative Agents: Interactive Simulacra of Human Behavior（2023 · UIST 2023）**

**作者：** Joon Sung Park、Joseph C. O'Brien、Carrie J. Cai、Meredith Ringel Morris、Percy Liang、Michael S. Bernstein

**书目：** 年份 2023；载体 UIST 2023；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Generative Agents；优先级 high；相关性排序 10；时间尺度 模拟内跨天持久；原型运行约两天

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** memory-stream、reflection、planning、social-simulation

**定位：** 以自然语言 memory stream、重要性／新近性／相关性检索、反思和规划组成完整记忆—认知闭环。

**问题：** 怎样让多个 LLM 智能体在持续社会模拟中保持经历连续性并产生可信行为？

**机制：** 所有观察写入带时间戳的记忆流；查询时按新近性、重要性、相关性打分；定期从近期记忆生成高层反思，再用于规划与行动。

**步骤：**

1. 把感知、行动和对话以自然语言事件写入 memory stream
2. 对候选按 recency、importance、relevance 组合检索
3. 累积重要性超过阈值时生成反思并把反思再写回记忆
4. 检索记忆和反思生成日程／即时计划并执行

**证据：**

- Figure 5／§4.1–§4.2 给出 memory stream 与 reflection 机制
- Figure 8 的架构消融表明记忆、反思、规划共同影响行为评价
- §6.5.2／§7.2 记录检索失败、只取到不完整片段以及模型补写虚构细节等失败

**局限：**

- 25 个智能体仅模拟两天，运行成本达数千美元，长期稳定性没有得到证明
- 检索可漏掉关键信息，LLM 会把不完整记忆补写成虚构细节
- 人类可信度评价不等同于事实正确性或真实心理模型

**意义：**

- 是情景记忆向语义反思巩固的代表系统
- 提供完整生命周期雏形，但也暴露检索遗漏和记忆诱发幻觉

**建议路线：** 情景／反思记忆

**边界：** ACM DOI、全文与作者代码核验；不把模拟可信度表述为独立现实有效性证据。

**版本：** 以 UIST 2023 DOI 正式版为主，arXiv 为公开全文入口。

**标识：** DOI 10.1145/3586183.3606763；工作族 ID generative-agents

**证据位置：**

- Figure 5：完整架构
- §4.1：记忆流与三因子检索
- §4.2：reflection
- Figure 8：消融
- §6.5.2／§7.2：失败分析
- §8.2：短时尺度、运行成本和可扩展性限制

**资源：** [一手入口](<https://doi.org/10.1145/3586183.3606763>) · [PDF](<https://arxiv.org/pdf/2304.03442>) · [代码](<https://github.com/joonspk-research/generative_agents>)

**关联 ID：** `agent-amem-2025` · `agent-reflexion-2023` · `locomo-2024`

---

<a id="paper-agent-memgpt-2023"></a>
**2. MemGPT：迈向作为操作系统的大语言模型｜MemGPT: Towards LLMs as Operating Systems（2023 · arXiv preprint）**

**作者：** Charles Packer、Sarah Wooders、Kevin Lin、Vivian Fang、Shishir G. Patil、Ion Stoica、Joseph E. Gonzalez

**书目：** 年份 2023；载体 arXiv preprint；状态 预印本；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 C；简称 MemGPT；优先级 high；相关性排序 14；时间尺度 跨会话持久；层级外存与上下文分页

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P1 / Q=Q1

**标签：** virtual-context、paging、function-calling、hierarchical-memory

**定位：** 用函数调用让 LLM 自主管理主上下文、召回存储和档案存储，提出‘虚拟上下文’操作系统类比。

**问题：** 固定上下文模型如何在长文档和多会话聊天中获得近似无限上下文的体验？

**机制：** 把有限 prompt 视为主存、外部档案视为磁盘；系统以中断／函数调用触发消息、搜索、分页、读写和上下文腾挪。

**步骤：**

1. 将系统指令、工作记忆和消息放入主上下文
2. 收到内存压力或任务需要时由模型调用外存函数
3. 把旧内容写入 archival／recall storage，并检索或分页读回
4. 用系统心跳／中断继续内部处理，最终回复用户

**证据：**

- Figure 3 给出分层虚拟上下文与函数调用控制
- Tables 2–3 评估多会话记忆，Figure 5 评估长文档问答，Figure 7 评估嵌套键值检索
- 错误分析指出 embedding 检索漏掉金标准、模型过早停止翻页；GPT-3.5 因函数调用弱而退化，嵌套超过两层性能下降

**局限：**

- 截至检索日未核验到同行评议正式版本，证据等级必须保持预印本 C
- 性能高度依赖底座模型的函数调用和自我调度；分页策略可能提前停止
- 官方项目已演化并更名 Letta，当前软件行为不应直接回填到 2023 论文结论

**意义：**

- 是记忆层级、分页和自主读写控制的标志性原型
- 后续 MemoryOS、Mem0、Memory-R1 等应与其比较生命周期而不只比较检索准确率

**建议路线：** 记忆生命周期与层级管理

**边界：** arXiv 全文与官方项目核验；出版状态明确保持 preprint。

**版本：** 论文为 2023／2024 arXiv 版本；原 MemGPT 项目后来更名 Letta。代码链接指当前继承项目，仅作版本族入口，不声称等同论文实现。

**标识：** 工作族 ID memgpt-letta

**证据位置：**

- Figure 3：体系结构
- §3：函数调用、主上下文与外存
- Tables 2–3：多会话聊天
- Figure 5：文档 QA
- Figure 7：nested KV
- 正文错误分析：检索漏失、早停、函数调用失败

**资源：** [一手入口](<https://arxiv.org/abs/2310.08560>) · [PDF](<https://arxiv.org/pdf/2310.08560>) · [项目页](<https://www.letta.com/>) · [代码](<https://github.com/letta-ai/letta>)

**关联 ID：** `agent-memory-r1-2026` · `memorybank-2024` · `memoryos-2025`

---

<a id="paper-agent-reflexion-2023"></a>
**3. Reflexion：具有语言强化学习的语言智能体｜Reflexion: Language Agents with Verbal Reinforcement Learning（2023 · NeurIPS 2023）**

**作者：** Noah Shinn、Federico Cassano、Ashwin Gopinath、Karthik Narasimhan、Shunyu Yao

**书目：** 年份 2023；载体 NeurIPS 2023；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 会话或任务期；阅读层级 核心；证据等级 A；简称 Reflexion；优先级 high；相关性排序 9；时间尺度 会话或任务期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** reflection、episodic-buffer、verbal-RL、trial-and-error

**定位：** 把失败反馈转成自然语言反思并存入后续试验上下文，以无需权重更新的方式实现跨试验学习。

**问题：** 黑盒语言智能体能否从试错经验学习，而无需梯度更新模型参数？

**机制：** 行动轨迹接受环境或自我反馈，反思器生成文字教训，写入情景记忆缓冲并在下一次试验提示中读取。

**步骤：**

1. 运行 Actor 产生一条完整轨迹
2. Evaluator 给出成功／失败或文本反馈
3. Self-Reflection 模型把轨迹和反馈压缩为语言经验
4. 把近期反思加入下一试验上下文并重复

**证据：**

- Figure 2／§3 给出 Actor—Evaluator—Self-Reflection 闭环
- Figure 3、Figure 4 与 Table 1 分别覆盖 ALFWorld、HotpotQA、HumanEval
- Appendix B.1／Figure 6 报告 WebShop 上相对 ReAct 无显著收益，且需要探索多样性的任务会陷入局部最优

**局限：**

- 记忆是有限滑动窗口，通常只跨同一任务试验，不等于跨会话长期个体记忆
- 自我反思可重复错误并陷入局部最优；缺乏探索多样性时无改进
- 评估反馈质量决定记忆质量，错误反思没有显式纠错或删除治理

**意义：**

- 奠定 agent episodic reflection 谱系
- 说明程序性改善可由自然语言情景记忆实现，但需与永久语义记忆区分

**建议路线：** 情景／反思记忆

**边界：** NeurIPS 正式页、全文和作者仓库核验；WebShop 负结果来自论文附录而非二手评论。

**版本：** 以 NeurIPS 2023 正式版为准。

**标识：** DOI 10.52202/075280-0377；工作族 ID reflexion

**证据位置：**

- Figure 2／§3：机制
- Figure 3：ALFWorld
- Figure 4：HotpotQA
- Table 1：HumanEval
- §5：滑动窗口容量和局部最优
- Appendix B.1／Figure 6：WebShop 负结果

**资源：** [一手入口](<https://papers.nips.cc/paper_files/paper/2023/hash/1b44b878bb782e6954cd888628510e90-Abstract-Conference.html>) · [PDF](<https://papers.nips.cc/paper_files/paper/2023/file/1b44b878bb782e6954cd888628510e90-Paper-Conference.pdf>) · [代码](<https://github.com/noahshinn024/reflexion>)

**关联 ID：** `agent-expel-2024` · `agent-generative-agents-2023` · `agent-voyager-2024`

---

<a id="paper-agent-expel-2024"></a>
**4. ExpeL：大语言模型智能体是经验学习者｜ExpeL: LLM Agents Are Experiential Learners（2024 · AAAI 2024）**

**作者：** Andrew Zhao、Daniel Huang、Quentin Xu、Matthieu Lin、Yong-Jin Liu、Gao Huang

**书目：** 年份 2024；载体 AAAI 2024；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 ExpeL；优先级 high；相关性排序 12；时间尺度 跨训练任务提炼经验，并在新任务检索

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** experience-pool、insights、transfer、procedural-memory

**定位：** 从多条试验轨迹抽取可编辑、可升降权的自然语言 insight，再以成功轨迹示例与 insight 指导新任务。

**问题：** 不可微、闭源 LLM 智能体如何在不微调的情况下跨任务积累经验并迁移？

**机制：** 先采集成功／失败轨迹，从成对或跨任务比较中提炼 insights，通过 ADD／EDIT／UPVOTE／DOWNVOTE 维护经验池，推理时检索示例与经验。

**步骤：**

1. 在训练任务上反复试验并收集轨迹
2. 比较成功和失败轨迹，抽取自然语言 insights
3. 用新增、编辑、升权、降权操作维护经验池
4. 新任务检索相关成功轨迹和 insights 作为提示

**证据：**

- Figure 1 与 §4 给出 gather—extract—retrieve 流程
- Figure 5／Figure 6 展示经验累积和任务表现关系，Table 3 给出消融
- §6 明确指出当前 insights 尚可全部放入上下文，真正 lifelong 规模需要额外 insight retrieval

**局限：**

- 主要是文本任务和闭源 API 模型，真实长期部署与多模态未验证
- 当前经验池足够小，可直接放入上下文；规模扩大后的检索、冲突和遗忘未解决
- 自然语言 insight 可能过度概括或受初始试验偏差影响

**意义：**

- 连接 Reflexion 的单任务反思与跨任务程序性经验库
- 展示记忆维护操作可作用于经验规则，而不只是事实条目

**建议路线：** 程序／技能记忆

**边界：** AAAI 正式页、PDF 与作者代码核验。

**版本：** 以 AAAI 2024 正式版为准；作者代码自述为 AAAI Oral 官方实现。

**标识：** DOI 10.1609/aaai.v38i17.29936；工作族 ID expel

**证据位置：**

- Figure 1：总览
- §4.1：轨迹采集
- §4.2：经验池与四类操作
- §4.3：推理检索
- Table 1：迁移
- Table 3：消融
- §6：限制

**资源：** [一手入口](<https://ojs.aaai.org/index.php/AAAI/article/view/29936>) · [PDF](<https://ojs.aaai.org/index.php/AAAI/article/download/29936/31644>) · [代码](<https://github.com/LeapLabTHU/ExpeL>)

**关联 ID：** `agent-reflexion-2023` · `agent-voyager-2024`

---

<a id="paper-agent-voyager-2024"></a>
**5. Voyager：基于大语言模型的开放式具身智能体｜Voyager: An Open-Ended Embodied Agent with Large Language Models（2024 · Transactions on Machine Learning Research）**

**作者：** Guanzhi Wang、Yuqi Xie、Yunfan Jiang、Ajay Mandlekar、Chaowei Xiao、Yuke Zhu、Linxi Fan、Anima Anandkumar

**书目：** 年份 2024；载体 Transactions on Machine Learning Research；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Voyager；优先级 high；相关性排序 11；时间尺度 跨任务／新世界复用的持续技能库

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** procedural-memory、skills、code、embodied-agent

**定位：** 把可执行代码技能及自然语言描述写入不断增长的技能库，并按新任务检索组合，是程序记忆的代表。

**问题：** 开放世界具身智能体如何持续积累技能、避免灾难性遗忘并迁移到新任务？

**机制：** 自动课程提出下一目标；GPT-4 生成代码；环境反馈和自验证迭代修正；成功程序按描述嵌入写入技能库并按需检索。

**步骤：**

1. 自动课程根据状态提出可达的新目标
2. 从技能库按任务描述检索相关程序
3. GPT-4 生成／组合代码并执行
4. 利用错误、环境反馈和自验证迭代，成功后写回技能库

**证据：**

- 摘要报告相对既有方法获得 3.3 倍独特物品、2.3 倍探索距离、关键技术树最多快 15.3 倍
- §2.2／Figure 4 给出技能库的写入、索引与检索
- Figure 9 消融显示移除技能库后进展平台化；移除自动课程或自验证也显著退化

**局限：**

- 实验集中于 Minecraft，依赖 GPT-4 黑盒和文本化感知，外推到现实具身环境不确定
- 会生成错误技能、不可用物品或 API 调用，自验证也会失败并卡住
- 代码技能适合作为程序记忆，但不包含用户事实更新、隐私和删除治理

**意义：**

- 确立可执行技能库作为程序性长期记忆的清晰范式
- 与对话情景／语义记忆互补，不能只按向量检索统一解释

**建议路线：** 程序／技能记忆

**边界：** TMLR 录用状态、arXiv 全文、项目页与作者代码核验。

**版本：** 2023 年 arXiv 后由 TMLR 正式接收；TMLR submissions／event certification 已核验录用状态，全文入口保留 arXiv。

**标识：** 稳定 ID openreview:ehfRiF0R3a；工作族 ID voyager

**证据位置：**

- Abstract：3.3×、2.3×、15.3×
- §2.2／Figure 4：skill library
- §2.3：iterative prompting
- Table 2：零样本任务
- Figure 9：消融
- §4：成本、卡死、错误技能和幻觉限制

**资源：** [一手入口](<https://openreview.net/forum?id=ehfRiF0R3a>) · [PDF](<https://arxiv.org/pdf/2305.16291>) · [项目页](<https://voyager.minedojo.org/>) · [代码](<https://github.com/MineDojo/Voyager>)

**关联 ID：** `agent-expel-2024` · `agent-reflexion-2023`

---

<a id="paper-agent-amem-2025"></a>
**6. A-Mem：面向大语言模型智能体的自主记忆｜A-Mem: Agentic Memory for LLM Agents（2025 · NeurIPS 2025）**

**作者：** Wujiang Xu、Zujie Liang、Kai Mei、Hang Gao、Juntao Tan、Yongfeng Zhang

**书目：** 年份 2025；载体 NeurIPS 2025；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 A-Mem；优先级 high；相关性排序 19；时间尺度 跨会话持久、自组织并可演化的笔记网络

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** Zettelkasten、self-organizing、memory-evolution、linked-notes

**定位：** 借鉴 Zettelkasten，把每段经验转成带内容、时间、关键词、标签、上下文和链接的原子笔记，并让新记忆反向更新旧笔记。

**问题：** 固定 schema 和固定操作限制记忆适应性，能否让记忆结构本身随经验自组织演化？

**机制：** LLM 构造富属性原子笔记；向量先筛近邻，再由 LLM 建立语义链接；新记忆触发旧笔记上下文／标签演化；查询按嵌入取回相关笔记。

**步骤：**

1. 为新交互生成原子 note、关键词、标签和上下文描述
2. 向量检索 top-k 历史候选
3. LLM 判断并建立链接形成多个 box
4. 用新邻域更新相关旧笔记的属性，再按查询检索

**证据：**

- Figure 2／§3.1–§3.4 给出 note、link、evolve、retrieve 全流程
- Table 1 在多底座 LoCoMo 上总体排名领先；Table 3 消融中移除 link 和 evolution 明显退化
- Table 4 只测向量检索延迟且所有方法存储均线性；并未把 LLM 建链／演化成本完整计入检索时间

**局限：**

- 记忆组织质量依赖底座 LLM；同一输入可能生成不同链接和上下文
- Table 4 的微秒级结果只覆盖向量检索，不代表包含多次 LLM 调用的端到端写入／演化成本
- 仅文本交互，错误演化可能污染旧记忆，缺乏回滚和来源置信治理

**意义：**

- 把结构化语义记忆从固定图推进到自组织演化
- 后续 MemoryOS、Mem0、LightMem、RecMem 都以其作为强基线，形成明确前向引用链

**建议路线：** 语义／结构化记忆

**边界：** NeurIPS 正式页和 PDF 全文核验；对效率结论按实际测量边界降格。

**版本：** 以 NeurIPS 2025 正式版为准；正式页给出的 anonymous 代码归档不稳定，故不填 code\_url。

**标识：** DOI 10.52202/085713-0593；工作族 ID amem

**证据位置：**

- Figure 2／§3.1–§3.4：机制
- Table 1：LoCoMo
- Table 3／§4.4：消融
- Table 4／§4.6：规模与检索时间
- §6：底座依赖和仅文本限制

**资源：** [一手入口](<https://papers.nips.cc/paper_files/paper/2025/hash/19909c36f51abc4856b4560aff3d36d6-Abstract-Conference.html>) · [PDF](<https://papers.nips.cc/paper_files/paper/2025/file/19909c36f51abc4856b4560aff3d36d6-Paper-Conference.pdf>)

**关联 ID：** `agent-generative-agents-2023` · `agent-mem0-2025` · `memoryos-2025`

---

<a id="paper-agent-mem0-2025"></a>
**7. Mem0：用可扩展长期记忆构建可生产部署的智能体｜Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory（2025 · ECAI 2025）**

**作者：** Prateek Chhikara、Dev Khant、Saket Aryan、Taranjeet Singh、Deshraj Yadav

**书目：** 年份 2025；载体 ECAI 2025；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Mem0；优先级 high；相关性排序 18；时间尺度 跨会话持久事实库；增量抽取与更新

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** CRUD、fact-memory、graph-memory、production

**定位：** 从每轮对话抽取 salient facts，再以 ADD／UPDATE／DELETE／NOOP 合并；另有图版本表示实体关系。

**问题：** 如何在跨会话对话中保持紧凑、可更新且低延迟的长期记忆？

**机制：** 用摘要与近期消息辅助事实抽取；为每条候选事实召回相似旧记忆；LLM 选择四类更新操作；查询时检索紧凑事实，图变体额外抽取实体关系。

**步骤：**

1. 对新消息对、全局摘要和近期窗口做事实抽取
2. 为候选事实检索 top-s 相似记忆
3. LLM 选择 ADD、UPDATE、DELETE 或 NOOP 并执行
4. 查询时检索相关事实；图版本并行遍历关系

**证据：**

- Figure 2／§2.1 给出抽取和四操作更新；Figure 3 给出图版本
- 摘要报告相对 OpenAI 的 LLM-as-Judge 指标提升 26%、图版本再高约 2%，p95 延迟降低 91%、token 节省超过 90%
- §4.3 明确 full-context 的 Judge 分约 73% 仍高于 Mem0 约 67%／图版约 68%，但延迟更大，这是关键反证

**局限：**

- 主要在 LoCoMo 与 LLM-as-a-Judge 上由系统作者评测，独立跨域证据有限
- full-context 在该数据上的准确率仍更高；宣称生产就绪主要由延迟／成本支持，不等于长期安全可靠
- 论文代码／产品持续快速演化，当前 SDK 默认行为可能不同于论文版本

**意义：**

- 四操作生命周期已成为后续 Memory-R1 的直接基础
- 是普通 RAG 与真正可写／更新／删除记忆的清晰分界案例

**建议路线：** 记忆生命周期与层级管理

**边界：** ECAI 接收页／DOI、arXiv 全文和官方代码核验。

**版本：** 以 ECAI 2025 正式收录状态和 DOI 为出版依据；公开 arXiv 是稳定全文。当前 GitHub 为持续迭代产品，需与论文快照区分。

**标识：** DOI 10.3233/FAIA251160；稳定 ID doi:10.3233/FAIA251160；工作族 ID mem0

**证据位置：**

- Figure 2／§2.1：基础 Mem0
- Figure 3／§2.2：图版
- Table 2／§4.2–§4.5：LoCoMo、延迟、token
- §4.3：full-context 准确率优势
- §5：未来工作与范围

**资源：** [一手入口](<https://doi.org/10.3233/FAIA251160>) · [PDF](<https://arxiv.org/pdf/2504.19413>) · [代码](<https://github.com/mem0ai/mem0>)

**关联 ID：** `agent-amem-2025` · `agent-memory-r1-2026` · `agent-zep-2025`

---

<a id="paper-agent-memllm-2025"></a>
**8. MemLLM：微调大语言模型使用显式读写记忆｜MemLLM: Finetuning LLMs to Use Explicit Read-Write Memory（2025 · Transactions on Machine Learning Research）**

**作者：** Ali Modarressi、Abdullatif Köksal、Ayyoob Imani、Mohsen Fayyaz、Hinrich Schütze

**书目：** 年份 2025；载体 Transactions on Machine Learning Research；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 MemLLM；优先级 high；相关性排序 8；时间尺度 跨文本／跨会话持久关系三元组，可顺序编辑

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** read-write、triples、knowledge-editing、explicit-memory

**定位：** 通过微调让模型主动发出关系三元组的读写 API 调用，是从被动 RAG 到显式、可编辑、可互操作记忆的重要跃迁。

**问题：** 如何让 LLM 自主写入和读取结构化外部记忆，并支持大规模事实更新而不修改模型权重？

**机制：** 以实体—关系—实体三元组数据库为外存，构造读写 API 训练数据，微调模型在生成中选择写入、查询和使用返回实体。

**步骤：**

1. 从输入句子抽取关系并生成 memory-write 调用
2. 把实体、关系与向量索引写入去重的三元组模式
3. 生成带一个变量的 memory-read 查询并做相似匹配
4. 将返回实体注入生成上下文；编辑时覆盖同实体关系的旧对象

**证据：**

- Table 2 中 TARGET PPL 从 memory-disabled 的 3.510 降至 full-Wikipedia memory 的 2.986；金标准消融进一步定位写、查、用三类误差
- Table 3 的 1000 次连续 ZsRE 编辑平均分 0.84，REL／GEN／LOC 分别 0.78／0.76／0.97
- Qualitative Analysis 将 216 个可靠性错误分解为写失败 45、未检索 95、检索后未有效使用 63，提供强负面证据

**局限：**

- 当前仅支持 96 类 Wikidata 关系，不能自动推导组合关系
- 论文明确称 MemLLM 不是 memory-aware：事实未写入时仍依赖参数知识或幻觉
- 没有与 RAG 直接比较，理由是 RAG 的非结构化事实难编辑；因此跨范式优越性尚未实证

**意义：**

- 满足真正记忆的写入、更新、读取和审计条件，属于核心结构化语义记忆
- 把普通 RAG 的不可编辑文本块与显式原子记忆清晰区分

**建议路线：** 语义／结构化记忆

**边界：** TMLR OpenReview 元数据、正式 PDF、作者代码和限制段全文核验。

**版本：** RET-LLM（arXiv:2305.14322）是概念前身；本记录以 2025 年 4 月 TMLR 正式录用版为主，不把后来的其他投稿页面当作新工作。

**标识：** 工作族 ID memllm

**证据位置：**

- §3.1／Figure 1：三元组 schema
- §3.2／Figure 2：读写 API
- Table 2／§4.2：困惑度与 gold ablations
- Table 3／§4.3：1000 次连续编辑
- Limitations：96 种关系、无组合推理、缺失事实时仍会幻觉

**资源：** [一手入口](<https://openreview.net/forum?id=dghM7sOudh>) · [PDF](<https://openreview.net/pdf?id=dghM7sOudh>) · [代码](<https://github.com/amodaresi/MemLLM>)

**关联 ID：** `agent-mem0-2025` · `agent-memory-r1-2026` · `ext-rag-2020`

---

<a id="paper-agent-zep-2025"></a>
**9. Zep：面向智能体记忆的时序知识图架构｜Zep: A Temporal Knowledge Graph Architecture for Agent Memory（2025 · arXiv preprint）**

**作者：** Preston Rasmussen、Pavlo Paliychuk、Travis Beauvais、Jack Ryan、Daniel Chalef

**书目：** 年份 2025；载体 arXiv preprint；状态 预印本；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 桥接；证据等级 C；简称 Zep／Graphiti；优先级 medium；相关性排序 17；时间尺度 跨会话动态双时态知识图，保留历史有效期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P1 / Q=Q1

**标签：** temporal-KG、bi-temporal、Graphiti、enterprise-memory

**定位：** 以 Graphiti 构建 episode—entity/fact—community 三层双时态图，显式表示事实有效期和失效。

**问题：** 静态 RAG 无法表示持续对话和企业数据中的变化事实，怎样同时保留当前状态与历史？

**机制：** 原始消息作为 non-lossy episodes；抽取并消歧实体／事实；用有效时间和事务时间标注边，矛盾时失效旧边；混合搜索、图遍历和重排构造上下文。

**步骤：**

1. 写入带参考时间的 message／text／JSON episode
2. 抽取、消歧实体和事实并链接回原始 episode
3. 比较新旧事实，设置 valid／invalid 与 created／expired 时间
4. 用向量、BM25、BFS 和 reranker 检索事实／实体／社区

**证据：**

- §2／§2.2.3 给出三层图、双时态与边失效机制
- Table 1 的 DMR 上 Zep 94.8% 仅略高于 full-context 94.4%；作者明确称 DMR 太小且不足以评测记忆
- Table 2 的 LongMemEval 中 gpt-4o 从 full-context 60.2% 到 Zep 71.2%，但 Table 3 的 assistant-side 问题下降 17.7%

**局限：**

- 公司作者对自家生产系统的预印本评价，未同行评议且缺少独立复现
- DMR 优势仅 0.4 个百分点且 full-context 已很强；LongMemEval 某些类别明显退化
- 作者无法复现 MemGPT 的 gpt-4o-mini 对照，也未评估完整图能力和生产可扩展性

**意义：**

- 提供时间有效期、矛盾失效和原始 episode 可追溯的结构化记忆设计
- 也是强反证案例：总体提升不能掩盖助理侧记忆下降和基准饱和

**建议路线：** 语义／结构化记忆

**边界：** arXiv 全文与官方开源 Graphiti 核验；因公司自评与预印本状态降级。

**版本：** 预印本描述的是 2025 年 Zep／Graphiti 系统；当前产品迭代不得回填为论文证据。

**标识：** 工作族 ID zep-graphiti

**证据位置：**

- §2：三层图
- §2.1：双时态 episode
- §2.2.3：事实失效
- §3：search—rerank—construct
- Table 1／§4.2：DMR 与其反证
- Tables 2–3／§4.3：LongMemEval 与分项退化

**资源：** [一手入口](<https://arxiv.org/abs/2501.13956>) · [PDF](<https://arxiv.org/pdf/2501.13956>) · [代码](<https://github.com/getzep/graphiti>)

**关联 ID：** `agent-amem-2025` · `agent-mem0-2025` · `longmemeval-2025`

---

<a id="paper-arigraph-2025"></a>
**10. AriGraph：为大模型智能体学习带情景记忆的知识图世界模型｜AriGraph: Learning Knowledge Graph World Models with Episodic Memory for LLM Agents（2025 · IJCAI 2025）**

**作者：** Petr Anokhin、Nikita Semenov、Artyom Sorokin、Dmitry Evseev、Andrey Kravchenko、Mikhail Burtsev、Evgeny Burnaev

**书目：** 年份 2025；载体 IJCAI 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、外部检索与非参数记忆；层级 会话或任务期；阅读层级 桥接；证据等级 A；简称 AriGraph；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把交互中抽取的语义关系与原始情景观察连成可更新世界图，供智能体联合检索、规划和行动。

**问题：** 全文历史、摘要或平面向量检索难以表达动态环境中的关系、过时事实与具体经历，妨碍规划和探索。

**机制：** AriGraph 同时维护语义节点与关系、情景观察节点以及两者的连接；每步从观察中抽取新关系、删除冲突旧关系，再先检索语义子图、后按覆盖关系取回相关情景。

**步骤：**

1. 把每个新观察保存为情景节点，并抽取对象—关系—对象三元组。
2. 与已有关系比对，删除已过时边，再扩展语义知识图。
3. 从查询出发做语义相似加图扩展，得到相关关系；再按这些关系覆盖度取回情景观察。
4. 把相关语义、情景、近期轨迹和目标放入工作记忆，分离规划与行动选择。

**证据：**

- 图 3 在控制相同决策组件的文本游戏比较中报告 Ariadne 相对全文历史、摘要、RAG、Generative Agents 与 Reflexion 等记忆实现更高的任务表现；正文说明结果取五次运行中最佳三次的平均。
- 表 1 的 NetHack 部分观察条件下，Ariadne 得分为 593.00±202.62，只有房间观察的 NetPlay 为 341.67±109.14，拥有整层信息的 NetPlay 为 675.33±130.27。
- 表 2 的静态多跳问答显示 AriGraph 具有竞争力而非全面最优：GPT-4 版本在 HotpotQA 上 EM 68.0、F1 74.7，但 HOLMES 的 F1 为 78.0。

**局限：**

- 文本游戏结果采用五次运行中最佳三次的平均，可能高估典型运行表现。
- 世界图依赖语言模型抽取与冲突检测；错误三元组或漏检会累积。
- 当前只处理文本观察，未建模多模态与程序记忆；静态问答样本也较小。

**意义：**

- 图式记忆的独特价值可能出现在需要更新世界状态和规划的动态环境，而不是所有长对话检索。
- 将语义事实与原始情景连接可在结构推理和细节追溯之间折中。

**边界：** 采用 IJCAI 2025 正式论文集并以 DBLP 和 DOI 交叉核验。OpenAlex 与 Crossref 的 10.24963/ijcai.2024/2 元数据误挂同题名，但该正式 PDF 实为另一论文；本记录已更正为 2025/2。

**引用：** Anokhin et al., IJCAI 2025, DOI 10.24963/ijcai.2025/2。

**版本：** arXiv:2407.04363 为早期版本；主记录采用 IJCAI 2025 正式版，不能使用错误的 2024/2 DOI 记录。

**标识：** DOI 10.24963/ijcai.2025/2；稳定 ID doi:10.24963/ijcai.2025/2；工作族 ID arigraph-2407-04363

**证据位置：**

- 第 2 节、算法 1 与图 2，正式页码 13–15：语义和情景图更新及两阶段检索。
- 图 3–5 与第 5.1–5.2 节，正式页码 16–17：文本游戏、NetHack 和扩展结果。
- 表 2 与第 5.3 节，正式页码 17：多跳问答的竞争性结果。
- 第 7 节，正式页码 18：多模态、程序记忆与更复杂图检索的缺口。

**资源：** [一手入口](<https://www.ijcai.org/proceedings/2025/2>) · [PDF](<https://www.ijcai.org/proceedings/2025/0002.pdf>)

**关联 ID：** `g-memory-2025` · `hipporag-2024` · `agent-voyager-2024` · `agent-generative-agents-2023`

---

<a id="paper-g-memory-2025"></a>
**11. G-Memory：面向多智能体系统的分层轨迹记忆｜G-Memory: Tracing Hierarchical Memory for Multi-Agent Systems（2025 · NeurIPS 2025）**

**作者：** Guibin Zhang、Muxin Fu、Kun Wang、Frank Wan、Miao Yu、Shuicheng Yan

**书目：** 年份 2025；载体 NeurIPS 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、外部检索与非参数记忆；层级 跨会话长期；阅读层级 桥接；证据等级 A；简称 G-Memory；优先级 medium；时间尺度 跨试次、多智能体协作历史

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** multi-agent、hierarchical-memory、graph-memory、cross-trial

**定位：** 用 insight、query 与 interaction 三层图谱同时保存可迁移洞见和压缩后的多智能体协作轨迹。

**问题：** 现有多智能体记忆忽略协作轨迹并缺少跨试次、按智能体定制的演化，如何让团队从历史协作中持续改进？

**机制：** 把协作历史组织为洞见图、查询图和交互图，查询时双向遍历取得高层经验与细粒度轨迹，执行后再用新轨迹更新整层结构。

**步骤：**

1. 将历史协作压缩为三层图
2. 新查询触发自顶向下与自底向上的双向遍历
3. 取回通用洞见与任务相关交互轨迹
4. 执行任务并把新协作轨迹写回各层

**证据：**

- 官方摘要报告覆盖 5 个基准、3 个底座和 3 个多智能体框架
- 具身行动成功率最高提升 20.89%，知识问答准确率最高提升 10.12%
- 无需修改原多智能体框架即可挂接

**局限：**

- 五个受控基准不能代表真实组织跨月部署
- 洞见与轨迹由模型压缩，错误协作经验可能被固化并跨智能体传播
- 图构建、更新、检索与长期维护的端到端成本仍需独立部署测量

**意义：**

- 把智能体记忆从单体笔记扩展到组织级协作记忆
- 为多智能体长期学习形成独立但仍可放入现有 agentic 一级路线的分支

**边界：** NeurIPS 2025 正式会议页和正式 PDF 全文核验；HTML 的 Frank Wan 与 PDF 首页的 Guancheng Wan 存在作者名冲突，合并时必须保留来源注记。

**标识：** DOI 10.52202/085713-0439；稳定 ID doi:10.52202/085713-0439

**证据位置：**

- claim 三层 insight/query/interaction 图及跨试验更新；location 正式 PDF §3–4；来源 PDF
- claim 五个基准、三类骨干、三个多智能体框架及主结果；location 正式 PDF §5、Tables 1–3；来源 PDF
- claim 局限及与 A-Mem、Mem0 的差异；location 正式 PDF §6、Appendix D；来源 PDF

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2025/hash/136a45cd9b841bf785625709a19c6508-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2025/file/136a45cd9b841bf785625709a19c6508-Paper-Conference.pdf>)

**关联 ID：** `agent-amem-2025` · `agent-expel-2024` · `agent-reflexion-2023`

---

<a id="paper-sagallm-2025"></a>
**12. SagaLLM：多智能体大模型规划的上下文管理、验证与事务保证｜SagaLLM: Context Management, Validation, and Transaction Guarantees for Multi-Agent LLM Planning（2025 · PVLDB 18(12)）**

**作者：** Edward Y. Chang、Longling Geng

**书目：** 年份 2025；载体 PVLDB 18(12)；状态 同行评议；出版状态 peer-reviewed；来源类型 journal/conference paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、评测、安全与治理；层级 跨会话长期；阅读层级 核心；证据等级 B；简称 SagaLLM；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把大模型生成的目标、理由、约束和补偿行动持久化为事务状态，使受扰动的多智能体计划可恢复、回滚并保持一致。

**问题：** 一般工作流日志只能重放调用，无法表达大模型计划中的语义依赖、已成事实的动作和失败后的补偿。

**机制：** 不可变动作日志与检查点保存语义计划对象，Saga 式补偿和约束验证维护跨智能体一致性。

**步骤：**

1. 将目标、操作输入输出、时间戳、大模型推理／理由、依赖约束和补偿元数据写入持久状态。
2. 每个已承诺动作进入不可变日志，并在语义边界生成检查点。
3. 并发规划按依赖和验证规则提交，使最终状态与某个串行顺序等价并保持已提交状态持久。
4. 环境扰动或失败触发结构化补偿；不可变过去不被重写，只调整尚可改变的后续行动。

**证据：**

- §3.1／表 2 明列应用语义实体、检查点、操作输入输出、时间、推理理由、补偿元数据、依赖约束及其证据。
- 四个 REALM 问题与四个大模型构成评测；旅行规划扰动中，只有 SagaLLM 正确处理交通警报，一般大模型会改写已发生的过去。
- 表 10 比较上下文、验证和事务能力，支持其机制覆盖，但主要是定性能力矩阵而非统计显著性结果。

**局限：**

- 只有少量规划场景，许多结论为定性案例，缺少聚合显著性。
- 采用放宽的 ACID 语义，不等于通用数据库事务。
- 大模型生成的补偿动作没有形式化验证其现实正确性。
- 没有生产多租户并发和长期故障压力测试。

**意义：**

- 大模型状态恢复必须持久化语义目标、理由与约束，而不只是 RPC 或工作流游标。
- 不可变过去与可补偿未来的划分是长期行动记忆的重要一致性原则。

**边界：** 书目信息、状态对象与场景证据由 PVLDB 正式全文核验。

**引用：** 作者项目：https://github.com/genglongling/SagaLLM。

**版本：** 以 PVLDB 18(12) 正式全文为准；作者项目仅作实现补充。

**标识：** DOI 10.14778/3750601.3750611；稳定 ID doi:10.14778/3750601.3750611；工作族 ID sagallm-2025

**证据位置：**

- 摘要与 §1，PDF 第 1–2 页：持久记忆和 Saga 补偿问题
- §3.1 与表 2，PDF 第 4 页：语义状态对象
- §5.1–§5.3，PDF 第 9–11 页：REALM 场景与扰动
- 表 10，PDF 第 12 页：能力比较

**资源：** [一手入口](<https://www.vldb.org/pvldb/vol18/p4874-chang.pdf>) · [PDF](<https://www.vldb.org/pvldb/vol18/p4874-chang.pdf>)

**关联 ID：** `memoryagentbench-2026` · `mem2actbench-2026` · `agent-reflexion-2023` · `agent-voyager-2024`

---

<a id="paper-agent-agemem-2026"></a>
**13. 自主记忆：学习统一管理大语言模型智能体的长期与短期记忆｜Agentic Memory: Learning Unified Long-Term and Short-Term Memory Management for Large Language Model Agents（2026 · ACL 2026）**

**作者：** Yi Yu、Liuyi Yao、Yuexiang Xie、Qingquan Tan、Jiaqi Feng、Yaliang Li、Libing Wu

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 AgeMem；优先级 high；相关性排序 24；时间尺度 任务内 STM 与跨任务／会话 LTM 统一策略

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** unified-memory、tool-actions、GRPO、STM-LTM

**定位：** 把长期 ADD／UPDATE／DELETE 与短期 RETRIEVE／SUMMARY／FILTER 都暴露为同一 LLM 策略的工具动作，并以分阶段 GRPO 训练。

**问题：** LTM 与 STM 通常由独立启发式控制，怎样让同一 agent policy 学会联合存、取、改、压缩和丢弃？

**机制：** 统一状态包含当前上下文、长期库和任务；六种记忆工具进入动作空间；三阶段课程先学 LTM 写入，再在干扰下学 STM 控制，最后联合任务；step-wise GRPO 把终局奖励传回早期操作。

**步骤：**

1. Stage 1 从预任务信息选择 ADD／UPDATE／DELETE 建 LTM
2. Stage 2 重置上下文、加入 distractors，学习 SUMMARY／FILTER
3. Stage 3 用 RETRIEVE 协调 LTM 与 STM 完成任务
4. step-wise GRPO 用任务、上下文、记忆质量和惩罚联合更新

**证据：**

- Table 1／§3.2 明确六类工具及目标层；§3.3–§3.4 给出三阶段和 step-wise GRPO
- Table 2：Qwen2.5-7B 与 Qwen3-4B 的五基准平均分分别 41.96 和 54.31，比最强对照高 4.82／8.57 个百分点
- Table 4 显示 All-Returns Judge 0.544、Memory Quality 0.533，高于 Answer-Only 0.509／0.479，但 token 和 tool calls 也增加

**局限：**

- 工具集固定，尚未支持更细粒度生命周期控制
- 五个基准仍是受控环境，未评测真实持久用户对话
- 三阶段轨迹只由 HotpotQA 训练，跨域成功不代表开放式长期部署；评价含 LLM-as-Judge 与自定义 Memory Quality

**意义：**

- 代表从分立记忆模块走向统一 agent policy 的前沿方向
- 提示未来评测应把 task、memory quality、context cost 和 tool behavior 联合报告

**建议路线：** 记忆生命周期与层级管理

**边界：** ACL Anthology 正式页、PDF 和作者代码核验。

**版本：** 以 ACL 2026 正式版为准；arXiv 2601.01885 v3 为同期公开全文。

**标识：** DOI 10.18653/v1/2026.acl-long.981；工作族 ID agemem

**证据位置：**

- Figure 1：独立与统一管理对比
- Table 1／§3.2：六类工具
- §3.3–§3.4：三阶段和 step-wise GRPO
- Table 2／§4.2：五基准
- Figure 4／Table 4：消融与奖励
- Limitations：固定工具集、受控环境、HotpotQA 训练来源

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.981/>) · [PDF](<https://aclanthology.org/2026.acl-long.981.pdf>) · [代码](<https://github.com/y1y5/AgeMem>)

**关联 ID：** `agent-lightmem-2026` · `agent-memory-r1-2026` · `agent-recmem-2026`

---

<a id="paper-agent-lightmem-2026"></a>
**14. 使用小语言模型的轻量级大语言模型智能体记忆｜Lightweight LLM Agent Memory with Small Language Models（2026 · ACL 2026）**

**作者：** Jiaquan Zhang、Chaoning Zhang、Shuxu Chen、Zhenzhen Huang、Pengcheng Zheng、Zhicheng Wang、Ping Guo、Fan Mo、Sung-Ho Bae、Jie Zou、Jiwei Wei、Yang Yang

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 LightMem；优先级 medium；相关性排序 22；时间尺度 跨会话 STM／MTM／LTM；在线写入、离线巩固

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** SLM、online-offline、bounded-retrieval、latency

**定位：** 用多个小模型拆分查询规划、语义重排和写入，把重型长期巩固移到离线，兼顾固定预算与低延迟。

**问题：** 纯向量检索噪声大，在线多次调用大模型又慢，怎样在固定预算下兼顾效果与延迟？

**机制：** STM 是当前窗口，MTM 存用户情景摘要，LTM 存去标识化语义知识；SLM-1 生成假设查询和预算，SLM-2 从 2K 候选重排为 K，SLM-3 写 MTM，离线大模型巩固 LTM。

**步骤：**

1. SLM-1 推断意图、生成 HQ、元数据过滤和 MTM／LTM 预算
2. 向量粗检索固定 2K 候选
3. SLM-2 语义一致性筛为最终 K 条
4. SLM-3 增量写 MTM；离线把高价值片段去标识化巩固进图式 LTM

**证据：**

- 正式摘要：LoCoMo 平均 F1 比 A-MEM 高约 2.5，检索中位延迟 83ms、端到端 581ms
- Table 4 给出 P50／P95；LightMem 并非检索最快，但在效果和延迟间取平衡
- Table 6 的级联错误使 DialSim F1 从 4.12 崩至 1.85；Table 7 的 MTM 噪声饱和使多跳 F1 从 28.85 降到 23.10

**局限：**

- 只研究一种在线／离线拆分和固定巩固策略，替代策略未探索
- 固定 Top-K 会被错误 HQ 或新近噪声占用，多模块错误可跨轮级联放大
- LTM 去标识化与跨用户知识共享的隐私／泄露边界没有独立验证

**意义：**

- 把效率、尾延迟和错误累积纳入记忆机制评价，而不只报 QA 分数
- 表明小模型可以承担高频控制，但不是对级联污染的万能解

**建议路线：** 记忆生命周期与层级管理

**边界：** ACL Anthology 正式页和 PDF 全文核验；未把搜索到的同名 GitHub 误认作本论文代码。

**版本：** 与同名或近名的其他 LightMem／memory-augmented generation 工作不是同一版本族；以 ACL 2026 Anthology ID 唯一化。

**标识：** DOI 10.18653/v1/2026.acl-long.588；工作族 ID lightmem-agent-memory

**证据位置：**

- Figure 2／§3.2–§3.6：三层和在线／离线流程
- Table 2：LoCoMo
- Figure 3：消融
- Table 4：延迟
- Tables 6–7：错误注入与 update-gap 压力测试
- §6／Appendix §9.2：限制与 intent focus dilution

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.588/>) · [PDF](<https://aclanthology.org/2026.acl-long.588.pdf>)

**关联 ID：** `agent-amem-2025` · `agent-recmem-2026` · `memoryos-2025`

---

<a id="paper-agent-memory-r1-2026"></a>
**15. Memory-R1：通过强化学习增强大语言模型智能体的记忆管理与利用｜Memory-R1: Enhancing Large Language Model Agents to Manage and Utilize Memories via Reinforcement Learning（2026 · ACL 2026）**

**作者：** Sikuan Yan、Xiufeng Yang、Zuchao Huang、Ercong Nie、Zifeng Ding、Zonggen Li、Xiaowen Ma、Jinhe Bi、Kristian Kersting、Jeff Z. Pan、Hinrich Schuetze、Volker Tresp、Yunpu Ma

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Memory-R1；优先级 high；相关性排序 21；时间尺度 跨会话持久；学习四类写操作和回答时蒸馏

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** reinforcement-learning、CRUD、memory-distillation、adaptive-policy

**定位：** 分别用 RL 训练 Memory Manager 选择 ADD／UPDATE／DELETE／NOOP，训练 Answer Agent 从大量召回记忆中蒸馏相关项。

**问题：** 启发式 CRUD 和无差别把召回结果塞入上下文会误更新并受噪声干扰，能否用下游答案奖励直接学习？

**机制：** Manager 对每轮候选事实和相关旧记忆选择操作；Answer Agent 从每方 top-30、共 60 条中选择并作答；PPO／GRPO 以最终正确性更新。

**步骤：**

1. 从对话轮抽取候选事实并检索旧记忆
2. RL Manager 选择 ADD、UPDATE、DELETE、NOOP 形成新 memory bank
3. 对问题检索 60 条候选记忆
4. RL Answer Agent 做 memory distillation 并生成短答案

**证据：**

- 正式摘要：仅 152 个训练 QA，跨 LoCoMo、MSC、LongMemEval 和 3B–14B 模型泛化
- Table 1：LLaMA-3.1-8B 上 GRPO 相对最强 MemoryOS 的整体 F1／B1／Judge 分别相对提升约 28.5%／34.0%／30.2%
- Table 2 显示 Judge 奖励虽 Judge 分更高但 F1／B1 更差，揭示 reward hacking／指标错配

**局限：**

- 只在对话型基准训练／验证，多模态和开放式真实长期部署未覆盖
- Manager 与 Answer Agent 分开训练以稳定稀疏奖励，未实现端到端协调
- 依赖下游答案奖励，可能优化基准答案而非记忆真实性；不同 Judge／EM 奖励明显改变行为

**意义：**

- 把生命周期管理从 prompt 启发式推进到可学习策略
- 显示记忆评价必须同时报告准确率、蒸馏行为和奖励敏感性

**建议路线：** 记忆生命周期与层级管理

**边界：** ACL Anthology 正式元数据与 PDF 全文核验；未找到可确认作者代码入口，故不填 code\_url。

**版本：** 正式 ACL 版作者为 13 人，较早 arXiv 版本作者列表较短；本记录以 ACL 2026 正式元数据为准。

**标识：** DOI 10.18653/v1/2026.acl-long.583；工作族 ID memory-r1

**证据位置：**

- Figure 2／§3：双智能体 RL 管线
- Table 1／§4.2：LoCoMo 主结果
- Figure 5／§4.4：Manager、Answer、distillation 消融
- Table 2：奖励设计冲突
- Limitations：对话中心、分开训练
- Appendix Tables 4–5：LongMemEval 零样本迁移

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.583/>) · [PDF](<https://aclanthology.org/2026.acl-long.583.pdf>)

**关联 ID：** `agent-agemem-2026` · `agent-mem0-2025` · `longmemeval-2025` · `memoryos-2025`

---

<a id="paper-agent-recmem-2026"></a>
**16. RecMem：面向长期运行大语言模型智能体的复现驱动记忆巩固｜RecMem: Recurrence-based Memory Consolidation for Efficient and Effective Long-Running LLM Agents（2026 · Findings of ACL 2026）**

**作者：** Zijie Dai、Shiyuan Deng、Sheng Guan、Yizhou Tian、Xin Yao、Xiao Yan、James Cheng

**书目：** 年份 2026；载体 Findings of ACL 2026；状态 同行评议；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 RecMem；优先级 medium；相关性排序 23；时间尺度 跨会话持久；按语义复现触发情景／语义巩固

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** consolidation、recurrence、episodic-semantic、efficiency

**定位：** 不再每轮调用 LLM 巩固，而把原始交互先存潜意识层，只有相似内容持续复现时才生成情景与语义记忆。

**问题：** eager consolidation 对每轮都调用 LLM，长期运行成本过高；什么时候才值得巩固？

**机制：** 所有原始交互以嵌入写入 subconscious store；新条目找到足够多相似历史时触发聚类；LLM 生成带时间情景摘要，再回看原文提取被摘要遗漏的语义事实。

**步骤：**

1. 原样保存每条交互并做轻量嵌入
2. 按 similarity 与 count 阈值检测持续复现
3. 触发时按时间排序生成 episodic summary
4. 以 episode 为参照回看原文，提取遗漏事实到 semantic memory；查询并行检索三层

**证据：**

- Table 1：LoCoMo 上 GPT-4.1-mini 构建 token 193.2K，相对 Mem0 1520.8K 降 87.3%，总体 Judge 81.10 高于受测 memory baselines，但 full-context 84.18 仍更高
- Table 2：LongMemEval-S 总体 76.80，为表中最高，但若看单类并非普遍最佳
- Figure 2 消融：移除 subconscious 总分从 81.10 降至 51.88；去 semantic 降至 70.58，去 episodic 仅降至 79.94

**局限：**

- 依赖手工 similarity／count 阈值，换领域需重校准
- 复现只是 salience 的代理，罕见但关键事件不会获得跨轮链接和语义细化；虽原文仍可直接检索，但推理可能更弱
- 主要以 LLM-as-a-Judge 为主指标；full-context 在较短 LoCoMo 上仍略高

**意义：**

- 首次把‘何时巩固’和构建成本提升为一等研究问题
- 提供对 eager 每轮抽取范式的直接反证，同时保留原始情景层作为安全网

**建议路线：** 情景／反思记忆

**边界：** ACL Anthology 正式页、全文、作者代码与限制段核验。

**版本：** 以 Findings of ACL 2026 正式版为准；arXiv 2605.16045 为公开全文版本。

**标识：** DOI 10.18653/v1/2026.findings-acl.1619；工作族 ID recmem

**证据位置：**

- Figure 1／§1：成本动机
- §3.1–§3.5：三层机制
- Tables 1–2／§4.2：准确率与构建／查询 token
- Figure 2／§4.3：消融
- §6：阈值与 rare-but-critical 限制

**资源：** [一手入口](<https://aclanthology.org/2026.findings-acl.1619/>) · [PDF](<https://aclanthology.org/2026.findings-acl.1619.pdf>) · [代码](<https://github.com/CaiusDai/RecMem>)

**关联 ID：** `agent-lightmem-2026` · `memorybank-2024` · `memoryos-2025`

---

<a id="paper-agenticcache-2026"></a>
**17. AgenticCache：面向具身智能体的缓存驱动异步规划｜AgenticCache: Cache-Driven Asynchronous Planning for Embodied AI Agents（2026 · MLSys 2026）**

**作者：** Hojoon Kim、Yuheng Wu、Thierry Tambe

**书目：** 年份 2026；载体 MLSys 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 conference paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、评测、安全与治理；层级 跨会话长期；阅读层级 桥接；证据等级 B；简称 AgenticCache；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把反复出现的计划状态转移沉淀为可在线修正的程序性记忆，在前台快速执行与后台大模型规划间切换。

**问题：** 具身规划每一步调用大模型带来高延迟和成本，而静态缓存无法适应环境变化。

**机制：** 缓存链存储状态到动作的程序片段；后台更新器验证和重写片段，前台在发现纠正结果后原子切换。

**步骤：**

1. 从历史执行中提取频繁的局部计划状态转移，写入缓存链。
2. 前台沿缓存链快速生成动作，同时后台大模型异步检查环境和计划有效性。
3. 后台发现失配时生成纠正片段并验证，随后替换对应链段。
4. 冷启动可由直接规划积累，热启动可载入既有程序记忆。

**证据：**

- 表 2 覆盖 4 个具身基准和 3 个模型，作者汇总称成功率提高 22%、延迟降低 65%、令牌成本降低 50%。
- TDW-COOK 的 GPT-5 设置中，总耗时从 12.86 小时降至 1.75 小时，成本从 21 美元降至 4.4 美元。
- 冷启动与热启动比较显示缓存由在线经验形成后继续提升；论文讨论把情景经验转为可复用程序记忆。

**局限：**

- 只在具身规划环境评测，不能视为跨域通用程序记忆。
- 二元词组局部性可能漏掉长延迟依赖。
- 并行前后台会产生协调和资源竞争。
- 高阶与分层程序记忆仍是未来工作。

**意义：**

- 缓存只有在保存可复用语义计划并测量行动成功率时才进入记忆图谱。
- 在线纠正机制可作为程序记忆更新的系统模板。

**边界：** 因任务域单一，尽管系统证据完整仍定为 bridge。

**引用：** 区别于一般命中率缓存：其缓存对象是能改变行动结果的语义计划转移。

**版本：** 以 MLSys 2026 正式论文集全文为准。

**标识：** 稳定 ID mlsys-2026-c66a9db149261435664284a20b6f1d42；工作族 ID agenticcache-2026

**证据位置：**

- §4，PDF 第 4–5 页：缓存链、异步验证和替换
- 表 2，PDF 第 7 页：成功率、延迟和成本
- §5.4 与讨论，PDF 第 9–10 页：冷／热启动及限制

**资源：** [一手入口](<https://proceedings.mlsys.org/paper_files/paper/2026/hash/c66a9db149261435664284a20b6f1d42-Abstract-Conference.html>) · [PDF](<https://proceedings.mlsys.org/paper_files/paper/2026/file/c66a9db149261435664284a20b6f1d42-Paper-Conference.pdf>)

**关联 ID：** `agent-expel-2024` · `agent-voyager-2024` · `agent-reflexion-2023` · `mem2actbench-2026`

---

<a id="paper-ama-2026"></a>
**18. AMA：通过多智能体协作实现自适应记忆｜AMA: Adaptive Memory via Multi-Agent Collaboration（2026 · Findings of ACL 2026）**

**作者：** Weiquan Huang、Zixuan Wang、Hehai Lin、Sudong Wang、Bo Xu、Qian Li、Beier Zhu、Linyi Yang、Chengwei Qin

**书目：** 年份 2026；载体 Findings of ACL 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、外部检索与非参数记忆、评测、安全与治理；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 AMA；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 以构建、检索、判断和刷新四个角色把逻辑冲突转成对具体记忆条目的更新或删除。

**问题：** 长期记忆常以固定粒度检索并不断追加，导致相关性错配和逻辑矛盾累积。

**机制：** 三级记忆按任务意图路由；Judge验证证据充足性和一致性，冲突时调用Refresher对目标条目更新或删除。

**步骤：**

1. Constructor生成原文、事实和事件三级记忆
2. Retriever按任务复杂度选择粒度并检索
3. Judge检查相关性、证据充分性和逻辑冲突
4. Refresher对冲突条目执行更新或删除

**证据：**

- Tables 1至2显示AMA在LoCoMo和LongMemEval上优于对照，同时大幅降低全上下文令牌消耗。
- Figure 3与第3.4节给出目标条目更新和删除，而非再次追加冲突版本。
- 消融结果显示Judge和Refresher分别贡献一致性与成本收益。

**局限：**

- 多智能体编排增加调用开销和延迟。
- 更新决策依赖模型判断，可能把不确定冲突误当成应删除信息。
- 验证仍基于既有对话基准，不证明真实部署中的审计和恢复能力。

**意义：**

- 新增“冲突检测到原子更新或删除”的自动维护闭环。
- 应为刷新操作保留来源、理由和可回滚日志，避免不可见的错误删除。

**边界：** 正式论文页核验元数据与出版状态；公开全文核验机制、表图证据和局限。

**引用：** Huang等，Findings of ACL 2026，DOI 10.18653/v1/2026.findings-acl.152。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.18653/v1/2026.findings-acl.152；稳定 ID doi:10.18653/v1/2026.findings-acl.152；工作族 ID ama-2026

**证据位置：**

- Figure 2与第3节，印刷第3100至3101页：四角色生命周期
- 第3.1至3.4节和Figure 3，印刷第3102至3104页：三级记忆与更新删除
- Tables 1至4，印刷第3103至3106页：主结果、消融和成本
- Limitations，印刷第3107页

**资源：** [一手入口](<https://aclanthology.org/2026.findings-acl.152/>) · [PDF](<https://aclanthology.org/2026.findings-acl.152.pdf>)

**关联 ID：** `agent-zep-2025` · `memoryos-2025` · `chronomem-2026`

---

<a id="paper-automating-data-access-permissions-ai-agents-2026"></a>
**19. 面向人工智能智能体的数据访问权限自动化｜Towards Automating Data Access Permissions in AI Agents（2026 · IEEE Symposium on Security and Privacy 2026）**

**作者：** Yuhao Wu、Ke Yang、Franziska Roesner、Tadayoshi Kohno、Ning Zhang、Umar Iqbal

**书目：** 年份 2026；载体 IEEE Symposium on Security and Privacy 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、个性化与用户长期记忆、评测、安全与治理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 智能体权限历史学习；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把个人权限决定历史与相似用户模式联合为可增长的访问控制状态，以置信度决定自动执行还是交还用户。

**问题：** 智能体调用多种工具和个人数据时，逐次询问破坏自动化，而一次性静态权限又不能覆盖动态任务和用户差异。

**机制：** 系统以个人历史做 LLM 上下文学习，以全体用户的相似权限模式做协同过滤，再联合两者预测新请求并对低置信度请求保留人工决策。

**步骤：**

1. 通过八个领域、二十一个工具和六十五个任务问题收集用户对数据访问的允许与拒绝选择。
2. 把个人历史编码为查询、数据类型、请求工具和决定，作为上下文样例预测未见请求。
3. 从用户与数据类型矩阵学习相似用户的权限模式，只采用高置信度协同过滤建议。
4. 联合个人上下文与协同过滤，并按置信度阈值在自动覆盖率与错误权限之间取舍。

**证据：**

- 第 4.1 节报告招募 205 人，移除两份无效回答后分析 203 人；任务覆盖八个领域、二十一个工具、142 个唯一数据类型和 75 个归并数据类型。
- 表 5 在全覆盖下报告联合模型准确率 85.1±0.4%，高于单独协同过滤和个人上下文学习。
- 图 6 显示高置信度联合预测准确率 94.4%，但只覆盖 25.9%；表 6 显示无历史时准确率 66.9±1.6%，加入一至四条历史后为 77.7±1.3%，提高 10.8 个百分点。

**局限：**

- 研究使用情境网站并在任务前强化隐私意识，不是端到端真实智能体部署。
- 论文明确把权限助手的安全实现、沙箱、可信执行和数据流执法排除在范围外。
- 自然语言歧义、个体偏好变化和非零错误授权仍要求低置信度回退与持续用户监督。

**意义：**

- 权限历史本身是需要版本、删除和跨领域隔离的个人长期记忆对象。
- 自动访问控制必须同时报告准确率、错误授权率和覆盖率，不能只用单一平均准确率宣称可部署。

**边界：** 正式接收与 DOI 由 IEEE S&amp;P 2026 页面核验；作者公开全文用于核对第 4 至第 7 节、图 1 至图 7及表 1 至表 6。结果只对应情境研究和离线预测，不等同于已执行的权限系统。

**引用：** Wu et al., IEEE S&amp;P 2026, DOI 10.1109/SP63933.2026.00018。

**版本：** 采用 IEEE S&amp;P 2026 正式版本族。

**标识：** DOI 10.1109/SP63933.2026.00018；稳定 ID doi:10.1109/sp63933.2026.00018；工作族 ID automating-data-access-permissions-ai-agents-2026

**证据位置：**

- 第 4.1.1 至第 4.1.4 节：研究网站、问题构造、八领域二十一工具和 205 人招募。
- 第 4.2 节与图 1 至图 5、表 1 至表 4：权限偏好、上下文差异和用户一致性。
- 第 5.1 至第 5.2 节、图 6 至图 7、表 5 至表 6：联合模型、置信度、覆盖率和历史量消融。
- 第 6 节的模型稳健性与局限部分以及第 7 节：歧义、执法、界面和部署未解问题。

**资源：** [一手入口](<https://doi.org/10.1109/SP63933.2026.00018>) · [PDF](<https://homes.cs.washington.edu/~franzi/pdf/wu-agentperms-sp26.pdf>) · [代码](<https://github.com/llm-platform-security/ai-agent-permissions>)

**关联 ID：** `memoryos-2025` · `text2mem-2026` · `mextra-2025`

---

<a id="paper-chronomem-2026"></a>
**20. ChronoMem：大模型智能体记忆的版本控制与语义回滚｜ChronoMem: Version Control and Semantic Rollback for Large Language Model Agent Memory（2026 · arXiv）**

**作者：** Yongye Su、Wujiang Xu、Chaoji Zuo、Elisa Bertino

**书目：** 年份 2026；载体 arXiv；状态 预印本；出版状态 preprint；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、外部检索与非参数记忆、评测、安全与治理；层级 跨会话长期；阅读层级 桥接；证据等级 C；简称 ChronoMem；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P1 / Q=Q1

**定位：** 把长期智能体记忆变成可提交、定位和原子恢复的版本状态机，并支持自然语言指定历史状态。

**问题：** 智能体已经接触后续信息后，怎样恢复此前可信的全局记忆状态，而不是仅靠提示忽略或局部删除？

**机制：** 每次写入保存全局快照和版本描述，使用词法与语义混合检索解析自然语言撤销目标，然后在用户范围内原子恢复并同步索引。

**步骤：**

1. 拦截每次长期记忆写入，保存带描述和时间信息的全局快照提交
2. 把自然语言撤销请求映射为词法和语义候选版本，并经融合与重排选择目标
3. 在每个应用与用户范围内以事务原子恢复目标快照并更新当前指针
4. 同步检索索引，再以回滚后的状态执行问答和历史概括

**证据：**

- 表2第8页：LoCoMo 版本选择 Recall@1为20.5%，混合检索基线为12.0%；Recall@5为38.9%，基线28.1%。
- 表4第8页：LoCoMo 回滚后问答F1在三个骨干上为36.1/38.5/31.3，均高于RAG-only的28.9/27.1/19.4。
- 表6第9页：LoCoMo 回滚后概括ROUGE-1为0.25/0.22/0.17，高于RAG-only的0.21/0.20/0.14。
- 正文对 MemoryAgentBench 版本选择数字与表3存在冲突，已降为Q1并禁止引用冲突数字作为主结论。

**局限：**

- PDF 使用带占位 ISBN 和 DOI 的 ACM 模板，DBLP和正式会刊均无记录，必须视为预印本。
- 表3的 MemoryAgentBench 数字与同页正文不一致，降低结果可信度。
- 当前只支持线性历史，回滚后截断后续版本，不支持分支和合并。
- 未评测高并发多用户写入，且不恢复第三方服务等外部副作用。

**意义：**

- 新增“全局记忆版本控制与语义回滚”二级分支。
- 把记忆治理从删除单条内容扩展为恢复一致历史状态的系统控制原语。

**边界：** arXiv 原始全文核验；机制不可替代但出版与数值一致性存在明确风险，故只作为桥接预印本。

**引用：** Su et al., arXiv:2607.27773, 2026；不得写成 ACM CAIS 正式论文。

**版本：** 冻结时采用 arXiv v2；PDF中的 ACM 会议信息、ISBN和DOI仍含占位符，不能视为正式版。

**标识：** arXiv DOI 10.48550/arXiv.2607.27773；稳定 ID arxiv:2607.27773；工作族 ID chronomem-2607-27773

**证据位置：**

- 图1及表1，第3页：全局回滚范围与相邻系统对比
- 第3.1至3.3节及图2，第4至5页：快照、语义版本解析与控制层
- 算法2，第6页：原子回滚及索引同步
- 第4节，第7页：后暴露评测协议与基线
- 表2至5，第8页；表6至7及局限，第9页：版本选择、行为一致性与边界

**资源：** [一手入口](<https://arxiv.org/abs/2607.27773>) · [PDF](<https://arxiv.org/pdf/2607.27773v2>)

**关联 ID：** `agent-zep-2025` · `memoryos-2025` · `text2mem-2026` · `automating-data-access-permissions-ai-agents-2026` · `agentpoison-2024`

---

<a id="paper-hippocampus-2026"></a>
**21. Hippocampus：高效可扩展的智能体记忆模块｜Hippocampus: An Efficient and Scalable Memory Module for Agentic AI（2026 · MLSys 2026）**

**作者：** Yi Li、Lianjie Cao、Faraz Ahmed、Puneet Sharma、Bingzhe Li

**书目：** 年份 2026；载体 MLSys 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 conference paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、上下文与隐状态记忆、评测、安全与治理；层级 跨会话长期；阅读层级 核心；证据等级 B；简称 Hippocampus；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 在压缩令牌域内追加记忆，以分层二进制签名作近似寻址，并在命中后精确重建原文。

**问题：** 长期智能体记忆在持续写入后面临向量存储、检索计算和送入上下文令牌三重增长。

**机制：** 双字典词法压缩和多层二进制特征索引将检索保留在压缩域，避免先解压全库。

**步骤：**

1. 以两个字典把记忆内容编码为令牌编号，同时为令牌构造固定长度二进制签名。
2. 新条目采用追加式写入；每层签名追加一位形成层次索引，不重建全部表示。
3. 查询经同一编码后在汉明球内逐层筛选候选，并按近似相似度排序。
4. 仅对入选记录执行字典反查，精确恢复原文后交给大模型作答。

**证据：**

- 摘要报告相对基线 1.1–31.5 倍加速与 1.1–14.5 倍令牌占用降低，同时保持有竞争力的准确率。
- 表 1：LoCoMo 时间问题 F1 为 38.30，MemGPT 为 26.48；开放域 F1 为 48.38。
- 表 3：汉明半径 1 时 F1 38.12、平均搜索 0.13 秒、1333 个上下文令牌；半径 5 时 F1 37.85、0.34 秒、3548 个令牌，给出明确质量—成本曲线。
- §3.4 提出墓碑与周期重建删除路径，但该部分是设计而非实证结果。

**局限：**

- 关键词抽取依赖大模型，近似签名存在碰撞与漏召回。
- 结构适合追加，任意更新和合并仍受限制。
- 删除仅以墓碑和周期重建设想呈现，未实验验证。
- 评测为 LoCoMo 和 LongMemEval，缺少真实多租户长期流量。

**意义：**

- 压缩域检索可以把存储、检索和上下文成本放进同一优化曲线。
- 删除能力不能因架构可支持就视作已被验证。

**边界：** 全文表格和删除措辞均已核验；未把未来工作写成已证实能力。

**引用：** 正式论文集无 DOI 字段；使用 MLSys 稳定哈希 URL。

**版本：** 以 MLSys 2026 正式论文集版本为准。

**标识：** 稳定 ID mlsys-2026-a1d04870cf83a0f29819d66f1dfdbfcb；工作族 ID hippocampus-2026

**证据位置：**

- §3.1–§3.3，PDF 第 5–8 页：双字典、签名、追加和重建
- §3.4，PDF 第 9 页：删除设计
- 表 1–表 3，PDF 第 10 页：精度、搜索与令牌成本

**资源：** [一手入口](<https://proceedings.mlsys.org/paper_files/paper/2026/hash/a1d04870cf83a0f29819d66f1dfdbfcb-Abstract-Conference.html>) · [PDF](<https://proceedings.mlsys.org/paper_files/paper/2026/file/a1d04870cf83a0f29819d66f1dfdbfcb-Paper-Conference.pdf>)

**关联 ID：** `agent-memgpt-2023` · `longmemeval-2025` · `locomo-2024` · `memory-management-impact-2026`

---

<a id="paper-memp-2026"></a>
**22. Memp：探索智能体程序记忆｜Memp: Exploring Agent Procedural Memory（2026 · Findings of ACL 2026）**

**作者：** Runnan Fang、Yuan Liang、Xiaobin Wang、Jialong Wu、Shuofei Qiao、Pengjun Xie、Fei Huang、Huajun Chen、Ningyu Zhang

**书目：** 年份 2026；载体 Findings of ACL 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、评测、安全与治理；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Memp；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把程序记忆拆成构建、检索、更新三阶段，直接比较追加、验证淘汰和失败后原位纠错。

**问题：** 经验型智能体常只报告最终得分，缺少对程序记忆粒度、检索和持续更新政策的系统拆解。

**机制：** 从成功与失败轨迹生成脚本或具体轨迹，按任务检索，并在每组新任务后新增、删除冗余失败项或依据反馈修改原记忆。

**步骤：**

1. 从任务轨迹构建脚本级或实例级程序记忆
2. 按任务语义检索候选程序
3. 用新任务奖励验证记忆效用
4. 选择追加、删除或依据失败反馈修改
5. 把强模型生成的记忆迁移给较弱模型复用

**证据：**

- Tables 1至2分离存储粒度和检索策略的影响。
- Figure 3比较普通追加、验证过滤和反馈调整三种在线更新政策。
- Figure 5显示强模型构建的程序记忆可迁移给较弱模型。

**局限：**

- 主要使用向量检索，未系统比较图或符号索引。
- 更新依赖基准提供的显式奖励，开放环境难以获得同等反馈。
- 只覆盖TravelPlanner和ALFWorld。

**意义：**

- 程序记忆不应是只追加档案，应支持验证、删除和原位纠错。
- 基准需要把记忆构建者与使用者分开，以测跨模型可迁移性。

**边界：** 正式论文页核验元数据与出版状态；公开全文核验机制、表图证据和局限。

**引用：** Fang等，Findings of ACL 2026，DOI 10.18653/v1/2026.findings-acl.866。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.18653/v1/2026.findings-acl.866；稳定 ID doi:10.18653/v1/2026.findings-acl.866；工作族 ID memp-2026

**证据位置：**

- Figure 2与第3.1节，印刷第17492至17494页：构建、检索、更新及新增删除修改
- 第4.2节和Tables 1至2，印刷第17494至17495页：粒度与检索
- 第4.3节和Figure 3，印刷第17495至17496页：三种更新政策
- 第5节和Figure 5，印刷第17496至17497页：跨模型迁移
- Limitations，印刷第17498页

**资源：** [一手入口](<https://aclanthology.org/2026.findings-acl.866/>) · [PDF](<https://aclanthology.org/2026.findings-acl.866.pdf>)

**关联 ID：** `agent-expel-2024` · `agent-reflexion-2023` · `agent-memory-r1-2026`

---

<a id="paper-mragent-2026"></a>
**23. 记忆是重构出来的，而不是一次检索得到的：面向大模型智能体的图记忆｜Memory is Reconstructed, Not Retrieved: Graph Memory for LLM Agents（2026 · ICML 2026）**

**作者：** Shuo Ji、Yibo Li、Bryan Hooi

**书目：** 年份 2026；载体 ICML 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、外部检索与非参数记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 MRAgent；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把长期对话写成线索、标签和内容异质图，并在推理过程中主动扩展、剪枝和重构证据。

**问题：** 固定前若干检索或预设图遍历无法根据中间证据生成新线索，如何让记忆访问随推理动态改变？

**机制：** MRAgent 以多粒度异质图保存记忆，模型根据当前重构上下文选择遍历动作，路由器控制候选并迭代至证据充足。

**步骤：**

1. 将对话蒸馏为情景、语义和主题内容，并构造线索、标签、内容异质图
2. 从查询抽取细粒度线索，初始化活跃节点与重构上下文
3. 依据当前证据选择线索到标签、线索标签到内容或内容到线索标签的遍历动作
4. 筛选和剪枝候选，更新上下文；证据不足继续迭代，充足后作答

**证据：**

- 表1第7页：LoCoMo 上 Gemini 总体判断分84.21，对最强基线68.31；Claude 为88.32，对照78.61。
- 表2第7页：LongMemEval 为72.95，对最强基线54.92；带星号的跨骨干86.76需单独解释。
- 表3第7页：118千标记为最低，但运行时间586.11秒不是最优，不作运行时最优表述。
- 图5至6第8页：推理和遍历消融以及多跳召回随迭代增加支持主动重构机制。

**局限：**

- 重构成本随遍历深度增加，图随交互单调增长。
- 当前静态构图不包含更新、合并或遗忘，不能替代完整记忆生命周期管理。
- 只在 LoCoMo、LongMemEval 和两个闭源骨干上验证。

**意义：**

- 新增“主动关联重构而非一次检索”二级分支。
- 把图记忆从静态存储结构推进为由推理中间状态控制的访问策略。

**边界：** ICML 官方海报页核验状态；arXiv 全文核验机制、表格、消融和局限。

**引用：** Ji, Li, and Hooi, ICML 2026；冻结时未解析到会议 DOI或PMLR文章页。

**DOI：** arxiv\_doi 仅标识预印本，不是 ICML 会议 DOI。

**版本：** 作品族含 arXiv v1 与 ICML 官方 Poster 60697；DBLP 冻结时仍仅有 CoRR。

**标识：** arXiv DOI 10.48550/arXiv.2606.06036；稳定 ID icml-poster:60697；工作族 ID mragent-2606-06036

**证据位置：**

- 第3.1至3.3节及图4，第3至5页：异质图与主动重构架构
- 第4.1至4.2节，第5至6页：操作空间、路由与迭代过程
- 表1至3，第7页：LoCoMo、LongMemEval与效率
- 图5至6，第8页：消融及多轮重构分析
- 第7节，第9页：成本、静态构图和部署边界

**资源：** [一手入口](<https://icml.cc/virtual/2026/poster/60697>) · [PDF](<https://arxiv.org/pdf/2606.06036v1>) · [arXiv](<https://arxiv.org/abs/2606.06036>)

**关联 ID：** `agent-amem-2025` · `agent-mem0-2025` · `memoryos-2025` · `arigraph-2025` · `associa-2025` · `does-memory-need-graphs-2026`

---

<a id="paper-promptx-2026"></a>
**24. PromptX：具备长期记忆的认知智能体平台｜PromptX: A Cognitive Agent Platform with Long-term Memory（2026 · The Web Conference 2026 Companion）**

**作者：** Binhao Wang、Jianglin Huang、Xiao Hu、Shan Jiang、Maolin Wang、Ching-Ho Yang、Jian Jiang、Junhao Ye、Yaozu Cen、Rui Zeng、Yingtong Zhou、Yingjie Luo、Guanjie Wu、Wangzhong Xu、Feiyu Zhou、Xiangyu Zhao

**书目：** 年份 2026；载体 The Web Conference 2026 Companion；状态 同行评议；出版状态 peer-reviewed；来源类型 companion demo paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、个性化与用户长期记忆、评测、安全与治理；层级 跨会话长期；阅读层级 桥接；证据等级 C；简称 PromptX；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q1

**定位：** 以 Engram 把跨会话经验编码为带模式和强度的图节点，再用激活扩散召回；正式论文仅支持机制可行性，部署效果为合作方自报。

**问题：** 平台型智能体需要在会话结束后保留经验，并在新会话中恢复相关偏好和事实。

**机制：** Remember 编码 Engram，Recall 从查询种子沿图扩散激活；PML 和 ACP 提供可移植描述与工具调用。

**步骤：**

1. Remember 把原始经验编码为含内容、模式、强度和类型的 Engram。
2. Engram 持久化到 JSON、SQLite 或 PostgreSQL，并通过关联构成记忆图。
3. 新会话的 Recall 以查询为种子进行多跳激活扩散，取回高激活 Engram。
4. 被召回的经历与角色和工具上下文合并，影响当前分析或行动。

**证据：**

- §3 的跨会话股票组合演示在新会话中召回 TSLA 与 AMZN 持仓，并把它们用于新的市场分析；这只支持机制可行性。
- §4／表 1 报告 5 个月、15 家以上企业、6 个行业、5 万下载和 3000 星标；旅游案例称成本下降 30%、收入翻倍，入职培训称从 6 个月降至 6 周。
- 上述采用量和商业结果来自作者及合作方自报，论文没有对照组、显著性检验或独立复现，不能作为通用有效性证据。

**局限：**

- 仅 4 页 Companion 演示论文，没有受控基准或系统基线。
- 企业采用和商业影响均为作者／合作方自报，未独立验证。
- 跨会话演示只证明可行性，不证明通用效果。
- 没有保留期、删除、并发冲突或多租户隔离评测。

**意义：**

- 平台部署分支应区分机制演示、采用量和受控效果三类证据。
- Engram 激活扩散是可复现的机制假设，但现阶段不应升级为核心效果结论。

**边界：** 机制由全文核验；部署数字明确标作自报，未提升证据质量。

**引用：** 四页演示论文，证据等级 C、Q1。

**版本：** 正式 WWW Companion DOI 与作者项目全文对应同一作品族。

**标识：** DOI 10.1145/3774905.3793108；稳定 ID doi:10.1145/3774905.3793108；工作族 ID promptx-2026

**证据位置：**

- §2.1 与 §2.5，PDF 第 2–3 页：Engram、Remember、Recall 和存储后端
- §3，PDF 第 3 页：跨会话演示
- §4 与表 1，PDF 第 4 页：作者／合作方自报部署

**资源：** [一手入口](<https://dl.acm.org/doi/10.1145/3774905.3793108>) · [PDF](<https://research.deepractice.ai/2026_WWW_Demo.pdf>)

**关联 ID：** `agent-mem0-2025` · `memoryos-2025` · `agent-zep-2025` · `memorybank-2024`

---

<a id="paper-text2mem-2026"></a>
**25. Text2Mem：面向记忆操作系统的统一记忆操作语言｜Text2Mem: A Unified Memory Operation Language for Memory Operating System（2026 · Findings of ACL 2026）**

**作者：** Leo Wang、Lihai Yang、Boyu Chen、Kerun Xu、Gongyi Zou、Bo Tang、Feiyu Xiong、Siheng Chen、Zhiyu Li

**书目：** 年份 2026；载体 Findings of ACL 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 智能体记忆管理；相关路线 智能体记忆管理、个性化与用户长期记忆、评测、安全与治理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Text2Mem；优先级 high；相关性排序 4；时间尺度 跨会话外部记忆的完整生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** memory-operations、delete、expire、lock、user-control、memory-os

**定位：** 把自然语言记忆指令编译为带模式、类型和不变量的可执行操作，系统覆盖更新、合并、删除、锁定与过期等生命周期动作。

**问题：** 记忆框架操作不一致，删除、合并和过期常由提示临时实现，难以确认目标、验证执行或跨后端复现。

**机制：** 定义十二类操作和类型化参数；验证器检查前置条件与不变量，解析器生成操作对象，适配器在参考后端或现有框架执行；破坏性操作要求确认或干运行。

**步骤：**

1. 把自然语言映射到十二类记忆操作
2. 校验目标、权限、锁与前置条件
3. 破坏性动作先确认或干运行
4. 经适配器执行并返回可检查状态变化

**证据：**

- 正式 PDF Table 1 列出十二类操作及现有框架覆盖差异；§4 明确设置确认、干运行和不变量检查。
- Table 2 中许多受测配置的执行成功率超过 90%，但期望匹配率显著更低，模型平均约 0.25–0.31；因此语法或执行成功不能等同用户意图正确。
- 实证主要来自 SQL 参考后端，现有框架适配以定性展示为主，尚未证明端到端智能体任务收益。

**局限：**

- 形式检查不能保证自然语言意图语义等价
- 量化验证以 SQL 参考后端为主，真实适配器证据有限
- 未证明生命周期操作提升长期任务成功或用户信任

**意义：**

- 删除、过期和锁定应是一等公民操作
- 应区分解析、执行和意图匹配
- 操作日志、确认与干运行可成为治理审计接口

**建议路线：** 记忆生命周期与层级管理

**边界：** ACL 正式页与 PDF 全文核验；作者大小写按 PDF；不把执行成功率改写为安全语义保证。

**版本：** 以 ACL Anthology 正式 Findings 版本为准。

**标识：** DOI 10.18653/v1/2026.findings-acl.100；稳定 ID doi:10.18653/v1/2026.findings-acl.100；工作族 ID text2mem-2026

**证据位置：**

- claim 十二类操作；location 正式 PDF §§2–3、Table 1，pp. 2–3；来源 PDF
- claim 确认、干运行和不变量；location 正式 PDF §4、Figure 2，p. 4；来源 PDF
- claim 执行成功与期望匹配；location 正式 PDF §5、Figure 3、Table 2，p. 7；来源 PDF
- claim 后端与端到端边界；location 正式 PDF §6、Limitations，p. 8；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2026.findings-acl.100/>) · [PDF](<https://aclanthology.org/2026.findings-acl.100.pdf>)

**关联 ID：** `agent-memgpt-2023` · `agent-memllm-2025` · `memoryos-2025` · `steem-2026`

---

<a id="paper-a12"></a>
**26. 超越固定上下文的注意语言模型｜Transformer-XL: Attentive Language Models beyond a Fixed-Length Context（2019 · ACL 2019）**

**作者：** Zihang Dai、Zhilin Yang、Yiming Yang、Jaime Carbonell、Quoc V. Le、Ruslan Salakhutdinov

**书目：** 年份 2019；载体 ACL 2019；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆；层级 会话或任务期；阅读层级 核心；证据等级 A；简称 Transformer-XL；优先级 core；相关性排序 10；时间尺度 会话内

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 以停止梯度的跨片段隐藏状态缓存和相对位置编码奠定段级循环记忆。

**问题：** 如何缓解固定片段的上下文碎裂并复用过去状态？

**机制：** 缓存上一片段各层状态，当前查询同时注意缓存和当前片段，再滑动更新。

**步骤：**

1. 缓存前一片段隐藏状态并停止梯度
2. 当前查询注意缓存与当前片段
3. 用相对位置编码处理跨片段位置
4. 滑动更新有限缓存

**证据：**

- 正式摘要报告依赖长度较 RNN 长 80%、较普通 Transformer 长 450%，评测最高加速 1,800 倍。

**局限：**

- 缓存有限且旧状态被丢弃
- 没有语义写入或跨会话持久化
- 基准与模型较早

**意义：**

- 成为压缩、循环与长时缓存工作的共同祖先

**边界：** 正式会议页、PDF 与作者代码核验。

**标识：** DOI 10.18653/v1/P19-1285；arXiv 1901.02860；稳定 ID doi:10.18653/v1/P19-1285

**证据位置：**

- claim 依赖长度与速度；source ACL Anthology formal version；location abstract

**资源：** [一手入口](<https://aclanthology.org/P19-1285/>) · [PDF](<https://aclanthology.org/P19-1285.pdf>) · [代码](<https://github.com/kimiyoung/transformer-xl>)

---

<a id="paper-a13"></a>
**27. 面向长程序列建模的压缩 Transformer｜Compressive Transformers for Long-Range Sequence Modelling（2020 · ICLR 2020）**

**作者：** Jack W. Rae、Anna Potapenko、Siddhant M. Jayakumar、Chloe Hillier、Timothy P. Lillicrap

**书目：** 年份 2020；载体 ICLR 2020；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆；层级 会话或任务期；阅读层级 核心；证据等级 A；简称 Compressive Transformer；优先级 core；相关性排序 11；时间尺度 会话内

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把将被逐出的高分辨率激活压成较长期的低分辨率记忆。

**问题：** 短期缓存满后能否压缩旧激活而非直接丢弃？

**机制：** 维护短期缓存和压缩缓存，以可学习压缩器和辅助损失写入，当前令牌同时读取两级记忆。

**步骤：**

1. 维持高分辨率短期缓存
2. 压缩被逐出激活
3. 写入低分辨率长期缓存
4. 用重建或注意匹配损失训练

**证据：**

- ICLR 摘要报告 WikiText-103 困惑度 17.1、Enwik8 每字符比特 0.97，并引入 PG-19。
- 正文说明语音实验尚未完全收敛。

**局限：**

- 压缩损失细节
- 记忆仍为固定容量
- 部分实验未收敛
- 规模较早

**意义：**

- 确立多分辨率激活记忆路线

**边界：** ICLR 正式页、OpenReview PDF 与机构项目说明核验。

**标识：** arXiv 1911.05507；稳定 ID openreview:SylKikSYDH

**证据位置：**

- claim 语言建模结果；source ICLR official page；location abstract
- claim 语音实验未收敛；source OpenReview PDF；location speech experiment discussion

**资源：** [一手入口](<https://iclr.cc/virtual/2020/poster/1602>) · [PDF](<https://openreview.net/pdf?id=SylKikSYDH>) · [项目页](<https://deepmind.google/blog/a-new-model-and-dataset-for-long-range-memory/>)

---

<a id="paper-a14"></a>
**28. 可记忆 Transformer｜Memorizing Transformers（2022 · ICLR 2022 Spotlight）**

**作者：** Yuhuai Wu、Markus N. Rabe、DeLesley Hutchins、Christian Szegedy

**书目：** 年份 2022；载体 ICLR 2022 Spotlight；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆、外部检索与非参数记忆；层级 会话或任务期；阅读层级 桥接；证据等级 A；简称 Memorizing Transformer；优先级 bridge；相关性排序 14；时间尺度 会话内

**核验：** 来源层级 T1；核验状态 abstract-checked；V/D/P/Q V=V2 / D=D2 / P=P2 / Q=Q2

**定位：** 把近期内部键值写入非可微缓存，并以近邻检索突破局部注意窗口。

**问题：** 如何从远超注意窗口的近期表示精确检索而不穿越整段历史反向传播？

**机制：** 写入近期注意键值，近似近邻读取并与局部注意融合，推理时持续追加和逐出。

**步骤：**

1. 写入近期注意键值
2. 当前查询执行近似近邻检索
3. 融合检索值与局部注意
4. 推理时追加和逐出缓存

**证据：**

- ICLR 摘要报告在五类语言/代码数据上改善，并观测记忆扩展到 262K 令牌仍有收益。

**局限：**

- 属于外部非参数缓存
- 存储与检索随历史增长
- 参数更新后旧键可能陈旧
- 缺乏跨会话治理

**意义：**

- 把内部表示记忆与近邻检索相连
- 暴露模型—记忆耦合的陈旧问题

**边界：** 正式 ICLR 页、OpenReview 与机构代码核验。

**标识：** arXiv 2203.08913；稳定 ID openreview:TrjbxzRcnf-

**证据位置：**

- claim 数据集覆盖与 262K 记忆长度；source ICLR official page；location abstract

**资源：** [一手入口](<https://iclr.cc/virtual/2022/poster/6064>) · [PDF](<https://openreview.net/pdf?id=TrjbxzRcnf->) · [项目页](<https://openreview.net/forum?id=TrjbxzRcnf->) · [代码](<https://github.com/google-research/memorizing-transformers>)

---

<a id="paper-a15"></a>
**29. 分块循环 Transformer｜Block-Recurrent Transformers（2022 · NeurIPS 2022）**

**作者：** DeLesley Hutchins、Imanol Schlag、Yuhuai Wu、Ethan Dyer、Behnam Neyshabur

**书目：** 年份 2022；载体 NeurIPS 2022；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆；层级 会话或任务期；阅读层级 核心；证据等级 A；简称 BRT；优先级 core；相关性排序 16；时间尺度 会话内

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 以固定状态向量和门控跨块循环，实现随序列长度线性增长的计算。

**问题：** 如何用固定状态获得长序列线性复杂度？

**机制：** 令牌块与固定状态通过自注意和交叉注意交互，门控更新后状态传入下一块。

**步骤：**

1. 切分令牌块
2. 维护固定状态向量
3. 执行令牌与状态内外注意
4. 门控更新并循环传递

**证据：**

- 固定门控加 skip 的块循环模型在 PG19、arXiv、GitHub 上的每词元 bit 数为 3.53、1.24、0.976，优于 XL2048 的 3.58、1.31、1.01；单步时间为 0.99 对 2.11，因此“约快两倍”只适用于该表配置。
- 1.3B 模型报告词级困惑度 26.50，但作者明确提醒跨架构比较会受参数量、词表和训练计划混杂。
- 五本书的定性分析中，20 个改善案例有 17 个涉及专有名称、19 个位于普通注意窗口之外；这支持长程检索线索，不足以证明普遍的复杂长期记忆。

**局限：**

- 实验主要是 PG19、arXiv 和 GitHub 的下一词元语言建模，未验证下游任务。
- 最佳固定门控近似指数移动平均，定性结果主要涉及专有名称，尚未显示复杂推理。
- 状态数从 1,024 增至 2,048 没有继续改善；“固定状态形成信息瓶颈”是合理推断，但不是论文直接证实的失效机制。

**意义：**

- 展示固定循环状态可替代不断增长缓存

**边界：** 正式会议页与 PDF 核验；未找到可靠作者代码入口。

**标识：** DOI 10.52202/068431-2409；arXiv 2203.07852；稳定 ID arxiv:2203.07852

**证据位置：**

- claim 块循环架构让词元流和固定数量状态流通过自注意力与交叉注意力交互，并跨块缓存门控状态。；source Block-Recurrent Transformers；location §3.1–3.4，印刷第 4–6 页；来源 PDF
- claim 固定门控加 skip 的循环模型在相近成本下优于 XL2048，单步时间约快两倍。；source Block-Recurrent Transformers；location §4，Table 1，印刷第 6–7 页；来源 PDF
- claim 1.3B 模型困惑度结果及跨架构原始困惑度比较的混杂因素。；source Block-Recurrent Transformers；location §4.5，Table 2，印刷第 9 页；来源 PDF
- claim 五本书的定性分析主要显示窗口外专有名称检索，并非复杂推理。；source Block-Recurrent Transformers；location §4.6，印刷第 9 页；§5–6，印刷第 10 页；来源 PDF

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2022/hash/d6e0bbb9fc3f4c10950052ec2359355c-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2022/file/d6e0bbb9fc3f4c10950052ec2359355c-Paper-Conference.pdf>)

---

<a id="paper-a16"></a>
**30. 循环记忆 Transformer｜Recurrent Memory Transformer（2022 · NeurIPS 2022）**

**作者：** Aydar Bulatov、Yury Kuratov、Mikhail S. Burtsev

**书目：** 年份 2022；载体 NeurIPS 2022；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆；层级 会话或任务期；阅读层级 核心；证据等级 A；简称 RMT；优先级 core；相关性排序 13；时间尺度 会话内

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 用片段首尾特殊记忆令牌把压缩状态递归传过任意多片段。

**问题：** 能否仅用标准 Transformer 和少量特殊令牌实现跨片段记忆？

**机制：** 记忆读取令牌与正文共同处理，尾部写入令牌变成下一片段的读取状态，并跨段训练。

**步骤：**

1. 片段首尾加入读写记忆令牌
2. 与正文共同执行自注意
3. 尾部记忆传给下一片段
4. 跨多个片段反向传播训练

**证据：**

- PDF 结论报告相近质量下记忆可比 Transformer-XL 小至 10 倍。
- 正文明确理论无限受实际容量与访问限制。

**局限：**

- 记忆令牌数固定
- 跨段反向传播增加训练难度
- 含大量合成任务
- 理论无限不等于实证无限

**意义：**

- 建立记忆令牌循环这一简洁架构族

**边界：** 正式版作者拼写采用 Yury；PDF 早期拼写存在差异。

**标识：** DOI 10.52202/068431-0805；arXiv 2207.06881；稳定 ID arxiv:2207.06881

**证据位置：**

- claim 结构；source NeurIPS PDF；location Figure 1 p.1; §3 and Figure 2 pp.3–4
- claim 10× 较小记忆与实际限制；source NeurIPS PDF；location conclusion p.9; introduction

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2022/hash/47e288629a6996a17ce50b90a056a0e1-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2022/file/47e288629a6996a17ce50b90a056a0e1-Paper-Conference.pdf>) · [代码](<https://github.com/booydar/recurrent-memory-transformer>)

---

<a id="paper-a17"></a>
**31. 用长期记忆增强语言模型｜Augmenting Language Models with Long-Term Memory（2023 · NeurIPS 2023）**

**作者：** Weizhi Wang、Li Dong、Hao Cheng、Xiaodong Liu、Xifeng Yan、Jianfeng Gao、Furu Wei

**书目：** 年份 2023；载体 NeurIPS 2023；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆、外部检索与非参数记忆；层级 会话或任务期；阅读层级 核心；证据等级 A；简称 LongMem；优先级 core；相关性排序 15；时间尺度 会话内

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 冻结骨干编码历史键值，以残差侧网检索融合，专门缓解长期缓存陈旧。

**问题：** 如何检索久远内部键值并避免编码空间随模型更新失配？

**机制：** 冻结骨干写入历史键值，当前查询近邻读取，由可训练 SideNet 融合局部和长期记忆。

**步骤：**

1. 冻结骨干编码历史键值
2. 写入可扩展缓存
3. 当前查询近邻检索
4. 训练残差 SideNet 融合

**证据：**

- PDF 报告 65K 测试长度、困惑度降低 1.38–1.62、ChapterBreak 40.5% 与最高 2K 演示。
- 表 6 去掉记忆使平均准确率下降 7.3 点。

**局限：**

- 依赖外部非可微缓存
- 实证主要至 65K
- 存储增长和检索错误仍存在
- 需训练 SideNet

**意义：**

- 以模型—记忆解耦处理键空间陈旧

**边界：** 正式会议页、PDF 与作者短链核验。

**标识：** DOI 10.52202/075280-3259；arXiv 2306.07174；稳定 ID doi:10.52202/075280-3259

**证据位置：**

- claim 65K 与解耦结构；source NeurIPS PDF；location abstract and Figure 1, pp.1–2
- claim 困惑度、ChapterBreak、2K demos；source NeurIPS PDF；location p.3
- claim 无记忆消融 -7.3；source NeurIPS PDF；location Table 6, p.9

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2023/hash/ebd82705f44793b6f9ade5a669d0f0bf-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2023/file/ebd82705f44793b6f9ade5a669d0f0bf-Paper-Conference.pdf>) · [项目页](<https://aka.ms/LongMem>) · [代码](<https://aka.ms/LongMem>)

---

<a id="paper-a18"></a>
**32. 迈向可自更新的大语言模型｜MEMORYLLM: Towards Self-Updatable Large Language Models（2024 · ICML 2024, PMLR 235:50453–50466）**

**作者：** Yu Wang、Yifan Gao、Xiusi Chen、Haoming Jiang、Shiyang Li、Jingfeng Yang、Qingyu Yin、Zheng Li、Xian Li、Bing Yin、Jingbo Shang、Julian McAuley

**书目：** 年份 2024；载体 ICML 2024, PMLR 235:50453–50466；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 MEMORYLLM；优先级 core；相关性排序 17；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 以每层固定潜空间令牌池反复写入新文本，并通过随机逐出实现指数式遗忘。

**问题：** 能否不改普通权重而反复更新内部潜空间记忆？

**机制：** 每层维护记忆池，新上下文与最近记忆共同更新，随机删除旧令牌并追加新输出。

**步骤：**

1. 每层维护固定记忆令牌池
2. 新上下文与最近记忆共同前向
3. 随机删除部分旧令牌
4. 追加新输出并专项训练抗遗忘

**证据：**

- §3.3 报告 Llama2-7B 增加 1.066B 记忆参数。
- 表 1 的 zsRE/CounterFact 为 79.2/75.3，对照 ROME 69.3/69.2。
- §4.4 报告 Qasper 不理想和短上下文分布偏移。

**局限：**

- 固定容量导致遗忘
- 额外记忆超过十亿参数
- 注意成本随池增长
- Qasper 与短上下文存在负面结果

**意义：**

- 参数编辑与可更新隐状态在此交汇

**边界：** PMLR 正式版全文与作者代码核验。

**标识：** arXiv 2402.04624；稳定 ID arxiv:2402.04624

**证据位置：**

- claim 更新流程；source PMLR PDF；location Figure 1 and §3.1
- claim 1.066B 记忆参数；source PMLR PDF；location §3.3
- claim 知识编辑结果；source PMLR PDF；location Table 1
- claim 负面结果；source PMLR PDF；location §4.4

**资源：** [一手入口](<https://proceedings.mlr.press/v235/wang24s.html>) · [PDF](<https://proceedings.mlr.press/v235/wang24s/wang24s.pdf>) · [代码](<https://github.com/wangyu-ustc/MemoryLLM>)

---

<a id="paper-a19"></a>
**33. 使用 Infini-attention 的高效无限上下文 Transformer｜Leave No Context Behind: Efficient Infinite Context Transformers with Infini-attention（2024 · arXiv）**

**作者：** Tsendsuren Munkhdalai、Manaal Faruqui、Siddharth Gopal

**书目：** 年份 2024；载体 arXiv；状态 预印本；出版状态 preprint；来源类型 preprint

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆；层级 会话或任务期；阅读层级 桥接；证据等级 C；简称 Infini-attention；优先级 bridge；相关性排序 20；时间尺度 会话内

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P1 / Q=Q2

**定位：** 把局部窗口外历史压入固定大小关联矩阵，以有界状态持续读写。

**问题：** 如何以固定状态和有界计算保留被局部窗口淘汰的历史？

**机制：** 局部注意处理当前片段，旧键值写入关联矩阵，当前查询读取后与局部输出门控融合。

**步骤：**

1. 执行片段内局部因果注意
2. 把旧键值压入关联矩阵
3. 从压缩矩阵读取
4. 门控融合局部与长期输出

**证据：**

- §3.1.2 式 7–10 给出读写规则。
- 表 2 报告 1.6M 记忆参数与约 114× 压缩。
- 表 3 报告 5K 微调后最高 1M 合成密钥检索。

**局限：**

- 只有预印本
- 没有作者官方代码
- 无限是结构性质而非无限实证
- 密钥检索是合成任务
- 关联压缩可能碰撞

**意义：**

- 把压缩记忆嵌入标准注意层

**边界：** arXiv 全文核验；截至检索日未确认正式同行评议版。

**标识：** arXiv 2404.07143；稳定 ID arxiv:2404.07143；工作族 ID infini-attention

**证据位置：**

- claim 读写规则；source arXiv HTML；location §3.1.2, Eq.7–10
- claim 参数与压缩率；source arXiv HTML；location Table 2
- claim 1M 合成检索；source arXiv HTML；location Table 3
- claim 500K BookSum；source arXiv HTML；location Table 4

**资源：** [一手入口](<https://arxiv.org/abs/2404.07143>) · [PDF](<https://arxiv.org/pdf/2404.07143>)

---

<a id="paper-a20"></a>
**34. 学习在测试时学习：具有表达性隐藏状态的循环网络｜Learning to (Learn at Test Time): RNNs with Expressive Hidden States（2025 · ICML 2025, PMLR 267:57503–57522）**

**作者：** Yu Sun、Xinhao Li、Karan Dalal、Jiarui Xu、Arjun Vikram、Genghan Zhang、Yann Dubois、Xinlei Chen、Xiaolong Wang、Sanmi Koyejo、Tatsunori Hashimoto、Carlos Guestrin

**书目：** 年份 2025；载体 ICML 2025, PMLR 267:57503–57522；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆；层级 会话或任务期；阅读层级 核心；证据等级 A；简称 TTT；优先级 core；相关性排序 19；时间尺度 会话内

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把循环隐藏态升级为在线梯度更新的线性层或 MLP。

**问题：** 隐藏态能否成为测试时学习的模型而不是固定向量？

**机制：** 从当前令牌构造自监督损失，对隐藏模型在线梯度更新，再用更新状态响应查询。

**步骤：**

1. 以线性层或 MLP 作为隐藏状态
2. 构造自监督损失
3. 测试时做梯度更新
4. 外层训练投影与更新行为

**证据：**

- PMLR 摘要报告 125M–1.3B 模型中，TTT 在超过 16K 后困惑度继续下降，而受测 Mamba 不再改善。
- 摘要明确 TTT-MLP 有内存 I/O 挑战。

**局限：**

- 最大实验模型 1.3B
- 主要证据为困惑度
- 测试时梯度增加算力和隔离风险
- TTT-MLP 受内存 I/O 限制

**意义：**

- 测试时学习成为记忆更新规则

**边界：** PMLR 正式页、全文及作者代码核验。

**标识：** arXiv 2407.04620；稳定 ID arxiv:2407.04620

**证据位置：**

- claim 规模、16K 后趋势与 I/O 限制；source PMLR formal version；location abstract

**资源：** [一手入口](<https://proceedings.mlr.press/v267/sun25h.html>) · [PDF](<https://raw.githubusercontent.com/mlresearch/v267/main/assets/sun25h/sun25h.pdf>) · [代码](<https://github.com/test-time-training/ttt-lm-jax>)

---

<a id="paper-a21"></a>
**35. 在测试时学习记忆｜Titans: Learning to Memorize at Test Time（2025 · NeurIPS 2025）**

**作者：** Ali Behrouz、Peilin Zhong、Vahab Mirrokni

**书目：** 年份 2025；载体 NeurIPS 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆；层级 会话或任务期；阅读层级 核心；证据等级 A；简称 Titans；优先级 core；相关性排序 21；时间尺度 会话内

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 以惊讶驱动的深层神经记忆在测试时在线更新，并统一短期注意、长期状态与持久参数。

**问题：** 如何让深层网络承担可更新长期记忆并与注意融合？

**机制：** 以关联损失梯度定义惊讶，动量累计并自适应遗忘，查询更新后 MLP，再按三种架构融合。

**步骤：**

1. 深 MLP 学习键值关联
2. 以梯度大小定义惊讶
3. 动量累计并自适应衰减
4. 读取并融合短期注意与持久参数

**证据：**

- 全文 §3.1 式 8–15 给出惊讶、动量和遗忘。
- 正式摘要报告多领域优于受测基线；作者另报告超过 2M 上下文的合成检索。

**局限：**

- 两百万级主要为合成检索
- 测试时更新有算力和状态污染风险
- 没有作者官方代码
- 未证明跨会话治理

**意义：**

- 代表测试时神经记忆的前沿汇合点

**边界：** NeurIPS 正式页、arXiv 全文与机构项目页核验。

**标识：** DOI 10.52202/085713-3786；arXiv 2501.00663；稳定 ID arxiv:2501.00663

**证据位置：**

- claim 更新与遗忘规则；source arXiv full text；location §3.1, Eq.8–15
- claim 多领域结果；source NeurIPS official proceedings；location abstract
- claim &gt;2M 合成检索；source paper full text；location long-context experiments

**资源：** [一手入口](<https://papers.neurips.cc/paper_files/paper/2025/hash/a4ca07aa108036f80cbb5b82285fd4b1-Abstract-Conference.html>) · [PDF](<https://arxiv.org/pdf/2501.00663>) · [项目页](<https://research.google/pubs/titans-learning-to-memorize-at-test-time/>)

---

<a id="paper-emllm-2025"></a>
**36. 面向无限上下文大模型的类人情景记忆｜Human-inspired Episodic Memory for Infinite Context LLMs（2025 · ICLR 2025）**

**作者：** Zafeirios Fountas、Martin A Benfeghoul、Adnan Oomerjee、Fenia Christopoulou、Gerasimos Lampouras、Haitham Bou Ammar、Jun Wang

**书目：** 年份 2025；载体 ICLR 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆、外部检索与非参数记忆；层级 会话或任务期；阅读层级 桥接；证据等级 A；简称 EM-LLM；优先级 medium；相关性排序 7；时间尺度 单任务内的超长连续上下文

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** episodic-memory、event-segmentation、infinite-context、non-graph-baseline

**定位：** 按惊奇度切分事件边界并结合相似性与时间连续性检索情景片段，为固定块 RAG 和图式记忆提供非图事件分段对照。

**问题：** 完整 KV 随序列增长，固定块检索会切断事件边界并忽略相邻情景。

**机制：** 以贝叶斯惊奇和边界细化在线形成事件块；按注意力头相似性选取事件并补入时间邻近片段，恢复相关 KV。

**步骤：**

1. 逐 token 估计预测惊奇形成候选边界
2. 细化边界得到语义连贯事件
3. 按注意力头查询相似性选择事件
4. 补入时间邻近事件并恢复相关 KV

**证据：**

- ICLR 正式摘要报告在 LongBench 与 InfiniteBench 多项任务上优于 InfLLM，并能在 1000 万 token 范围执行检索；这是论文配置中的检索范围，不是所有任务端到端处理能力。
- 正式 PDF Table 1 和 Appendix Tables 8–9 给出 InfLLM、普通 RAG与完整上下文对照，支持事件分段和时间连续性作为控制变量。
- 该工作直接在范围内，但上下文路线已有 Transformer-XL、LongMem、RMT、Titans、SUPO 等代表，因此降为 bridge。

**局限：**

- 属于单任务上下文流，不是跨日用户记忆
- 仍需保存和检索事件 KV，存储与延迟成本没有消失
- 神经科学类比不能外推为人类记忆机制证据

**意义：**

- 图记忆比较应加入事件边界与时间连续性强基线
- 评测应同时报告 KV、外部存储、延迟和返回 token
- 不能与可删除的跨会话用户记忆混写

**建议路线：** 上下文情景记忆

**边界：** ICLR 正式页和 PDF 全文核验；因路线已有多项核心代表而降 tier，不因证据不足降 V。

**版本：** 以 ICLR 2025 正式 proceedings 为准；正式题名使用 Human-inspired，作者名按正式页。

**标识：** 稳定 ID iclr:2025:c05144b635df16ac9bbf8246bbbd55ca；工作族 ID emllm-2025

**证据位置：**

- claim 事件分段与两阶段检索；location 正式 PDF §3、Figures 1–2；来源 PDF
- claim LongBench 与 InfiniteBench 主结果；location 正式 PDF §4、Table 1；来源 PDF
- claim RAG 与完整上下文对照；location 正式 PDF Appendix Tables 8–9；来源 PDF
- claim 局限；location 正式 PDF Appendix E；来源 PDF

**资源：** [一手入口](<https://proceedings.iclr.cc/paper_files/paper/2025/hash/c05144b635df16ac9bbf8246bbbd55ca-Abstract-Conference.html>) · [PDF](<https://proceedings.iclr.cc/paper_files/paper/2025/file/c05144b635df16ac9bbf8246bbbd55ca-Paper-Conference.pdf>)

**关联 ID：** `a12` · `a13` · `a17` · `ext-rag-2020` · `does-memory-need-graphs-2026`

---

<a id="paper-mplus-2025"></a>
**37. M+：以可扩展长期记忆扩展 MEMORYLLM｜M+: Extending MemoryLLM with Scalable Long-Term Memory（2025 · ICML 2025, PMLR 267:63308–63323）**

**作者：** Yu Wang、Dmitry Krotov、Yuanzhe Hu、Yifan Gao、Wangchunshu Zhou、Julian McAuley、Dan Gutfreund、Rogerio Feris、Zexue He

**书目：** 年份 2025；载体 ICML 2025, PMLR 267:63308–63323；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆、外部检索与非参数记忆；层级 会话或任务期；阅读层级 桥接；证据等级 A；简称 M+；优先级 medium；时间尺度 超长序列内，从潜空间短记忆扩展到检索式长记忆

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** latent-memory、long-term-retrieval、scalability、MemoryLLM

**定位：** 在 MEMORYLLM 的潜空间池外增加共同训练的长期存储与动态检索器，把远距保持扩展到 160K 以上。

**问题：** MEMORYLLM 拥有约十亿记忆参数却在 20K 之后明显难以保持知识，如何在近似显存开销下扩展远距记忆？

**机制：** 保留 MEMORYLLM 的潜空间记忆，同时训练检索器从额外长期记忆中按当前生成需求动态取回信息。

**步骤：**

1. 把近期内容写入潜空间记忆
2. 把更久远内容保存在长期层
3. 生成时由共同训练的检索器选取相关长时信息
4. 融合取回信息继续生成

**证据：**

- 正式论文把 MEMORYLLM 在约 20K 之后的保持困难作为起点
- 在所测保持任务和配置中，M+ 将可用范围扩展到 160K 以上
- GPU 显存开销相近不等于查询延迟或端到端成本相近

**局限：**

- 160K 保持任务不能等同于真实跨会话个体记忆
- 128K 设置的查询延迟更高，CPU 卸载没有消除数据移动与端到端成本
- 隐藏状态难以解释，最旧记忆仍会被淘汰，且缺少污染与删除治理

**意义：**

- 是 MEMORYLLM 不可跳过的正式后继与负面结果修正
- 显示潜空间记忆与外部检索会在规模化时重新融合

**边界：** ICML 2025 / PMLR 267 正式页和 PDF 全文核验；按 MEMORYLLM 的独立后续工作处理，未将相近显存外推为相近延迟或端到端成本，也不声称真实跨会话记忆。

**版本：** ICML 2025 正式后继；不是 MEMORYLLM 2024 的版本替换。

**标识：** 稳定 ID pmlr:v267:wang25au；工作族 ID mplus-2025

**证据位置：**

- claim 短期/长期隐藏向量和共同训练检索器；location 正式 PDF §3.2；来源 一手入口
- claim 160K 以上容量、质量、延迟和 CPU 卸载；location 正式 PDF §4.2–4.6、Tables 1–3、Figures 5–7；来源 一手入口
- claim 隐藏状态解释性、淘汰与部署边界；location 正式 PDF §5；来源 一手入口

**资源：** [一手入口](<https://proceedings.mlr.press/v267/wang25au.html>) · [PDF](<https://raw.githubusercontent.com/mlresearch/v267/main/assets/wang25au/wang25au.pdf>)

**关联 ID：** `a18` · `a17` · `a21`

---

<a id="paper-supo-2026"></a>
**38. 超越上下文窗口：通过端到端优化的上下文压缩扩展智能体强化学习｜Beyond the Context Window: Scaling Agentic RL via End-to-end Optimized Context Compression（2026 · ACL 2026）**

**作者：** Miao Lu、Weiwei Sun、Weihua Du、Zhan Ling、Xuesong Yao、Kang Liu、Jiecao Chen

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；来源类型 paper

**分类：** 主路线 上下文与隐状态记忆；相关路线 上下文与隐状态记忆、智能体记忆管理；层级 会话或任务期；阅读层级 核心；证据等级 A；简称 SUPO；时间尺度 单次长程工具任务中的多段会话状态

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**标签：** context-compression、summarization、tool-agent、reinforcement-learning、long-horizon

**定位：** 让工具智能体在上下文将满时把轨迹压缩成新的工作状态，并用强化学习联合优化摘要内容和后续行动。

**问题：** 固定窗口下的多轮工具轨迹如何跨越长度上限，同时让压缩策略真正服务于最终任务，而不是用独立摘要器静态截断？

**机制：** 上下文达到预算后，模型生成保留任务状态的摘要并以其作为下一段轨迹的起点；SUPO 将整条 rollout 拆为由摘要连接的子轨迹，推导可用于现有 RL 基础设施的策略梯度，并以长度遮罩和组相对优势联合训练工具行为与摘要。

**步骤：**

1. 执行多轮推理和工具调用直到工作上下文接近预算
2. 生成任务相关摘要并用摘要替换过长历史，开启下一段子轨迹
3. 把终局可验证奖励分配到由摘要连接的各段并执行 SUPO 更新
4. 在测试时重复摘要，使有效轨迹长度超过训练时的固定工作窗口

**证据：**

- Table 1 中，CodeGym 的 SUPO 在 4K 工作窗口、32K 有效长度下达到 47.7%，高于 32K GRPO 的 44.5%；BrowseComp-Plus 在 64K 工作窗口、192K 有效长度下为 53.0%，GRPO 为 39.0%。
- Table 2 的 SWE-Bench-Verified 结果中，两者同为 32K 工作窗口，SUPO 通过 320K 有效长度达到 55.0%，GRPO 为 48.0%。
- Figure 4 的消融显示去掉 overlong masking 后摘要模式会坍塌，支持联合训练机制而不只是增加推理轮数。

**局限：**

- 当前依赖可验证的终局奖励；在稠密、噪声或延迟奖励下，摘要动作的信用分配仍未解决。
- 没有训练 critic，优势估计继承 GRPO 类方法的局限。
- 摘要触发由固定长度预算决定，而不是由策略学习何时压缩；实验主要集中于代码、搜索和软件工程任务。

**意义：**

- 摘要不只是预处理器，也可视为会话内记忆写入动作并与后续读取效用端到端优化。
- 工作窗口长度和有效轨迹长度应分开报告，避免把多轮摘要误写成原生超长上下文。
- 该路线连接 Transformer 内部循环状态与 MemGPT 类上下文管理，为现代工具智能体补上训练式压缩代表。

**边界：** ACL 正式页与 52 页正式 PDF 全文核验；明确区分工作窗口与通过摘要得到的有效长度。

**版本：** 以 ACL 2026 主会长文正式版为准。

**标识：** DOI 10.18653/v1/2026.acl-long.966；工作族 ID supo

**证据位置：**

- claim 摘要连接子轨迹和策略梯度机制；location PDF 第 2–3 节；来源 PDF
- claim CodeGym 与 BrowseComp-Plus 主结果和消融；location PDF Table 1／第 4.2 节／Figure 4；来源 PDF
- claim SWE-Bench-Verified 结果；location PDF Table 2／第 4.4 节；来源 PDF
- claim 奖励和触发机制边界；location PDF Limitations and Broader Impact；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.966/>) · [PDF](<https://aclanthology.org/2026.acl-long.966.pdf>)

**关联 ID：** `agent-memgpt-2023` · `a13` · `agent-agemem-2026`

---

<a id="paper-knowledgeable-educated-guess-2021"></a>
**39. 真的有知识，还是有根据的猜测｜Knowledgeable or Educated Guess? Revisiting Language Models as Knowledge Bases（2021 · ACL-IJCNLP 2021）**

**作者：** Boxi Cao、Hongyu Lin、Xianpei Han、Le Sun、Lingyong Yan、Meng Liao、Tong Xue、Jin Xu

**书目：** 年份 2021；载体 ACL-IJCNLP 2021；状态 同行评议；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Educated Guess

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 通过控制提示偏差、类型线索和答案泄漏，重新检验‘模型即知识库’的可靠性。

**问题：** 完形提示上的高准确率是稳定知识召回，还是数据集和提示的伪线索？

**机制：** 拆解多种知识抽取范式，分别控制提示偏差、实体类型指导和外部上下文的答案泄漏。

**步骤：**

1. 复现完形知识抽取
2. 控制提示与数据集偏差
3. 分离类型指导与答案泄漏
4. 重新评估参数知识的可靠性

**证据：**

- 在 LAMA/T-REx 与 WIKI-UNI 上，P@1 分别为 30.36 和 16.47；WIKI-UNI 中模型前五名预测值覆盖 52.18% 样本，而真实答案落在高频前五的比例只有 7.78%，显示强提示先验。
- 超过一半关系中，仅由提示产生的预测分布与完整查询预测分布相关系数高于 0.6；提示先验越接近答案分布，探针表现通常越高。
- 上下文包含答案时 P@1 从 34.83 升至 64.13，不含答案时从 25.37 降至 23.26；可从输入重建的上下文提升 21.24 点，不能重建的只提升 6.99 点。

**局限：**

- 实验限于 BERT、RoBERTa 一类掩码语言模型，不能直接外推到自回归生成式大模型。
- 结论依赖 LAMA/T-REx、所选提示和检索上下文，说明探针测量受到混淆，但没有证明模型参数中完全不存在事实知识。

**意义：**

- 必须把知识存储、提示可访问性和数据偏差分开测量。

**边界：** 题名、作者、DOI 与核心主张来自 ACL Anthology 正式页和摘要。

**标识：** DOI 10.18653/v1/2021.acl-long.146

**证据位置：**

- claim LAMA 与 WIKI-UNI 的表现以及模型预测分布和答案分布的频率差异。；source Are Pretrained Language Models Knowledgeable or Just Good at Guessing?；location §3，Table 1，印刷第 3–4 页；来源 PDF
- claim 超过一半关系中，提示先验分布与完整查询预测分布的相关系数高于 0.6。；source Are Pretrained Language Models Knowledgeable or Just Good at Guessing?；location §3.3–3.4，Table 2，印刷第 4–5 页；来源 PDF
- claim 示例主要帮助对象类型识别，类型内排序近似随机。；source Are Pretrained Language Models Knowledgeable or Just Good at Guessing?；location §4，Figure 6、Table 4，印刷第 6–7 页；来源 PDF
- claim 上下文帮助很大程度来自答案复制或输入重建。；source Are Pretrained Language Models Knowledgeable or Just Good at Guessing?；location §5，Tables 5–7，印刷第 7–8 页；来源 PDF
- claim 研究只覆盖掩码语言模型，生成式模型留待未来。；source Are Pretrained Language Models Knowledgeable or Just Good at Guessing?；location §6，印刷第 8 页；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2021.acl-long.146/>) · [PDF](<https://aclanthology.org/2021.acl-long.146.pdf>)

---

<a id="paper-a09"></a>
**40. 因果定位是否能指导知识编辑｜Does Localization Inform Editing? Surprising Differences in Causality-Based Localization vs. Knowledge Editing in Language Models（2023 · NeurIPS 2023）**

**作者：** Peter Hase、Mohit Bansal、Been Kim、Asma Ghandeharioun

**书目：** 年份 2023；载体 NeurIPS 2023；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Localization vs Editing；优先级 core；相关性排序 4；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P2 / Q=Q2

**定位：** 独立检验并削弱了因果定位自然指向最佳编辑层的关键假设。

**问题：** 因果方法定位的知识层是否预测最佳编辑层？

**机制：** 逐层对照因果定位分数、编辑结果和仅依赖层身份的预测器。

**步骤：**

1. 用表征去噪等方法定位事实
2. 逐层执行可比编辑
3. 相关分析定位与编辑表现
4. 同层身份简单预测器比较

**证据：**

- 正式摘要报告表征去噪定位不能说明哪一前馈层最适合编辑；某变体虽有关联，但层身份预测更强。

**局限：**

- 不证明定位完全无分析价值
- 仅覆盖选定模型、事实任务与编辑器

**意义：**

- 定位证据不能自动升级为编辑机制证据
- 构成对 ROME 隐含桥梁的直接反证

**边界：** 正式会议页、PDF 与机构代码核验。

**标识：** DOI 10.52202/075280-0774；arXiv 2301.04213；稳定 ID arxiv:2301.04213

**证据位置：**

- claim 定位与最佳编辑层脱钩；source NeurIPS official proceedings；location abstract and full-text experiments

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2023/hash/3927bbdcf0e8d1fa8aa23c26f358a281-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2023/file/3927bbdcf0e8d1fa8aa23c26f358a281-Paper-Conference.pdf>) · [代码](<https://github.com/google/belief-localization>)

---

<a id="paper-a10"></a>
**41. 用多跳问题评测知识编辑｜MQuAKE: Assessing Knowledge Editing in Language Models via Multi-Hop Questions（2023 · EMNLP 2023）**

**作者：** Zexuan Zhong、Zhengxuan Wu、Christopher D. Manning、Christopher Potts、Danqi Chen

**书目：** 年份 2023；载体 EMNLP 2023；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改、外部检索与非参数记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 MQuAKE；优先级 core；相关性排序 5；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P2 / Q=Q2

**定位：** 把编辑评测从目标事实召回推进到需要被改事实的多跳推理。

**问题：** 直接改对一条事实后，依赖它的多跳答案是否同步改变？

**机制：** 构造会改变多跳答案的编辑，对比分步召回与多跳推理，并提供外部编辑存储 MeLLo 基线。

**步骤：**

1. 构造影响多跳答案的事实编辑
2. 分别测直接召回与多跳问答
3. 比较参数编辑器
4. 以外部编辑表和迭代提示构建 MeLLo

**证据：**

- 正式摘要报告编辑器能召回所改事实却在多跳问题上严重失效；MeLLo 可用于 175B 模型并在该基准优于受测编辑器。

**局限：**

- 多为受控反事实问答
- MeLLo 与参数编辑部署约束不同
- 不能证明参数编辑原则上不可能

**意义：**

- 目标准确率不足以证明知识更新
- 展示外部编辑记忆替代路线

**边界：** 正式会议页、PDF 和作者代码核验。

**标识：** DOI 10.18653/v1/2023.emnlp-main.971；arXiv 2305.14795；稳定 ID doi:10.18653/v1/2023.emnlp-main.971

**证据位置：**

- claim 直接召回与多跳失效差距、175B MeLLo；source ACL Anthology formal version；location abstract; benchmark sections in PDF

**资源：** [一手入口](<https://aclanthology.org/2023.emnlp-main.971/>) · [PDF](<https://aclanthology.org/2023.emnlp-main.971.pdf>) · [代码](<https://github.com/princeton-nlp/MQuAKE>)

---

<a id="paper-a11"></a>
**42. 评测知识编辑的涟漪效应｜Evaluating the Ripple Effects of Knowledge Editing in Language Models（2024 · TACL 12:283–298）**

**作者：** Roi Cohen、Eden Biran、Ori Yoran、Amir Globerson、Mor Geva

**书目：** 年份 2024；载体 TACL 12:283–298；状态 同行评议；出版状态 peer-reviewed；来源类型 journal-paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 RippleEdits；优先级 core；相关性排序 6；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P2 / Q=Q2

**定位：** 用涟漪效应基准检查蕴含、组合、关联实体和应保持知识的同步变化；摘要称 5,000 条编辑，正文三组表列合计 4,000 条。

**问题：** 编辑是否正确传播到所有相关事实并保持无关事实？

**机制：** 定义涟漪类型，构造编辑与关联问题，同时测传播、保持和上下文基线。

**步骤：**

1. 定义多类涟漪效应
2. 构建编辑及关联问题；保留摘要 5,000 与正文表列 4,000 的版本内冲突
3. 测目标、传播与保持
4. 加入上下文提供新事实的基线

**证据：**

- 基准评估逻辑泛化、组合性 I、组合性 II、主语别名、保持性和关系特异性六类两跳内涟漪，而不是五类。
- 论文摘要称包含 5K 条事实编辑，但正文 §4.2 与 Table 1 列出的 RECENT 2,000、RANDOM 1,000、POPULAR 1,000 合计为 4,000；这是论文版本内的数量不一致，atlas 应显式披露。
- 现有参数编辑方法在多类涟漪标准上的作者汇总平均约为 38–66；上下文内编辑 ICE 的整体平均最好，但并非每个模型、数据子集和指标单元格都领先。
- 评测只在方法成功写入且基础模型原本能正确回答测试问题的样本上计分，过滤集合会随模型与方法变化。

**局限：**

- 只评估编辑事实两跳以内的影响，并且只研究单条编辑。
- 构造依赖可能不完整或过时的 Wikidata，且没有纳入改写鲁棒性、主语特异性和更远的无关事实。
- 摘要的 5K 与正文表格的 4,000 不一致，在作者勘误前不应把任一数字写成无争议事实。

**意义：**

- 把语义一致性纳入编辑成功标准

**边界：** TACL 正式版核验；未找到可靠作者代码入口。

**标识：** DOI 10.1162/tacl\_a\_00644；稳定 ID doi:10.1162/tacl\_a\_00644

**证据位置：**

- claim 基准定义六类两跳内涟漪标准。；source Evaluating the Ripple Effects of Knowledge Editing；location §3.1，印刷第 3–4 页；来源 PDF
- claim 摘要称 5K 编辑，但正文 RECENT、RANDOM、POPULAR 三组数量合计为 4,000。；source Evaluating the Ripple Effects of Knowledge Editing；location 摘要，印刷第 1 页；§4.2、Table 1，印刷第 7 页；来源 PDF
- claim 评测只保留编辑成功且基础模型原本能正确回答相应测试问题的样本。；source Evaluating the Ripple Effects of Knowledge Editing；location §5.1、Table 2，印刷第 8–9 页；来源 PDF
- claim 参数编辑方法的涟漪表现有限，ICE 整体平均最好但并非逐项胜出。；source Evaluating the Ripple Effects of Knowledge Editing；location §5.2，Tables 3–6，印刷第 9–10 页；来源 PDF
- claim 基准限于两跳、单条编辑和 Wikidata，且未测改写、主语特异性和远距离无关事实。；source Evaluating the Ripple Effects of Knowledge Editing；location §6 与 Limitations，印刷第 11–12 页；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2024.tacl-1.16/>) · [PDF](<https://aclanthology.org/2024.tacl-1.16.pdf>)

---

<a id="paper-a22"></a>
**43. 迷失在中间：语言模型如何使用长上下文｜Lost in the Middle: How Language Models Use Long Contexts（2024 · TACL 12:157–173）**

**作者：** Nelson F. Liu、Kevin Lin、John Hewitt、Ashwin Paranjape、Michele Bevilacqua、Fabio Petroni、Percy Liang

**书目：** 年份 2024；载体 TACL 12:157–173；状态 同行评议；出版状态 peer-reviewed；来源类型 journal-paper

**分类：** 主路线 评测、安全与治理；相关路线 上下文与隐状态记忆、评测、安全与治理；层级 即时或单轮；阅读层级 核心；证据等级 A；简称 Lost in the Middle；优先级 core；相关性排序 22；时间尺度 即时或单轮

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P2 / Q=Q2

**定位：** 证明窗口能容纳信息不等于模型能均匀读取，中间位置表现呈系统性低谷。

**问题：** 长上下文模型能否不受位置影响地使用相关证据？

**机制：** 在多文档问答和键值检索中控制证据位置，扫描长度并与闭卷及多模型对照。

**步骤：**

1. 控制关键信息位置
2. 扫描上下文长度与文档数
3. 比较开头中间结尾
4. 加入闭卷和不同模型对照

**证据：**

- 图 1 与 §2.3 展示 U 形位置效应。
- 第 158 页 GPT-3.5 中间位置可低于 56.1% 闭卷基线。
- 第 159 页 20 到 50 文档只增约 1.5/1 点，图 15 的 GPT-4 仍有 U 形。

**局限：**

- 模型快照主要来自 2023
- 任务是受控问答与键值检索
- 不能区分存储、检索、注意或推理瓶颈

**意义：**

- 长窗口长度不能直接当作记忆质量

**边界：** TACL 正式版全文和作者代码核验。

**标识：** DOI 10.1162/tacl\_a\_00638；arXiv 2307.03172；稳定 ID doi:10.1162/tacl\_a\_00638

**证据位置：**

- claim U 形位置效应；source TACL PDF；location Figure 1 and §2.3
- claim 低于 56.1% 闭卷基线；source TACL PDF；location p.158
- claim 文档数边际增益；source TACL PDF；location p.159
- claim GPT-4 位置效应；source TACL PDF；location Figure 15

**资源：** [一手入口](<https://aclanthology.org/2024.tacl-1.9/>) · [PDF](<https://aclanthology.org/2024.tacl-1.9.pdf>) · [代码](<https://github.com/nelson-liu/lost-in-the-middle>)

---

<a id="paper-agentpoison-2024"></a>
**44. AgentPoison：通过记忆或知识库污染对 LLM 智能体进行安全评测｜AgentPoison: Red-teaming LLM Agents via Poisoning Memory or Knowledge Bases（2024 · NeurIPS 2024）**

**作者：** Zhaorun Chen、Zhen Xiang、Chaowei Xiao、Dawn Song、Bo Li

**书目：** 年份 2024；载体 NeurIPS 2024；状态 同行评议；来源类型 security-evaluation

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、智能体记忆管理、外部检索与非参数记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 AgentPoison；优先级 1；相关性排序 15

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 证明未经验证的持久记忆或知识库可成为长期完整性风险面。

**问题：** 智能体把历史经验或外部知识当作可信记忆，一旦存储内容被篡改，影响可能在后续任务中持续。

**机制：** 在可向长期记忆或知识库写入少量记录的威胁模型下，受约束优化使异常记录更可能被特定查询检索，同时尽量维持普通查询的正常检索与效用；不修改模型权重。

**步骤：**

1. 定义持久记忆完整性威胁
2. 在受控环境评测多类智能体
3. 比较目标行为变化与正常效用
4. 分析持久化影响
5. 提出写入验证与隔离需求

**证据：**

- 表 1 与第 4.2 节在自动驾驶、问答和电子健康记录三类代理上分别测量检索成功、目标动作、端到端影响和普通任务效用；正文汇总报告平均检索成功率 81.2%、端到端影响率 62.6%，普通任务性能平均变化 0.74%。
- 表 4 和附录表 6显示两类简单输入防护未消除受控实验中的风险，但论文没有评测完整的纵深防御体系。

**局限：**

- 第 3.2 节假设能够向记忆或知识库注入少量记录；作者限制段还明确要求优化时对白盒嵌入器有访问权。
- 跨嵌入器迁移是经验结果，依赖嵌入空间与训练分布的相似性，不能消除威胁模型前提。
- 三个受控代理不能给出生产系统中的事件发生率，论文也没有提供完整防御方案。

**意义：**

- 持久记忆需要来源追踪、写入授权、内容验证、租户隔离、监控和回滚。
- 仅看正常任务平均分不足以发现记忆完整性问题。

**边界：** 已核验 NeurIPS 正式全文；安全内容经过降敏，只保留威胁前提、分项结果、限制和防护含义。

**标识：** DOI 10.52202/079017-4136

**证据位置：**

- claim 威胁模型要求部分数据库写入与白盒嵌入器访问；location 第 3.2 节 Threat model；Limitations；来源 PDF
- claim 三类代理上的分项结果与汇总结果；location 表 1；第 4.1 至 4.2 节；来源 PDF
- claim 简单输入防护的有限效果；location 表 4；附录表 6；来源 PDF

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2024/hash/eb113910e9c3f6242541c1652e30dfd6-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2024/file/eb113910e9c3f6242541c1652e30dfd6-Paper-Conference.pdf>)

---

<a id="paper-beyond-prompts-2024"></a>
**45. 超越静态提示：大语言模型动态对话评测｜Beyond Prompts: Dynamic Conversational Benchmarking of Large Language Models（2024 · NeurIPS 2024 Datasets and Benchmarks Track）**

**作者：** David Castillo-Bolado、Joseph Davidson、Finlay Gray、Marek Rosa

**书目：** 年份 2024；载体 NeurIPS 2024 Datasets and Benchmarks Track；状态 同行评议；出版状态 peer-reviewed；来源类型 benchmark-paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、上下文与隐状态记忆、智能体记忆管理；层级 会话或任务期；阅读层级 核心；证据等级 A；简称 Beyond Prompts；优先级 high；时间尺度 单个超长、任务交错的模拟用户—智能体会话

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** dynamic-benchmark、context-switching、interleaving、failure-analysis

**定位：** 把多个任务在一段长对话中交错并反复切换上下文，暴露静态单任务评测看不到的长期记忆失败。

**问题：** 模型在独立任务上表现良好，是否也能在更自然的并发长对话中维持记忆、持续学习和信息整合？

**机制：** 基准用一个模拟用户与智能体长对话逐步引入多个任务，并周期性切换，使模型必须跨任务保存和恢复状态。

**步骤：**

1. 在长对话中引入多个任务
2. 交错推进并定期切换上下文
3. 记录各任务状态与答案
4. 比较单任务、交错任务、长上下文与外接长期记忆

**证据：**

- 动态协议将 11 类场景交错，并在每轮安排 33 个测试
- 多数受测配置在交错和长历史下退化，但并非每一模型与配置都下降
- 部分长期记忆配置优于部分长上下文配置，结论不能外推为普遍架构优势

**局限：**

- 每个场景只重复三次，作者明确承认重复数不足以形成稳定估计
- 自动评分在边缘案例可能失败，且模拟交错不等于真实长期用户部署
- MemoryBank 与 MemGPT 的检索内容会膨胀上下文，不能统一称为短上下文方案
- 评测混合记忆、任务切换与持续学习，单一失败不能完全归因于记忆

**意义：**

- 证明记忆评测必须包含中断、恢复和任务交错
- 为‘长上下文即可替代记忆’提供直接反证

**边界：** NeurIPS 2024 Datasets and Benchmarks 正式页和 PDF 全文核验；动态退化结论限定于所测模型、基线和上下文配置，并保留三次重复、自动评分及检索上下文膨胀局限。

**标识：** DOI 10.52202/079017-1347；稳定 ID doi:10.52202/079017-1347

**证据位置：**

- claim 动态交错协议和 11 个情景；location 正式 PDF §3.1–3.2；来源 PDF
- claim bare LLM、MemoryBank、MemGPT 基线及不同上下文长度；location 正式 PDF §3.4、§4、Table 1；来源 PDF
- claim 重复次数和自动评分局限；location 正式 PDF §7；来源 PDF

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2024/hash/4aedf0cba303537fcb6cf948bb41b2df-Abstract-Datasets_and_Benchmarks_Track.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2024/file/4aedf0cba303537fcb6cf948bb41b2df-Paper-Datasets_and_Benchmarks_Track.pdf>)

**关联 ID：** `longmemeval-2025` · `locomo-2024` · `context-length-alone-hurts-2025`

---

<a id="paper-can-sensitive-information-be-deleted-2024"></a>
**46. 敏感信息能否从大语言模型中删除：面向提取攻击的防御目标｜Can Sensitive Information Be Deleted From LLMs? Objectives for Defending Against Extraction Attacks（2024 · ICLR 2024）**

**作者：** Vaidehi Ramesh Patil、Peter Hase、Mohit Bansal

**书目：** 年份 2024；载体 ICLR 2024；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Sensitive Deletion Audit；优先级 high；时间尺度 模型发布后的删除验证

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P2 / Q=Q2

**标签：** sensitive-deletion、extraction-attack、hidden-state、rephrasing

**定位：** 不以模型拒答作为删除成功，而是用隐藏状态、概率差和输入改写主动恢复被删答案，并检验防御能否跨攻击成立。

**问题：** 模型编辑可让目标提示输出空响应，却可能只把知识藏到中间表示或特定问法之外。

**机制：** 先用 ROME 或 MEMIT及不同删除目标编辑模型，再在白盒和黑盒访问下生成最多 B 个候选；只要被删答案仍在候选集合中即记为攻击成功，并同时检查随机知识和邻域知识损伤。

**步骤：**

1. 用事实擦除、空响应或防御目标编辑模型，使直接查询不再给出目标答案
2. 在中间隐藏状态上做头部投影或比较编辑前后概率差，产生白盒候选
3. 自动改写输入并采样输出，产生黑盒候选
4. 按预算 B 统计恢复成功率，并联合报告重写分数、随机与邻域准确率变化

**证据：**

- PDF Figure 4 中，预算 B=20 时恢复攻击成功率最高约38%；黑盒改写最高约29%，B=1 的概率差攻击也约18%。
- PDF Table 1 中，MEMIT 加空响应目标在 zsRE 上被头部投影攻击恢复的成功率为88.55%。
- 最大熵防御可把 ROME 的白盒攻击成功率降至约2.4%，但黑盒改写仍在 CounterFact 为约28%、zsRE 为约19%。
- 输入改写防御没有泛化到未见改写；更激进编辑虽能降低攻击，却使随机与邻域知识损伤大幅上升。

**局限：**

- 主要实验是 GPT-J、ROME、MEMIT 及 CounterFact、zsRE 的单 token 事实，不能代表所有模型和敏感内容。
- 预算型候选命中是特定威胁模型，不等同于所有现实攻击概率。
- 没有一种防御同时挡住全部白盒和黑盒攻击，且更强编辑会损害邻域知识。

**意义：**

- 删除验收应从输出拒答升级为跨层、跨改写的可提取性测试。
- MUSE 等综合评测应补充本工作的隐藏状态与改写恢复协议。
- 任何产品删除声明都应公开攻击预算、模型访问级别和非目标知识损伤。

**边界：** 正式 ICLR 状态、攻击定义、表格数值和结论由 ICLR Proceedings 与全文核验；安全细节仅保留审计所需的降敏概述。

**标识：** 稳定 ID iclr-2024-c652e5f62fd1f5acbbf0d6413a1113e7；工作族 ID sensitive-information-deletion

**证据位置：**

- claim 威胁模型与成功指标；location PDF 第3节、Figure 1；来源 PDF
- claim 三类恢复攻击；location PDF 第4节、Figure 2；来源 PDF
- claim 攻击预算与防御结果；location PDF 第7.1–7.3节、Figure 4、Table 1；来源 PDF
- claim 结论与边界；location PDF 第8节；来源 PDF

**资源：** [一手入口](<https://proceedings.iclr.cc/paper_files/paper/2024/hash/c652e5f62fd1f5acbbf0d6413a1113e7-Abstract-Conference.html>) · [PDF](<https://proceedings.iclr.cc/paper_files/paper/2024/hash/c652e5f62fd1f5acbbf0d6413a1113e7-Paper-Conference.pdf>)

**关联 ID：** `muse-2025` · `unlearning-or-obfuscating-2025` · `a05` · `a06` · `larimar-2024`

---

<a id="paper-conflictbank-2024"></a>
**47. ConflictBank：评测知识冲突影响的大语言模型基准｜ConflictBank: A Benchmark for Evaluating the Influence of Knowledge Conflicts in LLMs（2024 · NeurIPS 2024 Datasets and Benchmarks Track）**

**作者：** Zhaochen Su、Jun Zhang、Xiaoye Qu、Tong Zhu、Yanshu Li、Jiashuo Sun、Juntao Li、Min Zhang、Yu Cheng

**书目：** 年份 2024；载体 NeurIPS 2024 Datasets and Benchmarks Track；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改、外部检索与非参数记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 ConflictBank；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 用错误信息、时间差异和语义分歧三类大规模数据，联合测参数知识与外部证据的冲突。

**问题：** 现有冲突评测通常只看模型内部知识与一段检索文本，无法分解冲突来源和二者交互。

**机制：** 从知识图谱事实生成三类冲突证据，经过过滤和确认后构造问答，并在外部检索、内部编码及二者交互三种设置下评测。

**步骤：**

1. 从知识图谱提取事实并生成三类冲突主张
2. 生成多样证据并经分类与一致性筛选
3. 合成问答对并控制内部与外部冲突比例
4. 跨模型比较记忆、原答案和反答案倾向

**证据：**

- 数据包含约745万条主张证据对和55.3万问答对，覆盖三种冲突原因。
- Figures 3至6显示外部证据冲突与参数冲突对模型产生不同影响。
- Figure 7和第3.4节显示两种冲突同时出现时不是简单叠加。

**局限：**

- 大量文本由模型生成并依赖自动过滤。
- 基准主要是事实问答，不覆盖长期用户偏好或程序记忆。
- 冲突暴露后的行为不等同于系统已完成持久更新。

**意义：**

- 冲突治理应分开记录参数状态、外部库状态和证据来源。
- 单一准确率不足以诊断模型是在坚持旧记忆还是盲从外部文本。

**边界：** 正式论文页核验元数据与出版状态；公开全文核验机制、表图证据和局限。

**引用：** Su等，NeurIPS 2024 Datasets and Benchmarks Track，DOI 10.52202/079017-3280。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.52202/079017-3280；稳定 ID doi:10.52202/079017-3280；工作族 ID conflictbank-2024

**证据位置：**

- Figure 1与第2节，PDF第3至5页：构造流程
- Figures 3至8与第3节，PDF第5至10页：三种实验设置
- Table 3，PDF第18至19页：各阶段数据量和成本

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2024/hash/baf4b960d118f838ad0b2c08247a9ebe-Abstract-Datasets_and_Benchmarks_Track.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2024/file/baf4b960d118f838ad0b2c08247a9ebe-Paper-Datasets_and_Benchmarks_Track.pdf>)

**关联 ID：** `taming-knowledge-conflicts-2025` · `rag-privacy-good-bad-2024` · `poisonedrag-2025`

---

<a id="paper-locomo-2024"></a>
**48. 评估 LLM 智能体的超长期对话记忆｜Evaluating Very Long-Term Conversational Memory of LLM Agents（2024 · ACL 2024）**

**作者：** Adyasha Maharana、Dong-Ho Lee、Sergey Tulyakov、Mohit Bansal、Francesco Barbieri、Yuwei Fang

**书目：** 年份 2024；载体 ACL 2024；状态 同行评议；来源类型 benchmark+dataset

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 LoCoMo；优先级 1；相关性排序 5

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V4 / D=D3 / P=P3 / Q=Q3

**定位：** 以约六百轮、最多三十二个会话的长对话评测问答、事件摘要和多模态对话生成。

**问题：** 短会话基准不能衡量跨多次会话的时间、因果、事件和人物记忆。

**机制：** 用人机协作流程生成并审核超长对话；从对话构造单跳、多跳、时间和开放域问答，以及事件摘要与生成任务。

**步骤：**

1. 生成跨多会话人物互动
2. 人工审核和修订
3. 构造不同推理类型的问题
4. 比较长上下文、检索与摘要记忆
5. 以自动和人工指标评价

**证据：**

- 10 段对话平均约 600 轮、约 16K tokens，最长 32 个会话。
- 论文 Table 3 中 GPT-4-Turbo 的问答 F1 为 51.6，人类为 87.9。
- 增加检索条目不总是更好；摘要会损失细节，长上下文在时间和对抗问题上仍明显退化。

**局限：**

- 对话主要由 LLM 生成再经人工编辑，不等同自然发生的长期关系。
- 网络图片缺少同一人物跨时间的视觉一致性。
- 样本只有 10 段对话，系统差异可能被特定故事结构放大。

**意义：**

- 把长期对话记忆从展示案例推进到可分能力比较的标准基准。
- 直接反驳“更长上下文或更多检索必然解决记忆”的简单假设。

**边界：** ACL 正式页和全文深读；数字均定位到表格或局限章节。

**标识：** DOI 10.18653/v1/2024.acl-long.747

**证据位置：**

- claim 数据规模和任务组成；location PDF §2、Table 1；来源 PDF
- claim 人类与 GPT-4-Turbo 差距、检索数量和摘要损失；location PDF Table 3；§4.2–§4.3；来源 PDF
- claim 合成对话和图像一致性限制；location PDF Limitations；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2024.acl-long.747/>) · [PDF](<https://aclanthology.org/2024.acl-long.747.pdf>)

---

<a id="paper-model-editing-harms-2024"></a>
**49. 模型编辑会损害大语言模型的一般能力：用正则化缓解｜Model Editing Harms General Abilities of Large Language Models: Regularization to the Rescue（2024 · EMNLP 2024）**

**作者：** Jia-Chen Gu、Hao-Xiang Xu、Jun-Yu Ma、Pan Lu、Zhen-Hua Ling、Kai-Wei Chang、Nanyun Peng

**书目：** 年份 2024；载体 EMNLP 2024；状态 同行评议；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Editing Harms；优先级 1；相关性排序 21

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 跨四种编辑方法、三种 LLM 和八类任务显示，局部知识修改可损害推理、自然语言推断和问答等一般能力。

**问题：** 知识编辑常只报告目标事实成功率和局部性，可能忽略模型整体能力旁损。

**机制：** 在多次编辑前后用通用任务套件评测模型，并提出正则化方案 RECT 缓解能力退化。

**步骤：**

1. 对模型执行知识编辑
2. 检查目标事实成功
3. 在八类通用任务复测
4. 量化一般能力损失
5. 用正则化约束编辑更新

**证据：**

- 论文比较 KN、MEND、ROME、MEMIT 在 GPT-2 XL、LLaMA-1 7B、LLaMA-2 7B 上的影响，并以八类零样本任务评估推理、自然语言推断、问答、对话、摘要、命名实体识别和情感分析等能力。
- 随顺序编辑次数或批量规模增加，多项通用能力总体下降；KN 对 LLaMA-1 的单次编辑近乎使所有任务归零是一个特定灾难性案例，损伤强度并不在所有方法、模型和任务间一致。
- RECT 在仅保留相对权重变化最大的前 40 个更新元素时，编辑可靠性和泛化性可保持原方法的 94% 以上，并在多数下游任务中改善能力保持；94% 不代表所有通用能力指标的统一保留率。

**局限：**

- 实验只使用 ZsRE，尚不清楚更大且更多样的编辑集合会产生何种副作用。
- RECT 在更大编辑规模上的扩展性尚未验证，情感分析等任务仍有明显困难。
- 权重相对变化与能力损害相伴支持作者的过拟合解释，但未建立适用于所有模型编辑方法的单一因果定律；论文也未解决理论瓶颈。

**意义：**

- 知识修改安全不能只看编辑命中与邻域局部性。
- 长期部署应设置编辑前后通用能力回归测试与回滚门槛。

**边界：** ACL Anthology 正式页核验；保留模型编辑作为参数记忆治理的桥接边界。

**标识：** DOI 10.18653/v1/2024.emnlp-main.934

**证据位置：**

- claim 四种编辑方法、三个模型、ZsRE 和八类零样本下游任务的实验设置。；source Model Editing Harms General Abilities of Large Language Models: Regularization to the Rescue；location §4.1–4.2，印刷第 4–5 页；来源 PDF
- claim 通用能力随编辑次数或批量总体下降，但损伤随方法、模型和任务变化。；source Model Editing Harms General Abilities of Large Language Models: Regularization to the Rescue；location §4.3，Figures 3–4，印刷第 5–6 页；来源 PDF
- claim 权重相对变化随编辑扩大，并与下游副作用相伴。；source Model Editing Harms General Abilities of Large Language Models: Regularization to the Rescue；location §4.4，Table 1、Figure 5，印刷第 6–7 页；来源 PDF
- claim RECT 保留相对变化最大的更新元素，并在 top-40 时保留超过 94% 的可靠性和泛化性。；source Model Editing Harms General Abilities of Large Language Models: Regularization to the Rescue；location §5.1，Eq. (3)、Figure 6，印刷第 7–8 页；§5.3，Figures 7–8，印刷第 8–9 页；来源 PDF
- claim 数据集、编辑规模、扩展性和理论解释的局限。；source Model Editing Harms General Abilities of Large Language Models: Regularization to the Rescue；location Limitations，印刷第 9 页；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2024.emnlp-main.934/>) · [PDF](<https://aclanthology.org/2024.emnlp-main.934.pdf>)

---

<a id="paper-rag-privacy-good-bad-2024"></a>
**50. 好与坏：检索增强生成中的隐私问题｜The Good and The Bad: Exploring Privacy Issues in Retrieval-Augmented Generation (RAG)（2024 · Findings of ACL 2024）**

**作者：** Shenglai Zeng、Jiankun Zhang、Pengfei He、Yue Xing、Yiding Liu、Han Xu、Jie Ren、Shuaiqiang Wang、Dawei Yin、Yi Chang、Jiliang Tang

**书目：** 年份 2024；载体 Findings of ACL 2024；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、外部检索与非参数记忆、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 RAG Privacy；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 揭示 RAG 的双重隐私效应：外部检索库可能泄露，但提供检索上下文也可能降低模型复现训练数据的倾向。

**问题：** 把外部数据库作为非参数记忆后，隐私风险不再只来自模型参数；检索库和模型训练数据可能呈现相反的泄露变化。

**机制：** 论文在黑盒条件下分别测量检索库内容复现和模型训练数据复现，再改变检索阈值、重排与摘要等防护，观察泄露与任务效用的权衡。

**步骤：**

1. 构建含敏感内容的检索库并用多种语言模型形成 RAG 系统。
2. 以受控探测分别测量外部检索内容和模型训练内容的复现；本地图不保存可执行探测提示。
3. 改变检索与后处理策略，比较泄露指标和任务性能的共同变化。

**证据：**

- 表 1 的 250 次受控探测中，Enron 检索库与 GPT-3.5-turbo 组合出现 116 个逐字复现提示、122 个唯一逐字片段，并有 121 个高相似输出提示；这说明检索库本身可成为泄露源，但数值不应外推到其他部署。
- 表 3 在 GPT-Neo-1.3B 与指定数据上报告：无检索时 1000 个前缀探测有 213 次训练文本重构；加入三种检索数据后分别为 34、70、33，支持检索上下文会改变参数记忆复现倾向。
- 图 4–5 显示重排、摘要和检索阈值可降低部分泄露，但与回答性能存在权衡，不构成形式化隐私保证。

**局限：**

- 检索库实验只覆盖 Enron 与 HealthcareMagic，生成模型和嵌入器范围有限。
- 训练数据记忆部分主要使用 GPT-Neo-1.3B，结果不能直接推广到闭源或更大模型。
- 研究聚焦推理时 RAG；防护是经验性缓解，不提供差分隐私或访问控制保证。

**意义：**

- 外部记忆把隐私边界从模型权重扩展到索引、检索结果和生成上下文。
- 评估删除或保密不能只测参数记忆，也要分别审计检索库泄露与训练数据复现。

**边界：** 一手正式入口为 ACL Anthology；已核对威胁模型、表 1–3、图 4–5、结论和局限。为降低滥用风险，本记录只保留评测结构和结果，不转录攻击命令或提示模板。

**引用：** Zeng et al., Findings of ACL 2024, DOI 10.18653/v1/2024.findings-acl.267。

**版本：** 采用 Findings of ACL 2024 正式版本。

**标识：** DOI 10.18653/v1/2024.findings-acl.267；稳定 ID doi:10.18653/v1/2024.findings-acl.267；工作族 ID rag-privacy-good-bad-2024

**证据位置：**

- 第 3–4 节与表 1–2，正式页码 4507–4509：检索库泄露评测与主结果。
- 图 4–5，正式页码 4510–4511：经验性防护和隐私—效用权衡。
- 第 5 节与表 3，正式页码 4511–4512：训练数据复现随检索上下文变化。
- 第 7 节，正式页码 4513：推理阶段、数据集和架构范围限制。

**资源：** [一手入口](<https://aclanthology.org/2024.findings-acl.267/>) · [PDF](<https://aclanthology.org/2024.findings-acl.267.pdf>)

**关联 ID：** `poisonedrag-2025` · `agentpoison-2024` · `mextra-2025` · `ext-rag-2020`

---

<a id="paper-streambench-2024"></a>
**51. StreamBench：语言智能体持续改进基准｜StreamBench: Towards Benchmarking Continuous Improvement of Language Agents（2024 · NeurIPS 2024 Datasets and Benchmarks Track）**

**作者：** Cheng-Kuang Wu、Zhi Rui Tam、Chieh-Yen Lin、Yun-Nung Chen、Hung-yi Lee

**书目：** 年份 2024；载体 NeurIPS 2024 Datasets and Benchmarks Track；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、智能体记忆管理；层级 跨会话长期；阅读层级 桥接；证据等级 A；简称 StreamBench；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把任务、预测、反馈和更新排成严格数据流，统一测代理部署后能否持续变好。

**问题：** 静态基准无法衡量智能体在连续输入和反馈下是否更新提示、检索器、外部记忆或参数。

**机制：** 每步只暴露当前任务和二元反馈；系统可更新提示、检索器、记忆或参数，再在七类任务上计算随时间变化的表现。

**步骤：**

1. 把静态数据随机串行化为任务流
2. 代理预测后获得二元正确性反馈
3. 允许逐步更新提示、检索器、外部记忆或参数
4. 以在线曲线和最终表现比较持续改进

**证据：**

- Algorithm 1定义了严格输入反馈协议，避免未来样本泄漏。
- Table 1覆盖Text-to-SQL、代码、工具、医疗和问答等七个数据集。
- Table 2显示不同流式基线在任务间差异大，连续改进不是静态准确率的简单延伸。

**局限：**

- 数据流由静态数据随机串行化，不是真实长期部署。
- 反馈是模拟二元信号，缺少开放式人类纠正。
- 协议不专属于记忆，改进也可来自参数或提示更新。

**意义：**

- 新增跨任务连续改进协议桥接节点，但不把它误标为专用记忆基准。
- 记忆系统应报告在线学习曲线而非只报告冻结测试集结果。

**边界：** 正式论文页核验元数据与出版状态；公开全文核验机制、表图证据和局限。

**引用：** Wu等，NeurIPS 2024 Datasets and Benchmarks Track，DOI 10.52202/079017-3398。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.52202/079017-3398；稳定 ID doi:10.52202/079017-3398；工作族 ID streambench-2024

**证据位置：**

- Figure 1、第2节和Algorithm 1，PDF第2至3页：输入反馈流
- 第3.1节和Table 1，PDF第3至4页：七数据集与指标
- 第4.1节、Algorithm 2和Table 2，PDF第4至6页：流式基线和共享记忆
- 第8节，PDF第9页：限制

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2024/hash/c189915371c4474fe9789be3728113fc-Abstract-Datasets_and_Benchmarks_Track.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2024/file/c189915371c4474fe9789be3728113fc-Paper-Datasets_and_Benchmarks_Track.pdf>)

**关联 ID：** `memoryagentbench-2026` · `agent-expel-2024` · `agent-memory-r1-2026`

---

<a id="paper-big-help-big-brother-2025"></a>
**52. 大帮助还是老大哥：生成式人工智能助手的跟踪、画像与个性化审计｜Big Help or Big Brother? Auditing Tracking, Profiling, and Personalization in Generative AI Assistants（2025 · 34th USENIX Security Symposium (USENIX Security 25)）**

**作者：** Yash Vekaria、Aurelio Loris Canino、Jonathan Levitsky、Alex Ciechonski、Patricia Callejo、Anna Maria Mandalari、Zubair Shafiq

**书目：** 年份 2025；载体 34th USENIX Security Symposium (USENIX Security 25)；状态 同行评议；出版状态 peer-reviewed；来源类型 deployment-audit

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆、智能体记忆管理；层级 跨会话长期；阅读层级 桥接；证据等级 B；简称 Big Help or Big Brother?；优先级 medium；相关性排序 10；时间尺度 真实浏览器助手的页面、标签页与部分跨会话状态

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D1 / P=P3 / Q=Q2

**标签：** deployment-audit、privacy、profiling、personalization、persistent-history、browser-agent

**定位：** 对九个已部署浏览器助手做网络与行为审计，提供页面采集、历史持久化、画像和跨上下文个性化的真实部署边界证据。

**问题：** 用户难以知道助手读取什么、保存多久、发送给谁，以及状态是否用于跨上下文画像和个性化。

**机制：** 研究以受控账户、合成敏感页面、网络流量和重复提示实验审计九个扩展的触发、传输、历史、画像与个性化行为；它是系统审计而非新记忆算法。

**步骤：**

1. 在受控页面放置非真实敏感字段
2. 记录触发、端点与上传内容
3. 检查历史与上下文是否跨标签页或会话保留
4. 重复测试属性画像与个性化

**证据：**

- 正式 PDF §1：九个助手中七个隔离标签页或会话上下文，只有两个不隔离；不能概括为大多数具有跨会话记忆。
- §6.1/Table 3：Merlin、MaxAI、Harpa、Copilot 四个向第一方共享聊天历史；只有 Harpa 和 Copilot 在 IndexedDB 保存完整历史并跨浏览器重启持久化。
- §6.3/Table 4：Monica 和 Sider 在五类属性上显示最强画像/个性化证据；每个场景重复 15 次并做二项检验，但服务端保留期仍不可见。

**局限：**

- 特定时间点和版本快照会随产品更新
- 黑盒审计无法观察服务端保留期与内部处理
- 浏览器扩展不能代表全部聊天助手或企业智能体

**意义：**

- 地图应纳入真实数据流和持久化位置
- 控制应覆盖采集、保存、画像、跨场景使用和第三方共享
- 部署效用与隐私成本应并列

**建议路线：** 部署隐私与治理

**边界：** 正式部署系统审计可记 P3；与记忆机制主张仅为桥接，因此 D1、tier=bridge。

**版本：** USENIX Security 25 正式版，pp. 8115–8134，ISBN 978-1-939133-52-6。

**标识：** 稳定 ID usenix-security-2025:309652；工作族 ID big-help-big-brother-2025

**证据位置：**

- claim 九个助手与上下文隔离；location 正式 PDF Abstract、§1，pp. 8115–8117；来源 PDF
- claim 历史共享和 IndexedDB 持久化；location 正式 PDF §6.1、Table 3，pp. 8124–8125；来源 PDF
- claim 画像与个性化；location 正式 PDF §6.3、Table 4，pp. 8126–8128；来源 PDF
- claim 结论与适用范围；location 正式 PDF §7，pp. 8129–8130；来源 PDF

**资源：** [一手入口](<https://www.usenix.org/conference/usenixsecurity25/presentation/vekaria>) · [PDF](<https://www.usenix.org/system/files/usenixsecurity25-vekaria.pdf>)

**关联 ID：** `mextra-2025` · `relational-gains-privacy-strains-2026` · `steem-2026`

---

<a id="paper-context-length-alone-hurts-2025"></a>
**53. 即使完美召回，上下文长度本身仍损害大模型表现｜Context Length Alone Hurts LLM Performance Despite Perfect Retrieval（2025 · Findings of EMNLP 2025）**

**作者：** Yufeng Du、Minyang Tian、Srikanth Ronanki、Subendhu Rongali、Sravan Babu Bodapati、Aram Galstyan、Azton Wells、Roy Schwartz、Eliu A Huerta、Hao Peng

**书目：** 年份 2025；载体 Findings of EMNLP 2025；状态 同行评议；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 上下文与隐状态记忆、评测、安全与治理；层级 即时或单轮；阅读层级 核心；证据等级 A；简称 Length Alone Hurts

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 在显式控制证据召回的条件下，隔离‘找不到’与‘输入变长后不会用’。

**问题：** 长上下文任务的退化是否只由召回失败造成？

**机制：** 在数学、问答和代码任务中固定相关证据，改变输入长度与干扰形式，并分别测量召回和任务表现。

**步骤：**

1. 从短上下文任务分离证据与问题
2. 用多种最小干扰扩展输入
3. 显式验证相关证据可被完整召回
4. 比较不同长度下的推理、问答和编程表现

**证据：**

- 正式摘要报告，5 个开放或闭源模型在数学、问答和代码任务中，即使能完美召回全部相关信息，表现仍随输入变长而下降 13.9%–85%。

**局限：**

- 任务与干扰是受控实验，不覆盖所有开放式长文本场景。
- 结果不证明所有模型和长度都按同一幅度退化。

**意义：**

- 记忆评测必须分离存储、召回、证据阅读和后续推理。

**边界：** 元数据与数字来自 ACL Anthology 正式页和摘要；正文用于确认召回控制设计。

**标识：** DOI 10.18653/v1/2025.findings-emnlp.1264

**证据位置：**

- claim 5 个模型与 13.9%–85% 退化；location ACL 正式摘要；来源 一手入口
- claim 完美召回的控制设计；location PDF 第 3–4 节；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2025.findings-emnlp.1264/>) · [PDF](<https://aclanthology.org/2025.findings-emnlp.1264.pdf>)

---

<a id="paper-faithun-2025"></a>
**54. FaithUn：通过知识关联性研究实现语言模型可信遗忘｜FaithUn: Toward Faithful Forgetting in Language Models by Investigating the Interconnectedness of Knowledge（2025 · EMNLP 2025）**

**作者：** Nakyeong Yang、Minsung Kim、Seunghyun Yoon、Joongbo Shin、Kyomin Jung

**书目：** 年份 2025；载体 EMNLP 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 FaithUn；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把遗忘测试从原问题扩展到同答案异上下文和多跳关联，专门识别表面遗忘。

**问题：** 模型在原提示上答错不代表关联知识已被移除，也可能错误损伤无关知识。

**机制：** 从知识图谱构造基础、泛化和多跳问答，并同时测忘却成功、测试准确、相邻知识保持与多跳泄漏。

**步骤：**

1. 从知识图谱生成基础事实问答
2. 构造同答案异上下文问题测泛化
3. 构造多跳问题测关联知识残留
4. 在多种遗忘方法和模型上联合比较忘却与保持

**证据：**

- Table 1明确区分泛化、多跳与捷径三类挑战。
- Tables 2至3显示现有方法即使在原忘却题上成功，也常在关联问题上暴露残留或产生连带损伤。
- Table 7给出三类数据拆分，支持对不同失败类型复现。

**局限：**

- 知识主要来自结构化事实图谱，难覆盖风格、长文和程序知识。
- 多跳模板和自动生成可能简化真实关联。
- 论文同时提出方法，基准结论需与方法增益区分。

**意义：**

- 删除验证必须覆盖同义、跨上下文和多跳关联。
- 效用保持应按知识邻域分层，防止把无关损伤误判为强遗忘。

**边界：** 正式论文页核验元数据与出版状态；公开全文核验机制、表图证据和局限。

**引用：** Yang等，EMNLP 2025，DOI 10.18653/v1/2025.emnlp-main.657。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.18653/v1/2025.emnlp-main.657；稳定 ID doi:10.18653/v1/2025.emnlp-main.657；工作族 ID faithun-2025

**证据位置：**

- Table 1与第3节，PDF第2至6页：任务定义和数据构造
- Tables 2至3，PDF第7至8页：主结果
- Table 7，PDF第11页：数据统计

**资源：** [一手入口](<https://aclanthology.org/2025.emnlp-main.657/>) · [PDF](<https://aclanthology.org/2025.emnlp-main.657.pdf>)

**关联 ID：** `muse-2025` · `unlearning-or-obfuscating-2025` · `can-sensitive-information-be-deleted-2024`

---

<a id="paper-fast-exact-unlearning-2025"></a>
**55. 面向大语言模型上下文学习数据的快速精确遗忘｜Fast Exact Unlearning for In-Context Learning Data for LLMs（2025 · ICML 2025, PMLR 267）**

**作者：** Andrei Ioan Muresanu、Anvith Thudi、Michael R. Zhang、Nicolas Papernot

**书目：** 年份 2025；载体 ICML 2025, PMLR 267；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、上下文与隐状态记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 ERASE；优先级 high；时间尺度 任务适配数据的可证明删除生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** exact-unlearning、in-context-learning、quantized-k-means、cost-analysis

**定位：** 把可撤回的任务适配数据留在可重建的上下文示例结构中，用量化 k-means 使精确删除成本与模型和数据集规模解耦。

**问题：** 权重微调后的精确删除通常需要接近重新训练的成本，而近似遗忘又缺少统一保证。

**机制：** ERASE 不用 SGD 把适配数据写入权重，而是对示例嵌入做量化 k-means，按簇选代表作为上下文。删除请求只需检查量化质心是否变化；大多数情况下无需重聚类，必要时重算仍保持期望成本不依赖数据集规模。

**步骤：**

1. 把任务适配样本编码为向量
2. 用带随机格点量化的 k-means 聚类，并从每簇选择上下文示例
3. 用选出的少量示例进行上下文任务适配，不改模型权重
4. 删除样本时检查量化簇是否稳定，仅在必要时重聚类，从而得到与从未见过该样本一致的学习结果

**证据：**

- PDF Algorithm 1 给出 ERASE 的量化 k-means 选择流程；第3–4节说明期望删除操作成本与模型及数据集规模无关。
- PDF Table 1 对比 SISA、随机上下文、普通 k-means 和 ERASE 的渐近删除与推理成本，明确揭示删除越便宜、推理上下文可能越贵的权衡。
- PDF Figure 3 显示3或4个上下文示例的 ERASE 在超过一半任务上可达到各 SISA 变体的准确度水平。
- PDF Table 2 显示2-shot ERASE 的整体删除成本在四个任务上约有两倍改善；4-shot 则可能因推理成本过高而不占优。
- PDF 第6节明确限制：上下文学习不适合大量数据、受上下文长度限制，且任务需要更多示例时权衡可能不泛化。

**局限：**

- 保证针对与预训练数据独立的任务适配数据，不解决预训练知识删除。
- 依赖上下文学习能有效完成任务；需要大量样本或学习新事实时不适用。
- 删除操作便宜会增加每次推理的上下文成本，必须按请求频率联合计算。
- 实验集中于少量 BigBench 指令任务，跨模型和真实合规流程仍需复现。

**意义：**

- 要实现可删除记忆，可以在学习算法设计时避免把高撤回风险数据写入权重。
- 删除成本不能单独报告，必须与长期推理次数和上下文长度联合核算。
- 精确删除、输出不可提取性和下游效用是三个不同验收层，需要分别测试。

**边界：** 正式 ICML 状态、精确删除定义、算法、成本表和限制均由 PMLR 正式页与全文核验。 独立合并审计将方法自身实验的直接性校准为 D2；D3 仅保留给独立验证或反驳。

**标识：** 稳定 ID pmlr-v267-muresanu25a；工作族 ID erase-exact-unlearning

**证据位置：**

- claim 威胁模型与精确删除范围；location PDF 第1–3节、Figure 1；来源 PDF
- claim ERASE 机制；location PDF 第4.1节、Algorithm 1；来源 PDF
- claim 成本定义与渐近比较；location PDF Table 1、第4.2节、Definition 4.1；来源 PDF
- claim 性能与整体成本；location PDF 第5节、Figures 2–3、Table 2、Appendix Tables 3–4；来源 PDF
- claim 适用限制；location PDF 第6节前后段；来源 PDF

**资源：** [一手入口](<https://proceedings.mlr.press/v267/muresanu25a.html>) · [PDF](<https://raw.githubusercontent.com/mlresearch/v267/main/assets/muresanu25a/muresanu25a.pdf>)

**关联 ID：** `muse-2025` · `can-sensitive-information-be-deleted-2024` · `unlearning-or-obfuscating-2025`

---

<a id="paper-fuma-2025"></a>
**56. 通过成员身份推断识别大语言模型已遗忘数据｜Identifying Unlearned Data in LLMs via Membership Inference Attacks（2025 · EMNLP 2025）**

**作者：** Advit Deepak、Megan Mou、Jing Huang、Diyi Yang

**书目：** 年份 2025；载体 EMNLP 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 FUMA；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 即使内容看似已忘，外部审计仍可能判断哪条知识曾被指定删除。

**问题：** 删除评测通常只问模型还能否输出内容，却忽略忘却集身份本身可能形成新的隐私信号。

**机制：** 在受控候选集上跨多种遗忘方法和模型，比较黑盒与白盒审计信号能否区分被遗忘项；本图只保留防御性结论。

**步骤：**

1. 构造包含目标知识和候选项的受控集合
2. 对多种模型执行不同遗忘设置
3. 用输出与模型信号计算候选可区分性
4. 跨258个模型组比较风险随方法与训练变化

**证据：**

- Figure 1定义了从候选知识中识别忘却项的审计任务。
- Tables 1至2显示多种遗忘配置下忘却集身份仍可被高于随机水平识别。
- Table 8给出258个模型组，覆盖不同数据、方法、训练轮次和适配设置。

**局限：**

- 研究依赖受控候选集合和一定的模型访问假设。
- 风险大小会随模型、数据和遗忘方法变化。
- 本文结果是防御性审计证据，不能直接推断所有在线服务可被同样识别。

**意义：**

- 删除证明应同时检查内容恢复和删除对象身份泄漏。
- 服务方应避免公开过细的删除前后差分信号，并提供独立审计接口。

**边界：** 正式论文页和全文已核验；为安全降敏，仅记录审计任务、聚合结果和治理含义，不复述可操作攻击流程或样例。

**引用：** Deepak等，EMNLP 2025，DOI 10.18653/v1/2025.emnlp-main.551。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.18653/v1/2025.emnlp-main.551；稳定 ID doi:10.18653/v1/2025.emnlp-main.551；工作族 ID fuma-2025

**证据位置：**

- Figure 1与第3节，PDF第2至4页：任务定义
- Tables 1至2，PDF第4至6页：受控审计结果
- Table 8，PDF第14至16页：模型组设计

**资源：** [一手入口](<https://aclanthology.org/2025.emnlp-main.551/>) · [PDF](<https://aclanthology.org/2025.emnlp-main.551.pdf>)

**关联 ID：** `muse-2025` · `can-sensitive-information-be-deleted-2024` · `pii-cue-controlled-2026`

---

<a id="paper-hoovered-data-point-2025"></a>
**57. 被吸纳为数据点：英国大语言模型对话代理用户的隐私行为、认知与关切｜“Hoovered up as a data point”: Exploring Privacy Behaviours, Awareness, and Concerns Among UK Users of LLM-based Conversational Agents（2025 · Proceedings on Privacy Enhancing Technologies 2025(4)）**

**作者：** Lisa Mekioussa Malki、Akhil Polamarasetty、Majid Hatamian、Mark Warner、Enrico Costanza

**书目：** 年份 2025；载体 Proceedings on Privacy Enhancing Technologies 2025(4)；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 英国用户隐私行为；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把用户真实采取的隐私行为与对训练退出、聊天删除、账户删除和模型遗留影响的理解放到同一测量中。

**问题：** 技术研究已证明训练记忆和删除困难，但日常 LLM 对话代理用户是否使用现有控制、如何理解退出和删除的时间与范围仍缺乏证据。

**机制：** 研究对英国月度用户进行问卷，结合回溯行为、披露边界、仿真界面任务、风险认知和开放回答，区分功能存在、使用和正确理解。

**步骤：**

1. 从 375 名筛选者中获得 221 份完整问卷，经质量控制保留 211 名英国月度用户。
2. 回溯测量删除聊天、训练退出、导出和提示最小化等实际隐私行为。
3. 用无品牌仿真界面测试训练退出、删除全部聊天和删除账户的预期后果与时间。
4. 结合描述统计、非参数检验、回归与定性编码分析理解偏差和治理需求。

**证据：**

- 第 4.2 节和图 2 报告只有 15/211 人使用过训练退出，说明控制可用不等于采用。
- 第 4.4 节中 114/211 人正确理解训练退出只影响未来训练，但 76/211 人误以为已经学到的模式也会被移除。
- 表 3 显示许多参与者预计删除聊天或账户后，模型学到的影响也会迅速消失，暴露数据库删除与模型反学习的心理模型混淆。

**局限：**

- 样本来自 Prolific，且大学教育程度和 ChatGPT 使用比例较高，不能代表所有英国用户。
- 隐私行为为自报，功能后果通过仿真界面测量，并非后台遥测或实际擦除审计。
- 横截面设计不能观察同一用户的偏好、理解和控制使用如何随时间变化。

**意义：**

- 训练退出应明确区分未来数据使用与既有模型影响，删除界面也应分离聊天、账户、备份和模型影响。
- 控制评测应同时报告发现率、采用率和正确理解率，避免把设置存在当作有效治理。

**边界：** 正式元数据和全文均来自 PoPETs；核对正式页码 838 至 860、第 3 至第 6 节、表 1 至表 3和图 1 至图 3。用户敏感内容只按聚合结果呈现。

**引用：** Malki et al., PoPETs 2025(4), DOI 10.56553/popets-2025-0160。

**版本：** 采用 PoPETs 2025 第四期正式版本。

**标识：** DOI 10.56553/popets-2025-0160；稳定 ID doi:10.56553/popets-2025-0160；工作族 ID hoovered-data-point-2025

**证据位置：**

- 第 3.1 至第 3.5 节，正式页码 841 至 843：招募、仿真界面、测量和分析。
- 第 4.2 节与图 2，正式页码 844：八类隐私行为、控制认知和采用率。
- 第 4.4 节与表 3，正式页码 844 至 845：训练退出和删除后果理解。
- 第 6 节，正式页码 850：样本、自报、横截面和外推边界。

**资源：** [一手入口](<https://petsymposium.org/popets/2025/popets-2025-0160.php>) · [PDF](<https://petsymposium.org/popets/2025/popets-2025-0160.pdf>)

**关联 ID：** `big-help-big-brother-2025` · `mextra-2025` · `relational-gains-privacy-strains-2026`

---

<a id="paper-kv-cache-compression-audit-2025"></a>
**58. 重新审视大模型服务中的键值缓存压缩技术｜Rethinking Key-Value Cache Compression Techniques for Large Language Model Serving（2025 · MLSys 2025）**

**作者：** Wei Gao、Xinyu Zhou、Peng Sun、Tianwei Zhang、Yonggang Wen

**书目：** 年份 2025；载体 MLSys 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 conference paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、上下文与隐状态记忆；层级 会话或任务期；阅读层级 桥接；证据等级 B；简称 KV Compression Audit；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 不是把 KV 压缩当作记忆系统，而是用其直接反证：改变被保留的上下文状态会系统性改变答案长度、语义和端到端延迟。

**问题：** KV 压缩论文常把局部精度或内核速度等同于系统和回答质量，忽略压缩状态对生成行为的非线性影响。

**机制：** 统一复现量化、稀疏与混合压缩方法，同时测量语义、回答长度分布和端到端延迟。

**步骤：**

1. 在同一模型、数据和服务设置下复现多类 KV 压缩算法。
2. 对压缩后的状态执行完整生成，比较语义相似度和回答长度，而非只测逐令牌误差。
3. 统计回答至少缩短或增长 50% 的比例，以暴露平均分掩盖的行为失效。
4. 把压缩开销纳入端到端延迟，核验理论内存节省是否转化为真实服务收益。

**证据：**

- 表 5 的 1000 条 ShareGPT、Llama-3.1-8B 结果中，KIVI、GEAR、H2O、StreamingLLM 分别有 10.9%、6.8%、9.5%、16.5% 的回答至少缩短 50%，并有 24.5%、27.1%、21.3%、26.4% 至少增长 50%。
- 表 4 同时报告语义分数和输出长度，显示平均语义变化较小时，生成长度仍可能大幅偏移。
- 图 5 与 §4.4 显示当前压缩实现的额外开销可能增加端到端延迟，不能由内存节省直接推导生产吞吐收益。
- 附录表 9 在 Mistral 上复现相同方向，说明并非单一模型的偶发现象。

**局限：**

- 属于单次会话内的上下文状态，不是持久长期记忆。
- 只覆盖若干 7B／8B 模型和 ShareGPT，不能直接外推长期回忆任务。
- 回答长度和语义相似度只是行为代理，没有长期事实保持或行动评测。
- 未在真实生产流量中验证。

**意义：**

- 任何记忆压缩都必须测答案／行动层失效分布，不能只报告压缩比和平均相似度。
- 该论文仅作为评价桥接和直接反证，不代表一般 KV 缓存工程进入研究图谱。

**边界：** 纳入依据严格限定为表 5 的答案层反证与端到端审计。

**引用：** 不把 Keyformer、Prompt Cache、PagedAttention、Mooncake 等纯缓存系统视为同类纳入项。

**版本：** 以 MLSys 2025 正式论文集全文为准。

**标识：** 稳定 ID mlsys-2025-26289c647c6828e862e271ca3c490486；工作族 ID kv-cache-compression-audit-2025

**证据位置：**

- §4.3 与表 4，PDF 第 8 页：语义与长度
- §4.4、表 5 与图 5，PDF 第 9 页：回答层偏移和端到端延迟
- 附录表 9：跨模型复核

**资源：** [一手入口](<https://proceedings.mlsys.org/paper_files/paper/2025/hash/26289c647c6828e862e271ca3c490486-Abstract-Conference.html>) · [PDF](<https://proceedings.mlsys.org/paper_files/paper/2025/file/26289c647c6828e862e271ca3c490486-Paper-Conference.pdf>)

**关联 ID：** `memory-management-impact-2026` · `context-length-alone-hurts-2025` · `a13` · `supo-2026`

---

<a id="paper-longmemeval-2025"></a>
**59. LongMemEval：评测聊天助手的长期交互记忆｜LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory（2025 · ICLR 2025）**

**作者：** Di Wu、Hongwei Wang、Wenhao Yu、Yuwei Zhang、Kai-Wei Chang、Dong Yu

**书目：** 年份 2025；载体 ICLR 2025；状态 同行评议；来源类型 benchmark+dataset

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 LongMemEval；优先级 1；相关性排序 6

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V4 / D=D3 / P=P3 / Q=Q3

**定位：** 以 500 个问题覆盖抽取、跨会话推理、时间推理、知识更新和拒答，并把系统分成索引、检索、阅读三阶段。

**问题：** 已有评测难以隔离长期聊天系统在写入组织、召回和读懂证据三个阶段的故障。

**机制：** 把经校验的问题嵌入可扩展聊天历史；提供较短和超大规模版本，并以 oracle 历史、检索历史和长上下文分解误差来源。

**步骤：**

1. 构造带时间与更新关系的问题
2. 嵌入多会话历史
3. 建立索引
4. 检索候选会话
5. 阅读证据并回答或拒答

**证据：**

- 正式摘要给出 500 个问题和五种核心能力；商业助手与长上下文模型在持续互动中约有 30% 准确率下降。
- 论文试验显示即使在约 115K-token 设置中，相比 oracle 会话仍可下降 30%–60%。
- 完美召回并不等于正确阅读，压缩事实也可能丢失细节。

**局限：**

- 聊天历史与问题是可控合成构造，不等于真实用户需求分布。
- 官方仓库在 2025 年 9 月清理了会干扰答案正确性的历史，未锁定数据版本会破坏排行榜可比性。
- 大量评价仍依赖模型裁判与封闭模型，存在复现漂移。

**意义：**

- 提供跨系统可复用的故障分解框架。
- 提示研究者报告数据版本、检索召回与最终回答，而不是只报一个总分。

**边界：** ICLR 正式 Proceedings、全文与作者仓库交叉核验；未以 OpenReview 验证页代替正式出版证明。

**版本：** 基准存在论文发布版与 2025-09 清理版；合并时应保存数据 commit 或版本号。

**证据位置：**

- claim 500 问题、五种能力和约 30% 下降；location ICLR 正式页摘要；来源 一手入口
- claim 30%–60% oracle 差距及索引—检索—阅读分解；location 论文 §3–§4、pilot study 表格；来源 PDF
- claim 2025-09 数据清理；location 官方仓库 README 的 Update 段；来源 代码

**资源：** [一手入口](<https://proceedings.iclr.cc/paper_files/paper/2025/hash/d813d324dbf0598bbdc9c8e79740ed01-Abstract-Conference.html>) · [PDF](<https://proceedings.iclr.cc/paper_files/paper/2025/file/d813d324dbf0598bbdc9c8e79740ed01-Paper-Conference.pdf>) · [代码](<https://github.com/xiaowu0162/LongMemEval>)

---

<a id="paper-memsim-2025"></a>
**60. MemSim：评测 LLM 个人助手记忆的贝叶斯模拟器｜MemSim: A Bayesian Simulator for Evaluating Memory of LLM-based Personal Assistants（2025 · NeurIPS 2025）**

**作者：** Zeyu Zhang、Quanyu Dai、Luyu Chen、Zeren Jiang、Rui Li、Jieming Zhu、Xu Chen、Yi Xie、Zhenhua Dong、Ji-Rong Wen

**书目：** 年份 2025；载体 NeurIPS 2025；状态 同行评议；来源类型 benchmark+dataset

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 MemSim；优先级 1；相关性排序 9

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V4 / D=D3 / P=P3 / Q=Q3

**定位：** 用贝叶斯关系网络和因果生成自动构造可校验、可扩展的个人消息问答。

**问题：** 从自由生成的个人消息自动得到可靠问答容易受生成幻觉污染，人工标注又难以扩展。

**机制：** 先用贝叶斯关系网络定义事实依赖，再沿因果结构生成消息与问题，从结构中得到答案并形成 MemDaily。

**步骤：**

1. 定义日常事实变量及依赖
2. 从贝叶斯网络采样
3. 生成与事实一致的用户消息
4. 按因果关系生成问题与答案
5. 用 MemDaily 比较记忆系统

**证据：**

- NeurIPS 正式摘要确认 BRNet 与因果生成用于降低事实幻觉影响。
- 官方仓库报告 MemDaily 含 2,954 条轨迹、26,003 条消息和 2,954 个问题。

**局限：**

- 作者明确指出当前只评测事实信息，没有覆盖隐性偏好。
- 数据不是完整对话形式，不能验证互动中的语用、信任或关系变化。
- 生成数据的结构正确性不等于真实用户分布正确性。

**意义：**

- 提高长期记忆基准的规模和答案可校验性。
- 同时清楚展示自动合成对真实性和任务范围的代价。

**边界：** NeurIPS 正式页、最终 PDF 与官方仓库核验。

**标识：** DOI 10.52202/085713-3025；工作族 ID memsim-2409.20163

**证据位置：**

- claim BRNet、因果生成与正式状态；location NeurIPS 正式摘要；PDF pp. 1–2；来源 一手入口
- claim 事实型、无完整对话和偏好缺口；location PDF Limitations；来源 PDF
- claim MemDaily 规模；location 官方仓库 README；来源 代码

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2025/hash/826aea2253363fe04e8c4991b2a8869e-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2025/file/826aea2253363fe04e8c4991b2a8869e-Paper-Conference.pdf>) · [代码](<https://github.com/nuster1128/MemSim>)

---

<a id="paper-mextra-2025"></a>
**61. 揭示 LLM 智能体记忆中的隐私风险｜Unveiling Privacy Risks in LLM Agent Memory（2025 · ACL 2025）**

**作者：** Bo Wang、Weiyi He、Shenglai Zeng、Zhen Xiang、Yue Xing、Jiliang Tang、Pengfei He

**书目：** 年份 2025；载体 ACL 2025；状态 同行评议；来源类型 security-evaluation

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 MEXTRA；优先级 1；相关性排序 17；时间尺度 跨会话长期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 以黑盒实验系统评估智能体长期记忆中私人交互被泄露的保密性风险。

**问题：** 为提高个性化而保存的用户—智能体交互可能包含个人信息，但传统记忆评测只衡量效用。

**机制：** 在两个代表性智能体上进行受控隐私抽取评测，并分析记忆配置与泄露之间的关系。

**步骤：**

1. 定义记忆保密性威胁
2. 在代表性智能体中保存私人交互
3. 以黑盒方式测量泄露
4. 分析配置因素
5. 提出最小化与访问控制需求

**证据：**

- ACL 正式摘要报告两个代表性智能体存在记忆隐私暴露，并指出记忆配置会影响泄露。

**局限：**

- 只有两个代表性智能体，不能覆盖所有存储、模型和部署架构。
- 实验风险证明不等于真实事件流行率。
- 研究未建立端到端、经部署验证的完整防护。

**意义：**

- 个人化记忆必须同时报告效用和保密性。
- 设计上应采用数据最小化、检索授权、输出检查、租户隔离和可审计删除。

**边界：** 只记录风险与防护含义；删去任何提示设计或自动化抽取方法。

**标识：** DOI 10.18653/v1/2025.acl-long.1227

**证据位置：**

- claim 私人交互存储、黑盒威胁和两个代理实验；location ACL 正式摘要；PDF pp. 1–2；来源 一手入口
- claim 记忆配置影响泄露；location PDF 实验分析节；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2025.acl-long.1227/>) · [PDF](<https://aclanthology.org/2025.acl-long.1227.pdf>)

---

<a id="paper-minja-2025"></a>
**62. 通过仅查询交互对 LLM 智能体进行记忆注入的安全研究｜Memory Injection Attacks on LLM Agents via Query-Only Interaction（2025 · NeurIPS 2025）**

**作者：** Shen Dong、Shaochen Xu、Pengfei He、Yige Li、Jiliang Tang、Tianming Liu、Hui Liu、Zhen J. Xiang

**书目：** 年份 2025；载体 NeurIPS 2025；状态 同行评议；来源类型 security-evaluation

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 MINJA；优先级 1；相关性排序 16；时间尺度 跨会话长期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 证明即使没有直接数据库权限，普通交互也可能使不可信内容进入持久记忆并影响未来查询。

**问题：** 自动写记忆的智能体可能把用户输入或模型输出当作长期可信经验，形成跨时间甚至跨用户的影响。

**机制：** 在代理会从用户交互自动形成并保存长期记录的设置中，研究者只通过查询和观察输出，诱导代理自行写入会影响后续检索的异常记录；本文条目不展开具体构造。

**步骤：**

1. 定义仅交互写入威胁
2. 在受控智能体中观察记忆形成
3. 检查后续行为是否受持久记录影响
4. 比较框架脆弱性
5. 推导写入授权与来源隔离要求

**证据：**

- 表 1 覆盖三类代理和四个数据集；不同配置的平均写入成功率为 95.6% 至 100%，后续目标行为率为 57.0% 至 98.9%，摘要给出的跨设置平均值分别为 98.2% 和 76.8%。
- 三个任务的普通效用平均下降小于 2 个百分点，但 MMLU 配置下降 10 个百分点；表 4还显示结果会随任务和良性记录密度显著变化。
- 表 5 的有限检测实验呈现定向检测难迁移、通用检测漏报或误报较高的权衡。

**局限：**

- 仅查询仍要求代理自动保存交互记录，并让这些记录可被后续任务检索。
- 核心实验默认共享记忆库；隔离记忆下的跨用户影响还需要额外的身份或权限条件。
- 结果对任务、基础模型、良性记录密度和记忆策略敏感，不能外推到所有生产服务。

**意义：**

- 记忆写入应视为高权限操作，需要来源标签、用户隔离、延迟提交、异常检测和可回滚审计。
- “用户不能直接访问数据库”不足以构成安全边界。

**边界：** 已核验 NeurIPS 正式全文；不记录异常内容、提示构造、目标对或复现参数。

**版本：** 早期 arXiv 题名不同；本记录采用 NeurIPS 2025 正式题名和状态。

**标识：** DOI 10.52202/085713-1554；工作族 ID minja-2503.03704

**证据位置：**

- claim 仅查询威胁模型及共享记忆前提；location 第 3 节 Threat Model；来源 PDF
- claim 跨代理主结果和普通效用变化；location 表 1；第 5.2 节；来源 PDF
- claim 对良性记录密度的敏感性和检测权衡；location 表 4；表 5；第 5.3 至 5.4 节；来源 PDF

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2025/hash/42a97bbd9844d2bf68596730af80bcdf-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2025/file/42a97bbd9844d2bf68596730af80bcdf-Paper-Conference.pdf>)

---

<a id="paper-muse-2025"></a>
**63. MUSE：语言模型机器遗忘六维评测｜MUSE: Machine Unlearning Six-Way Evaluation for Language Models（2025 · ICLR 2025）**

**作者：** Weijia Shi、Jaechan Lee、Yangsibo Huang、Sadhika Malladi、Jieyu Zhao、Ari Holtzman、Daogao Liu、Luke Zettlemoyer、Noah A. Smith、Chiyuan Zhang

**书目：** 年份 2025；载体 ICLR 2025；状态 同行评议；来源类型 benchmark

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 MUSE；优先级 1；相关性排序 19

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V4 / D=D3 / P=P3 / Q=Q3

**定位：** 以六个维度同时衡量删除知识、隐私泄露、保留效用、规模、连续删除与可持续性。

**问题：** 只看忘却集准确率会把输出抑制误当作真实删除，也忽略效用、规模和连续请求。

**机制：** 用书籍和新闻语料构造遗忘/保留集合；从记忆、知识、隐私、效用、规模和持续性六维比较八种方法。

**步骤：**

1. 定义遗忘与保留目标
2. 评测逐字与知识记忆
3. 检查隐私泄露
4. 测量保留效用
5. 评估规模和连续遗忘

**证据：**

- ICLR 最终论文报告八种方法中只有一种避免严重隐私泄露。
- 多种方法在大规模删除、连续删除或保留效用上失败。

**局限：**

- 主要使用 7B 级模型和两个语料域。
- 评价的是参数训练记忆，不等同于删除外部对话库、缓存、日志或备份。
- 机器遗忘为近似方法，不能从指标分数推导法律意义上的删除完成。

**意义：**

- 删除声明必须多维审计，不能用单一忘却分数。
- 外部记忆删除与参数遗忘应在系统架构和报告中分开。

**边界：** ICLR 正式 Proceedings 与全文核验；将参数遗忘边界显式标注。

**证据位置：**

- claim 六维框架和八种方法总体失败；location PDF 摘要、§1、Table 1；来源 PDF
- claim 记忆、知识、隐私和效用指标定义；location PDF §2–§3；来源 PDF

**资源：** [一手入口](<https://proceedings.iclr.cc/paper_files/paper/2025/hash/4556f5398bd2c61bd7500e306b4e560a-Abstract-Conference.html>) · [PDF](<https://proceedings.iclr.cc/paper_files/paper/2025/file/4556f5398bd2c61bd7500e306b4e560a-Paper-Conference.pdf>)

---

<a id="paper-openunlearning-2025"></a>
**64. OpenUnlearning：通过统一方法与指标基准加速大语言模型遗忘研究｜OpenUnlearning: Accelerating LLM Unlearning via Unified Benchmarking of Methods and Metrics（2025 · NeurIPS 2025 Datasets and Benchmarks Track）**

**作者：** Vineeth Dorna、Anmol Mekala、Wenlong Zhao、Andrew McCallum、Zico Kolter、Zachary C. Lipton、Pratyush Maini

**书目：** 年份 2025；载体 NeurIPS 2025 Datasets and Benchmarks Track；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 OpenUnlearning；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把遗忘方法、数据和指标装进同一框架，并反过来测试指标是否真的可信和稳健。

**问题：** 遗忘论文使用的实现、数据和指标碎片化，单个分数可能无法区分真正忘却与表面退化。

**机制：** 统一十三种方法、十六项评测和三大基准，在四百五十余检查点上比较，并以受控模型序列和部署变换元评测指标。

**步骤：**

1. 把遗忘方法、数据集和指标模块化
2. 在统一配置下生成或载入大量检查点
3. 构造已知忘却程度的模型序列测指标可信度
4. 施加重学或量化等变换测指标稳健性
5. 统一聚合并比较方法排序

**证据：**

- Figure 1和Table 1列出十三种算法、十六项评测、三个基准与四百五十余公开检查点。
- Table 2显示多种常用指标在可信度或稳健性元评测中表现不一致。
- Table 3显示方法排名对指标集合和聚合方式敏感。

**局限：**

- 统一实现仍受所选十三种方法和三个基准限制。
- 指标元评测依赖作者构造的受控模型序列。
- 公开检查点不等同于真实删除请求与监管审计。

**意义：**

- 遗忘基准必须同时验证指标本身，而不是假定低分等于已删除。
- 方法排序应披露指标集合、聚合方式和部署变换后的稳定性。

**边界：** 正式论文页核验元数据与出版状态；公开全文核验机制、表图证据和局限。

**引用：** Dorna等，NeurIPS 2025 Datasets and Benchmarks Track，DOI 10.52202/085713-1458。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.52202/085713-1458；稳定 ID doi:10.52202/085713-1458；工作族 ID openunlearning-2025

**证据位置：**

- Figure 1与第3节，PDF第3至5页：框架组件
- Figure 3、Table 2与第4节，PDF第6至9页：指标元评测
- Table 3与第5节，PDF第9至11页：统一方法比较

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2025/hash/3e4a38f228427ab819ba7899003a44b1-Abstract-Datasets_and_Benchmarks_Track.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2025/file/3e4a38f228427ab819ba7899003a44b1-Paper-Datasets_and_Benchmarks_Track.pdf>)

**关联 ID：** `muse-2025` · `fast-exact-unlearning-2025` · `unlearning-or-obfuscating-2025`

---

<a id="paper-poisonedrag-2025"></a>
**65. PoisonedRAG：检索增强生成的知识污染风险｜PoisonedRAG: Knowledge Corruption Attacks to Retrieval-Augmented Generation of Large Language Models（2025 · USENIX Security 2025）**

**作者：** Wei Zou、Runpeng Geng、Binghui Wang、Jinyuan Jia

**书目：** 年份 2025；载体 USENIX Security 2025；状态 同行评议；来源类型 security-evaluation

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、外部检索与非参数记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 PoisonedRAG

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 把外部知识库识别为 RAG 的独立信任边界，并在受控实验中检验少量污染记录对目标回答的影响。

**问题：** 可更新的外部知识库是否会成为能够持续改变模型回答的污染面？

**机制：** 在多数据集、检索器、模型和实际 RAG 系统上执行受控知识库污染评测，并测试若干通用防御。

**步骤：**

1. 定义外部知识库污染威胁
2. 在受控环境注入少量测试记录
3. 检查检索和生成链路的目标回答变化
4. 比较多数据集、模型与防御的鲁棒性

**证据：**

- USENIX 正式页报告，在其受控设定中，对每个目标问题注入 5 条测试文本可在百万级文本库上达到约 90% 的目标操纵成功率，而所测若干防御不充分。

**局限：**

- 这是指定威胁模型下的受控安全评测，不等于真实部署中的发生率。
- 结果对检索器、数据源、写库权限和防御组合敏感。

**意义：**

- 写库授权、来源签名、冲突检测、异常召回监测和可回滚性必须与 RAG 效用一起评估。

**边界：** 仅记录威胁类型、聚合实验证据与治理含义；不保留任何可操作攻击细节。

**证据位置：**

- claim 受控实验成功率与防御结论；location USENIX 正式页摘要与 PDF 摘要；来源 一手入口

**资源：** [一手入口](<https://www.usenix.org/conference/usenixsecurity25/presentation/zou-poisonedrag>) · [PDF](<https://www.usenix.org/system/files/usenixsecurity25-zou-poisonedrag.pdf>)

---

<a id="paper-quantization-unlearning-failure-2025"></a>
**66. 量化导致大语言模型遗忘灾难性失效｜Catastrophic Failure of LLM Unlearning via Quantization（2025 · ICLR 2025）**

**作者：** Zhiwei Zhang、Fali Wang、Xiaomin Li、Zongyu Wu、Xianfeng Tang、Hui Liu、Qi He、Wenpeng Yin、Suhang Wang

**书目：** 年份 2025；载体 ICLR 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 量化后遗忘失效；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 模型在全精度下看似已忘的知识，可在常见部署量化后大幅恢复。

**问题：** 遗忘验证通常只检查训练完成后的原始检查点，无法保证压缩和部署变换后仍然忘却。

**机制：** 对多种遗忘模型施加不同量化方法与精度，比较忘却知识和通用效用，并用量化稳健方案作对照。

**步骤：**

1. 在两个数据域和多种方法上生成遗忘模型
2. 记录全精度下的忘却与效用
3. 应用多种量化方式和精度
4. 重新测知识恢复和效用变化
5. 与量化稳健训练对照

**证据：**

- Table 1显示带效用约束的方法在全精度仍保留部分目标知识，四位量化后平均恢复显著增加。
- Table 2表明该现象跨多种量化方法和精度存在。
- Table 3显示面向量化稳健性的对照方法能缓解但不能消除权衡。

**局限：**

- 实验集中于特定开源模型、两个数据域和若干量化方法。
- 量化是部署变换之一，不能代表蒸馏、剪枝或模型合并。
- 恢复风险是聚合研究结果，不说明任何具体服务当前可被利用。

**意义：**

- 删除证明必须覆盖预期部署格式，不能只认证训练后原始权重。
- 模型版本、精度与压缩链应进入遗忘审计清单。

**边界：** ICLR正式页核验出版状态，公开同作品族全文核验表图；只报告防御性聚合结果。

**引用：** Zhang等，ICLR 2025；正式入口为OpenReview，未虚构DOI。

**版本：** ICLR 2025正式版本与arXiv 2410.16454属于同一作品族；全文位置核验使用作者公开版本。

**标识：** 稳定 ID openreview:lHSeDYamnz；工作族 ID quantization-unlearning-failure-2025

**证据位置：**

- Figure 1与第1节，PDF第2页：量化后恢复现象
- Table 1与第4节，PDF第6页：全精度和量化对照
- Table 2，PDF第7页：量化方法和精度
- Table 3与第6节，PDF第10页：稳健方案

**资源：** [一手入口](<https://openreview.net/forum?id=lHSeDYamnz>) · [PDF](<https://arxiv.org/pdf/2410.16454>)

**关联 ID：** `unlearning-or-obfuscating-2025` · `muse-2025` · `can-sensitive-information-be-deleted-2024`

---

<a id="paper-raguard-2025"></a>
**67. 比无检索更差？评测检索增强生成抵抗误导检索的事实核查数据集｜Worse than Zero-shot? A Fact-Checking Dataset for Evaluating the Robustness of RAG Against Misleading Retrievals（2025 · NeurIPS 2025 Datasets and Benchmarks Track）**

**作者：** Linda Zeng、Rithwik Gupta、Divij Motwani、Yi Zhang、Diji Yang

**书目：** 年份 2025；载体 NeurIPS 2025 Datasets and Benchmarks Track；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、外部检索与非参数记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 RAGuard；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 用自然发生的支持、误导和无关证据测试RAG，所有被测系统在误导检索下都可能劣于无检索。

**问题：** 干净金标准文档或人工合成噪声会高估RAG在现实信息环境中的稳健性。

**机制：** 从公开讨论构造自然证据库，标注支持、误导和无关三类文档，分别测检索、事实判断和解释，并与人工及无检索对照。

**步骤：**

1. 收集公开事实主张和核查结论
2. 从自然讨论构建候选检索文档
3. 把证据标注为支持、误导或无关
4. 运行三类任务并与无检索基线比较
5. 用人工子集核验难度和标签

**证据：**

- Table 1说明RAGuard不同于干净或合成扰动数据，证据来自自然发生的信息环境。
- Table 2显示在潜在误导检索下，所有测试的RAG系统均低于各自无检索基线。
- 第5节显示人工标注者在相同条件下持续优于模型，问题不只是数据不可判定。

**局限：**

- 领域集中于政治事实核查，不能直接推广到医疗或企业知识库。
- 自然讨论含语言和群体偏差。
- 基准评测暴露，不提供真实检索库的访问控制或删除机制。

**意义：**

- 外部记忆库基准必须包含自然误导证据和无检索对照。
- 检索器命中率之外，还应报告证据类型和错误放大率。

**边界：** 正式论文页与全文已核验；为安全和隐私降敏，不复述误导内容或个体样例。

**引用：** Zeng等，NeurIPS 2025 Datasets and Benchmarks Track，DOI 10.52202/085713-5409。

**版本：** 仅采用NeurIPS 2025数据集与基准轨正式版本；同名ResponsibleFM研讨会防御框架是另一作品族，未混入本记录。

**标识：** DOI 10.52202/085713-5409；稳定 ID doi:10.52202/085713-5409；工作族 ID raguard-2025

**证据位置：**

- Figure 2、Table 1与第2节，PDF第3至5页：证据分类
- Figure 4与第3节，PDF第6至7页：数据构造
- Table 2与第4节，PDF第7至10页：模型与无检索对照
- 第5节，PDF第10至11页：人工研究

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2025/hash/ed25c00ff6900989116d3ba5d607d33d-Abstract-Datasets_and_Benchmarks_Track.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2025/file/ed25c00ff6900989116d3ba5d607d33d-Paper-Datasets_and_Benchmarks_Track.pdf>)

**关联 ID：** `poisonedrag-2025` · `rag-privacy-good-bad-2024` · `taming-knowledge-conflicts-2025`

---

<a id="paper-ragva-deployment-2025"></a>
**68. RAGVA：真实环境中检索增强虚拟助手的工程实践｜RAGVA: Engineering retrieval augmented generation-based virtual assistants in practice（2025 · Journal of Systems and Software 226, 112436）**

**作者：** Rui Yang、Michael C. Fu、Chakkrit Tantithamthavorn、Chetan Arora、Lisa Vandenhurk、Joey Chua

**书目：** 年份 2025；载体 Journal of Systems and Software 226, 112436；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、外部检索与非参数记忆、个性化与用户长期记忆；层级 模型生命周期；阅读层级 桥接；证据等级 B；简称 RAGVA；优先级 medium；时间尺度 企业客服知识库与运行日志跨版本持久；对话会话内状态短期保留

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** rag、industry-deployment、llmops、virtual-assistant、focus-group

**定位：** 以Transurban收费公路客服的真实替换项目为对象，把RAG数据摄取、检索生成、日志评测和工程挑战连接成部署生命周期证据。

**问题：** 实验室RAG结果很少说明企业如何整理持续变化的资料、设置回退与日志、评价答案，并在真实组织内处理数据、质量和运维问题。

**机制：** 团队从Linkt支持资料构建带元数据的检索库，以检索和生成流水线回答客户问题，并通过回退、日志和仪表盘观察系统；研究者再用九名工程人员焦点组提炼八类挑战和二十二个研究问题。

**步骤：**

1. 收集并清洗收费公路支持文档，将FAQ、主题、类型和关键词等元数据写入摄取管线。
2. 对用户问题执行检索与生成，按置信与业务规则处理低置信回退和响应。
3. 记录查询、检索输出、回答与运行统计，通过测试和仪表盘检查主题匹配及异常。
4. 从九名Transurban工程人员的两次焦点组和后续核对中归纳数据摄取、响应生成与评测阶段的八类工程挑战。

**证据：**

- §3与Figure 2给出Transurban RAGVA从数据摄取、响应生成到评测的工程流程，并以实际Linkt支持文档说明元数据整理。
- §4报告九名工程人员参加两次各3至3.5小时的焦点组，另有简短后续核对。
- §5将真实工程问题整理为八类挑战和二十二个研究问题，覆盖多模态数据工程、检索生成和评价。
- §8明确单一组织、九名参与者、焦点组方法和快速变化技术带来的外部效度限制。

**局限：**

- 证据来自单一公司、单一收费公路客服场景和九名工程人员，不能直接代表其他行业与规模。
- 焦点组提供工程经验而非独立随机对照，没有量化系统替换对用户任务成功率或长期质量的因果影响。
- 论文不提出新的检索或记忆算法，价值在于部署生命周期和工程治理桥接。
- 知识库删除、个人数据保留期限和多租户访问控制没有形成可验证保证。

**意义：**

- 外部记忆部署要把数据摄取、元数据、回退、日志和评价视为同一生命周期。
- 方法论文的离线准确率不足以替代真实组织中的数据工程、运维和治理证据。

**边界：** ScienceDirect正式页核验卷期、文章号、DOI与开放获取状态；正式全文核验流程、参与者、挑战和效度限制。

**引用：** Yang等，Journal of Systems and Software 226（2025）112436，DOI 10.1016/j.jss.2025.112436。

**版本：** 以Journal of Systems and Software 2025正式开放获取版本为主；arXiv是早期版本族。

**标识：** DOI 10.1016/j.jss.2025.112436；arXiv 2502.14930；稳定 ID doi:10.1016/j.jss.2025.112436；工作族 ID ragva-transurban-deployment

**证据位置：**

- Figure 1，PDF第2页：客户虚拟助手开发的四步过程
- Figure 2、§3.1，PDF第3至5页：RAGVA数据摄取、元数据、检索生成与评测流程
- §4，PDF第5至6页：九名工程人员、两次3至3.5小时焦点组及后续核对
- §5，PDF第6至12页：八类挑战与二十二个研究问题
- §8至§9，PDF第13至14页：效度威胁与结论

**资源：** [一手入口](<https://www.sciencedirect.com/science/article/pii/S0164121225001049>) · [PDF](<https://research.monash.edu/files/721514111/681204101-oa.pdf>) · [arXiv](<https://arxiv.org/abs/2502.14930>)

**关联 ID：** `ext-rag-2020` · `rag-privacy-good-bad-2024` · `agent-mem0-2025` · `kv-cache-compression-audit-2025`

---

<a id="paper-superficial-editing-2025"></a>
**69. 揭示知识编辑的表面性｜Revealing the Deceptiveness of Knowledge Editing: A Mechanistic Analysis of Superficial Editing（2025 · ACL 2025）**

**作者：** Jiakuan Xie、Pengfei Cao、Yubo Chen、Kang Liu、Jun Zhao

**书目：** 年份 2025；载体 ACL 2025；状态 同行评议；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Superficial Editing

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 证明常规指标近乎满分时，被编辑模型仍可生成原知识，并定位其内部残留通路。

**问题：** 目标问答改对后，原知识是否已真正不可用？

**机制：** 对多种编辑算法执行超出常规指标的原知识恢复测试，再以因果操作追踪残差流和后层注意模块。

**步骤：**

1. 对已编辑模型测试原知识恢复
2. 定位早层主语位置的残差信号
3. 分析后层注意头与输出矩阵方向
4. 用因果干预检验其与表面编辑的关系

**证据：**

- 在经过可恢复性过滤的 CF-a 和 ZsRE-a 上，常规编辑成功率可以接近满分，但三类攻击前缀仍能大幅恢复旧答案；例如 Llama3 的 CF-a 上多种方法 efficacy 为 94.92–100，而百科前缀下部分旧答案恢复指标约为 70。
- 因果 patching 显示，攻击下早层最后主语位置首先表现为新答案表征受抑制，旧答案并未在早层占优；后层注意力把旧答案线索整合到最后位置后，旧答案概率才反超新答案。
- 后层注意模块、注意头和左奇异向量消融能降低旧答案并提高新答案，但仅是部分缓解，不能据此声称底层旧知识已被删除。

**局限：**

- 只测试百科摘要、旧答案重复和原事实提问三类攻击上下文，无法覆盖真实部署中的全部触发形式。
- 实验覆盖三个开源模型，CF-a 与 ZsRE-a 经过可恢复性过滤后仅有 1,011 与 469 条；攻击恢复率不是自然流量中的发生率。
- 旧知识在特定前缀下可恢复不等于系统会自动或高频触发这些前缀，完整防御仍是开放问题。

**意义：**

- 编辑评估必须包含对抗性恢复、关联问题和内部残留检查。

**边界：** 正式 ACL 页、摘要与作者代码入口核验。

**标识：** DOI 10.18653/v1/2025.acl-long.868

**证据位置：**

- claim 论文用百科摘要、旧答案重复和原事实提问三类前缀定义旧答案恢复测试。；source Knowledge Editing Is Superficial: When LLMs Forget Their True Knowledge；location §2，Eq. (3)–(4)，印刷第 3 页；来源 PDF
- claim 七种编辑方法、三个模型和经过可恢复性过滤的 CF-a、ZsRE-a 上的常规成功率与旧答案恢复率。；source Knowledge Editing Is Superficial: When LLMs Forget Their True Knowledge；location §3.1，Table 1，印刷第 3–4 页；Appendix A，Table 6，印刷第 13 页；来源 PDF
- claim 早层新答案受抑制，后层注意整合旧知识并导致旧答案反超。；source Knowledge Editing Is Superficial: When LLMs Forget Their True Knowledge；location §4.1，Figures 2–5，印刷第 4–6 页；来源 PDF
- claim 后层注意模块、注意头和奇异向量消融只带来部分缓解。；source Knowledge Editing Is Superficial: When LLMs Forget Their True Knowledge；location §4.3，Tables 2–4，印刷第 6–8 页；来源 PDF
- claim 攻击上下文有限，无法穷举，完整缓解仍是开放问题。；source Knowledge Editing Is Superficial: When LLMs Forget Their True Knowledge；location Limitations，印刷第 9 页；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2025.acl-long.868/>) · [PDF](<https://aclanthology.org/2025.acl-long.868.pdf>) · [代码](<https://github.com/jiakuan929/superficial-editing>)

---

<a id="paper-taming-knowledge-conflicts-2025"></a>
**70. 驯服语言模型中的知识冲突｜Taming Knowledge Conflicts in Language Models（2025 · ICML 2025（PMLR 267:34074–34104））**

**作者：** Gaotang Li、Yuzhong Chen、Hanghang Tong

**书目：** 年份 2025；载体 ICML 2025（PMLR 267:34074–34104）；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改、上下文与隐状态记忆；层级 会话或任务期；阅读层级 桥接；证据等级 B；简称 JuICE；优先级 medium；相关性排序 9；时间尺度 推理时参数记忆与当前上下文冲突

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D1 / P=P2 / Q=Q2

**标签：** knowledge-conflict、parametric-memory、contextual-knowledge、mechanistic-intervention

**定位：** 发现参数记忆和上下文信号可叠加于同一注意力头，并以双次推理分别增强某一来源；它是利用层桥梁，不是持久存储。

**问题：** 参数知识与上下文冲突时，简单提示或把头分成互斥类别无法稳定控制模型信任哪一来源。

**机制：** 在小型识别集上测量注意力头缩放方向，筛选跨冲突类型稳定的头；第二次推理重用第一次运行缓存并定向缩放。

**步骤：**

1. 构造参数—上下文冲突识别集
2. 测量各头缩放对两类答案概率的影响
3. 筛选作用方向稳定的头
4. 双次推理定向增强指定知识来源

**证据：**

- 正式 PMLR 论文在 11 个数据集、6 种开源架构上分别测试增强参数信念和上下文依赖；Tables 3–4 支持受测配置中的一致改进。
- Figure 1/§3 的主要贡献是同一注意力头中的上下文—参数叠加；这是一项推理时机制发现与干预，不是写入、保持、删除或更新。
- 双次推理和小型头识别集引入额外延迟、内存与分布迁移边界；不能把论文的 SOTA 叙述外推到所有 RAG 冲突。

**局限：**

- 双次推理增加延迟和激活缓存
- 实验冲突答案较清晰，真实检索常混合部分相关信息
- 不负责修复、删除或追踪冲突来源

**意义：**

- 冲突处理应区分写入、存储和利用层
- 简单要求相信上下文不保证可靠
- 应与编辑和 RAG 相连但不得作为新持久存储路线

**建议路线：** 参数记忆—上下文冲突利用

**边界：** 全文核验；贡献类型是推理时利用层桥接，因此 D1、bridge、level 1。

**版本：** ICML 2025，PMLR 267:34074–34104；无 DOI；使用 PMLR 正式 raw PDF。

**标识：** 稳定 ID pmlr:v267:li25c；工作族 ID taming-knowledge-conflicts-2025

**证据位置：**

- claim 上下文—参数叠加；location 正式 PDF Figure 1、§3；来源 PDF
- claim 头筛选和双次推理；location 正式 PDF §4、Algorithm 1；来源 PDF
- claim 参数与上下文两组实验；location 正式 PDF §5、Tables 3–4；来源 PDF
- claim PMLR 正式元数据；location PMLR 267 正式页；来源 一手入口

**资源：** [一手入口](<https://proceedings.mlr.press/v267/li25c.html>) · [PDF](<https://raw.githubusercontent.com/mlresearch/v267/main/assets/li25c/li25c.pdf>) · [代码](<https://github.com/GaotangLi/JUICE>)

**关联 ID：** `a01` · `knowledgeable-educated-guess-2021` · `a03` · `ext-rag-2020`

---

<a id="paper-unlearning-or-obfuscating-2025"></a>
**71. 遗忘还是混淆：通过正常再学习检验 LLM 遗忘的稳健性｜Unlearning or Obfuscating? Jogging the Memory of Unlearned LLMs via Benign Relearning（2025 · ICLR 2025）**

**作者：** Shengyuan Hu、Yiwei Fu、Zhiwei Steven Wu、Virginia Smith

**书目：** 年份 2025；载体 ICLR 2025；状态 同行评议；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Unlearning or Obfuscating?；优先级 1；相关性排序 20

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V4 / D=D3 / P=P3 / Q=Q3

**定位：** 显示多种近似遗忘结果在普通、无恶意的后续再学习后会被逆转，提示其可能只是输出混淆。

**问题：** 静态遗忘基准通过不代表模型在继续训练或更新后仍然忘记目标知识。

**机制：** 在多个遗忘基准上先应用近似遗忘，再进行受控的正常后续学习，观察目标知识和效用是否恢复。

**步骤：**

1. 执行近似遗忘
2. 记录目标知识与保留效用
3. 进行正常后续学习
4. 重新评测目标知识
5. 判断遗忘是否稳健

**证据：**

- ICLR 论文报告，少量、松散相关的正常再学习可逆转多种遗忘方法的效果。
- 结果支持部分方法主要抑制可观察输出，而非稳健移除内部信息的解释。

**局限：**

- 需要后续微调访问，不能直接代表纯 API 使用者的能力。
- 只覆盖三个基准和所选模型/方法。
- 结果针对参数记忆，不是外部记忆库删除。

**意义：**

- 遗忘应在模型生命周期中做更新后复测，而非一次性验收。
- 对“已删除”或“不可恢复”的表述应采用更谨慎证据标准。

**边界：** 防御性稳健性结论；不提供用于恢复被遗忘信息的操作步骤或参数。

**标识：** 工作族 ID unlearning-obfuscating-2406.13356

**证据位置：**

- claim 正常再学习可逆转遗忘效果；location PDF 摘要与 §1；来源 PDF

**资源：** [一手入口](<https://proceedings.iclr.cc/paper_files/paper/2025/hash/18fd48d9cbbf9a20e434c9d3db6973c5-Abstract-Conference.html>) · [PDF](<https://proceedings.iclr.cc/paper_files/paper/2025/file/18fd48d9cbbf9a20e434c9d3db6973c5-Paper-Conference.pdf>)

---

<a id="paper-user-sp-concerns-conversational-ai-2025"></a>
**72. 理解用户对对话式人工智能平台的安全与隐私关切及态度｜Understanding Users’ Security and Privacy Concerns and Attitudes Towards Conversational AI Platforms（2025 · IEEE Symposium on Security and Privacy 2025）**

**作者：** Mutahar Ali、Arjun Arunasalam、Habiba Farrukh

**书目：** 年份 2025；载体 IEEE Symposium on Security and Privacy 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 桥接；证据等级 B；简称 对话平台纵向隐私关切；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 以两年自然发生的社区讨论追踪用户对数据收集、使用、保留、记忆默认和删除控制的关注如何随产品事件变化。

**问题：** 受控访谈通常只有单一时间点，难以观察对话平台上线记忆功能、政策变化和事故后，用户生命周期关切是否发生变化。

**机制：** 研究结合大规模自然语言语料、人工标注、二元分类、主题编码和中断时间序列，将用户关切映射到收集、使用、保留与透明控制阶段。

**步骤：**

1. 收集某大型对话人工智能社区自 2022 年 12 月至 2024 年 7 月约 250 万条帖子，并移除删除、机器人和重复内容。
2. 以 1200 条人工标注训练安全与隐私相关分类器，从全量语料识别 30240 条相关帖子。
3. 对 440 条帖子做主题饱和编码，并用经人工复核的多标签分类器扩展到全部相关帖子。
4. 将功能发布、政策和事故与周度主题量做中断时间序列分析，观察生命周期关注的变化。

**证据：**

- 第 3.1 节报告最终语料约 250 万条，其中分类器识别 30240 条安全与隐私相关帖子，来自 18851 名唯一用户。
- 第 3.2 节以 440 条人工主题编码建立收集、使用、保留、安全漏洞、监管合规及透明控制主题；主题分类器在人工集上报告较高准确率与 F1。
- 第 6 节和图 3 显示功能发布、政策变化与事故对应不同关切波动；第 7.1 节进一步把记忆默认、训练和遗忘诉求识别为仍需治理的问题。

**局限：**

- 只使用单一大型社区，参与者更可能技术熟练，不能代表所有平台、地区或不公开发言的用户。
- 公开讨论为匿名或化名数据，缺少人口属性和实际账户行为，无法把帖子等同于产品遥测。
- 关键词、分类器和事件回归仍可能出现漏检、误分及共同趋势混杂。

**意义：**

- 治理证据应记录功能和政策事件时间，避免把用户对记忆与删除的关注当作稳定不变的截面量。
- 对话平台需要把训练、聊天历史和跨会话记忆的默认值、范围与控制分开说明。

**边界：** 正式版本和 DOI 由 IEEE S&amp;P 2025 程序与 DOI 页面核验；作者公开全文用于核对第 3 至第 7 节、表 1 至表 4和图 1 至图 5。为保护用户，未保留账号名或原帖内容，也未在本记录中复述用户原文。

**引用：** Ali et al., IEEE S&amp;P 2025, DOI 10.1109/SP61157.2025.00241。

**版本：** 采用 IEEE S&amp;P 2025 正式版本族。

**标识：** DOI 10.1109/SP61157.2025.00241；稳定 ID doi:10.1109/sp61157.2025.00241；工作族 ID user-sp-concerns-conversational-ai-2025

**证据位置：**

- 第 3.1 至第 3.3 节：语料、1200 条人工标注、30240 条相关帖子、440 条主题编码和伦理处理。
- 第 4 至第 5 节与图 2：生命周期主题和用户态度。
- 第 6 节与图 3：重大事件的中断时间序列结果。
- 第 7.1 至第 7.3 节：记忆与训练默认、数据控制建议及单社区外推边界。

**资源：** [一手入口](<https://doi.org/10.1109/SP61157.2025.00241>) · [PDF](<https://habiba-farrukh.github.io/files/ConversationalAISP.pdf>)

**关联 ID：** `big-help-big-brother-2025` · `relational-gains-privacy-strains-2026` · `blenderbot3-2022`

---

<a id="paper-wikibigedit-2025"></a>
**73. WikiBigEdit：理解大模型终身知识编辑的极限｜WikiBigEdit: Understanding the Limits of Lifelong Knowledge Editing in LLMs（2025 · ICML 2025（PMLR 267:59326–59354））**

**作者：** Lukas Thede、Karsten Roth、Matthias Bethge、Zeynep Akata、Thomas Hartvigsen

**书目：** 年份 2025；载体 ICML 2025（PMLR 267:59326–59354）；状态 同行评议；出版状态 peer-reviewed；来源类型 benchmark

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改、外部检索与非参数记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 WikiBigEdit；优先级 high；相关性排序 2；时间尺度 超过 50 万问答更新的模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** lifelong-editing、scale、failure-curve、RAG、LoRA-merge、Wikidata

**定位：** 用 Wikidata 时间快照构造超过 50 万问答更新，给出局部编辑、RAG、持续 LoRA 与模型合并在现实规模下的完整失效曲线。

**问题：** 数十或数百个合成事实上的成功可能掩盖数十万次真实事实更新中的累积退化。

**机制：** 从 Wikidata 快照抽取事实变化和问答，按八个时间步连续更新五种模型，统一比较 ROME、R-ROME、MEMIT、WISE、RAG、LoRA-FT 与 LoRA-Merge。

**步骤：**

1. 从 Wikidata 快照抽取事实更新
2. 生成更新、改写、画像、多跳和局部性集合
3. 按八个时间步对五种模型连续应用方法
4. 同时测量准确率、遗忘、多跳与推理成本

**证据：**

- 首版超过 50 万问答对；§4/Figures 3–4 中 ROME、R-ROME、MEMIT 在前数百次顺序编辑内快速退化，WISE 在前 10K 更新内趋近预更新水平。
- RAG 在受测事实更新总体领先，但处理完整 WikiBigEdit 后前向时间最高约翻倍；§4.3/Figure 6 中两跳均正确取回仅约 10%。
- §4.4/Figure 7 中 LoRA-FT 随时间退化，LoRA-Merge 在约 100K 更新后更稳定；这些结论限于五个约 7B 模型、特定嵌入器和 top-2 RAG。

**局限：**

- Wikidata 问答不能代表全部非结构化知识、生成任务或用户记忆
- 自动生成与过滤可能保留模板和实体偏差
- RAG 对照使用特定嵌入器与 top-2，其他检索器会改变质量和成本

**意义：**

- 终身编辑必须报告随编辑数增长的曲线
- 参数编辑、外部检索和持续微调要在同一规模与预算比较
- RAG 的事实更新优势不能掩盖多跳瓶颈

**建议路线：** 终身编辑规模与失效评测

**边界：** PMLR 正式页与 PDF 全文核验；原 PMLR 子目录 PDF URL 已更正为正式 raw 入口。

**版本：** ICML 2025，PMLR 267:59326–59354；无 DOI；使用 PMLR 正式 raw PDF。

**标识：** 稳定 ID pmlr:v267:thede25a；工作族 ID wikibigedit-2025

**证据位置：**

- claim 数据构建、规模和评测轴；location 正式 PDF §3、Figures 1–2、Tables 1–3；来源 PDF
- claim 编辑、RAG 与 LoRA 总体比较；location 正式 PDF §§4.1–4.2、Figures 3–4，pp. 6–7；来源 PDF
- claim 多跳 RAG 瓶颈；location 正式 PDF §4.3、Figure 6；来源 PDF
- claim 持续微调与合并；location 正式 PDF §4.4、Figure 7；来源 PDF
- claim 顺序编辑和批量边界；location 正式 PDF Appendix Figures 19–20，pp. 27–28；来源 PDF

**资源：** [一手入口](<https://proceedings.mlr.press/v267/thede25a.html>) · [PDF](<https://raw.githubusercontent.com/mlresearch/v267/main/assets/thede25a/thede25a.pdf>) · [代码](<https://github.com/lukasthede/WikiBigEdit>)

**关联 ID：** `a03` · `a06` · `a07` · `a08` · `model-editing-harms-2024` · `hipporag2-2025`

---

<a id="paper-algorithmic-self-portrait-2026"></a>
**74. 算法自画像：解构 ChatGPT 的记忆｜The Algorithmic Self-Portrait: Deconstructing Memory in ChatGPT（2026 · The ACM Web Conference 2026）**

**作者：** Abhisek Dash、Soumi Das、Elisabeth Kirsten、Qinyuan Wu、Sai Keerthana Karnam、Krishna P. Gummadi、Thorsten Holz、Muhammad Bilal Zafar、Savvas Zannettou

**书目：** 年份 2026；载体 The ACM Web Conference 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Algorithmic Self-Portrait；优先级 high；时间尺度 真实产品的跨会话个性化记忆

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** product-memory-audit、data-donation、user-agency、privacy

**定位：** 用用户依法导出的真实交互和记忆条目，量化谁在创建记忆、记忆包含什么、是否忠实来源，并用查询改写降低个人归因。

**问题：** 产品记忆由封闭系统生成，用户难以知道系统何时保存、保存了哪些敏感或心理属性，以及这些内容如何形成。

**机制：** 研究者从真实用户数据导出中定位触发记忆更新的查询，分别审计创建能动性、个人数据、心理属性和来源；随后训练或提示开放模型模仿记忆抽取，并在高风险时生成更弱归因的查询改写。

**步骤：**

1. 从80名真实用户的数据导出中提取1058个相关会话、22971个轮次和2050条记忆更新
2. 检测显式记忆操作，区分用户主动与系统单方面触发
3. 按个人数据、心理属性和来源忠实度标注记忆内容
4. 用 Attribution Shield 预测潜在记忆和风险，再建议保留意图但降低个人归因的查询改写

**证据：**

- 摘要、第3–4节和 Figure 1 显示2050条记忆中仅84条约4.1%由用户显式发起，其余约95.9%由系统单方面创建。
- 第4–6节和 Figure 2 显示28%的记忆含个人数据，52%含至少一种心理属性，84%可直接追溯到用户文本。
- 第7.4节和 Table 5 的小规模验证中，细调与上下文改写分别有94.4%和100%被判断为更弱个人归因；人工检查称效用保留约87%和91%。
- 第8节明确限制为80名以美国和欧洲为主的用户、单一产品以及依赖不完美的模型标注代理。

**局限：**

- 参与者规模有限且主要来自美国和欧洲，不能代表全球用户。
- 只审计一个会持续变化的产品实现，跨平台外推需要复现。
- 部分敏感性和心理属性标注依赖模型判断，虽有人类复核仍可能遗漏语义细节。
- Attribution Shield 只在约36条首次触发记忆的查询上做直接隐私改写验证。

**意义：**

- 产品记忆审计应以实际导出条目为对象，而不能只依赖用户感受或设置说明。
- 显式查看、更新和删除入口不足以弥补系统单方面写入的能动性差距。
- 敏感信息治理应在写入前预测潜在推断，并保留来源和触发链。

**边界：** ACM DOI 与 WWW 2026 正式元数据由 DOI 和 TU Delft 机构页核验；全文证据由作者公开稿核验。敏感用户内容未在审计记录中复述。

**标识：** DOI 10.1145/3774904.3792671；稳定 ID doi:10.1145/3774904.3792671；工作族 ID algorithmic-self-portrait

**证据位置：**

- claim 数据集与用户能动性；location 正式全文摘要、第3–4节、Figure 1；来源 PDF
- claim 敏感信息与心理属性；location 正式全文第4–5节、Figure 2；来源 PDF
- claim 来源忠实度；location 正式全文第6节、Figure 4；来源 PDF
- claim Attribution Shield；location 正式全文第7节、Tables 4–5、Figure 5；来源 PDF
- claim 限制与伦理处理；location 正式全文第8节；来源 PDF

**资源：** [一手入口](<https://doi.org/10.1145/3774904.3792671>) · [PDF](<https://arxiv.org/pdf/2602.01450>)

**关联 ID：** `relational-gains-privacy-strains-2026` · `big-help-big-brother-2025` · `recallbot-2026`

---

<a id="paper-demystify-memory-mle-2026"></a>
**75. 揭示记忆在机器学习工程智能体中的作用｜Demystify the Role of Memory in Machine Learning Engineering Agents（2026 · Findings of ACL 2026）**

**作者：** Xinyu Zhao、Junpeng Wang、Yuzhong Chen、Menghai Pan、Chin-Chia Michael Yeh、Jiarui Sun、Yan Zheng、Mahashweta Das、Tianlong Chen

**书目：** 年份 2026；载体 Findings of ACL 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、智能体记忆管理；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Demystify Memory in MLE Agents；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 同一动态错误修复记忆帮助链式代理，却压缩树式搜索多样性并降低最终方案质量。

**问题：** 代理记忆的效果是否独立于智能体架构，还是会在可靠性和探索性之间产生方向相反的影响？

**机制：** 把任务、软件包、错误和修复写成结构记录，并用相同记忆接入链式OpenHands和树式AIDE作受控比较。

**步骤：**

1. 从跨运行日志抽取错误与修复记录
2. 按任务和嵌入检索相关记忆
3. 新错误经去重后选择不变或追加
4. 在链式和树式代理中比较可靠性、探索性与成绩

**证据：**

- Table 1显示记忆提高链式代理的可靠性和任务成功。
- Tables 1至2及Figures 3至4显示树式代理虽减少重复错误，却降低探索多样性并可能损害最终成绩。
- Table 7追踪错误复现，支持记忆打破重复故障的机制解释。

**局限：**

- 只覆盖两种预设代理架构和机器学习工程任务。
- 主要是同任务跨运行检索，不能证明广泛跨任务迁移。
- 错误记忆也可能传播，论文没有长期人工监督部署。

**意义：**

- 记忆评测必须按代理搜索架构分层，不能假设更多记忆总是更好。
- 可靠性与探索多样性应作为一对联合指标。

**边界：** 正式论文页核验元数据与出版状态；公开全文核验机制、表图证据和局限。

**引用：** Zhao等，Findings of ACL 2026，DOI 10.18653/v1/2026.findings-acl.525。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.18653/v1/2026.findings-acl.525；稳定 ID doi:10.18653/v1/2026.findings-acl.525；工作族 ID demystify-memory-mle-2026

**证据位置：**

- Figure 2与第3.3节，印刷第10813至10815页：构建、检索和更新
- 第4.2至4.6节、Tables 1至5、Figures 3至4，印刷第10816至10819页：正负作用反转
- Limitations，印刷第10820页

**资源：** [一手入口](<https://aclanthology.org/2026.findings-acl.525/>) · [PDF](<https://aclanthology.org/2026.findings-acl.525.pdf>)

**关联 ID：** `memory-management-impact-2026` · `agent-expel-2024` · `steem-2026`

---

<a id="paper-does-memory-need-graphs-2026"></a>
**76. 对话长期记忆需要图吗：统一框架与受控实证分析｜Does Memory Need Graphs? A Unified Framework and Empirical Analysis for Long-Term Dialog Memory（2026 · ACL 2026）**

**作者：** Sen Hu、Yuxiang Wei、Jiaxin Ran、Xueran Han、Zhiyuan Yao、Huacan Wang、Ronghao Chen、Lei Zou

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；来源类型 evaluation

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、智能体记忆管理、个性化与用户长期记忆、外部检索与非参数记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Does Memory Need Graphs?；时间尺度 跨会话对话历史的持久记忆

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**标签：** graph-memory、flat-memory、controlled-comparison、LongMemEval、HaluMem

**定位：** 在统一底座、预算与组件分解下比较图式和非图式对话记忆，证明图的收益取决于实现配置，不能把结构本身当成默认优势。

**问题：** 图记忆的公开增益究竟来自关系结构，还是来自键值表示、抽取器、返回预算、重排和底座差异？

**机制：** 把对话记忆系统分解为 memory key/value、写入与更新操作、平面或图索引、激活与扩展、重排和回答；在 LongMemEval 与 HaluMem 上逐阶段控制变量，再比较平面强基线与图强基线。

**步骤：**

1. 从会话抽取摘要、事实和关键词，或抽取实体、关系及实体描述
2. 分别执行平面索引与多种图构建，并控制 Add、Update、Noop 操作
3. 统一检索预算，比较实体激活、关系激活、一跳扩展和重排策略
4. 在 LongMemEval 与 HaluMem 上分离检索、抽取、更新和端到端问答结果

**证据：**

- Tables 2–6 显示键的组织、更新操作、图 schema 与重排都会显著改变结果；相似边会引入噪声，一跳扩展若重排不足也会退化。
- Table 7 中图强基线在 LongMemEval 的检索指标通常更好；但 Value=Key 时，四组端到端问答结果均由平面强基线领先，说明更高召回不会自动转化为更好回答。
- Table 8 与第 4.6 节显示 HaluMem 上结论依赖抽取模型与记忆粒度；弱抽取器下图方法的记忆召回和问答会明显变差。

**局限：**

- 统一框架不能覆盖 MemoryOS 等所有高度定制系统，也没有穷尽异构图、层级图、训练式图检索和全部更新操作。
- 主要只评测 LongMemEval 与 HaluMem，两套基准的任务粒度和标注方式会影响图与平面索引的相对优势。
- 底座组合主要是 LLaMA-3.1-8B＋Contriever 和 GPT-4o-mini＋text-embedding-3-small，仅对 Qwen3-8B 做部分补充。

**意义：**

- 图记忆必须与相同抽取器、返回预算、key/value 粒度和重排条件下的平面强基线比较。
- 部署报告应公开每个组件和预算，不能只报告端到端系统名与最终分数。
- 该工作应与 A-Mem、Zep 等图式正面方法配对，防止把结构复杂度误写为稳定收益。

**边界：** 题名、作者、DOI、机制、表格结果和限制均由 ACL Anthology 正式页与 PDF 核验；结论刻意保留图在部分检索与 Value=session 设置中的优势。

**版本：** 以 ACL 2026 主会长文正式版为准。

**标识：** DOI 10.18653/v1/2026.acl-long.1232；工作族 ID does-memory-need-graphs

**证据位置：**

- claim 统一组件框架和两套基准的受控设计；location PDF 第 3 节与第 4.1 节；来源 PDF
- claim 键、更新操作、图 schema 与检索策略的阶段实验；location PDF Tables 2–6／第 4.2–4.5 节；来源 PDF
- claim 平面与图强基线的端到端比较；location PDF Tables 7–8／第 4.6 节；来源 PDF
- claim 适用边界；location PDF Conclusion 与 Limitations；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.1232/>) · [PDF](<https://aclanthology.org/2026.acl-long.1232.pdf>) · [代码](<https://github.com/AvatarMemory/UnifiedMem>)

**关联 ID：** `agent-amem-2025` · `agent-zep-2025` · `agent-mem0-2025` · `longmemeval-2025`

---

<a id="paper-fragfuse-2026"></a>
**77. FragFuse：长期记忆跨交互重组导致的智能体访问控制绕过｜FragFuse: Bypassing Access Control of Large Language Model Agents via Memory-Based Query Fragmentation and Fusion（2026 · USENIX Security 2026（Short Presentation））**

**作者：** Zixin Rao、Wentian Zhu、Chan Aristella Lu、Zhaorun Chen、Wei Niu、Le Guan、Bo Li、Zhen Xiang

**书目：** 年份 2026；载体 USENIX Security 2026（Short Presentation）；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 FragFuse；优先级 high；相关性排序 6；时间尺度 跨多次交互的长期记忆安全

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** memory-security、access-control、cross-interaction、stateful-risk、deweaponized

**定位：** 证明逐轮访问控制会遗漏长期记忆跨交互组合形成的状态风险，把记忆安全从单消息过滤提升为有状态信息流问题。

**问题：** 只审查当前输入时，跨轮保留的信息可能在检索后形成新的组合语义，使权限判断与最终执行脱节。

**机制：** 论文构造跨交互威胁模型并在多种智能体和访问控制上做防御性压力测试。本记录只保留高层状态通道、汇总结果与防御含义，不记录片段选择、标记、提示、载荷或优化步骤。

**步骤：**

1. 识别长期记忆跨轮保留与组合形成的时间通道
2. 在四类智能体和三种访问控制机制上进行受控压力测试
3. 比较绕过、端到端风险和正常任务退化
4. 评估单轮检测与执行后检查的覆盖边界

**证据：**

- 正式全文 §5.2/Table 1：四类智能体、每类两种访问控制配置及多个骨干上的平均 BSR 为 86.3%，平均 E2E-SR 为 41.1%，直接基线为 5.7%。
- 在成功绕过的条件下，相对无访问控制配置的平均 TSR 下降 4.4 个百分点；这是受测配置平均，不是所有生产系统结论。
- §6–7 显示提示注入或困惑度检测不足，并明确任务域、相似性检索、黑盒假设和执行后防御等边界。

**局限：**

- 四类研究设置不能代表所有生产智能体
- 主要假设长期记忆可被后续相似性检索，其他隔离架构风险不同
- 证明风险存在但没有验证完整生命周期访问控制方案

**意义：**

- 访问控制必须审查记忆读写后的组合语义与来源
- 应按主体、会话和权限域隔离记忆并在执行前重新授权
- 公开卡片只给防御性汇总，不提供复现攻击细节

**建议路线：** 跨轮记忆安全与访问控制

**边界：** 纠正原非法 verification\_state；已全文核验，但普通同行评议不升级为 P3，且全部内容已降敏。

**版本：** USENIX Security 2026 正式节目收录为 Short Presentation；全文以同题同作者公开稿核验。

**标识：** 稳定 ID usenix-security-2026:rao；工作族 ID fragfuse-2606.15609

**证据位置：**

- claim 正式接收、作者和 Short Presentation 类别；location USENIX Security 2026 正式论文页；来源 一手入口
- claim 四类设置与汇总结果；location 公开全文 §5.2、Table 1，pp. 8–9；来源 PDF
- claim 防御分析；location 公开全文 §6，pp. 11–12；来源 PDF
- claim 局限和伦理；location 公开全文 §7、Ethical Considerations，pp. 12–13；来源 PDF

**资源：** [一手入口](<https://www.usenix.org/conference/usenixsecurity26/presentation/rao>) · [PDF](<https://arxiv.org/pdf/2606.15609>)

**关联 ID：** `agentpoison-2024` · `minja-2025` · `poisonedrag-2025` · `mextra-2025`

---

<a id="paper-lifedialbench-2026"></a>
**78. 连续生活日志场景中的记忆能力评测｜Evaluating Memory Capability in Continuous Lifelog Scenario（2026 · Findings of ACL 2026）**

**作者：** Jianjie Zheng、Zhichen Liu、Zhanyu Shen、Jingxiang Qu、Guanhua Chen、Yile Wang、Yang Xu、Yang Liu、Sijie Cheng

**书目：** 年份 2026；载体 Findings of ACL 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 benchmark

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆、外部检索与非参数记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 LifeDialBench；优先级 high；相关性排序 3；时间尺度 七天真实事件序列锚定的合成对话与一年模拟生活流

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** lifelog、online-causal-evaluation、future-leakage、synthetic-dialogue、graph-vs-rag

**定位：** 用真实第一视角事件序列作锚但由模型生成全部对话，并以严格先写入、冻结、再回答的在线协议量化未来信息泄漏。

**问题：** 离线基准常让系统看见查询之后的信息；同时，真实事件锚定不应被误写为真实用户对话或真实闭环部署。

**机制：** EgoMem 从真实 EgoLife/Ego-R1 事件序列的结构化摘要出发，由模型生成对话；LifeMem 模拟一年生活。在线协议逐会话写入，到查询时冻结，在未来数据进入前回答。

**步骤：**

1. 从真实事件摘要或模拟生活轨迹构造带时间戳对话
2. 逐会话写入且不暴露未来内容
3. 查询时冻结状态并立即回答
4. 与可见完整历史的离线结果比较未来泄漏

**证据：**

- 正式 PDF Table 1：EgoMem 为 7 天、约 1.7K sessions；LifeMem 为模拟 1 年、约 3.8K sessions。§3 明确全部对话和摘要由 Qwen 生成，EgoMem 只是由真实第一视角事件序列锚定。
- Table 3 中简单 RAG 与 A-Mem 多项指标高于 Mem0/MemoryOS；正文统计检验未支持 A-Mem 稳定优于 RAG。
- §5 中未来检索对答案正确性的 AUROC 约 0.64/0.68；Mem0 的 34.91 只来自带时间过滤并取 top-100 的选定子集，不能泛化到完整基准。

**局限：**

- 对话均为模型生成，不能称真实生活对话
- EgoMem 真实事件锚点仅七天，LifeMem 是年度模拟
- 文本化评测不等于多模态记忆或真实在线产品闭环

**意义：**

- 纵向证据应分别标记真实事件、真实对话和真实闭环
- 离线全历史评测必须审计未来泄漏
- 复杂记忆结构应与普通 RAG 做显著性检验

**建议路线：** 在线时序记忆评测

**边界：** ACL 正式页和 PDF 全文核验；纠正原 round4 对真实对话和 AUROC 的过度表述。

**版本：** 以 ACL Findings 正式版为准；必须保留真实事件序列锚点与合成对话的区别。

**标识：** DOI 10.18653/v1/2026.findings-acl.351；稳定 ID doi:10.18653/v1/2026.findings-acl.351；工作族 ID lifedialbench-2026

**证据位置：**

- claim 全部对话为模型生成及真实事件锚点；location 正式 PDF §§3.1–3.3、Table 1，pp. 2–3；来源 PDF
- claim 在线时序协议；location 正式 PDF §3.6、Figure 2，pp. 4–5；来源 PDF
- claim 系统比较；location 正式 PDF §4、Table 3，p. 7；来源 PDF
- claim 未来泄漏与选定子集分析；location 正式 PDF §5，p. 8；来源 PDF
- claim 伦理与边界；location 正式 PDF Limitations、Ethical Considerations；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2026.findings-acl.351/>) · [PDF](<https://aclanthology.org/2026.findings-acl.351.pdf>)

**关联 ID：** `locomo-2024` · `longmemeval-2025` · `does-memory-need-graphs-2026` · `agent-amem-2025` · `agent-mem0-2025`

---

<a id="paper-locomo-plus-2026"></a>
**79. Locomo-Plus：超越事实回忆的 LLM 智能体认知记忆评测框架｜Locomo-Plus: Beyond-Factual Cognitive Memory Evaluation Framework for LLM Agents（2026 · ACL 2026）**

**作者：** Yifei Li、Weidong Guo、Lingling Zhang、Rongman Xu、Muye Huang、Hui Liu、Lijiao Xu、Yu Xu、Jun Liu

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；来源类型 benchmark

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 LoCoMo-Plus；优先级 1；相关性排序 11

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 把长期记忆从显式事实召回扩展到用户状态、目标和价值等隐式约束的一致应用。

**问题：** 字符串匹配型问答无法判断智能体是否在未被直接提醒时仍遵守用户的潜在约束。

**机制：** 构造因果、状态、目标和价值四类隐式约束，使后续查询与早期线索缺少直接语义重合；经相似度过滤和人工验证后嵌入 LoCoMo 长对话，并以回答是否满足约束而非字符串重合评价。

**步骤：**

1. 从历史建立潜在用户约束
2. 在后续引入语义不直连的触发情境
3. 生成响应
4. 以约束一致性评价是否正确使用记忆

**证据：**

- 表 1 中所有模型和记忆系统在 LoCoMo-Plus 上均明显低于原 LoCoMo；三种专门记忆系统的认知记忆得分为 14.90 至 17.20，而原 LoCoMo 平均分为 57.24 至 59.64。
- 图 5 和图 6显示任务类型提示与输出长度会系统性影响传统评测，支持统一自然查询和约束一致性判断。
- 表 2 中模型裁判与两位人工的一致度为 0.801 和 0.820；表 3 中更换裁判后的分数差为 0.68 至 3.33，说明裁判具有实证可靠性但并非无误。

**局限：**

- 基准优先诊断价值而非规模，主要实例经生成、过滤与人工验证，不等于真实长期用户轨迹，也不适合训练大型模型。
- 不覆盖长期信念修订、情感动态和多智能体记忆交互。
- 仅英语，并依赖特定模型、检索系统、裁判模型与提示设计。

**意义：**

- 提醒系统不能只优化事实问答 F1。
- 把个性化质量推进到行为层面的潜在约束遵循。

**边界：** 已核验 ACL Anthology 正式全文；将该工作定位为较小的诊断性认知记忆基准，而非完整的人类记忆模型。

**标识：** DOI 10.18653/v1/2026.acl-long.1150

**证据位置：**

- claim 线索与触发语义断开及基准构造；location 第 3 至 4 节；图 3；来源 PDF
- claim 模型、检索方法和记忆系统的整体结果；location 表 1；第 6.1 至 6.2 节；来源 PDF
- claim 评测偏差、裁判可靠性与范围限制；location 图 5 至图 7；表 2 至表 3；第 6.3 至 6.5 节；Limitations；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.1150/>) · [PDF](<https://aclanthology.org/2026.acl-long.1150.pdf>) · [代码](<https://github.com/xjtuleeyf/Locomo-Plus>)

---

<a id="paper-mem-gallery-2026"></a>
**80. Mem-Gallery：多模态大模型智能体长期对话记忆基准｜Mem-Gallery: Benchmarking Multimodal Long-Term Conversational Memory for MLLM Agents（2026 · ACL 2026）**

**作者：** Yuanchen Bei、Tianxin Wei、Xuying Ning、Yanjun Zhao、Zhining Liu、Xiao Lin、Yada Zhu、Hendrik Hamann、Jingrui He、Hanghang Tong

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆、外部检索与非参数记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Mem-Gallery；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把视觉与文本信息放入跨会话长轨迹，首次同时测记忆抽取、推理、知识管理和成本。

**问题：** 文本长期记忆基准不能回答多模态信息在跨会话中如何保留、组织、演化以及代价多大。

**机制：** 构建图文共同驱动的多会话对话和评价问答，以统一环境运行十二种记忆系统，并按三个功能维度与效率拆解表现。

**步骤：**

1. 生成含视觉与文本依赖的多会话长轨迹
2. 从轨迹构造抽取与适应、推理、知识管理三类任务
3. 在统一对话环境中运行十二种记忆系统
4. 联合比较正确性、检索表现和令牌成本

**证据：**

- Table 4显示显式保留多模态信息和组织记忆是主要收益来源，但所有方案在推理与知识管理上仍有明显缺口。
- Table 5显示不同记忆系统的令牌消耗差异很大，性能提升不能脱离成本判断。
- Table 6显示检索规模变化会改变命中与效率，单纯扩大返回集合不是稳定解法。

**局限：**

- 对话由受控流程生成，不等同于多年真实用户交互。
- 主要评测现成系统，不证明某一记忆架构在部署中长期稳定。
- 多模态数据和评测器仍可能带有生成模型偏差。

**意义：**

- 长期记忆基准需要把模态保留、推理、管理和成本分开计分。
- 多模态跨会话状态应成为独立二级分支，不能由文本长上下文替代。

**边界：** 正式论文页核验元数据与出版状态；公开全文核验机制、表图证据和局限。

**引用：** Bei等，ACL 2026，DOI 10.18653/v1/2026.acl-long.1892。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.18653/v1/2026.acl-long.1892；稳定 ID doi:10.18653/v1/2026.acl-long.1892；工作族 ID mem-gallery-2026

**证据位置：**

- Figure 3与第3节，PDF第3至6页：数据生成、环境和三维评测框架
- Table 4，PDF第7页：十二种记忆系统主结果
- Tables 5至6，PDF第9页：令牌成本和检索规模

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.1892/>) · [PDF](<https://aclanthology.org/2026.acl-long.1892.pdf>)

**关联 ID：** `locomo-2024` · `longmemeval-2025` · `memora-2026`

---

<a id="paper-mem2actbench-2026"></a>
**81. Mem2ActBench：评估任务型自主智能体的长期记忆利用｜Mem2ActBench: A Benchmark for Evaluating Long-Term Memory Utilization in Task-Oriented Autonomous Agents（2026 · ACL 2026）**

**作者：** Yiting Shen、Kun Li、Wei Zhou、Songlin Hu

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；来源类型 benchmark

**分类：** 主路线 评测、安全与治理；相关路线 智能体记忆管理、评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Mem2ActBench

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 将长期记忆评价从被动事实问答推进到主动选工具、填参数和完成任务。

**问题：** 智能体能召回旧信息，是否意味着它能在新任务中主动使用这些信息？

**机制：** 构造长期、中断式的用户—助手—工具历史，将事实冲突整理为演化链，再反向生成必须借助历史才能正确填参的工具任务。

**步骤：**

1. 融合任务对话与普通对话噪声
2. 构建含冲突解决的事实演化链
3. 反向生成记忆依赖的工具使用任务
4. 分别检查工具选择与参数落地

**证据：**

- ACL 正式摘要报告数据包含 2,029 个会话、400 个工具任务，人工评估确认 91.3% 任务强依赖长期记忆；7 个所测记忆框架在参数落地上仍不充分。

**局限：**

- 数据由多源资料和模型生成流程组装，不等于真实用户的长期工具日志。
- 受测框架和工具类型有限，不能直接推广到所有自主智能体。

**意义：**

- 上线评测应把‘召回正确’和‘在行动中正确应用’作为两个独立门禁。

**边界：** 正式 ACL 页、摘要和 PDF 表 1/方法部分核验。

**标识：** DOI 10.18653/v1/2026.acl-long.370

**证据位置：**

- claim 2,029 会话、400 任务与 91.3% 强记忆依赖；location ACL 正式摘要；来源 一手入口
- claim 与被动问答基准的比较和任务构建；location PDF Table 1 与第 3 节；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.370/>) · [PDF](<https://aclanthology.org/2026.acl-long.370.pdf>) · [代码](<https://github.com/Cantaloupe-M/Mem2ActBench>)

---

<a id="paper-memora-2026"></a>
**82. 从回忆到遗忘：个性化智能体长期记忆基准｜From Recall to Forgetting: Benchmarking Long-Term Memory for Personalized Agents（2026 · Findings of ACL 2026）**

**作者：** Md Nayem Uddin、Kumar Shubham、Eduardo Blanco、Chitta Baral、Gengyu Wang

**书目：** 年份 2026；载体 Findings of ACL 2026；状态 同行评议；来源类型 benchmark+dataset

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Memora；优先级 1；相关性排序 10

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 用跨数周至数月的 Memora 和遗忘感知指标 FAMA 评价记忆、推理、推荐与旧记忆失效。

**问题：** 只考事实召回会奖励“记得越多”，却不惩罚系统继续使用已过时或被否定的信息。

**机制：** 构造含记忆巩固、更新和失效事件的长程用户历史；在三种任务上评价，并用 FAMA 对引用无效记忆施加惩罚。

**步骤：**

1. 生成跨周或月的用户互动
2. 注入更新、巩固与失效关系
3. 执行记忆、推理和推荐任务
4. 识别答案依据的记忆状态
5. 以 FAMA 惩罚无效记忆复用

**证据：**

- 正式摘要报告四个 LLM 和六个记忆智能体经常复用无效记忆，难以协调演化信息。
- 现有记忆智能体只带来有限改进，显示长期个性化的核心困难不只是检索召回。

**局限：**

- 长历史主要由模拟流程生成，不能代表真实生活事件频率。
- 指标和开放式任务部分依赖自动裁判，需报告裁判版本和人类一致性。
- 对话数据的敏感信息治理说明有限，不应当作隐私安全数据集。

**意义：**

- 把遗忘、更新和推荐纳入长期记忆的主评价目标。
- 与 Keep Me Updated 一起构成从机制到基准的直接证据链。

**边界：** 正式目录、全文与作者仓库交叉核验；若主站单篇 URL短时不可访问，DOI 和卷目录仍提供正式元数据。

**版本：** 题名最初以 arXiv 发布；出版状态依据 Findings ACL 2026 正式条目，不能标为预印本。

**标识：** DOI 10.18653/v1/2026.findings-acl.1337；工作族 ID memora-2604.20006

**证据位置：**

- claim 跨周至月、三类任务、FAMA 与无效记忆复用；location 论文摘要、§3–§5、Table 3；来源 定位入口 1
- claim 正式 Findings ACL 2026 状态；location ACL Anthology 卷目录与作者正式页；来源 定位入口 2
- claim 代码与数据入口；location 官方仓库 README；来源 代码

**证据定位入口：** [定位入口 1](<https://arxiv.org/abs/2604.20006>) · [定位入口 2](<https://aclanthology.org/volumes/2026.findings-acl/>)

**资源：** [一手入口](<https://aclanthology.org/2026.findings-acl.1337/>) · [PDF](<https://arxiv.org/pdf/2604.20006>) · [代码](<https://github.com/geniesinc/Memora>)

---

<a id="paper-memory-management-impact-2026"></a>
**83. 记忆管理如何影响 LLM 智能体：经验遵循行为的实证研究｜How Memory Management Impacts LLM Agents: An Empirical Study of Experience-Following Behavior（2026 · ACL 2026）**

**作者：** Zidi Xiong、Yuping Lin、Wenya Xie、Pengfei He、Zirui Liu、Jiliang Tang、Himabindu Lakkaraju、Zhen Xiang

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Memory Management Impact；优先级 1；相关性排序 13；时间尺度 跨会话长期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 通过控制记忆添加和删除，揭示智能体对相似经验的强跟随、错误传播与误导性经验重放。

**问题：** 记忆库被视为无害经验集合，但添加或保留哪些记录会系统性改变后续行为。

**机制：** 比较固定记忆、全部加入、评价器筛选加入，以及周期删除、基于未来使用效用的删除和组合删除；以四个代理量化经验遵循、错误传播与错位经验重放。

**步骤：**

1. 记录历史执行经验
2. 控制记忆添加或删除
3. 检索与当前任务相似的经验
4. 测量经验遵循与长期表现
5. 用未来评价回标经验质量

**证据：**

- 表 1 中全部加入在四个代理上均弱于严格筛选；例如 EHRAgent 为 13.05 对 38.50，AgentDriver 为 32.32 对 51.00，说明记忆质量与数量共同影响长期表现。
- 图 3 支持当前查询与检索记录的输入越相似，代理输出越倾向跟随既有执行；接近 1 的相关性只出现在 RegAgent 的特定设置。
- 图 4显示错误经验会传播；表 2显示可靠的未来效用评价可指导删除，但收益依赖评价器与代理，组合删除主要在性能和存储间折中。

**局限：**

- 严格人工评价器在实验中由真值比较模拟，生产任务未必具有同等可靠的反馈。
- 只研究添加和删除，未覆盖合并、重写、总结、反思和结构变换。
- 结论来自经验分析，没有形式理论保证；多数实验使用 GPT-4o-mini。

**意义：**

- 为记忆污染提供非攻击性的行为机制证据。
- 说明删除、隔离和质量审计是长期性能控制，而不只是隐私功能。

**边界：** 已核验 ACL Anthology 正式全文；将跨代理经验现象与普遍理论定律明确区分。

**标识：** DOI 10.18653/v1/2026.acl-long.27

**证据位置：**

- claim 不同添加策略的长期表现；location 表 1；第 3.1 至 3.2 节；来源 PDF
- claim 经验遵循与错误传播；location 图 3 至图 4；第 3.3 至 3.4 节；来源 PDF
- claim 删除策略、错位重放与范围限制；location 表 2；图 6；第 4 节；Limitations；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.27/>) · [PDF](<https://aclanthology.org/2026.acl-long.27.pdf>)

---

<a id="paper-memoryagentbench-2026"></a>
**84. 通过增量多轮交互评测大模型智能体记忆｜Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions（2026 · ICLR 2026）**

**作者：** Yuanzhe Hu、Yu Wang、Julian McAuley

**书目：** 年份 2026；载体 ICLR 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 benchmark

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、智能体记忆管理、外部检索与非参数记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 MemoryAgentBench；优先级 high；相关性排序 5；时间尺度 信息逐轮到达的增量长期交互

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** incremental-evaluation、test-time-learning、selective-forgetting、benchmark

**定位：** 把长上下文按时间拆成多轮输入，统一评测准确检索、测试时学习、长程理解和选择性遗忘，防止一次性读取完整历史。

**问题：** 静态长文问答无法测试系统何时写入、如何更新和何时遗忘，并让完整上下文模型获得协议优势。

**机制：** 逐块向被测系统提供信息并要求其自行维护状态；重构既有数据并增加任务，以同一协议比较完整上下文、RAG、外部记忆和工具型智能体。

**步骤：**

1. 按时间顺序把长上下文拆成多轮输入
2. 每轮由系统自行写入、更新或压缩
3. 在四类能力任务提出查询
4. 用同一协议比较上下文、RAG 与专用记忆系统

**证据：**

- 正式 PDF Table 1 将四项能力、增量协议和系统类型统一到 2,071 个问题、约 103K 到 1.44M token 的任务上。
- Table 3 所列系统没有一项同时掌握四种能力；表内最高总体分约 60.6，而多跳选择性遗忘最高约 28%，数字只适用于对应任务与模型。
- 第四能力在 ICLR 正式版本中是 selective forgetting；早期页面的 conflict resolution 表述不得继续使用。

**局限：**

- 部分任务由静态数据重构，交互形式真实不等于内容来自真实用户
- 参数记忆、多模态、隐私与成本覆盖有限
- 聚合分数不能替代四能力向量

**意义：**

- 基准应逐轮注入并防止未来信息泄漏
- 选择性遗忘应与冲突、删除合规和错误恢复分测
- 系统排名应按能力向量报告

**建议路线：** 增量交互记忆评测

**边界：** ICLR 正式节目页和 OpenReview 正式 PDF 全文核验。

**版本：** ICLR 2026 正式 Poster；OpenReview id ZgQ0t3zYTQ，正式能力名为 selective forgetting。

**标识：** 稳定 ID openreview:ZgQ0t3zYTQ；工作族 ID memoryagentbench-2026

**证据位置：**

- claim 四项能力与增量协议；location 正式 PDF §2.1、Figure 1，pp. 2–3；来源 PDF
- claim 数据规模与基准比较；location 正式 PDF §2.2、Table 1，p. 3；来源 PDF
- claim 系统与主结果；location 正式 PDF §§3–4、Table 3，p. 7；来源 PDF
- claim ICLR 接收状态；location ICLR 2026 正式 Poster 页面；来源 一手入口

**资源：** [一手入口](<https://iclr.cc/virtual/2026/poster/10010781>) · [PDF](<https://openreview.net/pdf?id=ZgQ0t3zYTQ>) · [OpenReview](<https://openreview.net/forum?id=ZgQ0t3zYTQ>)

**关联 ID：** `longmemeval-2025` · `memora-2026` · `locomo-plus-2026` · `beyond-prompts-2024`

---

<a id="paper-permemsafe-2026"></a>
**85. PerMemSafe：长程自演化智能体隐式个性化安全基准｜PerMemSafe: Benchmarking Implicit Personalized Safety of Long Horizon Self-Evolving Agents（2026 · Findings of ACL 2026）**

**作者：** Hengyu An、Minxi Li、Naen Xu、Chunyi Zhou、Xiaogang Xu、Tianyu Du、Jinbao Li、Shouling Ji

**书目：** 年份 2026；载体 Findings of ACL 2026；状态 同行评议；来源类型 benchmark

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 B；简称 PerMemSafe；优先级 2；相关性排序 18

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q2

**定位：** 评测会随长期互动演化、但不会在当前请求中被明确重述的个性化安全条件。

**问题：** 通用安全评测忽略用户历史中的隐式条件，而记忆系统可能召回错误或过时的安全上下文。

**机制：** 以安全感知和动态演化两条轨道评测长期隐式个性化安全；防御性框架通过推理式风险抽取、偏好与安全约束分离，以及保留相邻状态演化关系来组织记忆。

**步骤：**

1. 在历史中形成个性化安全条件
2. 让条件随长程对话变化
3. 在后续请求中隐式触发
4. 评价安全与帮助性
5. 用风险感知记忆筛选降低冲突

**证据：**

- 基准覆盖 5 个领域、276 段对话和 750 个测试实例；合成构造经过权威风险种子、多阶段筛选、人工复核并注入超过 90% 的无关对话干扰。
- 表 2 中现有自演化记忆框架的最佳整体个性化安全率约为 51.74%，动态演化轨道通常明显弱于安全感知轨道。
- 图 5 与第 5.2 节报告 SentinelMem 在同一评测协议下相对最强基线的安全率提高 23.80%，帮助性提高 10.73%；这些数值仅代表该合成基准。
- 自动裁判在 100 个随机案例上的安全率和帮助性 Cohen κ 分别为 0.892 和 0.820。

**局限：**

- 研究仅覆盖纯文本，基础模型集合受调用成本限制。
- 数据主要由多阶段流程合成；即使经过人工复核，也难覆盖真实对话的极端歧义、方言和复杂噪声。
- 23.80% 是论文在自身协议下报告的改进值，不能解释为生产环境风险下降或完整防御保证。

**意义：**

- 安全条件本身也需要随用户记忆更新，而不能只由全局静态策略处理。
- 个性化帮助性和个性化安全必须联合评价。

**边界：** 已核验 ACL Anthology 正式全文；只保留防御性评测和范围边界，不展开风险情境构造细节。

**标识：** DOI 10.18653/v1/2026.findings-acl.320

**证据位置：**

- claim 基准规模、两条轨道和构造流程；location 第 1 节；图 2；第 3.1 至 3.2 节；来源 PDF
- claim 既有框架的安全、检索和帮助性结果；location 表 2；第 4.2 至 4.3 节；来源 PDF
- claim SentinelMem 主结果、消融与合成数据限制；location 图 5；表 3；第 5 节；Limitations；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2026.findings-acl.320/>) · [PDF](<https://aclanthology.org/2026.findings-acl.320.pdf>)

---

<a id="paper-pii-cue-controlled-2026"></a>
**86. LLM 真的记住了个人身份信息吗：用线索受控框架重审 PII 泄露｜Do LLMs Really Memorize Personally Identifiable Information? Revisiting PII Leakage with a Cue-Controlled Memorization Framework（2026 · ACL 2026）**

**作者：** Xiaoyu Luo、Yiyi Chen、Qiongxiu Li、Johannes Bjerva

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；来源类型 benchmark+evaluation

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 桥接；证据等级 A；简称 Cue-Controlled PII；优先级 1；相关性排序 22

**核验：** 来源层级 T1；核验状态 abstract-checked；V/D/P/Q V=V2 / D=D3 / P=P3 / Q=Q3

**定位：** 用线索受控记忆框架区分提示中已提供的 PII 线索与模型真正从参数中重建的信息。

**问题：** PII 泄露实验若在提示中高度重合地给出姓名或上下文，会把条件生成能力误判为参数记忆化。

**机制：** 控制提示与目标 PII 的线索重合，比较有线索重建、无提示生成和成员推断，并覆盖 32 种语言。

**步骤：**

1. 定义提示—目标线索重合
2. 构造受控提示条件
3. 测量 PII 重建
4. 比较无提示生成与成员推断
5. 跨 32 种语言分析

**证据：**

- 正式摘要报告控制提示线索后，PII 重建显著下降。
- 无提示生成和成员推断的真正阳性率极低，说明部分既有“泄露”结果更可能由提示线索驱动。

**局限：**

- 研究聚焦参数训练记忆，不直接评测智能体外部用户记忆。
- 低可测记忆化不代表部署中不存在检索泄露、日志泄露或外部记忆访问控制问题。
- 评测结论依赖所选模型、语言和 PII 类型。

**意义：**

- PII 评测必须控制提示泄露和目标线索，避免夸大记忆化。
- 参数 PII 与外部个人记忆应使用不同威胁模型和删除验证。

**边界：** 作为 PII 评测有效性的关键反证；不削弱外部记忆泄露论文所证明的独立风险。

**标识：** DOI 10.18653/v1/2026.acl-long.1560

**证据位置：**

- claim 线索受控框架、32 种语言及重建下降；location ACL Anthology 正式摘要；来源 一手入口

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.1560/>) · [PDF](<https://aclanthology.org/2026.acl-long.1560.pdf>)

---

<a id="paper-privacy-control-conversational-llm-platforms-2026"></a>
**87. 对话式大语言模型平台中的隐私控制：穿行研究｜Privacy Control in Conversational LLM Platforms: A Walkthrough Study（2026 · CHI 2026）**

**作者：** Zhuoyang Li、Yanlai Wu、Yao Li、Xinning Gui、Yuhan Luo

**书目：** 年份 2026；载体 CHI 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 LLM 平台隐私控制；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q2

**定位：** 把消费级对话平台中的聊天历史、记忆片段与定制对象拆成可审计数据单元，并揭示相同删除词在不同层级产生不同效果。

**问题：** 对话 LLM 从交互中生成派生记忆，但平台所称的查看、编辑、删除和遗忘是否针对同一对象、能否跨会话生效，缺少跨平台的一手比较。

**机制：** 研究以政策文本加专家穿行同时核验六个平台，建立数据单元和控制操作矩阵，再比较图形界面与自然语言控制及其局部、全局后果。

**步骤：**

1. 从正式平台材料筛出六个消费级对话 LLM，并冻结 2024 年 11 月至 2025 年 1 月的观察窗口。
2. 用标准化账户和交互路径检查聊天、侧栏、设置、分享入口和记忆门户。
3. 把可控数据归为聊天历史、记忆片段和定制对象，并编码查看、编辑、删除、分享和训练退出。
4. 比较自然语言与图形界面命令的执行层级，区分只影响当前会话的局部效果和影响后续会话的全局效果。

**证据：**

- 表 4 至表 5 显示六个平台对聊天、记忆和定制对象采用不同粒度；记忆片段只在部分平台可逐项查看、编辑与删除。
- 第 4.2.2 节及图 2、图 5、图 6 直接展示记忆由对话派生、以片段或固定消息保存，并可通过界面或自然语言修改。
- 第 4.3.3 节发现删除聊天与删除记忆是相互独立的操作：只删会话可能保留派生记忆，只删记忆也可能保留原始对话。

**局限：**

- 这是专家穿行而非普通用户任务实验，不能推断理解率、使用率或信任。
- 界面快照覆盖六个主要由美国机构运营的平台，观察窗口为 2024 年 11 月至 2025 年 1 月，后续产品变化不在结论内。
- 研究只能观察可见界面和政策承诺，不能证明后端已按承诺擦除所有副本或模型影响。

**意义：**

- 记忆治理需要明确标注控制对象和传播范围，不能把删除、遗忘与移除视为同义的全局操作。
- 跨平台统一记忆面板应同时展示来源会话、派生片段、共享副本和删除依赖。

**边界：** 正式元数据由 ACM DOI 页面核验；公开全文用于核对第 3 至第 6 节、表 1 至表 5及图 2 至图 11。产品结论只对应论文冻结的观察窗口。

**引用：** Li et al., CHI 2026, DOI 10.1145/3772318.3791054。

**版本：** 采用 CHI 2026 正式版本族；全文核验使用与正式 DOI 同题作者稿。

**标识：** DOI 10.1145/3772318.3791054；稳定 ID doi:10.1145/3772318.3791054；工作族 ID privacy-control-conversational-llm-platforms-2026

**证据位置：**

- 第 3.2 至第 3.4 节、图 1 和表 1 至表 2：平台筛选、穿行流程和分析编码。
- 第 4.1 至第 4.2 节、表 3 至表 5、图 2 至图 8：数据治理、可控单元、记忆片段和控制操作矩阵。
- 第 4.3.1 至第 4.3.3 节：自然语言与图形界面控制、前摄与回溯控制、局部与全局效果。
- 第 6 节：平台数量、地域、时间快照和方法边界。

**资源：** [一手入口](<https://doi.org/10.1145/3772318.3791054>) · [PDF](<https://arxiv.org/pdf/2602.10684>)

**关联 ID：** `relational-gains-privacy-strains-2026` · `algorithmic-self-portrait-2026` · `big-help-big-brother-2025`

---

<a id="paper-relational-gains-privacy-strains-2026"></a>
**88. 关系收益与隐私压力：用户对 ChatGPT 记忆功能的感知与体验｜Relational Gains, Privacy Strains: Exploring Users' Perceptions and Experiences with ChatGPT's Memory Feature（2026 · CHI 2026）**

**作者：** Cheng Chen、Maria D. Molina、Mengqi Liao、Eugene Cho Snyder

**书目：** 年份 2026；载体 CHI 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 桥接；证据等级 B；简称 Relational Gains, Privacy Strains；优先级 medium；时间尺度 真实产品跨会话记忆使用

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D1 / P=P2 / Q=Q2

**标签：** ChatGPT-memory、privacy、mental-model、user-control

**定位：** 20 名美国 ChatGPT 用户在一次访谈中检查本人产品记忆，揭示可见性、画像、情境控制与隐私心智模型缺口。

**问题：** 当商业聊天机器人跨会话保存用户信息时，用户如何理解产品记忆，又需要哪些透明度、情境控制与定期复核？

**机制：** 研究不提出算法，而是让参与者在访谈中检查本人 ChatGPT 记忆，再分析感知、预期违背、画像风险和治理需求。

**步骤：**

1. 招募了解该功能的 ChatGPT 用户
2. 在访谈中让参与者检查本人产品记忆
3. 编码用户感知、预期违背与隐私顾虑
4. 归纳可见档案、保留时长、情境身份和周期复核要求

**证据：**

- 正式论文是 20 名美国 ChatGPT 用户的一次性定性访谈
- 难忘、细致、准确等描述是用户感知，不是客观系统测量
- 参与者报告意外保留、错误推断、模型间不透明与不期望的用户画像
- 论文提出可编辑档案、保留期限、情境身份和周期复核等设计要求

**局限：**

- 一次性美国样本不能代表跨文化、长期或所有产品用户
- 访谈引导参与者检查功能并介绍风险，可能影响后续建议
- 研究不测客观准确率、遗忘、删除完整性或纵向行为效果
- 产品实现持续变化，结论只适用于研究时可见体验

**意义：**

- 把产品记忆的可见性、情境身份和画像控制列为独立治理表面
- 用户缺乏可验证删除的信心不等于论文证明后端删除失效

**边界：** CHI 2026 ACM 正式 DOI 与全文核验；正式题名和作者取代错误候选名“AI Never Forgets”。研究是 N=20 的一次性定性访谈，用户感知不作为客观准确性、遗忘或删除测量。

**标识：** DOI 10.1145/3772318.3791635；稳定 ID doi:10.1145/3772318.3791635

**证据位置：**

- claim N=20、招募、访谈中检查本人 ChatGPT memory 及编码；location ACM 正式全文 §3.1–3.4；来源 一手入口
- claim 用户感知、预期违背及透明/控制需求；location ACM 正式全文 §4.1–4.3；来源 一手入口
- claim 不期望用户画像及可编辑档案、保留时长、情境身份、周期复核设计；location ACM 正式全文 §5.4–5.5、Table 2；来源 一手入口
- claim 单产品、一次性、美国样本和产品变化局限；location ACM 正式全文 §5.6；来源 一手入口

**资源：** [一手入口](<https://dl.acm.org/doi/10.1145/3772318.3791635>)

**关联 ID：** `mextra-2025` · `permemsafe-2026` · `memorybank-2024`

---

<a id="paper-reproducing-lightmem-2026"></a>
**89. 复现 LightMem：朴素检索增强生成同样可以做好记忆管理｜Reproducing LightMem: Naive RAG Is Just as Good for Memory Management（2026 · arXiv）**

**作者：** Yongjie Zhou、Shuai Wang、Bevan Koopman、Guido Zuccon

**书目：** 年份 2026；载体 arXiv；状态 预印本；出版状态 preprint；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、外部检索与非参数记忆、智能体记忆管理；层级 跨会话长期；阅读层级 桥接；证据等级 B；简称 LightMem Reproduction；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P1 / Q=Q2

**定位：** 在控制检索器、深度和回答预算后，独立复现显示构造式记忆对原始轮次检索没有普遍优势，并带来较高预处理成本。

**问题：** LightMem 的收益究竟来自记忆构造本身，还是检索器、上下文预算和评测设置的差异？

**机制：** 并行重建 LightMem 与保留原始用户轮次的朴素检索基线，通过受控检索和金标证据实验隔离记忆构造的贡献。

**步骤：**

1. 复现压缩、主题分组、阈值摘要和离线合并的 LightMem 写入流水线，同时保留原始用户轮次
2. 固定记忆库和生成器，在多种稀疏、稠密和混合检索器上比较
3. 以相同检索深度和相同回答标记预算对齐 LightMem 与朴素检索
4. 分别输入金标原始证据和对应构造记忆，测量构造阶段的信息损失

**证据：**

- 表4第7页：只更换检索器，LightMem 准确率从58.1%变化到75.5%，说明检索器选择足以改变结论。
- 图4第9页：同检索深度下多数比较偏向朴素检索；同约935标记预算时构造记忆转为小幅劣势。
- 表6第8页：金标原始轮次为89.0%，构造记忆为77.7%；构造平均消耗119,884标记和117次模型调用。
- 第9页成本分析估算约321个问题后才可能摊平写入标记成本。

**局限：**

- 仅使用 LongMemEval-S，排除56个 assistant 类问题，最终评测444题。
- 只使用一个回答生成模型，评判器与原论文不同；作者只重现趋势而非绝对准确率和成本。
- 尚未同行评议，结论应保留为强反证线索而非最终定论。

**意义：**

- 新增“记忆构造收益的受控独立复现”反证分支。
- 提示后续记忆系统比较必须固定检索器、回答预算并报告写入成本。

**边界：** arXiv 原始全文核验；作为独立复现直接反驳当前地图节点，但严格保留预印本状态。

**引用：** Zhou et al., arXiv:2607.29104, 2026；不得写成同行评议结果。

**版本：** 冻结时只有 arXiv v1；DBLP 精确题名无正式记录，Crossref 无会议或期刊条目。

**标识：** arXiv DOI 10.48550/arXiv.2607.29104；稳定 ID arxiv:2607.29104；工作族 ID reproducing-lightmem-2607-29104

**证据位置：**

- 图1及第3.1节，第2至3页：LightMem重建与朴素检索基线
- 图2及第3.3节，第4页：受控评测框架
- 第5至6节，第6至8页：检索器、预算与金标证据比较
- 图4及第7节，第9页：总体反证、成本与局限

**资源：** [一手入口](<https://arxiv.org/abs/2607.29104>) · [PDF](<https://arxiv.org/pdf/2607.29104v1>) · [代码](<https://github.com/ielab/Reproducing-LightMem>)

**关联 ID：** `agent-lightmem-2026` · `longmemeval-2025` · `agent-mem0-2025` · `agent-amem-2025` · `memoryos-2025`

---

<a id="paper-sp-controls-emotional-engagement-2026"></a>
**90. 安全与隐私控制对用户情感性使用生成式人工智能聊天机器人的影响｜The Impact of Security and Privacy Controls on Users' Emotional Engagement with Generative AI Chatbots（2026 · USENIX Security 2026）**

**作者：** Jabari Kwesi、Jiaxun Cao、Hailee Cunningham、Pardis Emami-Naeini

**书目：** 年份 2026；载体 USENIX Security 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 桥接；证据等级 B；简称 情感聊天控制评测；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 将记忆开关、对话删除、账户删除和训练退出放入同一实验，量化连续性价值、可逆性偏好与无法验证后端承诺之间的张力。

**问题：** 生成式聊天机器人提供多种控制，但用户是否理解它们、是否因此更愿意在情感支持场景使用，以及记忆连续性与隐私保护如何权衡，缺少系统比较。

**机制：** 研究先审计消费应用形成九类控制，再用随机化情境、序数混合模型和开放回答比较使用意愿、保护感与效用感。

**步骤：**

1. 从 344 个候选应用中按流行度和饱和抽样审计 87 个，归纳九种免费层安全与隐私控制。
2. 招募 354 名每月至少使用生成式聊天机器人获得情感或社交支持的美国成年人。
3. 每人随机评估九种控制中的四种，并分别评价使用意愿、保护感和支持效用。
4. 用累计链接混合模型与主题分析比较删除、记忆开关、训练退出、访问分享和本地处理的差异。

**证据：**

- 表 1 明确定义跨会话记忆开关，并与删除对话、删除账户、训练退出、分享控制和本地处理共同进入实验。
- 表 3 至表 5 显示删除类控制在多项结果中最受偏好；记忆开关保留了连续性效用，却降低部分参与者的保护感。
- 第 5.1 至第 5.2 节报告理解缺口与保证缺口：参与者既希望可选择删除，也普遍无法自行验证后台是否按承诺执行。

**局限：**

- 使用的是假设情境而非处于真实情感压力下的实际控制操作。
- Prolific 样本只覆盖美国、每月至少使用相关聊天机器人的成年人，且人口结构不代表全部用户。
- 应用控制分类冻结于 2025 年 8 月至 9 月，界面文案和功能可能继续变化。

**意义：**

- 记忆开关的设计不能只强调连续性，还应展示累计数据范围、关闭后的保留状态和可验证反馈。
- 删除控制需要提供范围说明和独立保证，避免用户把界面承诺误认为已经完成全部后端擦除。

**边界：** 正式接收与作者顺序由 USENIX Security 2026 论文页核验；公开全文核对方法、第 3 至第 5 节和表 1 至表 5。敏感场景只做聚合概括，未转录参与者原话。

**引用：** Kwesi et al., USENIX Security 2026。

**版本：** 采用 USENIX Security 2026 正式接收版本族；全文核验使用同题作者公开稿。

**标识：** 稳定 ID url:https://www.usenix.org/conference/usenixsecurity26/presentation/kwesi；工作族 ID sp-controls-emotional-engagement-2026

**证据位置：**

- 第 3.1 节与表 1：87 个应用的饱和审计和九种控制分类。
- 第 3.2 至第 3.5 节与表 2：354 人实验、随机化设计、分析和局限。
- 第 4.1 至第 4.3 节与表 3 至表 5：使用意愿、保护感、效用感及记忆连续性结果。
- 第 5.1 至第 5.2 节与图 2：理解缺口、可逆性控制和保证缺口。

**资源：** [一手入口](<https://www.usenix.org/conference/usenixsecurity26/presentation/kwesi>) · [PDF](<https://arxiv.org/pdf/2607.06371>)

**关联 ID：** `carecall-ltm-self-disclosure-2024` · `relational-gains-privacy-strains-2026` · `big-help-big-brother-2025`

---

<a id="paper-when-facts-change-2026"></a>
**91. 当事实发生变化：大语言模型中的时间知识冲突解决｜When Facts Change: Temporal Knowledge Conflict Resolution in LLMs（2026 · Findings of ACL 2026）**

**作者：** Jonas Wallat、Wolfgang Nejdl、Sandipan Sikdar

**书目：** 年份 2026；载体 Findings of ACL 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 评测、安全与治理；相关路线 评测、安全与治理、参数记忆与知识修改、外部检索与非参数记忆；层级 模型生命周期；阅读层级 桥接；证据等级 A；简称 When Facts Change；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 模型常能说出事实会变化，却不能把这一判断转化为接受最新证据的最终答案。

**问题：** 在参数知识与当前文档冲突时，模型是否能区分稳定事实和近期变化事实，并据此采取正确行动？

**机制：** WikiRecentChanges把稳定与已更新事实和真实、反事实上下文正交组合，再追踪冲突检测、可变性识别、推理使用和最终选择。

**步骤：**

1. 从近期知识变更构造稳定与更新事实对照
2. 为每项事实生成真实和反事实上下文
3. 记录模型是否检测冲突并识别事实可变性
4. 比较推理痕迹中的判断与最终答案行动

**证据：**

- Table 2显示稳定事实和已更新事实下的上下文接受行为显著不同。
- Table 4把冲突检测、可变性识别和实际使用分开后，揭示模型经常停在“知道会变”而未改变答案。
- Table 6显示显式时间提示只能部分改善该差距。

**局限：**

- 评测观察推理与答案，不执行持久写回或版本替换。
- 知识来源和变更窗口集中于维基数据。
- 提示中日期信息的效果不能等同于真实在线更新能力。

**意义：**

- 时间记忆评测必须区分检测、判断和行动，不能只看最终准确率。
- 更新系统应显式记录事实可变性和版本时间，并验证是否真正影响决策。

**边界：** 正式论文页核验元数据与出版状态；公开全文核验机制、表图证据和局限。

**引用：** Wallat等，Findings of ACL 2026，DOI 10.18653/v1/2026.findings-acl.103。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.18653/v1/2026.findings-acl.103；稳定 ID doi:10.18653/v1/2026.findings-acl.103；工作族 ID when-facts-change-2026

**证据位置：**

- Figure 2与第3节，PDF第3至5页：数据构造和测量设置
- Tables 2至4，PDF第5至7页：检测、识别和行动链
- Table 6，PDF第9页：提示策略反证

**资源：** [一手入口](<https://aclanthology.org/2026.findings-acl.103/>) · [PDF](<https://aclanthology.org/2026.findings-acl.103.pdf>)

**关联 ID：** `taming-knowledge-conflicts-2025` · `wikibigedit-2025` · `chronomem-2026`

---

<a id="paper-a01"></a>
**92. 语言模型能否作为知识库｜Language Models as Knowledge Bases?（2019 · EMNLP-IJCNLP 2019）**

**作者：** Fabio Petroni、Tim Rocktäschel、Sebastian Riedel、Patrick Lewis、Anton Bakhtin、Yuxiang Wu、Alexander Miller

**书目：** 年份 2019；载体 EMNLP-IJCNLP 2019；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 参数记忆与知识修改；相关路线 评测、安全与治理、参数记忆与知识修改；层级 模型生命周期；阅读层级 背景；证据等级 B；简称 LAMA；优先级 background；相关性排序 18；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 abstract-checked；V/D/P/Q V=V2 / D=D2 / P=P2 / Q=Q2

**定位：** 以完形提示证明预训练参数中存在可访问关系知识，同时开启了提示偏差争论。

**问题：** 不借助检索或任务微调，模型参数是否容纳可查询关系事实？

**机制：** 用完形探针把三元组映射为词表预测，并在 LAMA 上按关系分析。

**步骤：**

1. 把知识三元组改写为完形提示
2. 查询缺失实体的词表概率
3. 在多来源关系集合与传统方法比较
4. 按关系和提示分析可访问性

**证据：**

- 正式版摘要报告 BERT 在若干关系上可与依赖部分预言信息的传统方法竞争，并在开放域问答上超过其监督基线。

**局限：**

- 提示可访问性不等于稳定存储
- 结果易受提示偏差和数据伪线索影响

**意义：**

- 参数记忆研究的实验起点
- 必须把探针结果与可编辑性分开

**边界：** 元数据与核心主张来自 ACL Anthology；反证来自 Cao et al. 2021 ACL 正式版。

**标识：** DOI 10.18653/v1/D19-1250；稳定 ID doi:10.18653/v1/D19-1250

**证据位置：**

- claim 关系知识与问答结果；source ACL Anthology formal version；location abstract

**资源：** [一手入口](<https://aclanthology.org/D19-1250/>) · [PDF](<https://aclanthology.org/D19-1250.pdf>) · [代码](<https://github.com/facebookresearch/LAMA>)

---

<a id="paper-product-key-memory-2019"></a>
**93. 采用产品键的大规模记忆层｜Large Memory Layers with Product Keys（2019 · NeurIPS 2019）**

**作者：** Guillaume Lample、Alexandre Sablayrolles、Marc'Aurelio Ranzato、Ludovic Denoyer、Herve Jegou

**书目：** 年份 2019；载体 NeurIPS 2019；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改；层级 模型生命周期；阅读层级 背景；证据等级 B；简称 PKM；优先级 low；时间尺度 训练形成、部署时固定的模型记忆层

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** product-key、sparse-memory-layer、capacity-scaling、historical-predecessor

**定位：** 用两个子键码本的笛卡尔积隐式建立百万级键空间，只稀疏访问少数值槽，从而在近似固定计算下扩大前馈式记忆容量。

**问题：** 直接搜索巨大键值记忆随槽位数线性增长，无法把参数容量扩到百万或十亿级而保持推理效率。

**机制：** 查询被分成两半，分别在两个子键码本中取 top-k；两组候选组合成 k² 个产品键，再精确选出最终 top-k，并对对应值向量做稀疏加权。

**步骤：**

1. 由输入产生查询并拆成两个子查询
2. 分别在两个大小约为根号级的子键码本中取 top-k
3. 对候选子键做笛卡尔积并在 k² 个组合中精确选择最终键
4. 只读取并更新被选中的值槽，将结果通过残差接入 Transformer

**证据：**

- PDF 第3.1节、Figure 2 和 Equations 1–4 给出产品键构造与精确 top-k 过程；第3.2节给出根号级子键搜索加 k² 组合搜索的复杂度。
- PDF Table 1 和 Figure 4 中，12 层加一个 PKM 的模型优于同维度24层无记忆模型，推理接近快一倍。
- PDF Table 2 显示一百万槽位时，查询批归一化把记忆利用率从25.8%提高到80.3%，并把困惑度从19.8降到18.0。
- PDF Table 4 显示产品键在槽位增至一百万时推理速度基本稳定，而平坦键搜索显著变慢。

**局限：**

- 实验是2019年的 CC-News 语言建模，不能直接外推到指令模型和真实智能体。
- 记忆值在训练中学习，部署后没有跨会话在线写入、版本冲突、权限或删除。
- 大记忆会出现槽位利用不均，效果依赖查询批归一化。

**意义：**

- Memory Layers at Scale 的稀疏大容量路线可追溯到产品键的根号级精确寻址。
- 扩大记忆槽位不等于形成可治理长期记忆；运行期写入和删除仍是另一层问题。
- 记忆利用率应和模型质量、吞吐一起报告。

**边界：** 作为 Memory Layers at Scale 的直接历史前身纳入 background；正式状态、算法、复杂度和表图由 NeurIPS 正式页与全文核验。

**标识：** 稳定 ID neurips-2019-9d8df73a3cfbf3c5b47bc9b50f214aff；工作族 ID product-key-memory

**证据位置：**

- claim 产品键寻址机制；location PDF 第3.1–3.2节、Figures 1–2、Equations 1–4；来源 PDF
- claim Transformer 集成；location PDF Figure 3、第4.3节；来源 PDF
- claim 精度、速度、利用率与消融；location PDF Tables 1–4、Figures 4–7；来源 PDF
- claim 结论边界；location PDF 第5节；来源 PDF

**资源：** [一手入口](<https://papers.neurips.cc/paper_files/paper/2019/hash/9d8df73a3cfbf3c5b47bc9b50f214aff-Abstract.html>) · [PDF](<https://papers.neurips.cc/paper_files/paper/2019/file/9d8df73a3cfbf3c5b47bc9b50f214aff-Paper.pdf>)

**关联 ID：** `a02` · `memory-layers-scale-2025`

---

<a id="paper-a02"></a>
**94. Transformer 前馈层作为键值记忆｜Transformer Feed-Forward Layers Are Key-Value Memories（2021 · EMNLP 2021）**

**作者：** Mor Geva、Roei Schuster、Jonathan Berant、Omer Levy

**书目：** 年份 2021；载体 EMNLP 2021；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 FFN Key-Value Memory；优先级 core；相关性排序 3；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把占大量参数的前馈层解释为从输入模式到输出词表分布的组合键值记忆。

**问题：** 前馈层是否承担可解释的关联记忆功能？

**机制：** 分析前馈键的触发模式、值的词表分布及跨层精炼。

**步骤：**

1. 把前馈层写成键和值向量组合
2. 寻找激活键对应的输入模式
3. 把值映射到输出词表
4. 跨层跟踪残差流中的组合与精炼

**证据：**

- 正式版摘要报告键与输入模式相关、值诱导词表分布，低层偏浅层而高层偏语义模式。

**局限：**

- 功能类比不证明每个单元是一条离散事实
- 分析集中于较早期模型
- 定位层未必是最佳编辑层

**意义：**

- 为 ROME/MEMIT 的前馈层编辑提供解释背景

**边界：** 正式会议页与 PDF 核验。

**标识：** DOI 10.18653/v1/2021.emnlp-main.446；稳定 ID doi:10.18653/v1/2021.emnlp-main.446

**证据位置：**

- claim 键、值及跨层模式解释；source ACL Anthology formal version；location abstract and analysis sections

**资源：** [一手入口](<https://aclanthology.org/2021.emnlp-main.446/>) · [PDF](<https://aclanthology.org/2021.emnlp-main.446.pdf>)

---

<a id="paper-a03"></a>
**95. 编辑语言模型中的事实知识｜Editing Factual Knowledge in Language Models（2021 · EMNLP 2021）**

**作者：** Nicola De Cao、Wilker Aziz、Ivan Titov

**书目：** 年份 2021；载体 EMNLP 2021；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改；层级 模型生命周期；阅读层级 桥接；证据等级 A；简称 KnowledgeEditor；优先级 bridge；相关性排序 12；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 abstract-checked；V/D/P/Q V=V2 / D=D2 / P=P2 / Q=Q2

**定位：** 用受约束超网络为单条事实预测局部权重增量，是可训练编辑器的代表起点。

**问题：** 如何快速改变单条事实预测且尽量保持无关行为？

**机制：** 从编辑样本计算信号，由超网络生成受保持损失约束的权重更新。

**步骤：**

1. 计算编辑样本的基础模型信号
2. 超网络预测参数增量
3. 以保持损失限制无关变化
4. 用释义训练编辑泛化

**证据：**

- 正式版摘要报告在 BERT 事实核验和 BART 问答中可迁移到释义，且更新集中于较小参数子集。

**局限：**

- 需要预训练编辑器
- 依赖编辑分布
- 未覆盖长期连续编辑与多跳后果

**意义：**

- 奠定梯度到参数增量的元编辑路线

**边界：** 正式会议页及作者代码核验。

**标识：** DOI 10.18653/v1/2021.emnlp-main.522；稳定 ID doi:10.18653/v1/2021.emnlp-main.522

**证据位置：**

- claim 释义泛化与参数集中；source ACL Anthology formal version；location abstract

**资源：** [一手入口](<https://aclanthology.org/2021.emnlp-main.522/>) · [PDF](<https://aclanthology.org/2021.emnlp-main.522.pdf>) · [代码](<https://github.com/nicola-decao/KnowledgeEditor>)

---

<a id="paper-a04"></a>
**96. 可扩展快速模型编辑｜Fast Model Editing at Scale（2022 · ICLR 2022）**

**作者：** Eric Mitchell、Charles Lin、Antoine Bosselut、Chelsea Finn、Christopher D. Manning

**书目：** 年份 2022；载体 ICLR 2022；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 MEND；优先级 core；相关性排序 7；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 以低秩编辑网络把普通微调梯度变换为可扩展的快速参数更新。

**问题：** 如何把单次梯度转成快速、局部且可用于十亿级模型的编辑？

**机制：** 低秩分解梯度后由小型编辑网络变换为参数更新，并联合优化可靠性与局部性。

**步骤：**

1. 对编辑样本求梯度
2. 低秩分解梯度结构
3. 编辑网络输出权重更新
4. 元训练编辑成功、释义泛化和邻域保持

**证据：**

- 论文在 GPT-Neo 2.7B、GPT-J 6B、T5-XL 2.8B 与 T5-XXL 11B 上比较；Table 3 中 MEND 的编辑成功率分别为 0.81、0.88、0.88、0.89，相应性能回落为 0.057、0.031、0.001、低于 0.001。
- 摘要报告编辑器可在单张 GPU 上用不足一天完成 10B 以上模型的训练；这是作者报告的资源条件，不等同于所有硬件上的通用时延。
- Table 5 显示批量规模扩大时效果会下降：125 条同时编辑时编辑成功率为 0.67、性能回落为 0.012，因此小批量结果不能外推为长期或无限累积稳定。

**局限：**

- 方法需要从具有代表性的编辑任务分布学习编辑器，分布外编辑的可靠性仍需单独验证。
- 论文使用的随机局部性负例可能不够困难，模型会对相关但语义不同的输入过度泛化。
- 回译和截断构造的等价邻域没有检验编辑事实的逻辑或关系蕴含；125 条批量编辑时成功率已明显下降。

**意义：**

- 把模型编辑从逐任务优化转向可学习更新规则

**边界：** 正式 ICLR 页、OpenReview 与作者代码核验。

**标识：** arXiv 2110.11309；稳定 ID openreview:0DcZxeWfOPt

**证据位置：**

- claim MEND 将全连接层单样本梯度分解为激活与输出梯度的外积，分别变换低秩因子后形成秩一权重更新，并以编辑损失和 KL 局部性损失训练。；source MEND: Fast Model Editing at Scale；location §3.1–3.2，Algorithm 1–2，Eq. (2)–(4)，印刷第 3–4 页；来源 定位入口 1
- claim MEND 在 2.7B 至 11B 模型上的编辑成功率和性能回落结果。；source MEND: Fast Model Editing at Scale；location §5.1，Table 3，印刷第 7 页；来源 定位入口 1
- claim 批量达到 125 条时编辑成功率下降到 0.67，不能外推为无限累积稳定。；source MEND: Fast Model Editing at Scale；location §5.3，Table 5，印刷第 8 页；来源 定位入口 1
- claim 随机局部性样本过于简单会允许过度泛化，所用等价邻域也不测试事实蕴含。；source MEND: Fast Model Editing at Scale；location §6 Limitations &amp; Future Work，印刷第 9 页；来源 定位入口 1

**证据定位入口：** [定位入口 1](<https://arxiv.org/pdf/2110.11309.pdf>)

**资源：** [一手入口](<https://iclr.cc/virtual/2022/poster/6846>) · [PDF](<https://openreview.net/pdf?id=0DcZxeWfOPt>) · [项目页](<https://openreview.net/forum?id=0DcZxeWfOPt>) · [代码](<https://github.com/eric-mitchell/mend>)

---

<a id="paper-a05"></a>
**97. 定位并编辑 GPT 中的事实关联｜Locating and Editing Factual Associations in GPT（2022 · NeurIPS 2022）**

**作者：** Kevin Meng、David Bau、Alex Andonian、Yonatan Belinkov

**书目：** 年份 2022；载体 NeurIPS 2022；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 ROME；优先级 core；相关性排序 1；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 用因果追踪定位事实召回，再以秩一前馈权重更新重写单条关联。

**问题：** 事实关联在哪里被召回，能否解析地重写一条事实？

**机制：** 因果追踪定位主语令牌的中层前馈模块，求新客体目标值并执行保持其余映射的秩一更新。

**步骤：**

1. 干扰输入并恢复隐藏状态做因果追踪
2. 定位主语令牌处中层前馈模块
3. 求新客体目标值
4. 执行秩一权重更新

**证据：**

- 在 GPT-2 XL 的 1,000 个事实陈述上，因果追踪发现最后主语词元的中层 MLP 具有最强恢复信号；第 15 层平均间接效应为 8.7，最后主语位置的 MLP 与注意力贡献峰值约为 6.6 与 1.6。
- CounterFact 上，ROME Score 在 GPT-2 XL 和 GPT-J 上分别为 89.2 与 91.5；GPT-J 的编辑成功、改写成功、邻域成功分别为 99.9、99.1、78.9，GPT-2 XL 分别为 100.0、96.4、75.4。
- 15 人、50 个事实的人工评测中，ROME 相比 FT+L 被判断为一致的概率高 1.8 倍，但被判断为更流畅的概率低 1.3 倍，说明自动指标未完整捕捉生成质量代价。

**局限：**

- ROME 主要处理单一、方向性的事实关联，反向关联需要另行编辑。
- 论文没有覆盖逻辑、空间、数值等知识类型，编辑后模型仍可能编造貌似合理但没有根据的新事实。
- “因果定位不一定预测最佳编辑层”不是本论文自身证明的结论，应移到后续工作条目并单独归因。

**意义：**

- 建立定位式直接编辑范式
- 催生 MEMIT 与定位有效性反证

**边界：** 正式会议页、项目页与作者代码核验。

**标识：** DOI 10.52202/068431-1262；arXiv 2202.05262；稳定 ID doi:10.52202/068431-1262

**证据位置：**

- claim 因果追踪把事实回忆的强中介信号定位到最后主语词元附近的中层 MLP。；source Locating and Editing Factual Associations in GPT；location §2.1–2.3，Figure 2，印刷第 3–5 页；来源 PDF
- claim ROME 构造平均键、优化目标值，并求解带保持约束的秩一更新。；source Locating and Editing Factual Associations in GPT；location §3.1，Figure 4，Eq. (2)–(4)，印刷第 5–6 页；来源 PDF
- claim zsRE 与 CounterFact 上的 efficacy、paraphrase、specificity、邻域和总分结果。；source Locating and Editing Factual Associations in GPT；location §3.2，Table 1，印刷第 6 页；§3.3–3.4，Table 4，印刷第 7–8 页；来源 PDF
- claim 人工评测揭示自动指标未充分体现的流畅度代价。；source Locating and Editing Factual Associations in GPT；location §3.6，印刷第 8–9 页；来源 PDF
- claim 方法限于单一、方向性事实，未覆盖逻辑、空间和数值知识，模型仍可能生成无根据的新事实。；source Locating and Editing Factual Associations in GPT；location §3.7，印刷第 9 页；来源 PDF

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2022/hash/6f1d43d5a82a37e89b0665b33bf3a182-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2022/file/6f1d43d5a82a37e89b0665b33bf3a182-Paper-Conference.pdf>) · [项目页](<https://rome.baulab.info/>) · [代码](<https://github.com/kmeng01/rome>)

---

<a id="paper-a06"></a>
**98. 批量编辑 Transformer 记忆｜Mass-Editing Memory in a Transformer（2023 · ICLR 2023）**

**作者：** Kevin Meng、Arnab Sen Sharma、Alex Andonian、Yonatan Belinkov、David Bau

**书目：** 年份 2023；载体 ICLR 2023；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 MEMIT；优先级 core；相关性排序 2；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把 ROME 的单条闭式写入扩展为跨层最小二乘的数千条关联批量更新。

**问题：** 如何把单事实编辑扩展为数千条关联的一次批量写入？

**机制：** 为新关联估计目标值，利用旧知识键协方差跨多个前馈层求解批量权重增量。

**步骤：**

1. 估计每条关联的目标值
2. 收集旧知识键协方差
3. 跨多个前馈层求最小二乘更新
4. 批量写入并测可靠性和局部性

**证据：**

- 在 GPT-J 上批量编辑 10,000 条 zsRE 时，MEMIT 的总分、编辑成功、改写成功、specificity 分别为 50.7、96.7、89.7、26.6；specificity 接近未编辑模型的 27.0，表示额外溢出较小而非绝对准确率很高。
- 10,000 条 CounterFact 编辑后，GPT-J 的总分、编辑成功、改写成功、邻域成功分别为 85.8、98.9、88.6、73.7；GPT-NeoX 分别为 82.0、97.2、82.2、70.8。
- 当前实现总用时为 7.44 小时，目标表示优化仍按样本串行执行；不同关系难度差异显著，而论文测试的四组关系混合没有出现稳定的多样性正效应或负效应。

**局限：**

- 论文只验证方向性主语—关系—宾语三元组，未覆盖空间、时间、数学、语言学、程序性和对称关系知识。
- 不同关系的编辑与局部性难度差异明显，当前实现的目标优化仍有较高时间成本。
- 论文验证的是一次性批量更新，不等同于持续在线记忆维护；“批量目标必然互相干扰”也不是本文实验结论。

**意义：**

- 形成大规模直接参数写入基线

**边界：** 正式 ICLR 页、OpenReview、项目页及作者代码核验。

**标识：** arXiv 2210.07229；稳定 ID openreview:MkbcAHIYgyS

**证据位置：**

- claim MEMIT 选择一段关键 MLP 层，并在多层间分配目标残差。；source Mass-Editing Memory in a Transformer；location §4.1，Figure 3，印刷第 3–4 页；§4.3，Eq. (16)、Algorithm 1，印刷第 5–6 页；来源 定位入口 1
- claim 批量新关联的闭式更新以既有键的二阶统计约束旧知识保持。；source Mass-Editing Memory in a Transformer；location §4.2，Eq. (14)，印刷第 5 页；来源 定位入口 1
- claim GPT-J 上 10,000 条 zsRE 的批量编辑结果。；source Mass-Editing Memory in a Transformer；location §5.2.1，Table 1，印刷第 7 页；来源 定位入口 1
- claim GPT-J 和 GPT-NeoX 上 10,000 条 CounterFact 的结果及 7.44 小时实现用时。；source Mass-Editing Memory in a Transformer；location §5.2.2，Figure 5、Table 2，印刷第 7–8 页；来源 定位入口 1
- claim 实验仅覆盖方向性三元组，未覆盖多种其他知识形式。；source Mass-Editing Memory in a Transformer；location §6，印刷第 9 页；来源 定位入口 1

**证据定位入口：** [定位入口 1](<https://arxiv.org/pdf/2210.07229.pdf>)

**资源：** [一手入口](<https://iclr.cc/virtual/2023/poster/11880>) · [PDF](<https://openreview.net/pdf?id=MkbcAHIYgyS>) · [项目页](<https://memit.baulab.info/>) · [代码](<https://github.com/kmeng01/memit>)

---

<a id="paper-a07"></a>
**99. 面向终身模型编辑的知识记忆重构｜WISE: Rethinking the Knowledge Memory for Lifelong Model Editing of Large Language Models（2024 · NeurIPS 2024）**

**作者：** Peng Wang、Zexi Li、Ningyu Zhang、Ziwen Xu、Yunzhi Yao、Yong Jiang、Pengjun Xie、Fei Huang、Huajun Chen

**书目：** 年份 2024；载体 NeurIPS 2024；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 WISE；优先级 core；相关性排序 8；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 以主记忆、侧记忆、路由和子空间碎片隔离连续编辑冲突。

**问题：** 连续大量编辑时如何兼顾可靠写入、局部保持和分布外泛化？

**机制：** 复制前馈值矩阵为侧记忆，路由激活，随机子空间分片写入并以 Ties-Merge 合并。

**步骤：**

1. 保留主模型并建立侧记忆
2. 激活路由选择主或侧记忆
3. 在随机子空间碎片中写入编辑
4. 合并碎片并控制冲突

**证据：**

- 正式摘要报告在 GPT、LLaMA、Mistral 上覆盖问答、幻觉修正与分布内外测试。
- 正文给出经验性的可靠性—泛化—局部性三难。

**局限：**

- 增加侧记忆和路由状态
- 分片合并仍可能冲突
- 三难是经验观察而非不可能定理
- 缺乏部署级长期验证

**意义：**

- 把知识编辑从单次更新推进到持续隔离式写入

**边界：** 正式会议页与 PDF 全文核验；代码入口为作者工具库。

**标识：** DOI 10.52202/079017-1703；arXiv 2405.14768；稳定 ID doi:10.52202/079017-1703

**证据位置：**

- claim 侧记忆与路由；source NeurIPS PDF；location §2.2 and Table 1, pp.2–3
- claim 子空间分片与合并；source NeurIPS PDF；location §2.3 and Figure 2, pp.3–4

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2024/hash/60960ad78868fce5c165295fbd895060-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2024/file/60960ad78868fce5c165295fbd895060-Paper-Conference.pdf>) · [代码](<https://github.com/zjunlp/EasyEdit>)

---

<a id="paper-glame-2024"></a>
**100. 知识图增强的大语言模型编辑｜Knowledge Graph Enhanced Large Language Model Editing（2024 · EMNLP 2024）**

**作者：** Mengqi Zhang、Xiaotian Ye、Qiang Liu、Pengjie Ren、Shu Wu、Zhumin Chen

**书目：** 年份 2024；载体 EMNLP 2024；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改、外部检索与非参数记忆、评测、安全与治理；层级 模型生命周期；阅读层级 桥接；证据等级 A；简称 GLAME；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把外部知识图中的编辑关联传播编码进一次参数更新，使目标事实及其受影响关联事实共同变化。

**问题：** 仅编辑目标三元组往往不能更新由该事实引出的关联知识；逐条编辑高阶关系又可能反复扰动参数。

**机制：** GLAME 以新目标实体为中心采样关联子图，从模型早层取得实体与关系表征，再用关系图神经网络形成增强表示并嵌入秩一编辑。

**步骤：**

1. 从目标编辑三元组的新对象出发，在外部知识图采样多阶邻居并构造关联变化子图。
2. 把实体和关系文本送入语言模型，从早层隐藏状态初始化子图节点与关系。
3. 用关系图神经网络聚合结构化关联，生成包含目标与关联变化的编辑表示。
4. 在一次秩一参数更新中注入该表示，并保持无关知识约束。

**证据：**

- 表 1 中 GPT-J 上 GLAME 的综合编辑分数为 63.87，ROME 为 60.21、MEMIT 为 60.24；其可迁移分数为 33.04，高于两者的 29.67 与 29.77。
- 表 2 的 MQuAKE 多跳问题上，GPT-J 的平均分为 35.11，ROME 为 33.15、MEMIT 为 27.46；四跳得分为 21.33、18.27、13.57。
- 表 3 显示移除图式编辑模块或改为不利用关系结构的变体降低可迁移或综合编辑分数，但差距大小依模型和变体而异。

**局限：**

- 依赖外部知识图的覆盖和质量；邻居过多或阶数过高会引入噪声。
- 难以处理没有明确实体关系的事件型或非结构化编辑。
- 实验只覆盖 GPT-2 XL 与 GPT-J 及三组编辑数据，不能外推到更大模型、连续编辑流或生产知识库。

**意义：**

- 知识编辑可把检索到的结构化关联作为写入约束，而不只把图用于推理时检索。
- 它为参数记忆与外部图记忆之间提供了明确的机制桥梁。

**边界：** 一手正式入口为 ACL Anthology；已核对全文方法、主表、消融和局限。论文报告的是受控编辑任务，不等同于真实世界知识同步。

**引用：** Zhang et al., EMNLP 2024, DOI 10.18653/v1/2024.emnlp-main.1261。

**版本：** 采用 EMNLP 2024 正式版本。

**标识：** DOI 10.18653/v1/2024.emnlp-main.1261；稳定 ID doi:10.18653/v1/2024.emnlp-main.1261；工作族 ID glame-knowledge-graph-editing-2024

**证据位置：**

- 第 4.1–4.2 节与图 2，正式页码 22649–22651：子图构造、初始化和图式参数编辑。
- 表 1，正式页码 22652–22653：CounterFact 与 CounterFact+ 主结果。
- 表 2，正式页码 22653：MQuAKE 多跳结果。
- 表 3 与局限部分，正式页码 22655：组件消融及外部图、非结构化编辑边界。

**资源：** [一手入口](<https://aclanthology.org/2024.emnlp-main.1261/>) · [PDF](<https://aclanthology.org/2024.emnlp-main.1261.pdf>)

**关联 ID：** `a05` · `a06` · `a10` · `a11`

---

<a id="paper-larimar-2024"></a>
**101. Larimar：带情景记忆控制的大语言模型｜Larimar: Large Language Models with Episodic Memory Control（2024 · ICML 2024, PMLR 235）**

**作者：** Payel Das、Subhajit Chaudhury、Elliot Nelson、Igor Melnyk、Sarathkrishna Swaminathan、Sihui Dai、Aurelie Lozano、Georgios Kollias、Vijil Chenthamarakshan、Jiri Navratil、Soham Dan、Pin-Yu Chen

**书目：** 年份 2024；载体 ICML 2024, PMLR 235；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改、外部检索与非参数记忆、评测、安全与治理；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Larimar；优先级 high；时间尺度 模型生命周期中的在线知识更新与遗忘

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** episodic-memory、one-shot-editing、selective-forgetting、leakage-testing

**定位：** 在冻结解码器旁加入可单次写入的分布式情景记忆，用同一更新公式完成新增、连续编辑、选择性遗忘和泄漏抑制。

**问题：** 传统模型编辑要训练、定位或追加大量适配器，连续编辑慢且难以在同一机制中选择性删除。

**机制：** 编码器把事实压到潜在空间，生成式伪逆记忆以矩阵形式保存潜在事实；读出经线性变换成为解码器的压缩 KV 条件。写入系数为正时新增，系数为负时抵消已写事实。

**步骤：**

1. 把新事实或查询编码为潜在表示，并由参考记忆计算写入权重
2. 用伪逆式闭式更新把事实单次写入固定大小记忆矩阵
3. 按查询从记忆读出潜在状态，转换为解码器跨层条件后生成
4. 对待删除事实执行负写入并写入替代响应，再用改写攻击检查是否仍可恢复

**证据：**

- PDF 第5.1节和 Table 1 报告在统一 EasyEdit 设置下，Larimar 的十次编辑墙钟时间比 ROME 和 GRACE 快约4至10倍。
- PDF Table 3 中，1000 次连续编辑后的编辑保持率为 Larimar-1.3B 0.97、Larimar-6B 0.92；MEND 为0.27，GRACE 为0.93。
- PDF 第5.4节和 Table 4 显示容量为 K 时，删除目标的准确回忆接近0，同时保留事实回忆在 CounterFact 为0.997或0.993；当写入量达到2K时，保留回忆明显降至0.79或0.71，暴露容量边界。
- PDF Table 5 中改写攻击成功率仍为17.6%（单条）和21.5%（批量），说明泄漏被降低但未消失。

**局限：**

- 当前只处理较短事实，训练任务限于句子补全；问答、摘要和真实对话尚未验证。
- 固定容量记忆在写入量超过 K 后保留率明显下降。
- 改写恢复攻击仍有非零成功率，因此不能把负写入等同于密码学意义的删除。

**意义：**

- 把快速更新与删除放在同一可逆外部潜在记忆中，是参数编辑与检索记忆之间的重要混合路线。
- 删除评测必须同时报告目标遗忘、其余事实保留和改写恢复率。
- 容量、查询作用域检测和泄漏应成为长期潜在记忆的独立验收项。

**边界：** 题名、作者、正式 ICML 状态、机制、表格数值与限制均由 PMLR 正式页和全文核验。 独立合并审计将方法自身实验的直接性校准为 D2；D3 仅保留给独立验证或反驳。

**标识：** 稳定 ID pmlr-v235-das24a；工作族 ID larimar

**证据位置：**

- claim 架构、读写与负写入；location PDF Figure 1、第3节、Algorithm 1、Equations 2–6；来源 PDF
- claim 编辑速度与连续编辑；location PDF 第5.1–5.3节、Tables 1–3、Figure 2；来源 PDF
- claim 选择性遗忘与容量失效；location PDF 第5.4节、Table 4、Appendix Figures 3–5；来源 PDF
- claim 改写恢复攻击；location PDF 第5.4节、Table 5；来源 PDF
- claim 适用边界；location PDF 第7节末段；来源 PDF

**资源：** [一手入口](<https://proceedings.mlr.press/v235/das24a.html>) · [PDF](<https://raw.githubusercontent.com/mlresearch/v235/main/assets/das24a/das24a.pdf>)

**关联 ID：** `mplus-2025` · `a05` · `a06` · `can-sensitive-information-be-deleted-2024`

---

<a id="paper-a08"></a>
**102. 零空间约束的语言模型知识编辑｜AlphaEdit: Null-Space Constrained Knowledge Editing for Language Models（2025 · ICLR 2025 Oral）**

**作者：** Junfeng Fang、Houcheng Jiang、Kun Wang、Yunshan Ma、Jie Shi、Xiang Wang、Xiangnan He、Tat-Seng Chua

**书目：** 年份 2025；载体 ICLR 2025 Oral；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 AlphaEdit；优先级 core；相关性排序 9；时间尺度 模型生命周期

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把候选扰动投影到受保护知识键空间的零空间，以降低直接编辑的破坏。

**问题：** 如何从线性代数上限制编辑对保存知识映射的扰动？

**机制：** 用受保护键矩阵计算近似零空间，把 ROME/MEMIT 类候选更新投影后写入。

**步骤：**

1. 收集应保持知识的键矩阵
2. 计算近似零空间
3. 投影候选权重扰动
4. 嵌入定位式编辑流程

**证据：**

- ICLR 正式摘要给出受保护查询输出不变的线性条件，并报告跨 LLaMA3、GPT2-XL、GPT-J 和多种编辑法平均提升 36.7%。

**局限：**

- 保证依赖线性键空间与数值近似
- 不等于所有下游行为不变
- 协方差和零空间维护有成本
- 未直接解决多跳后果

**意义：**

- 为编辑局部性提供显式代数约束

**边界：** 正式版作者列表与早期 arXiv 不同，本记录采用 ICLR 正式版。

**标识：** arXiv 2410.02355；稳定 ID openreview:HvSytvg3Jh

**证据位置：**

- claim 零空间保证和 36.7% 平均提升；source ICLR official proceedings；location abstract

**资源：** [一手入口](<https://proceedings.iclr.cc/paper_files/paper/2025/hash/29c8c615b3187ee995029284702d3f43-Abstract-Conference.html>) · [PDF](<https://proceedings.iclr.cc/paper_files/paper/2025/file/29c8c615b3187ee995029284702d3f43-Paper-Conference.pdf>) · [项目页](<https://openreview.net/forum?id=HvSytvg3Jh>) · [代码](<https://github.com/jianghoucheng/AlphaEdit>)

---

<a id="paper-anyedit-2025"></a>
**103. AnyEdit：编辑语言模型中任意形式的知识｜AnyEdit: Edit Any Knowledge Encoded in Language Models（2025 · ICML 2025）**

**作者：** Houcheng Jiang、Junfeng Fang、Ningyu Zhang、Mingyang Wan、Guojun Ma、Xiang Wang、Xiangnan He、Tat-Seng Chua

**书目：** 年份 2025；载体 ICML 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改、评测、安全与治理；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 AnyEdit；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把长形式、多格式目标分解为顺序知识块，并自回归编辑多个关键标记，突破单标记编辑的长度和格式边界。

**问题：** 现有定位后编辑方法通常只扰动一个输入标记的隐藏状态；当目标知识很长或包含代码、数学表达等强依赖格式时，单点扰动难以稳定控制后续生成。

**机制：** AnyEdit 先把目标输出切成顺序块，以每块末端标记作为下一块的写入锚点；它依次输入原查询和已处理块，优化该锚点隐藏状态以提高下一块概率，再用现有编辑器的最小二乘更新把这些状态写回模型参数。

**步骤：**

1. 按固定窗口或语义边界把目标长文本切成顺序知识块。
2. 选择每块末端标记并用因果追踪定位影响层。
3. 将原查询与先前知识块作为条件，逐块优化所选标记的隐藏状态，使下一块生成概率提高。
4. 用定位后编辑方法的参数更新将多个目标状态写回模型，并按目标长度自适应决定编辑次数。

**证据：**

- 正式摘要报告 AnyEdit 在 UnKEBench、AKEW 和 EditEverything 的综合比较中相对强基线平均提高 21.5%；该数字只适用于论文给定模型、数据与指标。
- 第 4.2 节明确给出分块、定位、隐藏状态编辑和参数更新四步实现，证明它不是单纯的数据集或提示策略。
- 表 2 报告与 MEMIT、AlphaEdit 和 UnKE 组合后，平均单样本编辑时间相对增加约 24.7%，显示效果提升伴随可测计算代价。
- 结论与局限部分明确指出当前未为终身连续编辑优化，且只支持文本知识。

**局限：**

- 当前框架未显式优化连续、反复的终身编辑；多次长形式写入的累积干扰仍待验证。
- 目前只编辑文本知识，不支持跨文本、图像与音频的一致更新。
- 分块大小超过一定阈值时性能下降，且组合现有编辑器会增加单样本编辑时间。
- 主实验集中在约 7B 至 8B 参数模型和论文构造的数据集，不能直接外推到更大生产模型或真实更新流。

**意义：**

- 参数记忆编辑不必局限于主语—关系—宾语三元组；写入粒度可沿生成顺序扩展到多块依赖结构。
- 它把长形式编辑的关键瓶颈从单一参数定位转向分块策略、跨块一致性、累积代价和长期保持。

**边界：** 一手正式入口为 PMLR 的 ICML 2025 论文页；以正式全文和 arXiv v3 交叉核对第 3 至7节、图 1、图 4至5、表 1至2及附录。百分比不外推到论文未测试模型或部署环境。

**引用：** Jiang et al., Proceedings of the 42nd International Conference on Machine Learning, PMLR 267:27510–27533, 2025。

**版本：** 采用 ICML 2025 的 PMLR 正式版本；arXiv:2502.05628 属于同一版本族，不重复计数。

**标识：** arXiv 2502.05628；稳定 ID pmlr:v267:jiang25b；工作族 ID anyedit-long-form-editing-2502.05628

**证据位置：**

- 第 3 节、图 1至3，PMLR 正式页码 27511至27513：单标记编辑的格式与长度效能边界。
- 第 4.1至4.2 节，PMLR 正式页码 27513至27514：互信息链式分解及四步自回归编辑流程。
- 第 5.2至5.4 节、表 1、图 4至5，PMLR 正式页码 27515至27517：长形式、多格式与即插即用比较。
- 表 2，PMLR 正式页码 27517：组合现有编辑器后的单样本时间成本。
- 第 7 节，PMLR 正式页码 27518：终身编辑和多模态支持的局限。

**资源：** [一手入口](<https://proceedings.mlr.press/v267/jiang25b.html>) · [PDF](<https://raw.githubusercontent.com/mlresearch/v267/main/assets/jiang25b/jiang25b.pdf>) · [项目页](<https://github.com/jianghoucheng/AnyEdit>)

**关联 ID：** `a06` · `a08` · `unke-2025` · `dkme-2026`

---

<a id="paper-memory-layers-scale-2025"></a>
**104. 规模化记忆层｜Memory Layers at Scale（2025 · ICML 2025, PMLR 267:3831–3842）**

**作者：** Vincent-Pierre Berges、Barlas Oguz、Daniel Haziza、Wen-Tau Yih、Luke Zettlemoyer、Gargi Ghosh

**书目：** 年份 2025；载体 ICML 2025, PMLR 267:3831–3842；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 Memory Layers；优先级 high；时间尺度 随模型预训练并在模型生命周期内持久的稀疏键值容量

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** key-value-memory、sparse-retrieval、parameter-capacity、scaling

**定位：** 通过稀疏激活的可训练键值查找层增加大量事实容量，而不按同等比例增加每次前向计算。

**问题：** 语言模型的知识容量与稠密计算绑定，能否增加专用记忆参数而基本不增加 FLOPs？

**机制：** 在稠密前馈层旁加入可并行、稀疏读取的键值记忆层，只激活与当前查询匹配的少量槽位。

**步骤：**

1. 预训练可学习键和值
2. 查询时匹配少量键
3. 稀疏取回对应值
4. 把记忆输出与稠密网络融合

**证据：**

- 正式论文报告最高 128B 稀疏记忆参数并在 1T token 上训练
- 在论文所测计算预算与事实型任务中优于更大稠密或受测 MoE 对照
- 前向 FLOPs 没有同比增长不等于零存储、零带宽或零工程成本

**局限：**

- 这是训练期模型容量，不是交互后可写的个人记忆
- 128B 记忆参数仍有存储、通信和部署成本
- 没有按用户删除、回滚、来源或时间更新知识的机制

**意义：**

- 给参数记忆增加一条‘容量与计算解耦’分支
- 提醒记忆成本必须区分 FLOPs、参数存储、通信和更新成本

**边界：** ICML 2025 / PMLR 267 正式页和 PDF 全文核验；把无额外 FLOPs 限定为论文的机制比较，不外推为零带宽、零工程或零端到端成本。

**标识：** 稳定 ID pmlr:v267:berges25a

**证据位置：**

- claim 稀疏可训练键值记忆层机制；location 正式 PDF §3–4、Figure 1；来源 一手入口
- claim 最高 128B memory 参数、1T token 训练和最多 8B 主干的规模结果；location 正式 PDF §5–6、Tables 1–4；来源 一手入口

**资源：** [一手入口](<https://proceedings.mlr.press/v267/berges25a.html>) · [PDF](<https://raw.githubusercontent.com/mlresearch/v267/main/assets/berges25a/berges25a.pdf>)

**关联 ID：** `a02` · `a01` · `ext-knnlm-2020`

---

<a id="paper-ssu-copyright-2025"></a>
**105. 通过大模型遗忘避免版权侵权｜Avoiding Copyright Infringement via Large Language Model Unlearning（2025 · Findings of NAACL 2025）**

**作者：** Guangyao Dou、Zheyuan Liu、Qing Lyu、Kaize Ding、Eric Wong

**书目：** 年份 2025；载体 Findings of NAACL 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改、评测、安全与治理；层级 模型生命周期；阅读层级 核心；证据等级 B；简称 SSU；优先级 high；时间尺度 连续到达的版权删除请求；多时间步参数持久更新

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** sequential-unlearning、copyright、task-vector、weight-saliency、governance

**定位：** 把单次版权遗忘改造成连续请求协议，以稳定任务向量、随机标签损失和显著性掩码减轻累计效用损伤。

**问题：** 版权删除请求会分批到达；逐轮应用既有遗忘方法可能累积参数漂移、破坏保留知识，并使已遗忘内容在后续轮次重新出现。

**机制：** 每轮先在当前待遗忘书籍上学习任务向量，再从上一轮模型减去该向量；随机标签损失加强忘却，梯度显著性图限制更新参数以保持一般能力。

**步骤：**

1. 接收第t轮版权删除请求，并以待遗忘文本和随机标签联合构造忘却目标。
2. 从上一轮已遗忘模型出发微调得到任务向量，随后用向量相减抵消目标知识。
3. 依据梯度显著性只更新最相关权重，降低对未删除书籍和一般能力的附带损伤。
4. 对后续请求重复同一过程，并跨时间步同时检查当前、历史待遗忘集合和保留集合。

**证据：**

- Figure 2与§4.1至§4.2给出单轮稳定任务向量、随机标签损失、显著性映射和连续迭代流程。
- Figure 2的跨时间步曲线同时比较待遗忘书籍、语义相似书籍、语义不相似书籍和普通书籍，显示SSU在忘却与保留之间取得更稳定权衡。
- Table 1报告不同时间步与超参数下的待遗忘、历史待遗忘、普通书籍和MMLU、MT-Bench结果；Figure 5给出组件消融。
- Limitations明确报告词法指标可能高估隐私，并观察到所有测试遗忘算法在后续时间步都可能出现知识重新显现。

**局限：**

- 评价主要依赖Jaccard和ROUGE等词法重合，不能证明语义、参数或法律意义上的彻底删除。
- 作者观察到已遗忘书籍知识在后续时间步重新出现，说明SSU仍不是不可逆删除保证。
- 实验集中于书籍版权场景和所选开源模型，不能直接外推全部内容类型与商业模型。
- 方法处理参数记忆，不覆盖外部检索库、日志、缓存、备份和模型副本的删除。

**意义：**

- 遗忘请求应按时间序列评测，单轮成功不能代表长期合规。
- 版权治理需要把遗忘效果、历史请求保持、一般能力和生成时检测联合审计。

**边界：** ACL Anthology正式页核验题名、作者、页码和DOI；正式全文核验方法、表图、消融与局限。

**引用：** Dou等，Findings of NAACL 2025，5191至5215页，DOI 10.18653/v1/2025.findings-naacl.288。

**版本：** 以Findings of NAACL 2025正式论文集为主；arXiv仅作公开版本族入口。

**标识：** DOI 10.18653/v1/2025.findings-naacl.288；arXiv 2406.10952；稳定 ID doi:10.18653/v1/2025.findings-naacl.288；工作族 ID stable-sequential-unlearning-copyright

**证据位置：**

- Figure 1，PDF第1页：版权文本续写示例与问题动机
- Figure 2、§4.1至§4.2，PDF第4至6页：SSU流程、任务向量、随机标签损失和连续遗忘
- Figure 2、Table 1，PDF第7至8页及附录：跨时间步忘却、保留与一般能力结果
- Figure 5，附录：权重显著性与随机标签损失消融
- Limitations，PDF第9页：词法指标、知识重新显现和附带能力损失

**资源：** [一手入口](<https://aclanthology.org/2025.findings-naacl.288/>) · [PDF](<https://aclanthology.org/2025.findings-naacl.288.pdf>) · [arXiv](<https://arxiv.org/abs/2406.10952>)

**关联 ID：** `muse-2025` · `openunlearning-2025` · `stableedit-2026` · `unlearning-or-obfuscating-2025`

---

<a id="paper-unke-2025"></a>
**106. 一切皆可编辑：将大模型知识编辑扩展到非结构化数据｜Everything is Editable: Extend Knowledge Editing to Unstructured Data in Large Language Models（2025 · ICLR 2025）**

**作者：** Jingcheng Deng、Zihao Wei、Liang Pang、Hanxing Ding、Huawei Shen、Xueqi Cheng

**书目：** 年份 2025；载体 ICLR 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改、评测、安全与治理；层级 模型生命周期；阅读层级 桥接；证据等级 A；简称 UnKE；优先级 medium；相关性排序 8；时间尺度 模型生命周期中的长文本知识更新

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** unstructured-editing、long-form-knowledge、locality、sequential-editing

**定位：** 把知识编辑从原子三元组扩展到带噪长文本，通过跨 MLP 与注意力块的非局部存储和因果定位写入整段知识。

**问题：** 短事实编辑难以表达说明文、规则与例外；直接微调长文本又会损害无关知识。

**机制：** 定位驱动目标回答的末 token 表示，并跨多个 MLP 和注意力块优化键值式记忆，以目标、保持和因果约束支持批量与连续写入。

**步骤：**

1. 从非结构化文档和目标问答构造信号
2. 定位驱动回答的关键末 token 表示
3. 跨 MLP 与注意力块联合写入
4. 用目标、局部性和保持损失约束编辑

**证据：**

- Table 2 的 Llama2 配置中，UnKE 的 BERT 为 99.61/93.09、FactScore 42.49；Loc-FactScore 从底座 72.01 降至 70.95，成功与局部性代价必须并列。
- Table 3 人工评测仅抽 36 个样本、3 名标注者，Fleiss κ=0.57；不能把高主观分外推为稳定人工验证。
- Appendix E 的连续实验只展示前 64 个样本；所有方法随编辑数增加而退化，UnKE 只是下降较慢。

**局限：**

- 人工评测只有 36 个样本且一致性中等
- 连续实验规模小且仍随编辑数退化
- 长文本写入同样可能植入错误或恶意知识

**意义：**

- 编辑评测应纳入长文本、噪声与例外
- 必须并列编辑成功和局部性下降
- 生产系统需要来源、版本和撤销机制

**建议路线：** 非结构化知识编辑

**边界：** 正式全文支持非结构化编辑这一独特数据形态，但已有编辑核心路线，故保留为 bridge。

**版本：** 以 ICLR 2025 正式 proceedings 为准。

**标识：** 稳定 ID iclr:2025:02763667a5761ff92bb15d8751bcd223；工作族 ID unke-2025

**证据位置：**

- claim 非局部块存储与因果优化；location 正式 PDF §4、Figure 2；来源 PDF
- claim 自动和人工结果；location 正式 PDF §5、Tables 2–3，pp. 7–8；来源 PDF
- claim 连续编辑退化；location 正式 PDF Appendix E、Figure 3，p. 18；来源 PDF
- claim 风险边界；location 正式 PDF Broader Impact；来源 PDF

**资源：** [一手入口](<https://proceedings.iclr.cc/paper_files/paper/2025/hash/02763667a5761ff92bb15d8751bcd223-Abstract-Conference.html>) · [PDF](<https://proceedings.iclr.cc/paper_files/paper/2025/file/02763667a5761ff92bb15d8751bcd223-Paper-Conference.pdf>)

**关联 ID：** `a06` · `a07` · `a08` · `model-editing-harms-2024`

---

<a id="paper-dkme-2026"></a>
**107. DKME：重思大语言模型终身编辑中的耦合知识记忆｜DKME: Rethinking Coupled Knowledge Memory for Lifelong Model Editing of Large Language Models（2026 · Findings of ACL 2026）**

**作者：** Guanyu Zheng、Zhenyu Wang、Tingting He、Xv Wang、Haochang Wang、Yaokai Huang、Tiejun Zhao

**书目：** 年份 2026；载体 Findings of ACL 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改、评测、安全与治理；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 DKME；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把连续知识编辑中的寻址与写入拆开，并用事实感知路由和分区参数记忆限制跨编辑干扰。

**问题：** 既有基于记忆的编辑器常让路由决策与记忆参数更新共用同一机制；多领域或同一实体多属性的编辑流会改变旧编辑的激活与存储，造成累积遗忘。

**机制：** DKME 先学习独立的事实感知语义寻址空间，再按该空间聚类得到参数记忆分区；编辑与推理时只激活并更新一个分区，冻结骨干和其他分区。

**步骤：**

1. 用同事实正例和语义相近但事实不同的难负例微调语义编码器，形成事实边界更清晰的寻址流形。
2. 以相似度阈值判断查询是否落入历史编辑范围；范围外查询直接交给冻结骨干。
3. 在寻址流形上用降维与密度聚类发现记忆分区，不预先固定分区数。
4. 范围内编辑只更新所选前馈层分区的值侧参数；推理复用相同路由和分区选择。

**证据：**

- HalluEditBench 的 1000 次编辑条件下，表 2 报告 DKME 的综合均值为 0.825，高于 GRACE 的 0.578 与 WISE 的 0.554；同时保持可靠性 0.923、泛化 0.718、局部性 0.834。
- CKnowEdit 与 WikiDataCounterfact 的表 3–4 在各报告编辑规模上均给出 DKME 的最高综合均值；关联事实条件下 60、100、300、500 次编辑的综合均值分别为 0.662、0.716、0.694、0.678。
- 表 5 的 1000 次编辑消融中，移除独立语义寻址使综合均值从 0.825 降到 0.620，移除分区存储降到 0.665，直接支持两个阶段的必要性。

**局限：**

- 分区参数带来额外存储；极长编辑流中的分区数可能成为扩展瓶颈，需要合并、剪枝或回收。
- 依赖外部语义编码器、降维和聚类，事实边界模糊时路由与分区可能敏感。
- 主实验使用 LLaMA3.2-3B 与 Qwen2.5-1.5B 及人工构造编辑流，尚不能等同于生产模型上的长期部署稳定性。

**意义：**

- 持续编辑的关键不只是增加存储容量，还要把查询寻址和参数写入的干扰通道分开。
- 把记忆分区视为可增长资源会把知识编辑与智能体记忆中的压缩、合并和遗忘问题连接起来。

**边界：** 一手正式入口为 ACL Anthology；已核对全文第 4–6 节、图 2、表 2–5 与局限部分。数值只用于论文报告的模型、数据和编辑流。

**引用：** Zheng et al., Findings of ACL 2026, DOI 10.18653/v1/2026.findings-acl.792。

**版本：** 采用 Findings of ACL 2026 正式版本。

**标识：** DOI 10.18653/v1/2026.findings-acl.792；稳定 ID doi:10.18653/v1/2026.findings-acl.792；工作族 ID dkme-lifelong-editing-2026

**证据位置：**

- 第 4.1–4.2 节与图 2，正式页码 16131–16132：事实感知寻址、阈值路由和分区参数写入。
- 表 2–4，正式页码 16133–16134：三个基准及不同编辑规模的主结果。
- 表 5 与第 5.4 节，正式页码 16135：寻址、分区和路由消融及敏感性。
- 局限部分，正式页码 16136：存储、路由和聚类扩展边界。

**资源：** [一手入口](<https://aclanthology.org/2026.findings-acl.792/>) · [PDF](<https://aclanthology.org/2026.findings-acl.792.pdf>)

**关联 ID：** `a07` · `a08` · `superficial-editing-2025`

---

<a id="paper-stableedit-2026"></a>
**108. 编辑越多，反而越稳定：理解连续模型编辑中的持续归一化｜More Edits, More Stable: Understanding the Lifelong Normalization in Sequential Model Editing（2026 · ICML 2026）**

**作者：** Xin Ma、Wei Chen、Qi Liu、Derong Xu、Zhi Zheng、Tong Xu、Enhong Chen

**书目：** 年份 2026；载体 ICML 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改、评测、安全与治理；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 StableEdit；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 从统计与几何角度解释持续归一化为何稳定大规模连续编辑，并据此构造带预热和完整白化的 StableEdit。

**问题：** 连续参数编辑为什么会遗忘或崩溃，已有成功编辑器共同使用的持续归一化究竟如何发挥作用？

**机制：** 维护编辑梯度的运行均值与协方差，以完整白化形成有界且渐近正交的更新，并用预热阶段避开早期统计不可靠区间。

**步骤：**

1. 用少量预热编辑初始化每个可编辑层的运行均值和协方差
2. 在每次编辑提取隐藏态与 value gradient，并递归聚合历史统计
3. 对梯度中心化和完整白化，再以岭回归求解参数更新
4. 有界且弱干扰的更新减少未来分布漂移，形成自增强稳定循环

**证据：**

- 表1第6页：在 WikiBigEdit 的标准规模设置中，Mistral 上 StableEdit 的 78.90/72.57/47.36 高于 UltraEdit 的 76.00/70.15/46.09。
- 表2第7页：50万次编辑下三个模型的全部报告指标均高于 UltraEdit；GPT-J 泛化为65.04，对照为60.54。
- 表4第9页：移除持续归一化后，78.90/72.57/47.36 降至39.13/38.18/41.61。
- 图4第9页直接诊断更新几何，支持近正交与范数受控的理论解释。

**局限：**

- 理论依赖平滑性、非退化协方差、可跟踪漂移和有限四阶矩等假设。
- 论文没有独立局限节；附录指出标记匹配指标不总能反映真实编辑效果。
- 验证集中于参数模型编辑，不证明外部检索记忆或真实在线知识治理的长期效果。

**意义：**

- 新增“持续归一化的自增强稳定环”二级分支。
- 说明连续编辑的稳定性不只来自隔离存储，也可来自历史统计驱动的更新几何。

**边界：** ICML 官方海报页核验出版状态；arXiv v2 全文核验机制、表格、消融和理论边界。

**引用：** Xin Ma et al., ICML 2026；会议 DOI/PMLR文章页在冻结时未解析，不虚构页码或 DOI。

**DOI：** arxiv\_doi 仅标识预印本，不是 ICML 会议 DOI。

**版本：** 作品族含 arXiv v1、v2 与 ICML 官方 Poster 64339；主状态由 ICML 官方页确认，DBLP 冻结时仍仅有 CoRR。

**标识：** arXiv DOI 10.48550/arXiv.2605.11836；稳定 ID icml-poster:64339；工作族 ID stableedit-2605-11836

**证据位置：**

- 第2.2节，第2页：持续归一化定义与共同模式
- 第3.1至3.3节，第3至5页：递归统计、理论性质和 StableEdit
- 第4节及表1至3，第6至7页：主结果、超大规模编辑和效率
- 图2，第8页；表4至5及图4，第9页：通用能力、消融、预热稳健性与更新几何
- 附录C，第35页：理论假设边界

**资源：** [一手入口](<https://icml.cc/virtual/2026/poster/64339>) · [PDF](<https://arxiv.org/pdf/2605.11836v2>) · [arXiv](<https://arxiv.org/abs/2605.11836>)

**关联 ID：** `dkme-2026` · `a07` · `a08` · `wikibigedit-2025` · `model-editing-harms-2024`

---

<a id="paper-zk-apex-2026"></a>
**109. ZK-APEX：带可执行证明的零知识近似个性化遗忘｜ZK-APEX: Zero-Knowledge Approximate Personalized Unlearning with Executable Proofs（2026 · MLSys 2026）**

**作者：** Mohammad M Maheri、Sunil Cotterill、Alex Davidson、Hamed Haddadi

**书目：** 年份 2026；载体 MLSys 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 conference paper

**分类：** 主路线 参数记忆与知识修改；相关路线 参数记忆与知识修改、评测、安全与治理、个性化与用户长期记忆；层级 模型生命周期；阅读层级 桥接；证据等级 B；简称 ZK-APEX；优先级 medium

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把个性化模型遗忘变成客户端可执行、服务方可零知识证明的确定性变换，但只证明程序执行，不证明语义删除。

**问题：** 个性化模型持有者既要删除某类信息，又不能把私有数据或模型细节完全交给审计方。

**机制：** 服务方承诺稀疏显著性掩码，客户端用经验 Fisher 近似执行 Group-OBS 补偿，并为同一确定性变换生成 Halo2 证明。

**步骤：**

1. 服务方基于遗忘目标生成稀疏显著性掩码并作密码学承诺。
2. 客户端在本地估计经验 Fisher 分组信息，执行 Group-OBS 权重补偿以降低目标影响并保留个性化能力。
3. 电路把承诺、掩码和变换绑定，生成零知识执行证明。
4. 验证者核验变换确按协议执行；论文明确旧模型安全擦除和语义保证不在证明范围。

**证据：**

- 表 1：视觉设置中遗忘指标从 89.5 降至 50.2±0.1，个性化性能从 81.0 保持为 80.9±0.2，成员推断为 50.2±0.1；证明约 2.0±0.1 小时、0.7GB、401.5MB，验证约 10 分钟。
- 表 2：OPT-125M 中目标指标从 65.3 降至 61.8，个性化性能从 77.4 降至 75.6；精确基线为 61.5／77.3。
- §3 明确零知识证明只证明过程符合约定，不能证明被遗忘语义已完全消失；旧模型安全擦除也超出范围。
- 附录 K 显示在 20% 遗忘强度时固定 4% 掩码不足，遗忘效果变弱。

**局限：**

- 证明程序执行而非语义遗忘，也不保证旧模型副本已安全删除。
- 证明生成约两小时、证明文件数百 MB，运行成本高。
- 大模型实验仅 OPT-125M，不能外推现代大模型。
- 固定稀疏掩码、残差和交叉曲率假设在高重叠删除时失效。

**意义：**

- 删除治理应把语义效果、程序执行证明和旧副本擦除分成三个独立声明。
- 可验证遗忘可作为个性化模型生命周期的治理桥接，但不能替代端到端删除审计。

**边界：** 删除边界按原文降敏记录；不把过程证明误写成语义证明。

**引用：** 治理桥接项；未提供攻击或规避审计步骤。

**版本：** 以 MLSys 2026 正式论文集全文为准。

**标识：** 稳定 ID mlsys-2026-148865706acbd18627d3fc15cc3f3b93；工作族 ID zk-apex-2026

**证据位置：**

- §3–§5，PDF 第 6–9 页：协议、Group-OBS 与证明边界
- 表 1–表 3，PDF 第 10 页：效果、成本和移动分块
- 附录 K：高遗忘比例失效

**资源：** [一手入口](<https://proceedings.mlsys.org/paper_files/paper/2026/hash/148865706acbd18627d3fc15cc3f3b93-Abstract-Conference.html>) · [PDF](<https://proceedings.mlsys.org/paper_files/paper/2026/file/148865706acbd18627d3fc15cc3f3b93-Paper-Conference.pdf>)

**关联 ID：** `can-sensitive-information-be-deleted-2024` · `muse-2025` · `unlearning-or-obfuscating-2025` · `fast-exact-unlearning-2025`

---

<a id="paper-blenderbot3-2022"></a>
**110. BlenderBot 3：可持续学习并负责任交互的已部署对话智能体｜BlenderBot 3: a deployed conversational agent that continually learns to responsibly engage（2022 · Meta AI technical report）**

**作者：** Kurt Shuster、Jing Xu、Mojtaba Komeili、Da Ju、Eric Michael Smith、Stephen Roller、Megan Ung、Moya Chen、Kushal Arora、Joshua Lane、Morteza Behrooz、William Ngan、Spencer Poff、Naman Goyal、Arthur Szlam、Y-Lan Boureau、Melanie Kambadur、Jason Weston

**书目：** 年份 2022；载体 Meta AI technical report；状态 官方报告或标准；来源类型 official-report

**分类：** 主路线 个性化与用户长期记忆；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 桥接；证据等级 B；简称 BlenderBot 3；优先级 2；相关性排序 12

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=PX / Q=Q2

**定位：** 少数给出公开真实用户部署记录、长期记忆模块和部署安全问题的一手报告。

**问题：** 离线对话基准难以反映开放网络环境中的真实用户行为、反馈和安全故障。

**机制：** 系统结合检索、长期记忆、对话生成和安全模块；公开部署收集自然交互与反馈，并以批次方式改进。

**步骤：**

1. 公开部署对话系统
2. 在交互中调用网络与长期记忆
3. 收集用户反馈和安全信号
4. 在后续批次中更新系统

**证据：**

- 技术报告与官方博客记录了公开网页部署和自然用户反馈。
- 报告明确其持续学习是后续批次更新，而非每次交互后即时在线改权重。

**局限：**

- 这是机构技术报告，不是同行评议论文。
- 公开部署证明了可运行性，但没有给出受控的长期记忆因果效果或跨月留存研究。
- 部署范围、用户选择和安全暴露使结果不能直接泛化。

**意义：**

- 提供研究原型之外的稀缺部署证据。
- 提醒将产品存在、用户量与记忆有效性、安全性三者分开。

**边界：** 正式状态标为官方报告，未把 arXiv 页面误标为同行评议。

**标识：** 工作族 ID blenderbot3-2208.03188

**证据位置：**

- claim 互联网和长期记忆能力；location arXiv 摘要与 §1；来源 一手入口
- claim 公开部署、用户反馈和安全边界；location Meta AI 官方部署博客；来源 项目页

**资源：** [一手入口](<https://arxiv.org/abs/2208.03188>) · [项目页](<https://ai.meta.com/blog/blenderbot-3-a-175b-parameter-publicly-available-chatbot-that-improves-its-skills-and-safety-over-time/>) · [代码](<https://github.com/facebookresearch/ParlAI/tree/main/projects/bb3>) · [数据](<https://parl.ai/projects/bb3/>)

---

<a id="paper-keep-me-updated-2022"></a>
**111. 让我保持最新：长期对话中的记忆管理｜Keep Me Updated! Memory Management in Long-term Conversations（2022 · Findings of EMNLP 2022）**

**作者：** Sanghwan Bae、Donghyun Kwak、Soyoung Kang、Min Young Lee、Sungdong Kim、Yuin Jeong、Hyeri Kim、Sang-Woo Lee、Woomyoung Park、Nako Sung

**书目：** 年份 2022；载体 Findings of EMNLP 2022；状态 同行评议；来源类型 paper+dataset

**分类：** 主路线 个性化与用户长期记忆；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Keep Me Updated；优先级 1；相关性排序 3

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 较早把“记住最新状态”与“删除已失效或冗余信息”设为长期对话的核心问题。

**问题：** 不变的记忆库会在用户状态改变后继续暴露旧事实，导致后续对话混乱。

**机制：** 每个会话结束后把用户信息总结为文本句子，对旧记忆与新摘要逐对预测保留、替换、并存或双方删除，形成下一会话记忆；生成时检索与当前对话相关的记忆句。

**步骤：**

1. 抽取关键用户信息
2. 以文本条目保存
3. 比较新旧信息的有效性与冗余
4. 删除失效项并使用最新记忆回复

**证据：**

- CareCallmem 含 7,665 个韩语会话和 160,191 个轮次；表 3 中最佳更新模型达到 84.10% 的四类关系准确率和 88.69 的集合级句子 F1。
- 表 4显示选择性更新相对单纯累积的优势主要在后期及显式需要记忆的轮次增大。
- 表 5 的 155 个情节人评中，更新模型的可记忆性显著高于累积模型，且参与感和拟人感平均分更高；显著性直接报告在可记忆性上。

**局限：**

- 只建模单一对话方，数据仅为韩语，交互评测为五会话情节，未覆盖极长真实对话。
- 逐对更新复杂度为 O(|M||S|)，记忆规模增大时计算成本上升。
- 应用内删除不构成对备份、日志、缓存或模型权重的可验证删除保证。

**意义：**

- 证明长期记忆质量不仅取决于召回，也取决于更新和失效处理。
- 为 Memora/FAMA 等后续遗忘感知评测提供历史先导。

**边界：** 已核验 ACL Anthology 正式全文；人评文案区分显著的可记忆性结果与仅平均得分更高的参与感、拟人感结果。

**标识：** DOI 10.18653/v1/2022.findings-emnlp.276

**证据位置：**

- claim 四类文本记忆更新操作；location 算法 1；第 4.3 节；来源 PDF
- claim 更新分类、自动生成和人评结果；location 表 3 至表 5；第 5.1 至 5.2 节；来源 PDF
- claim 语言、参与方、长度与计算限制；location Limitations；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2022.findings-emnlp.276/>) · [PDF](<https://aclanthology.org/2022.findings-emnlp.276.pdf>)

---

<a id="paper-msc-2022"></a>
**112. 超越金鱼式记忆：长期开放域对话｜Beyond Goldfish Memory: Long-Term Open-Domain Conversation（2022 · ACL 2022）**

**作者：** Jing Xu、Arthur Szlam、Jason Weston

**书目：** 年份 2022；载体 ACL 2022；状态 同行评议；来源类型 paper+dataset

**分类：** 主路线 个性化与用户长期记忆；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 MSC；优先级 1；相关性排序 1

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V4 / D=D3 / P=P3 / Q=Q3

**定位：** 把开放域聊天从单会话扩展到最多五次会话，并比较读取整段历史、检索和摘要式记忆。

**问题：** 标准开放域对话数据几乎没有跨会话历史，无法检验系统能否延续人物事实和共同经历。

**机制：** Multi-Session Chat 数据集让同一人物在多次会话中继续互动；模型以原始历史、检索到的历史或摘要记忆为条件生成。

**步骤：**

1. 采集带人物设定的连续多会话对话
2. 保存原始对话与会话摘要
3. 在后续会话读取、检索或摘要召回
4. 比较困惑度、生成质量和人工评价

**证据：**

- 数据覆盖最多五次会话；论文报告检索与摘要式记忆优于不显式利用长期历史的标准编码器—解码器。
- MSC 成为 LoCoMo 等后续长期对话基准的直接数据与任务先导。

**局限：**

- 对话由众包角色扮演产生；同一角色跨会话可能由不同工作者扮演。
- 最多五次会话，不等于真实用户数月或数年的互动。
- 人物事实一致不等于偏好理解、隐式约束、安全或删除能力。

**意义：**

- 建立跨会话开放域对话的早期可复现实验底座。
- 后续评测必须把真实用户互动与角色扮演数据的外部效度分开。

**边界：** ACL Anthology 正式页与 PDF 核验；题名、作者、年份和 DOI 以正式页为准。

**标识：** DOI 10.18653/v1/2022.acl-long.356

**证据位置：**

- claim 任务、数据和记忆方案；location 摘要；§1；§3；来源 PDF
- claim 会话数与数据规模；location Table 1、Table 2；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2022.acl-long.356/>) · [PDF](<https://aclanthology.org/2022.acl-long.356.pdf>)

---

<a id="paper-plato-ltm-2022"></a>
**113. 好久不见：具有长期人物记忆的开放域对话｜Long Time No See! Open-Domain Conversation with Long-Term Persona Memory（2022 · Findings of ACL 2022）**

**作者：** Xinchao Xu、Zhibin Gou、Wenquan Wu、Zheng-Yu Niu、Hua Wu、Haifeng Wang、Shihang Wang

**书目：** 年份 2022；载体 Findings of ACL 2022；状态 同行评议；来源类型 paper+dataset

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 PLATO-LTM；优先级 1；相关性排序 2

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q2

**定位：** 以 DuLeMon 和 PLATO-LTM 建立用户与机器人双方人物信息的实时抽取和持续更新机制。

**问题：** 静态 persona 难以表示长期互动中不断出现或变化的用户与机器人信息。

**机制：** 从当前对话抽取用户与机器人的人物句，分别写入长期存储；相似度超过阈值时替换最相近条目，否则追加，再检索相关人物句作为 PLATO-2 生成条件。

**步骤：**

1. 从对话识别人格事实
2. 区分用户与机器人记忆
3. 持续合并或更新长期 persona
4. 用相关 persona 生成后续回复

**证据：**

- DuLeMon 含 27,501 个中文对话；表 3 中人物抽取器 F1 为 0.91，正文报告记忆读取 AUC 0.76、召回率@5 为 0.83。
- 表 5 的受控自对话人评中，PLATO-LTM 的一致性为 0.87，PLATO-FT 为 0.40，PLATO-2 为 0.13；参与感分别为 1.54、1.40 和 1.46。
- 完整系统连贯性为 1.67，略低于 PLATO-2 的 1.70，说明收益主要体现在人物一致性而非所有对话质量维度。

**局限：**

- 写入更新主要是相似项替换，不是显式语义冲突解析，也没有删除、授权或来源治理机制。
- 数据和评测聚焦中文可陈述人物事实；人评使用 PLATO-LTM 作为用户模拟器，每个系统仅 10 个情节。
- 无需多会话训练数据不等于无需训练；生成器、人物抽取器和检索器仍依赖训练或监督数据。

**意义：**

- 把个性化记忆从一次性 persona 提示推进到可更新的跨会话状态。
- 为后续更新、冲突消解和遗忘评测提出明确对象。

**边界：** 已核验 ACL Anthology 正式全文；作者的首创表述不作为独立事实，且将相似项替换与真正冲突消解区分。

**标识：** DOI 10.18653/v1/2022.findings-acl.207

**证据位置：**

- claim 人物抽取、相似项写入和检索生成机制；location 图 3；第 4.1 至 4.3 节；来源 PDF
- claim 抽取、检索和生成模块结果；location 表 3 至表 4；第 5.3.1 至 5.3.2 节；来源 PDF
- claim 受控自对话人评及其范围；location 表 5；第 5.3.3 节；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2022.findings-acl.207/>) · [PDF](<https://aclanthology.org/2022.findings-acl.207.pdf>)

---

<a id="paper-carecall-ltm-self-disclosure-2024"></a>
**114. 长期记忆对公共卫生大模型聊天机器人中自我披露的影响｜Understanding the Impact of Long-Term Memory on Self-Disclosure with Large Language Model-Driven Chatbots for Public Health Intervention（2024 · CHI 2024）**

**作者：** Eunkyung Jo、Yuin Jeong、SoHyun Park、Daniel A. Epstein、Young-Ho Kim

**书目：** 年份 2024；载体 CHI 2024；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆、评测、安全与治理；层级 跨会话长期；阅读层级 桥接；证据等级 A；简称 CareCall LTM；优先级 medium；时间尺度 真实公共卫生部署中的多周重复通话

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** long-term-memory、personalization、self-disclosure、public-health、privacy、longitudinal-deployment

**定位：** CareCall 把每次通话后的用户健康与日常摘要更新为跨会话记忆；正式部署日志显示更深的健康披露和更积极的反应，同时暴露慢性病追问与敏感信息记忆的隐私张力。

**问题：** 长期记忆可能让公共卫生聊天机器人表现得更熟悉并促进披露，但也可能让敏感追问变得侵入；此前缺少真实部署中把记忆机制、行为日志和用户感受结合起来的证据。

**机制：** 系统在每次通话结束后用大模型摘要器提取健康、饮食、睡眠、到访地点和宠物五类信息，由记忆管理层保存与更新，再把摘要注入后续通话的模型输入以触发针对性追问。

**步骤：**

1. 在当前通话中收集健康、饮食、睡眠、地点和宠物相关事实
2. 通话结束后由大模型摘要器生成与五类主题相关的摘要句
3. 记忆管理层保存新摘要并在状态变化时更新旧摘要
4. 后续通话把相关摘要加入模型输入，生成承接历史的追问

**证据：**

- 研究分析 1,252 份真实通话日志：有长期记忆组 66 人、576 次通话，无长期记忆组 81 人、676 次通话；另访谈 9 名长期记忆组用户。
- 有记忆组每次通话的简单披露和详细披露均更高：简单披露 p=0.01，95% 置信区间为每次通话高 0.05—0.41 个编码；详细披露 p&lt;0.001，95% 置信区间高 0.32—0.74 个编码。
- 效果并非所有主题一致：详细健康与临床信息显著增加，但简单睡眠披露更低，详细睡眠披露无显著差异，地点与宠物也未带来更高披露。
- 有记忆组的积极反应更高，p=0.001，95% 置信区间高 0.34—1.38 个编码；平均通话时长为 87.89 秒，对照组为 75.48 秒，p&lt;0.001。
- 访谈显示过细的敏感健康追问会引发隐私顾虑，论文明确提出公共卫生效用与用户删除、清除或不存储选择之间存在治理张力。

**局限：**

- 有记忆组与无记忆组来自不同日历时期，不是同期随机分组；季节、产品熟悉度和外部传播可能混杂组间差异，因此不能把观察结果直接解释为严格因果效应。
- 两组性别构成不同，且日志分析缺少更丰富的人口统计信息。
- 系统用于韩国独居中老年人的公共卫生通话，不能直接外推到一般聊天机器人、其他文化或低敏感任务。
- 记忆主题和优先级由公共卫生目标预先设定；对其他主题、用户自主选择及可逆删除的效果仍未证明。

**意义：**

- 长期记忆评测不能只测事实召回，还应测披露深度、互动持续时间、熟悉感、挫败与隐私顾虑。
- 记忆的主题选择和追问方式会改变用户结果；更多记忆并不在所有主题上都更好。
- 高敏感部署需要把记忆最小化、可见性、修正与删除同公共服务义务一起设计，而不能只优化个性化。
- 该工作为合成基准与真实部署之间提供桥接，但不提供随机因果证据。

**边界：** 题名、作者、年份和 DOI 以 ACM 正式入口为准；机制、数值、局限与治理讨论由作者开放的 CHI 正式论文全文核验。该研究是非随机的真实部署比较，组间差异按关联证据表述。

**标识：** DOI 10.1145/3613904.3642420；稳定 ID doi:10.1145/3613904.3642420

**证据位置：**

- claim 样本、部署与访谈规模；location 作者开放 PDF 第 1 页，第 4.1 节第 5—6 页；第 1 页正文与第 5 页流程图附近；来源 PDF
- claim 摘要—更新—后续注入机制；location 作者开放 PDF 第 3—4 页，Figure 1 及其图注；正文第 3.1—3.2 节；来源 PDF
- claim 披露主结果与主题异质性；location 作者开放 PDF 第 7—8 页，第 5.1.1 节与 Table 2；来源 PDF
- claim 积极反应与通话时长；location 作者开放 PDF 第 10 页，第 5.2 节与 Table 3；来源 PDF
- claim 隐私张力与研究局限；location 作者开放 PDF 第 13—15 页，第 5.3.2、6.3 与 6.4 节；来源 PDF

**资源：** [一手入口](<https://doi.org/10.1145/3613904.3642420>) · [PDF](<https://depstein.net/assets/pubs/ejo_chi24.pdf>)

**关联 ID：** `memorybank-2024` · `recallbot-2026` · `relational-gains-privacy-strains-2026`

---

<a id="paper-memorybank-2024"></a>
**115. MemoryBank：用长期记忆增强大语言模型｜MemoryBank: Enhancing Large Language Models with Long-Term Memory（2024 · AAAI 2024）**

**作者：** Wanjun Zhong、Lianghong Guo、Qiqi Gao、He Ye、Yanlin Wang

**书目：** 年份 2024；载体 AAAI 2024；状态 同行评议；来源类型 paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 MemoryBank；优先级 1；相关性排序 4

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V4 / D=D3 / P=P3 / Q=Q2

**定位：** 把日常对话、事件摘要和动态用户画像组织为可检索、可更新的长期记忆，并提出 SiliconFriend 应用。

**问题：** LLM 对话受上下文窗口约束，难以长期保存个人经历、关系和用户特征。

**机制：** 分层保存对话与事件摘要，检索相关记忆生成回复，并用受遗忘曲线启发的规则更新记忆强度和用户画像。

**步骤：**

1. 保存原始日常对话
2. 形成事件级摘要
3. 维护动态用户画像
4. 按相关性与记忆强度检索
5. 生成并更新长期记忆

**证据：**

- 正式 AAAI 页面核验其长期记忆目标和 SiliconFriend 应用。
- 论文实验显示记忆库有助于持续、多轮、个性化交互。

**局限：**

- 定量对话包含由 ChatGPT 模拟的用户，真实长期使用证据有限。
- 遗忘曲线是工程启发式，不等于经过认知实验验证的人类遗忘模型。
- 没有给出隐私、访问控制或可验证删除保证。

**意义：**

- 成为文本外部长期记忆与个性化助手的重要代表。
- 也暴露了生成式模拟用户与心理学类比可能造成的证据过度解释。

**边界：** 出版状态依据 AAAI 正式页；arXiv 仅作为同一版本族的全文入口。

**标识：** DOI 10.1609/aaai.v38i17.29946；工作族 ID memorybank-2305.10250

**证据位置：**

- claim 正式出版元数据与摘要；location AAAI 正式页；来源 一手入口
- claim 存储、检索和更新机制；location 论文 §2；来源 PDF

**资源：** [一手入口](<https://ojs.aaai.org/index.php/AAAI/article/view/29946>) · [PDF](<https://arxiv.org/pdf/2305.10250>)

---

<a id="paper-associa-2025"></a>
**116. 连接直觉联想与审议召回：以图结构长期记忆增强大模型个人助理｜Bridging Intuitive Associations and Deliberate Recall: Empowering LLM Personal Assistant with Graph-Structured Long-term Memory（2025 · Findings of ACL 2025）**

**作者：** Yujie Zhang、Weikang Yuan、Zhuoren Jiang

**书目：** 年份 2025；载体 Findings of ACL 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆、外部检索与非参数记忆、评测、安全与治理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 Associa；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 用事件中心个人记忆图连接分散对话证据，并把快速关联子图检索与迭代审议召回组合起来。

**问题：** 平面稠密检索容易忽略实体关系、混合多重意图，也难以把跨会话的多条证据聚合成可回答的记忆。

**机制：** Associa 将话语、事件、实体、时间与用户组成混合图；先以带奖赏的 Steiner 树选出关联子图，再由小型审议模型发现缺失证据、改写查询并迭代补充。

**步骤：**

1. 从会话抽取用户事件、实体、时间和原始话语，建立事件中心的混合图并增量去重。
2. 按查询给节点和边赋予相关性与成本，用 Prize-Collecting Steiner Tree 取回紧凑证据子图。
3. 将初次证据交给审议模型识别缺口并生成补充查询。
4. 再次关联检索并按证据重要性排序，形成回答上下文。

**证据：**

- 表 1 的 LongMemEval-s 上，Associa 两轮检索的 Recall@5 为 0.87、NDCG@5 为 0.87；BGE 为 0.75、0.76，带用户事实和时间过滤的 LongMemEval 基线为 0.70、0.72。
- 表 2 的问答结果中，Associa 在 LongMemEval-s、LongMemEval-m、LoCoMo 上分别为 0.81、0.66、0.69；对应 LongMemEval 用户事实基线为 0.80、0.64、0.62。
- 表 3 中移除关联机制使 LongMemEval-s 的 Recall@5 从 0.87 降至 0.67；移除审议模块降至 0.84，支持两部分贡献但幅度不同。

**局限：**

- 没有显式建模时间信息，时间冲突与演化仍是缺口。
- 只在英语基准验证，未包含真实用户长期部署或多语言外部验证。
- 图构建与审议依赖模型抽取，额外成本和错误传播需要在生产系统中验证。

**意义：**

- 图记忆是否优于平面记忆取决于任务是否需要跨事件关系和多意图证据聚合。
- 快速关联加慢速审议提供了检索时计算与记忆覆盖之间可调节的双过程设计。

**边界：** 一手正式入口为 ACL Anthology；已核对图构建、关联与审议检索、主表、消融、成本及局限。所有结果限于论文报告的基准。

**引用：** Zhang et al., Findings of ACL 2025, DOI 10.18653/v1/2025.findings-acl.901。

**版本：** 采用 Findings of ACL 2025 正式版本。

**标识：** DOI 10.18653/v1/2025.findings-acl.901；稳定 ID doi:10.18653/v1/2025.findings-acl.901；工作族 ID associa-personal-memory-2025

**证据位置：**

- 第 4.1 节与图 2，正式页码 17535–17536：事件中心个人记忆图及去重。
- 第 4.2 节与图 3，正式页码 17537–17538：关联检索、Steiner 树与审议召回。
- 表 1–3，正式页码 17539–17541：检索、问答和消融。
- 局限部分，正式页码 17541：时间、多语言和外部验证边界。

**资源：** [一手入口](<https://aclanthology.org/2025.findings-acl.901/>) · [PDF](<https://aclanthology.org/2025.findings-acl.901.pdf>)

**关联 ID：** `does-memory-need-graphs-2026` · `longmemeval-2025` · `secom-2025` · `hipporag2-2025`

---

<a id="paper-knoll-2025"></a>
**117. Knoll：为大语言模型构建知识生态｜Knoll: Creating a Knowledge Ecosystem for Large Language Models（2025 · UIST 2025）**

**作者：** Dora Zhao、Diyi Yang、Michael S. Bernstein

**书目：** 年份 2025；载体 UIST 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆、外部检索与非参数记忆、评测、安全与治理；层级 跨会话长期；阅读层级 桥接；证据等级 A；简称 Knoll；优先级 medium；时间尺度 跨会话、跨平台的用户管理知识模块

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P3 / Q=Q3

**标签：** user-governed-memory、knowledge-modules、access-control、field-deployment

**定位：** 把外部长期知识做成可创建、共享、启停和审计来源的模块，并在真实产品界面中按查询检索注入。

**问题：** 用户和社区掌握的本地知识分散且不在模型训练数据中，文件上传和普通 RAG 又缺少便捷创建、协作、权限和跨平台复用。

**机制：** 用户从文档或网页创建模块，沿用宿主文档的细粒度访问控制；浏览器扩展检索并重排激活模块，把相关内容注入 ChatGPT 或 Claude 请求，并以界面标签显示所用来源。

**步骤：**

1. 用户创建、剪藏或导入知识模块，并设置私有、群组或公开访问范围
2. 在扩展中启停、刷新和共享模块，沿用文档或代码托管平台权限
3. 对最近查询做检索和重排，选出相关模块
4. 把模块内容注入商业模型提示，并在回答旁显示所用模块以便检查

**证据：**

- PDF 第5节和 Figure 6 报告两个月公开部署：203名用户、1690条至少激活一个模块的查询、39个被加入的模块。
- PDF 第6节、Table 2 中，对100条真实查询的技术评估显示，当外部知识相关时，标注者对 Knoll 回答的偏好率为81.5%±6.4。
- PDF 第3.3节明确模块可继承 Google Docs 或 GitHub 的细粒度访问控制，用户还能在扩展中启停、共享和刷新模块。
- PDF 第6.2.4节和 Table 3 记录两类失败：相关但非必要的模块会让模型过度依赖，模块缺失时模型可能显式强调没有相关知识。
- PDF 第8.3节指出私有模块仍会发送给第三方模型，且只在会话首次触发时注入会随上下文增长被模型遗忘。

**局限：**

- 部署用户多来自教育和研究场景，且17份问卷和8次访谈不足以代表一般人群。
- 所有注入模块仍发送给第三方商业模型，宿主权限不等于端到端机密性。
- 当前仅支持文本，激活模块总量和平台上下文长度限制扩展性。
- 外部知识相关不代表有用，错误模块会诱发过度依赖或偏离用户任务。

**意义：**

- 长期外部记忆需要可见来源、启停、刷新、分享和权限，而不仅是自动检索。
- 真实部署应同时报告知识命中、回答收益、无关注入和过度依赖失败。
- 访问控制必须覆盖从源文档到模型提供方的完整数据路径。

**边界：** UIST 正式 DOI、全文机制、两个月部署、访问控制和失败案例均已核验；作为 personal/retrieval 之间的 bridge 纳入。

**标识：** DOI 10.1145/3746059.3747711；稳定 ID doi:10.1145/3746059.3747711；工作族 ID knoll

**证据位置：**

- claim 生态、模块权限与交互；location PDF Figures 1–3、第3节、Table 1；来源 PDF
- claim 检索重排和来源显示；location PDF 第4节、Figure 4；来源 PDF
- claim 真实部署与技术评估；location PDF 第5–7节、Tables 2–3、Figures 5–8；来源 PDF
- claim 隐私、上下文和泛化限制；location PDF 第8.3节；来源 PDF

**资源：** [一手入口](<https://doi.org/10.1145/3746059.3747711>) · [PDF](<https://arxiv.org/pdf/2505.19335>)

**关联 ID：** `text2mem-2026` · `recallbot-2026` · `memoryos-2025`

---

<a id="paper-memoryos-2025"></a>
**118. AI 智能体的记忆操作系统｜Memory OS of AI Agent（2025 · EMNLP 2025）**

**作者：** Jiazheng Kang、Mingming Ji、Zhe Zhao、Ting Bai

**书目：** 年份 2025；载体 EMNLP 2025；状态 同行评议；来源类型 paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 MemoryOS；优先级 1；相关性排序 8

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V4 / D=D3 / P=P3 / Q=Q3

**定位：** 用短期、中期和长期个人记忆三层架构统一写入、更新、检索和生成。

**问题：** 单一记忆池会在容量、可检索性和长期个性化之间失衡。

**机制：** 短期记忆按容量滚动，中期记忆分段和分页，长期层维护压缩后的用户画像与知识；查询时跨层组合相关信息。

**步骤：**

1. 新交互进入短期队列
2. 按主题组织为中期片段
3. 将稳定信息巩固为长期个人记忆
4. 跨层检索
5. 组合记忆生成回复

**证据：**

- 最终 PDF 报告在 LoCoMo 上相对基线取得显著平均 F1 提升，并给出调用次数和 token 开销。
- 正式代码仓库公开了系统实现。

**局限：**

- 话题提取依赖所用 LLM，结果会随模型变化。
- 论文明确缺少处理重叠或持续演化主题的动态合并机制。
- LoCoMo 成绩不是实际用户长期部署证据；也没有隐私和删除审计。

**意义：**

- 代表将认知层级类比转化为工程化外部记忆管理的路线。
- 展示记忆质量与推理调用成本之间的直接权衡。

**边界：** ACL Anthology 正式页、最终 PDF 和作者代码仓库交叉核验。

**引用：** 正式网页摘要与最终 PDF 的平均 F1 提升数字存在轻微差异；候选记录以 PDF 中 49.11 为准，不在正文混用。

**标识：** DOI 10.18653/v1/2025.emnlp-main.1318

**证据位置：**

- claim 三层架构与主要结果；location PDF 摘要与 pp. 1–2；来源 PDF
- claim LoCoMo 结果、调用次数和 token 开销；location PDF Table 2、Table 3；来源 PDF
- claim 模型依赖与动态合并局限；location PDF Limitations；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2025.emnlp-main.1318/>) · [PDF](<https://aclanthology.org/2025.emnlp-main.1318.pdf>) · [代码](<https://github.com/BAI-LAB/MemoryOS>)

---

<a id="paper-rmm-2025"></a>
**119. 前瞻与回顾：长期个性化对话智能体的反思式记忆管理｜In Prospect and Retrospect: Reflective Memory Management for Long-term Personalized Dialogue Agents（2025 · ACL 2025）**

**作者：** Zhen Tan、Jun Yan、I-Hung Hsu、Rujun Han、Zifeng Wang、Long T. Le、Yiwen Song、Yanfei Chen、Hamid Palangi、George Lee、Anand Iyer、Tianlong Chen、Huan Liu、Chen-Yu Lee、Tomas Pfister

**书目：** 年份 2025；载体 ACL 2025；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆、智能体记忆管理、外部检索与非参数记忆、评测、安全与治理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 RMM；优先级 high；相关性排序 1；时间尺度 跨会话个性化对话

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** reflective-memory、write-feedback、personalization、non-graph-baseline

**定位：** 用前瞻反思写出多粒度记忆，再用回答中的记忆引用形成回顾反馈闭环，是图式长期对话记忆必须面对的非图强基线。

**问题：** 长期对话系统常按相似度写入和取回，既缺少跨会话抽象，也无法知道一次检索是否真正支持回答。

**机制：** 前瞻反思把会话整理为主题、摘要和细粒度事件；回顾反思要求生成器引用所用记忆，再把引用支持度转为检索更新信号。

**步骤：**

1. 抽取主题、会话摘要和细粒度事实形成多粒度候选
2. 与已有记忆比较后新增、合并或更新
3. 按问题检索多个粒度并生成带引用回答
4. 依据引用是否支持回答更新排序信号

**证据：**

- ACL 正式 PDF Table 1 的三次运行均值中，GTE 配置的 RMM 在 MSC 为 33.4 METEOR／57.1 BERTScore，在 LongMemEval 为 69.8 检索召回／70.4 问答准确率；对应普通 RAG-GTE 为 27.5／52.1 与 62.4／63.6。
- Tables 2–3 显示前瞻、回顾均贡献增益；引用奖励判断精确率、召回率和 F1 为 89.4、91.1、90.2。
- 这些数字只支持所测对话基准和配置，不证明跨域、真实部署或长期因果收益。

**局限：**

- 反思和引用判断增加模型调用与训练成本
- 实验以文本基准为主，没有真实跨月部署、隐私或删除验证
- 在线奖励依赖模型判断，错误可能在长期运行中累积

**意义：**

- 写入质量与使用后反馈应分开评测
- 图式系统应与同样具有抽象和反馈闭环的非图基线比较
- 引用链可支持来源审计，但不能替代人工纠错和删除入口

**建议路线：** 情景／反思记忆

**边界：** ACL 正式页和 PDF 全文核验；普通同行评议与单篇主实验不升级为 D3/P3/Q3。

**版本：** 以 ACL Anthology 正式长文版本为准。

**标识：** DOI 10.18653/v1/2025.acl-long.413；稳定 ID doi:10.18653/v1/2025.acl-long.413；工作族 ID rmm-2025

**证据位置：**

- claim 前瞻与回顾机制；location 正式 PDF §2、Figures 2–3，pp. 3–5；来源 PDF
- claim MSC、LongMemEval 与 RAG 对照；location 正式 PDF §3.2、Table 1，p. 6；来源 PDF
- claim 消融与引用判断；location 正式 PDF §§3.3–3.4、Tables 2–3，pp. 6–7；来源 PDF
- claim 局限；location 正式 PDF Limitations；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2025.acl-long.413/>) · [PDF](<https://aclanthology.org/2025.acl-long.413.pdf>)

**关联 ID：** `does-memory-need-graphs-2026` · `steem-2026` · `longmemeval-2025` · `memoryos-2025`

---

<a id="paper-secom-2025"></a>
**120. SeCom：面向个性化对话智能体的记忆构建与检索｜SeCom: On Memory Construction and Retrieval for Personalized Conversational Agents（2025 · ICLR 2025）**

**作者：** Zhuoshi Pan、Qianhui Wu、Huiqiang Jiang、Xufang Luo、Hao Cheng、Dongsheng Li、Yuqing Yang、Chin-Yew Lin、H. Vicky Zhao、Lili Qiu、Jianfeng Gao

**书目：** 年份 2025；载体 ICLR 2025；状态 同行评议；来源类型 paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 评测、安全与治理、个性化与用户长期记忆；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 SeCom；优先级 1；相关性排序 7

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**定位：** 系统比较轮次、会话和摘要三种记忆粒度，并用主题分段和压缩降低检索噪声。

**问题：** 过细记忆丢失语境，过粗记忆引入无关信息，摘要又可能抹掉可检索细节。

**机制：** 先按主题边界切分长对话，再对片段进行压缩去噪；查询时检索语义完整的片段供生成模型使用。

**步骤：**

1. 检测话题边界
2. 构建语义连续的对话片段
3. 压缩片段以减少噪声
4. 检索相关片段
5. 基于片段生成个性化回复

**证据：**

- ICLR 正式摘要报告轮级和会话级记忆均存在缺陷，主题分段与压缩同时改善检索和最终响应。
- 在 LoCoMo 与 Long-MT-Bench+ 上进行了多粒度比较。

**局限：**

- 主要依据合成或半合成长对话基准，不能替代真实用户纵向研究。
- 个性化主要体现为检索和问答，没有覆盖用户授权、删除或隐私防护。

**意义：**

- 说明记忆单元粒度是独立于检索器和生成器的关键设计变量。
- 为 LongMemEval 的索引—检索—阅读分解提供机制呼应。

**边界：** 以 ICLR Proceedings 正式页确认同行评议状态；代码仓库由作者团队维护。

**证据位置：**

- claim 粒度问题、主题分段、压缩和基准结果；location ICLR 正式页摘要；论文实验节；来源 一手入口

**资源：** [一手入口](<https://proceedings.iclr.cc/paper_files/paper/2025/hash/e56f394bbd4f0ec81393d767caa5a31b-Abstract-Conference.html>) · [PDF](<https://proceedings.iclr.cc/paper_files/paper/2025/file/e56f394bbd4f0ec81393d767caa5a31b-Paper-Conference.pdf>) · [代码](<https://github.com/microsoft/SeCom>)

---

<a id="paper-ariel-privacy-decisions-2026"></a>
**121. 通过逻辑蕴含实现智能体隐私决策个性化｜Personalizing Agent Privacy Decisions via Logical Entailment（2026 · Proceedings on Privacy Enhancing Technologies 2026(3)）**

**作者：** James Flemings、Ren Yi、Octavian Suciu、Kassem Fawaz、Murali Annavaram、Maro Gruteser

**书目：** 年份 2026；载体 Proceedings on Privacy Enhancing Technologies 2026(3)；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆、智能体记忆管理、评测、安全与治理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 ARIEL 隐私决策记忆；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 将过去的数据分享判断保存为个人持久记忆，再用本体映射和规则蕴含决定哪些新请求可自动处理、哪些必须升级给用户。

**问题：** 一般隐私规范不能表达个体差异，直接把历史判断交给 LLM 做上下文学习又可能不可靠、不可追踪并掩盖无充分前例的请求。

**机制：** ARIEL 把每个数据请求表示为五字段信息流，以个人历史生成字段本体；新请求与历史前例映射到本体层级，再由确定规则判断同一允许或拒绝结论是否被蕴含。

**步骤：**

1. 把每条历史数据分享请求及用户判断存入个人记忆，字段包括数据类型、主体、发送者、接收者和传输条件。
2. 用 LLM 为个人历史中的字段值生成有序本体，并把历史请求和新请求映射到对应层级。
3. 只对相差一个参数的候选前例应用允许与拒绝的方向性蕴含规则。
4. 若任一前例产生一致蕴含则返回可追踪决定；没有蕴含或出现冲突时交还用户。

**证据：**

- 第 3.1 节显式定义持久记忆为用户过去的数据分享请求与判断集合；图 3 展示本体生成、映射、规则和人工升级链。
- 表 1 在预处理后使用 500 名智能家居助理数据用户和 302 名教育数据用户，每人六十条历史请求与十条新请求。
- 表 3 中 ARIEL 在两个数据集、四种推理模型上均优于零样本、一般规范和上下文学习基线；对 GPT-5 教育数据，合适类 F1 从 74.4 提升至 84.8，对应论文报告的合适类 F1 误差下降 40.6%。
- 图 6 显示历史从六十条降到二十条时，ARIEL 的合适类 F1 下降 3.4 个百分点，小于上下文学习基线的 8.8 个百分点。

**局限：**

- 评测把一次性情境问卷拆成历史集与新请求集，只近似长期个人决策，没有真实随时间变化的偏好轨迹。
- 数据最初并非为人工智能智能体代用户决策而采集，代理情境可能改变真实偏好。
- 本体没有建模字段间条件依赖，历史内部也可能矛盾；论文假定请求已认证，并把对抗性请求防护交给外部组件。

**意义：**

- 隐私偏好记忆不能只作为不透明提示上下文，应该保留可追踪前例、规则、冲突和升级记录。
- 偏好演化要求定期确认、版本化和撤销机制，否则旧前例可能持续支配新决策。

**边界：** 正式元数据和全文均来自 PoPETs 2026 第三期；核对正式页码 378 至 407、第 3 至第 8 节、图 1 至图 7和表 1 至表 5。安全边界只保留高层假设，不复述可操作攻击方法。

**引用：** Flemings et al., PoPETs 2026(3), DOI 10.56553/popets-2026-0087。

**版本：** 采用 PoPETs 2026 第三期正式版本。

**标识：** DOI 10.56553/popets-2026-0087；稳定 ID doi:10.56553/popets-2026-0087；工作族 ID ariel-privacy-decisions-2026

**证据位置：**

- 第 3.1 至第 3.3 节，正式页码 380 至 381：持久记忆、五字段请求和对齐、可解释、用户能动性要求。
- 第 4.1 至第 4.2 节与图 3 至图 4、算法 1，正式页码 381 至 384：本体、映射、蕴含规则和升级流程。
- 第 5.1 至第 6.3 节、表 1 至表 5、图 5 至图 7，正式页码 384 至 388：数据、基线、主结果、历史量消融和错误分析。
- 第 7.1 至第 7.4 节，正式页码 389 至 390：冲突、偏好演化、用户介入和数据集局限。

**资源：** [一手入口](<https://petsymposium.org/popets/2026/popets-2026-0087.php>) · [PDF](<https://petsymposium.org/popets/2026/popets-2026-0087.pdf>)

**关联 ID：** `memoryos-2025` · `text2mem-2026` · `mextra-2025`

---

<a id="paper-memweaver-personal-2026"></a>
**122. MemWeaver：从文本交互行为构建个性化生成的分层记忆｜MemWeaver: A Hierarchical Memory from Textual Interactive Behaviors for Personalized Generation（2026 · The Web Conference 2026）**

**作者：** Shuo Yu、Mingyue Cheng、Daoyu Wang、Qi Liu、Zirui Liu、Ze Guo、Xiaoyu Tao

**书目：** 年份 2026；载体 The Web Conference 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 conference paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆、外部检索与非参数记忆、评测、安全与治理；层级 跨会话长期；阅读层级 核心；证据等级 B；简称 MemWeaver；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把用户历史同时编织成可查询的行为图与分段认知叙事，并用增量追加接近全量重建效果。

**问题：** 单一摘要会抹平具体交互证据，单一检索库又难以形成稳定的高层用户画像。

**机制：** 行为图保留细粒度事件和连接，认知层形成局部摘要与全局叙事；查询时联合召回。

**步骤：**

1. 把交互转为行为节点，以时间邻接和语义相似构造两类边。
2. 按时间段切分历史，生成局部认知摘要，再综合为全局用户叙事。
3. 查询感知随机游走同时考虑语义、近期性和连续性，从行为图取回证据并与认知层融合。
4. 新批次仅追加节点和边，独立生成局部摘要，并执行一次高层叙事再综合。

**证据：**

- 表 1 在六个 LaMP 任务、两个 8B 模型的 12 个指标上均排名第一且报告 p&lt;0.05；例如 Qwen 的 LaMP-1 为 0.6733／0.3367，LaMP-3 MAE／RMSE 为 0.2800／0.3733。
- 表 2 消融行为层、认知层、边或权重均退化；去除行为层时 LaMP-3 MAE 从 0.2800 上升到 0.6400。
- 图 5 与 §4.4.2 显示增量更新曲线紧随全量重建，同时耗时接近不更新方案；图中主要是曲线比较，未报告完整精确数值。

**局限：**

- 采用离线时间切分而非真实用户连续部署。
- 批次独立聚类可能引入跨批漂移，高层全局叙事仍需再次生成。
- 未处理删除、同意撤回和多用户隔离。
- 仅使用 8B 模型和 LaMP 任务，跨域外推有限。

**意义：**

- 个人记忆需要同时保存可追溯事件和可压缩的高层画像。
- 增量更新应与全量重建和不更新三者同时比较质量与成本。

**边界：** ACM 正式 DOI 与作者全文已交叉核验。

**引用：** arXiv 2510.07713 与 ACM DOI 视作同一作品族，不重复计数。

**版本：** 以 WWW 2026 DOI 元数据为书目信息，以同作品族作者全文核验证据位置。

**标识：** DOI 10.1145/3774904.3792732；稳定 ID doi:10.1145/3774904.3792732；工作族 ID memweaver-personal-2026

**证据位置：**

- §3.3，PDF 第 4–5 页：行为图、认知记忆与查询游走
- 表 1，PDF 第 5 页：主结果
- 表 2，PDF 第 6 页：消融
- §4.4.2 与图 5，PDF 第 8 页：增量更新／重建曲线

**资源：** [一手入口](<https://dl.acm.org/doi/10.1145/3774904.3792732>) · [PDF](<https://arxiv.org/pdf/2510.07713>) · [arXiv](<https://arxiv.org/abs/2510.07713>)

**关联 ID：** `agent-mem0-2025` · `memorybank-2024` · `locomo-2024` · `chronomem-2026`

---

<a id="paper-ontology-agent-memory-2026"></a>
**123. 面向对话式 RAG 的本体引导长期智能体记忆｜Ontology-Guided Long-Term Agent Memory for Conversational RAG（2026 · MLSys 2026）**

**作者：** Shuang Cao、Rui Li

**书目：** 年份 2026；载体 MLSys 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 conference paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆、外部检索与非参数记忆、评测、安全与治理、智能体记忆管理；层级 跨会话长期；阅读层级 核心；证据等级 B；简称 Ontology Memory；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 把同一轮对话同时写入可解释图、向量索引和用户档案，并以来源断言、时间权重和反馈更新协调长期个人记忆。

**问题：** 单一向量库难以同时处理显式关系、隐式偏好、时间衰减、可解释来源和低延迟查询。

**机制：** 并行图—向量—档案架构，把结构化断言、语义片段和稳定偏好置于不同生命周期，再由预算路由器协调。

**步骤：**

1. 从每轮话语并行抽取实体关系、向量片段和候选用户档案属性，并附时间戳、置信权重与来源。
2. 将新断言写入带版本的本体图；重复证据经标签到图的提升更新稳定事实，冲突先经过隐私与一致性门控。
3. 查询路由器按问题类型和预算选择图遍历、向量召回或档案读取，并合并证据。
4. 显式或隐式反馈更新权重；来源记录支持审计与回滚。

**证据：**

- 表 5：Recall@10 为 0.706、nDCG 为 0.514，高于稠密基线 0.580／0.410 与 HippoRAG 0.671／0.491；归一化成本 1.31，长上下文为 7.24；P95 延迟 185 毫秒，长上下文为 310 毫秒。
- 表 6：LoCoMo 隐式问题 F1／nDCG 为 0.576／0.451，HippoRAG 为 0.531／0.412；全体 7182 问题保持优势。
- 人工评测共 240 个回答、3 名评分者，总体得分 4.14±0.11，HippoRAG 为 3.89±0.12，稠密基线为 3.49±0.14。
- 消融中移除标签到图提升后 Recall@10 从 0.706 降到 0.621，直接支持生命周期晋升机制。

**局限：**

- 主基准由作者构建，虽含 LoCoMo 外部数据集但没有真实多租户长期部署。
- 抽取、实体对齐和标签提升错误会在图中累积；长期图增长与漂移未充分量化。
- 隐私与冲突门控有机制描述，但未提供对抗性删除证明。

**意义：**

- 长期个人记忆可将关系事实、语义片段和用户档案拆成不同保留与更新通道。
- 来源断言和可回滚版本应成为治理层的一等对象。

**边界：** 题名、作者、机制、表格和页码均由 MLSys 正式全文核验。

**引用：** 正式论文集无 DOI 字段；使用 MLSys 稳定哈希 URL。

**版本：** 以 MLSys 2026 正式论文集版本为准。

**标识：** 稳定 ID mlsys-2026-2fb4be70fc9668e9ec2c71b34fb127d4；工作族 ID ontology-agent-memory-2026

**证据位置：**

- §3.1–§3.2，PDF 第 3–4 页：并行存储、路由、来源和更新规则
- 表 5，PDF 第 9 页：召回、成本与延迟
- 表 6，PDF 第 10 页：LoCoMo 外部数据集
- §5.4，PDF 第 11 页：人工评测与消融

**资源：** [一手入口](<https://proceedings.mlsys.org/paper_files/paper/2026/hash/2fb4be70fc9668e9ec2c71b34fb127d4-Abstract-Conference.html>) · [PDF](<https://proceedings.mlsys.org/paper_files/paper/2026/file/2fb4be70fc9668e9ec2c71b34fb127d4-Paper-Conference.pdf>)

**关联 ID：** `agent-zep-2025` · `hipporag-2024` · `locomo-2024` · `chronomem-2026`

---

<a id="paper-personalalign-2026"></a>
**124. PersonalAlign：基于长期用户记录的个性化图形界面智能体分层隐式意图对齐｜PersonalAlign: Hierarchical Implicit Intent Alignment for Personalized GUI Agent with Long-Term User-Centric Records（2026 · ACL 2026）**

**作者：** Yibo Lyu、Gongwei Chen、Rui Shao、Weili Guan、Liqiang Nie

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆、智能体记忆管理、评测、安全与治理；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 PersonalAlign；优先级 high

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**定位：** 从真实长期手机记录中分离稳定偏好和条件惯例，并流式更新后用于模糊指令与前摄服务。

**问题：** 个性化图形界面智能体如何从长期操作历史推断未明说的偏好和惯例，而不是只依赖当前指令？

**机制：** AndroidIntent提供长期行为记录；HIM-Agent以流式聚合形成候选记忆，再分别用执行成功和状态条件过滤偏好与惯例。

**步骤：**

1. 从两万余条用户记录构造偏好和惯例标注
2. 流式聚合近期历史为候选长期记忆
3. 用执行结果筛除不稳定偏好，用状态条件筛除偶然惯例
4. 在模糊指令执行和前摄服务中调用相应层级

**证据：**

- Figure 4和Table 2报告AndroidIntent含775项偏好和215项惯例，来源超过两万条长期记录。
- Tables 3至6显示历史记忆同时改善模糊指令执行和前摄行为，而错误触发仍是重要风险。
- Table 7的消融表明流式聚合和两类过滤器分别贡献增益。

**局限：**

- 数据来自安卓行为记录和受控任务，不代表所有设备与文化背景。
- 前摄服务的误触发可能打扰用户，论文评测不能证明长期接受度。
- 系统从行为推断意图，仍需清晰的用户查看、纠正和删除入口。

**意义：**

- 用户记忆应区分稳定偏好和带条件的惯例，并允许分别更新。
- 长期记录驱动的前摄行为必须与控制和误触发指标一起报告。

**边界：** 正式论文页核验元数据与出版状态；公开全文核验机制、表图证据和局限。

**引用：** Lyu等，ACL 2026，DOI 10.18653/v1/2026.acl-long.1669。

**版本：** 采用正式同行评议版本族；未把预印本另计为独立工作。

**标识：** DOI 10.18653/v1/2026.acl-long.1669；稳定 ID doi:10.18653/v1/2026.acl-long.1669；工作族 ID personalalign-2026

**证据位置：**

- Figure 2与第4节，PDF第4至5页：数据采集和人工核验
- Figure 5与第5节，PDF第6至7页：HIM-Agent三模块
- Tables 3至7，PDF第7至9页：执行、前摄和消融

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.1669/>) · [PDF](<https://aclanthology.org/2026.acl-long.1669.pdf>)

**关联 ID：** `permemsafe-2026` · `memsim-2025` · `ariel-privacy-decisions-2026`

---

<a id="paper-recallbot-2026"></a>
**125. RECALLbot：为人机关系设计智能体记忆与互惠披露｜RECALLbot: Designing Agentic Memory and Reciprocal Disclosure for Human–Chatbot Relationships（2026 · CHI 2026）**

**作者：** Zhaojun Jiang、Chunyuan Zheng、Hongyi Chen、Liuqing Chen

**书目：** 年份 2026；载体 CHI 2026；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆、智能体记忆管理、评测、安全与治理；层级 跨会话长期；阅读层级 桥接；证据等级 A；简称 RECALLbot；优先级 medium；时间尺度 至少两周、每日交互的社会聊天机器人关系

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** user-control、delete-memory、longitudinal、social-chatbot

**定位：** 把机器人自身经历、共同对话与用户画像分成 Me/We Memory，并让用户查看、编辑、删除和置顶。

**问题：** 长期记忆如何既支持可信身份与互惠披露，又让用户纠正、删除和控制被使用的记忆？

**机制：** 从设定和交互生成背景、日记、会话摘要与用户画像，按相关性、使用频率、时间衰减和置顶重排，并提供卡片式控制。

**步骤：**

1. 生成 Me Memory 与 We Memory
2. 按会话结束写入摘要和画像
3. 相关性检索并结合频率、时间、置顶重排
4. 用户查看、编辑、删除或置顶后用于互惠披露

**证据：**

- N=40 的随机组间实验持续 14 天，每天要求至少对话 10 分钟
- 社会身份相关指标显著；披露频率和深度增益主要集中在早期，随后回落
- 981 次控制操作中 63 次为记忆管理；20 人均使用会话控制，13 人使用记忆控制
- 信任结果主要限于感知风险项，能力项仅边缘显著，社会连接感无显著交互

**局限：**

- 基线同时移除记忆、披露策略和相关控制界面，不能把全部效果单因归于记忆
- Me Memory 不允许编辑，四类管理操作并非对所有记忆对象都可用
- 界面删除没有验证备份、日志、缓存、权重或后端数据是否彻底清除
- 两周与 40 人样本不足以验证长期关系、文化差异或多年记忆老化

**意义：**

- 提供可见记忆管理与两周真实使用结合的设计证据
- 结果同时显示控制使用有限、指标分化和人格漂移风险，不能把记忆概括为普遍提升信任

**边界：** CHI 2026 ACM 正式全文核验；同时记录 Me/We 控制差异、不显著指标、基线多因素变化和界面删除不等于后端删除审计。

**标识：** DOI 10.1145/3772318.3790714；稳定 ID doi:10.1145/3772318.3790714

**证据位置：**

- claim Me/We memory 构建、阈值、衰减、固定和 top-3 检索；location ACM 正式全文 §4.2.1–4.2.2；来源 一手入口
- claim N=40、随机分组、14 天实验及基线；location ACM 正式全文 §5.1–5.2；来源 一手入口
- claim 社会身份、披露、控制、信任和不显著结果；location ACM 正式全文 §6.1–6.3、Tables 2–6、Figure 7；来源 一手入口
- claim 人格漂移、依赖和样本局限；location ACM 正式全文 §7.3–7.5；来源 一手入口

**资源：** [一手入口](<https://dl.acm.org/doi/10.1145/3772318.3790714>)

**关联 ID：** `memorybank-2024` · `steem-2026` · `keep-me-updated-2022`

---

<a id="paper-steem-2026"></a>
**126. 可控记忆使用：在长期人机交互中平衡历史锚定与创新｜Controllable Memory Usage: Balancing Anchoring and Innovation in Long-Term Human–Agent Interaction（2026 · ACL 2026）**

**作者：** Zisu Huang、Muzhao Tian、Xiaohua Wang、Jingwen Xu、Zhengkang Guo、Qi Qian、Kaitao Song、Jiakang Yuan、Changze Lv、Xiaoqing Zheng

**书目：** 年份 2026；载体 ACL 2026；状态 同行评议；来源类型 paper

**分类：** 主路线 个性化与用户长期记忆；相关路线 个性化与用户长期记忆、智能体记忆管理、评测、安全与治理；层级 跨会话长期；阅读层级 核心；证据等级 A；简称 SteeM；时间尺度 长期项目与跨会话历史

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D3 / P=P3 / Q=Q3

**标签：** user-control、memory-anchoring、memory-dependence、personalization、GRPO

**定位：** 把模型对历史记忆的依赖程度变成用户可指定的控制轴，并训练代理在重新开始与忠实继承之间连续调节。

**问题：** 已有代理通常全量使用或完全屏蔽历史，如何让用户按当前意图调节记忆影响，而不只是控制召回条目？

**机制：** 以 1–5 级 rubric 定义生成结果的 memory-dependence score 和用户目标偏好；自动生成偏好对齐数据，先 SFT 再以对齐误差为奖励做 GRPO，训练 SteeM 读取自然语言或标签式依赖偏好。

**步骤：**

1. 模拟研究与辅导场景的长周期项目历史并构造查询相关记忆
2. 用 rubric judge 评估输出对记忆的 1–5 级依赖并与目标偏好计算误差
3. 生成覆盖不同依赖强度的对齐样本，执行偏好感知 SFT
4. 以依赖对齐误差和质量约束执行 GRPO，使自然语言偏好可控制最终生成

**证据：**

- Table 1 与 Figure 4 显示 SteeM 在研究和辅导任务上比直接提示更贴近目标依赖等级。
- 第 5.1 节的小规模人工验证中，Qwen3-8B 的依赖顺序与目标一致率为 77%，Rubric Instruct 为 65%。
- Figure 7 的成对比较显示 SteeM 相对记忆屏蔽具有整体优势；第 5.2 节同时表明标签接口虽更易对齐，却会更明显损害通用表现。

**局限：**

- 项目历史是为受控研究而模拟的，不能直接代表真实用户的长期交互分布。
- 依赖偏好被离散成 1–5 级，而真实用户可能提出更细粒度、条件化或相互冲突的约束。
- 当前只覆盖研究与辅导两类场景，人工验证规模也有限。

**意义：**

- 用户控制不应只等同于删除或屏蔽记忆，还应覆盖生成阶段对已检索记忆的依赖强度。
- 记忆系统评测可增加目标依赖与实际依赖的偏差，检测过度锚定和使用不足。
- 控制接口本身会影响通用能力，需把自然语言、标签和硬屏蔽作为不同产品权衡。

**边界：** ACL 正式页与 PDF 全文核验；数字仅在论文的模拟场景和人工验证边界内表述。

**版本：** 以 ACL 2026 主会长文正式版为准。

**标识：** DOI 10.18653/v1/2026.acl-long.670；工作族 ID steem

**证据位置：**

- claim 依赖指标、数据流程与 SFT＋GRPO 机制；location PDF Figure 2／第 3–4 节；来源 PDF
- claim 依赖对齐主结果；location PDF Table 1／Figure 4／第 5.1 节；来源 PDF
- claim 人工验证与质量保持；location PDF 第 5.1.1–5.1.2 节／Figures 5–6；来源 PDF
- claim 接口和记忆屏蔽比较；location PDF 第 5.2–5.3 节／Figure 7；来源 PDF
- claim 适用边界；location PDF Limitations；来源 PDF

**资源：** [一手入口](<https://aclanthology.org/2026.acl-long.670/>) · [PDF](<https://aclanthology.org/2026.acl-long.670.pdf>) · [代码](<https://github.com/Moore-Tian/SteeM-Memory-Control>)

**关联 ID：** `memory-management-impact-2026` · `memoryos-2025` · `keep-me-updated-2022`

---

<a id="paper-ext-knnlm-2020"></a>
**127. 通过记忆实现泛化：最近邻语言模型｜Generalization through Memorization: Nearest Neighbor Language Models（2020 · ICLR 2020）**

**作者：** Urvashi Khandelwal、Omer Levy、Dan Jurafsky、Luke Zettlemoyer、Mike Lewis

**书目：** 年份 2020；载体 ICLR 2020；状态 同行评议；来源类型 paper

**分类：** 主路线 外部检索与非参数记忆；相关路线 外部检索与非参数记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 kNN-LM；优先级 high；相关性排序 1；时间尺度 静态训练语料；可通过更换数据存储做域迁移

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** kNN、datastore、language-modeling、static-corpus

**定位：** 把任意预训练语言模型的下一词分布与训练语料隐状态最近邻分布插值，是非参数语言模型记忆的代表起点。

**问题：** 参数模型对罕见模式和事实记忆不足，能否不再训练而直接利用训练样本表示？

**机制：** 在训练语料上建立上下文隐状态到后继词的键值存储，推理时检索近邻并与基础模型概率插值。

**步骤：**

1. 离线运行基础 LM，把每个上下文表示与真实后继词写入 datastore
2. 对当前前缀计算表示并做近似 kNN 检索
3. 按距离得到邻居词分布
4. 与基础 LM 分布插值生成下一词

**证据：**

- 摘要与 Table 1 报告 WikiText-103 困惑度降至 15.79，相对同一基础模型改善 2.9，且无需追加训练
- Table 4 显示只替换 datastore 即可做跨域适配，支持非参数部分可独立更新
- 正文将收益主要归于罕见模式和事实性上下文

**局限：**

- datastore 是静态训练语料索引，没有交互写入、冲突消解、遗忘或巩固，因此不是智能体长期记忆
- 存储和近邻搜索成本随语料规模增长；论文把压缩 datastore 留作未来工作
- 近邻命中并不保证正确使用或事实一致性

**意义：**

- 奠定后续 token／chunk 级外部非参数记忆路线
- 应在地图中作为静态检索记忆，而非可演化的情景记忆

**建议路线：** 外部检索／非参数记忆

**边界：** 元数据由 arXiv 与 ICLR 录用页核验，机制和数字由全文核验；代码为作者仓库。

**版本：** 以 ICLR 2020 录用工作为出版状态；arXiv 为稳定全文入口。

**标识：** 稳定 ID openreview:HklBjCEKvH；工作族 ID knnlm

**证据位置：**

- Figure 1：LM 与 kNN 分布插值总览
- §2：datastore 构造、检索和概率插值
- Table 1：WikiText-103 困惑度
- Table 4：通过替换 datastore 做域适配
- §8：缩减 datastore 是未来工作

**资源：** [一手入口](<https://iclr.cc/virtual_2020/poster_HklBjCEKvH.html>) · [PDF](<https://arxiv.org/pdf/1911.00172>) · [代码](<https://github.com/urvashik/knnlm>)

**关联 ID：** `a14` · `ext-realm-2020` · `ext-retro-2022`

---

<a id="paper-ext-rag-2020"></a>
**128. 面向知识密集型自然语言处理任务的检索增强生成｜Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks（2020 · NeurIPS 2020）**

**作者：** Patrick Lewis、Ethan Perez、Aleksandra Piktus、Fabio Petroni、Vladimir Karpukhin、Naman Goyal、Heinrich Küttler、Mike Lewis、Wen-tau Yih、Tim Rocktäschel、Sebastian Riedel、Douwe Kiela

**书目：** 年份 2020；载体 NeurIPS 2020；状态 同行评议；来源类型 paper

**分类：** 主路线 外部检索与非参数记忆；相关路线 外部检索与非参数记忆；层级 模型生命周期；阅读层级 桥接；证据等级 A；简称 RAG；优先级 high；相关性排序 3；时间尺度 静态 Wikipedia 索引；可人工热替换

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** RAG、dense-retrieval、knowledge-intensive、static-corpus

**定位：** 将预训练 seq2seq 生成器与稠密 Wikipedia 索引组合，确立现代文档 RAG 的标准范式。

**问题：** 如何让生成模型访问可溯源、可替换的外部知识，并在知识密集任务中减少纯参数负担？

**机制：** DPR 类检索器为输入取 top-k 文档，生成器按整段序列或逐 token 边际化文档潜变量。

**步骤：**

1. 把文档编码并建立静态稠密索引
2. 对查询检索 top-k 文档
3. 将查询与文档送入生成器
4. 按 RAG-Sequence 或 RAG-Token 边际化并生成

**证据：**

- Table 1 与 Table 2 报告开放域问答和生成任务结果
- Table 6 通过消融区分检索与参数模型的贡献
- §6 的世界领导人实验中，2016／2018 索引在匹配年份问题上分别约 70%／68%，错配索引仅约 12%／4%，证明可替换外部库能更新输出

**局限：**

- 索引本身是静态外部知识库，更新依赖人工重建或替换；没有智能体自主写入与记忆治理
- Broader Impact 明确提示检索来源偏见和错误信息会被放大
- 检索到相关文档不等于能正确读取，且不存在个体化跨会话状态

**意义：**

- 应作为记忆领域的桥接基线，而非把所有 RAG 应用计作记忆机制
- 提供可替换非参数知识与参数模型解耦的清晰对照

**建议路线：** 外部检索／非参数记忆

**边界：** 正式会议页和全文核验；将其标为桥接路线是基于缺少交互写入、更新策略和生命周期管理的审计判断。

**版本：** 以 NeurIPS 2020 正式版为准；官方代码仓库现为归档状态，不代表当前维护。

**标识：** 工作族 ID rag-lewis-2020

**证据位置：**

- Figure 1：RAG 总体结构
- §2.1–§2.5：检索、两种边际化与训练
- Table 1–2：任务结果
- Table 6：消融
- §6：热替换 Wikipedia 索引实验

**资源：** [一手入口](<https://papers.nips.cc/paper_files/paper/2020/hash/6b493230205f780e1bc26945df7481e5-Abstract.html>) · [PDF](<https://papers.nips.cc/paper_files/paper/2020/file/6b493230205f780e1bc26945df7481e5-Paper.pdf>) · [代码](<https://github.com/facebookresearch/RAG>)

**关联 ID：** `ext-realm-2020` · `ext-retro-2022` · `locomo-2024`

---

<a id="paper-ext-realm-2020"></a>
**129. REALM：检索增强语言模型预训练｜Retrieval Augmented Language Model Pre-Training（2020 · ICML 2020, PMLR 119:3929–3938）**

**作者：** Kelvin Guu、Kenton Lee、Zora Tung、Panupong Pasupat、Ming-Wei Chang

**书目：** 年份 2020；载体 ICML 2020, PMLR 119:3929–3938；状态 同行评议；来源类型 paper

**分类：** 主路线 外部检索与非参数记忆；相关路线 外部检索与非参数记忆；层级 模型生命周期；阅读层级 桥接；证据等级 A；简称 REALM；优先级 high；相关性排序 2；时间尺度 静态文档语料；训练中异步刷新索引

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** retrieval-pretraining、MIPS、open-domain-QA、static-corpus

**定位：** 把潜变量文档检索器与掩码语言模型端到端预训练，证明外部语料索引可作为知识密集任务的非参数补充。

**问题：** 如何让模型在预训练阶段学会检索并利用大规模文本知识，而不是完全压入参数？

**机制：** 以文档为潜变量，双编码器检索候选文档，阅读器条件化于输入与文档，边际化检索结果并联合优化。

**步骤：**

1. 用 MIPS 索引静态文档库
2. 检索器为输入选取候选文档
3. 阅读器在输入与文档上预测掩码词或下游答案
4. 通过边际似然联合训练检索器与阅读器并周期性刷新索引

**证据：**

- 摘要报告在三项开放域问答上取得当时最优，并带来 4–16 个绝对百分点提升
- Table 1 给出开放域问答主结果
- Table 2 表明索引陈旧会伤害性能，是动态训练检索器的直接工程反证

**局限：**

- 知识库由外部预先灌入；没有从用户交互中持续写入、合并或遗忘
- 索引刷新昂贵且训练期间存在表示陈旧问题
- 回答能力提升不能证明系统形成了跨会话个体记忆

**意义：**

- 连接端到端检索预训练与后续 RAG／RETRO
- 是重要桥接工作，但普通静态 RAG 不应自动归为真正记忆生命周期

**建议路线：** 外部检索／非参数记忆

**边界：** PMLR 会议页与全文核验；代码为 Google Research 官方仓库路径。

**版本：** 以 PMLR ICML 2020 正式版为准。

**标识：** 工作族 ID realm

**证据位置：**

- Figure 1：预训练和微调流程
- §3.1：潜变量生成过程
- §3.2：神经检索器与 MIPS
- Table 1：主问答结果
- §4.5／Table 2：陈旧索引影响

**资源：** [一手入口](<https://proceedings.mlr.press/v119/guu20a.html>) · [PDF](<https://proceedings.mlr.press/v119/guu20a/guu20a.pdf>) · [代码](<https://github.com/google-research/language/tree/master/language/realm>)

**关联 ID：** `ext-rag-2020` · `ext-retro-2022`

---

<a id="paper-ext-retro-2022"></a>
**130. 通过从万亿词元中检索改进语言模型｜Improving language models by retrieving from trillions of tokens（2022 · ICML 2022, PMLR 162:2206–2240）**

**作者：** Sebastian Borgeaud、Arthur Mensch、Jordan Hoffmann、Trevor Cai、Eliza Rutherford、Katie Millican、George Bm Van Den Driessche、Jean-Baptiste Lespiau、Bogdan Damoc、Aidan Clark、Diego De Las Casas、Aurelia Guy、Jacob Menick、Roman Ring、Tom Hennigan、Saffron Huang、Loren Maggiore、Chris Jones、Albin Cassirer、Andy Brock、Michela Paganini、Geoffrey Irving、Oriol Vinyals、Simon Osindero、Karen Simonyan、Jack Rae、Erich Elsen、Laurent Sifre

**书目：** 年份 2022；载体 ICML 2022, PMLR 162:2206–2240；状态 同行评议；来源类型 paper

**分类：** 主路线 外部检索与非参数记忆；相关路线 外部检索与非参数记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 RETRO；优先级 high；相关性排序 4；时间尺度 静态 2T-token 训练语料索引

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** chunk-retrieval、pretraining、2T-tokens、cross-attention

**定位：** 把训练文本切成块并从 2T 词元数据库检索近邻，以编码器和分块交叉注意力增强自回归 LM。

**问题：** 外部检索能否在大规模预训练中替代一部分参数扩张，并在不显著增加参数的情况下提升困惑度和知识任务？

**机制：** 上一块作为查询，从冻结 BERT 表示索引中取近邻块，编码后经 chunked cross-attention 注入下一块生成。

**步骤：**

1. 将 2T 词元语料分块、编码并建 ANN 索引
2. 以前一文本块查询近邻块
3. 浅层双向编码器编码检索块及其续文
4. 在解码器指定层做分块交叉注意力生成下一块

**证据：**

- 摘要报告 7.5B RETRO 在多项设置上接近参数规模约 25 倍的模型
- Figure 4 与 Table 4 给出语言建模结果，Figure 5 展示 retrofitting
- Figure 1 右侧显示较远或低质量检索语料可能造成负收益，构成内部反证

**局限：**

- 数据库静态且构建昂贵，不具备对个体交互的选择性写入、遗忘、冲突消解
- 性能依赖检索语料相似性和质量；论文自身展示某些 Pile 子集收益很小或为负
- 大规模语料检索带来隐私、版权和偏见复制风险

**意义：**

- 代表 chunk 级非参数训练记忆的规模化路线
- 与智能体记忆的关键差异是没有在线生命周期控制

**建议路线：** 外部检索／非参数记忆

**边界：** PMLR 正式页、PDF 全文和图表核验。

**版本：** 以 ICML 2022 PMLR 正式版为准；未找到可确认的作者官方完整训练代码，故不填 code\_url。

**标识：** 工作族 ID retro

**证据位置：**

- Figure 2：RETRO 架构
- §2.3：最近邻块检索
- §2.4：编码器和 chunked cross-attention
- Figure 4／Table 4：主结果
- §3 与风险讨论：偏见、隐私、语料复制

**资源：** [一手入口](<https://proceedings.mlr.press/v162/borgeaud22a.html>) · [PDF](<https://proceedings.mlr.press/v162/borgeaud22a/borgeaud22a.pdf>)

**关联 ID：** `a17` · `ext-instructretro-2024` · `ext-knnlm-2020`

---

<a id="paper-serac-2022"></a>
**131. 基于显式记忆的可扩展模型编辑｜Memory-Based Model Editing at Scale（2022 · ICML 2022, PMLR 162:15817–15831）**

**作者：** Eric Mitchell、Charles Lin、Antoine Bosselut、Christopher D. Manning、Chelsea Finn

**书目：** 年份 2022；载体 ICML 2022, PMLR 162:15817–15831；状态 同行评议；来源类型 paper

**分类：** 主路线 外部检索与非参数记忆；相关路线 参数记忆与知识修改、外部检索与非参数记忆；层级 模型生命周期；阅读层级 桥接；证据等级 A；简称 SERAC

**核验：** 来源层级 T1；核验状态 abstract-checked；V/D/P/Q V=V2 / D=D2 / P=P2 / Q=Q3

**定位：** 不直接改写基础模型的全局权重，而是将编辑保存在显式记忆中，再学习判断何时覆盖原预测。

**问题：** 如何在累积大量局部更新时，准确判定一条编辑应影响哪些输入？

**机制：** 用显式记忆保存编辑样例，通过作用域分类器判断当前输入是否与某条编辑相关，并在相关时调用反事实模型调制基础模型输出。

**步骤：**

1. 将新编辑保存到显式记忆
2. 检索候选编辑
3. 作用域分类器判断是否应用
4. 用反事实模型调制基础预测

**证据：**

- PMLR 正式摘要报告，SERAC 在问答、事实核查和对话生成三类编辑任务中是所测方法中唯一全部保持高表现者。

**局限：**

- 需要额外记忆、作用域分类器和反事实模型。
- 编辑是否正确应用取决于作用域判断，长期冲突、删除与回滚仍需额外治理。

**意义：**

- 展示‘知识编辑’可以通过可检索外部记忆实现，而非必须置入权重。

**边界：** 题名、作者、页码、机制与摘要结论来自 PMLR 正式页。

**证据位置：**

- claim 显式编辑记忆、作用域分类器和三类任务结果；location PMLR 正式摘要；来源 一手入口

**资源：** [一手入口](<https://proceedings.mlr.press/v162/mitchell22a.html>) · [PDF](<https://proceedings.mlr.press/v162/mitchell22a/mitchell22a.pdf>) · [项目页](<https://sites.google.com/view/serac-editing>)

---

<a id="paper-ext-instructretro-2024"></a>
**132. InstructRetro：检索增强预训练后的指令微调｜InstructRetro: Instruction Tuning post Retrieval-Augmented Pretraining（2024 · ICML 2024, PMLR 235:51255–51272）**

**作者：** Boxin Wang、Wei Ping、Lawrence McAfee、Peng Xu、Bo Li、Mohammad Shoeybi、Bryan Catanzaro

**书目：** 年份 2024；载体 ICML 2024, PMLR 235:51255–51272；状态 同行评议；来源类型 paper

**分类：** 主路线 外部检索与非参数记忆；相关路线 外部检索与非参数记忆；层级 模型生命周期；阅读层级 桥接；证据等级 A；简称 InstructRetro；优先级 medium；相关性排序 7；时间尺度 静态 1.2T-token 预训练检索库

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** RETRO、instruction-tuning、encoder-ablation、scaling

**定位：** 把 RETRO 扩展到 48B 并研究指令微调，关键反证是去掉检索编码器后解码器仍取得近似下游结果。

**问题：** 检索增强预训练能否在大模型规模与指令微调后继续带来可用收益？

**机制：** 从 1.2T 词元索引检索近邻块，对 43B GPT 继续做 100B 词元检索增强预训练，再做指令微调。

**步骤：**

1. 构建 19B 个 64-token 块的 BERT／Faiss 索引
2. 对 43B GPT 做 Retro-fitting，联合训练检索编码器和解码器
3. 进行指令微调得到 InstructRetro
4. 比较保留检索器与仅使用解码器两种推理

**证据：**

- 摘要报告仅增加 2.58% GPU 小时即可改善困惑度，并在短问答、长问答、摘要上分别平均提高约 7%、10%、16%
- 摘要和 §1 明确报告移除编码器的 InstructRetro 43B 仍取得可比结果，说明部分收益已内化进参数而非来自实时检索
- Appendix Table 11 中角色扮演与编码类别低于 GPT 对照，构成任务边界

**局限：**

- 若去掉检索编码器仍近似有效，则不能把全部下游增益解释为可访问外部记忆
- 训练和数据库规模极大，静态语料没有用户交互生命周期
- 检索库可能包含私人信息并传播偏见；部分任务分项退化

**意义：**

- 为 RETRO 谱系提供规模化证据，也提示区分‘检索在训练时塑造参数’与‘推理时真正读取外存’
- 适合作为普通检索增强与可持久记忆的桥接／反证条目

**建议路线：** 外部检索／非参数记忆

**边界：** PMLR 元数据、全文、附录和 NVIDIA 模型页核验。

**版本：** RETRO 家族后续工作；以 ICML 2024 PMLR 正式版为准。

**标识：** 工作族 ID instructretro

**证据位置：**

- Abstract：规模、成本、三类下游提升与 encoder ablation
- Figure 1：训练管线
- §3.1：1.2T 检索库和 chunked attention
- Appendix Table 8：检索速度／召回权衡
- Appendix Table 11：MT-Bench 分项负结果
- Appendix E：隐私、偏见与错误信息风险

**资源：** [一手入口](<https://proceedings.mlr.press/v235/wang24bd.html>) · [PDF](<https://proceedings.mlr.press/v235/wang24bd/wang24bd.pdf>) · [项目页](<https://huggingface.co/nvidia/retro-48b-instruct-4k>)

**关联 ID：** `ext-rag-2020` · `ext-retro-2022`

---

<a id="paper-hipporag-2024"></a>
**133. HippoRAG：受神经生物学启发的大语言模型长期记忆｜HippoRAG: Neurobiologically Inspired Long-Term Memory for Large Language Models（2024 · NeurIPS 2024）**

**作者：** Bernal Jiménez Gutiérrez、Yiheng Shu、Yu Gu、Michihiro Yasunaga、Yu Su

**书目：** 年份 2024；载体 NeurIPS 2024；状态 同行评议；出版状态 peer-reviewed；来源类型 paper

**分类：** 主路线 外部检索与非参数记忆；相关路线 外部检索与非参数记忆；层级 模型生命周期；阅读层级 核心；证据等级 A；简称 HippoRAG；优先级 high；时间尺度 模型外长期知识库，可持续加入新材料

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** graph-memory、PageRank、multi-hop、non-parametric

**定位：** 将语言模型、知识图谱与个性化 PageRank 组合成可跨文档整合的新知识长期记忆。

**问题：** 普通向量 RAG 独立编码段落，难以跨文档关联新知识，能否用图式索引实现更深且更便宜的整合？

**机制：** 从文档抽取实体关系构成知识图，以查询实体为起点运行个性化 PageRank，再把高分关联材料交给语言模型回答。

**步骤：**

1. 从新材料抽取实体与关系
2. 构建或扩展知识图
3. 以查询种子运行个性化 PageRank
4. 检索关联段落并生成答案

**证据：**

- 在所测多跳问答设置中，正式论文报告最高比对照高 20%
- 论文报告单步在线检索相对受测迭代检索便宜 10–20 倍、快 6–13 倍
- 在线效率比较不包含更慢且更贵的离线 OpenIE 与知识图构建

**局限：**

- 主要测试多跳问答，不能外推到个性化对话或自主智能体写入
- 离线 OpenIE、命名实体识别和建图会引入额外成本与结构错误
- 没有用户级删除、冲突消解和来源可信度治理

**意义：**

- 是图式非参数长期记忆的正式里程碑
- 为比较纯向量、知识图和生成式工作区提供基线

**边界：** NeurIPS 2024 正式会议页和 PDF 全文核验；在线检索效率与离线 OpenIE/建图成本分开记录；与 HippoRAG 2 按两篇独立工作处理。

**版本：** 以 NeurIPS 2024 正式版为第一代方法；ICML 2025 的 HippoRAG 2 为后继而非同一版本重复。

**标识：** DOI 10.52202/079017-1902；稳定 ID doi:10.52202/079017-1902；工作族 ID hipporag-2024

**证据位置：**

- claim LLM OpenIE 知识图与 PPR 检索机制；location 正式 PDF §2.3；来源 PDF
- claim MuSiQue、2Wiki、Hotpot 设置及检索结果；location 正式 PDF §3–4；来源 PDF
- claim 成本、速度、离线索引与抽取/图搜索局限；location 正式 PDF §5.2–5.3、§7、Appendix F/G；来源 PDF

**资源：** [一手入口](<https://proceedings.neurips.cc/paper_files/paper/2024/hash/6ddc001d07ca4f319af96a3024f6dbd1-Abstract-Conference.html>) · [PDF](<https://proceedings.neurips.cc/paper_files/paper/2024/file/6ddc001d07ca4f319af96a3024f6dbd1-Paper-Conference.pdf>)

**关联 ID：** `ext-rag-2020` · `agent-zep-2025` · `ext-realm-2020` · `hipporag2-2025`

---

<a id="paper-hipporag2-2025"></a>
**134. 从检索增强生成到记忆：大语言模型的非参数持续学习｜From RAG to Memory: Non-Parametric Continual Learning for Large Language Models（2025 · ICML 2025, PMLR 267:21497–21515）**

**作者：** Bernal Jiménez Gutiérrez、Yiheng Shu、Weijian Qi、Sizhe Zhou、Yu Su

**书目：** 年份 2025；载体 ICML 2025, PMLR 267:21497–21515；状态 同行评议；出版状态 peer-reviewed；来源类型 conference-paper

**分类：** 主路线 外部检索与非参数记忆；相关路线 外部检索与非参数记忆；层级 模型生命周期；阅读层级 桥接；证据等级 A；简称 HippoRAG 2；优先级 medium；时间尺度 持续扩展的模型外知识记忆

**核验：** 来源层级 T1；核验状态 full-text-checked；V/D/P/Q V=V3 / D=D2 / P=P2 / Q=Q2

**标签：** graph-vs-vector、continual-learning、factual-memory、associative-memory

**定位：** 先承认图式 RAG 会牺牲基本事实记忆，再用更深的段落整合和在线语言模型推理统一事实、关联与意义建构。

**问题：** 知识图增强虽改善关联和意义建构，却可能明显弱于标准 RAG 的事实记忆，如何避免这一结构化代价？

**机制：** 在第一代 PageRank 图检索上加强段落级整合，并在查询时更有效地使用语言模型，使图关联与原文证据共同参与检索。

**步骤：**

1. 把新材料整合进图和段落索引
2. 查询时执行图传播
3. 联合段落证据与语言模型在线处理
4. 生成同时利用事实和关联记忆的回答

**证据：**

- 正式摘要明确报告近期结构化 RAG 在基本事实任务上可显著落后标准 RAG
- HippoRAG 2 在事实、意义建构和关联记忆上均优于标准 RAG
- 关联记忆较受测先进嵌入模型提升 7%

**局限：**

- 识别过滤可能删除相关三元组，正文失败样本存在过滤后零三元组
- 时间和内存成本仍高于标准稠密 RAG
- 结果集中于离线知识任务，没有用户查看、编辑和删除控制

**意义：**

- 证明图式记忆并非天然优于普通向量检索，必须逐项测事实、关联和意义建构
- 应与第一代 HippoRAG 作为纠错链成对展示

**边界：** ICML 2025 / PMLR 267 正式页和 PDF 全文核验；其段落节点、识别过滤、任务与作者组均表明它是 HippoRAG 的独立后续论文，不是同一版本。

**版本：** ICML 2025 正式后继工作；与 NeurIPS 2024 第一代共同构成方法—纠错链。

**标识：** 稳定 ID pmlr:v267:gutierrez25a；工作族 ID hipporag2-2025

**证据位置：**

- claim 段落节点、查询到三元组连接、识别过滤及 PPR；location 正式 PDF §3.1–3.5；来源 一手入口
- claim 事实、意义建构和关联记忆结果；location 正式 PDF Tables 2–7、Figure 3；来源 一手入口
- claim 识别过滤失败、图搜索错误和时间/内存成本；location 正式 PDF §7、Appendix E/F；来源 一手入口

**资源：** [一手入口](<https://proceedings.mlr.press/v267/gutierrez25a.html>) · [PDF](<https://raw.githubusercontent.com/mlresearch/v267/main/assets/gutierrez25a/gutierrez25a.pdf>)

**关联 ID：** `ext-rag-2020` · `agent-zep-2025` · `ext-retro-2022` · `hipporag-2024`

---

<a id="检索与证据审计"></a>
## 检索与证据审计

<details>
<summary><strong>展开完整检索、纳排、去重、证据分级与覆盖限制</strong></summary>

### 大模型记忆技术检索与证据审计

> 截止日期：2026-08-11；当前覆盖等级：L2。完整逐查询记录见 `planning/search_ledger.jsonl`，逐候选纳排见 `planning/screening.csv`，核心主张见 `planning/claim_ledger.csv`。

#### 1. 范围与操作定义

- 研究问题：大模型记忆技术如何写入、保存、寻址、读取、更新、压缩或删除跨时间状态；不同机制的直接证据、失败边界和部署约束是什么？
- 对象：大语言模型、对话系统、检索增强系统、自主智能体，以及直接评测这些对象记忆能力的基准。
- 场景：单轮提示、长文档和长任务、跨会话对话、开放世界知识更新、持续部署与模型生命周期。
- 时间：奠基工作至 2026-08-11。
- 语言：英文一手文献为主；中文用于检索辅助和交付叙事；题名保留原文。
- 成果类型：正式同行评议论文优先；高相关预印本、官方报告、数据/基准、作者项目和代码单列。
- “全面”的操作定义：七类查询家族均执行；六条机制路线均有代表、前沿和评价/反证或明确缺口；完成引用链补漏、两轮异源边际收敛、核心深读和主张审计。
- 分类说明：前五条路线按存储或管理机制划分，“评测、安全与治理”按主要贡献类型划分；评测条目的持续时间层级指主要被测记忆的持续时间。负面证据通过辅助路线连接到被评测的正面机制，但主要路线仍只计一次。
- 不可覆盖长尾：未公开商业实现、无一手入口成果、截止日后版本、只有普通 RAG/缓存/微调应用而无记忆贡献的工作。

#### 2. 来源层级与核验原则

##### T1：结论证据

正式会议、期刊与 DOI 入口，包括 ACL Anthology、PMLR、NeurIPS Proceedings、OpenReview 明确正式状态页面、ACM Digital Library、AAAI Proceedings、USENIX、期刊官方页和 arXiv 原始记录。核心机制、比较和数字只由 T1 直接支持。

##### T2：实现与材料边界

作者或机构官方项目页、代码仓库、数据集页面与技术材料。T2 用于核验代码、数据、版本变化和复现实用信息，不单独把营销性表述升级为论文结论。

##### T3：线索发现

搜索结果、综述、聚合页与博客只用于发现术语、候选或引用链。所有最终条目必须回到 T1/T2；无法回到一手入口的候选排除。

OpenReview 特别按页面状态处理：`Published as a conference paper`、正式 poster/oral 等状态可记为同行评议；`Submitted`、`Under review`、`Withdrawn` 和 workshop 不得误标为主会正式论文。预印本与正式版属于同一版本族时，以元数据最完整的正式版作为主入口。

#### 3. 查询矩阵

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

#### 4. 检索阶段与增量轮

##### 发现轮

用术语综述、官方会刊搜索和奠基论文引用发现参数、上下文、外部、智能体、个性化和治理六条候选路线。发现轮不直接支撑结论；其作用是建立作者、基准、同义词和历史转折候选池。

##### 核验轮

逐条打开正式页面，核验题名、作者、年份、venue、DOI/稳定 ID、出版状态、摘要与可用项目材料。核心条目至少达到摘要核验；涉及定量、比较、安全、首创或失败机制的主张继续读取正文相关章节、表格或限制部分。

##### 反证轮

主动检索 `failure`、`limitation`、`side effect`、`negative result`、`benchmark`、`privacy`、`poisoning`、`forgetting`、`distribution shift` 等。已确认的关键反证包括：

- 参数知识访问受提示与数据集偏差影响：[Knowledgeable or Educated Guess?](https://aclanthology.org/2021.acl-long.146/)。
- 知识编辑可能损害通用能力或只形成表面编辑：[Model Editing Harms General Abilities](https://aclanthology.org/2024.emnlp-main.934/)、[Revealing the Deceptiveness of Knowledge Editing](https://aclanthology.org/2025.acl-long.868/)。
- 输入长度本身可在完美检索条件下造成退化：[Context Length Alone Hurts LLM Performance Despite Perfect Retrieval](https://aclanthology.org/2025.findings-emnlp.1264/)。
- 智能体会把错误经验传给后续任务：[How Memory Management Impacts LLM Agents](https://aclanthology.org/2026.acl-long.27/)。
- 外部库和长期记忆形成污染面：[AgentPoison](https://proceedings.neurips.cc/paper_files/paper/2024/hash/eb113910e9c3f6242541c1652e30dfd6-Abstract-Conference.html)、[PoisonedRAG](https://www.usenix.org/conference/usenixsecurity25/presentation/zou-poisonedrag)。
- 反记忆算法需同时检查效用、隐私和连续请求：[MUSE](https://openreview.net/forum?id=TArmA033BU)。
- PII 重构若未控制表面线索，可能把模式补全误判为记忆化：[Cue-Resistant Memorization](https://aclanthology.org/2026.acl-long.1560/)。

##### 补漏轮

按每条路线检查“奠基/代表、前沿、评价/反证、实际系统”四类位置；沿核心论文参考文献做后向追踪，沿正式会刊和 2025–2026 引用线索做前向追踪。应用过窄、对象迁移过远或只在 workshop/撤稿状态出现的候选不会为凑数量纳入。

##### 增量轮次与撤销记录

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

#### 5. 纳入、排除与边界案例

##### 纳入标准

1. 直接定义一条记忆机制、关键操作或评测范式。
2. 有可核验的正式页、DOI、arXiv 或作者官方入口。
3. 对全貌、路线比较、负面边界、真实应用或治理贡献不可替代信息。
4. 与大模型/大模型智能体及截止日期一致。

##### 排除标准与代码

- `范围外`：对象不是 LLM/LLM 智能体，且迁移距离过大。
- `词面重合`：memory 指运行内存占用、缓存效率或一般心理学概念，却不处理跨时间状态。
- `重复记录`：同一成果的预印本、会议版或重复 URL。
- `无法核验一手来源`：只有博客、聚合页或搜索摘要。
- `证据不足`：只有愿景或营销声明，没有机制与评测。
- `非目标成果类型`：普通应用或工具介绍，没有记忆研究贡献。
- `被更新版本取代`：已有正式版本，旧预印本不再作为主记录。

##### 边界案例

- 长上下文只有在直接研究状态保持、利用或失效时纳入；单纯位置编码和注意力提速排除。
- RAG 奠基和检索/存储机制创新纳入；仅把现成 RAG 用于某行业问答的应用排除。
- 知识编辑与反记忆分别作为参数记忆的写入和删除操作纳入；普通 PEFT 不自动纳入。
- 训练数据记忆化主要作为参数记忆的隐私与测量边界纳入，不把所有版权/抽取研究扩张为本地图核心。
- 智能体工作只有在经验写入、组织、检索、反思或删除构成主要贡献时纳入；只有工具调用或规划者排除。

#### 6. 去重与版本族

规范键按 DOI → arXiv/OpenReview/ACL ID → 正式论文 URL → 规范化题名与第一作者建立。预印本、正式会议、期刊扩展和项目页放入同一 `work_family_id`；默认保留正式且元数据完整的版本。若版本改变结论、实验或出版状态，则在 `version_note` 和 `source_note` 明示，不静默合并。

重点版本风险包括：MemGPT 等仍以预印本/系统项目为主；OpenReview 上撤稿或 workshop 与正式主会记录并存；LongMemEval 等项目页、arXiv 和 ICLR 正式版并存；同名基准后续清洗数据不能倒推原始论文结果。所有最终状态以截止日前可核验的一手入口为准。

#### 7. 证据与主张审计

出版状态、阅读优先级和证据等级独立记录：`peer-reviewed/preprint/official-report`；`core/bridge/background`；`A/B/C`。核心条目保留 V/D/P/Q 四轴：核验深度、支持直接性、出版状态、研究质量。

数字、比较、因果、安全、首创和“最优”主张只有在直接相关摘要、正文、表格或图注可定位时进入叙事。作者报告与本文推断分开：`evidence` 记录作者直接实验或正式页面事实；`limitations` 记录适用边界；综合报告的跨论文判断使用“本地图归纳”措辞。

截至当前冻结点，地图含 134 项，其中 98 个核心条目全部为 `full-text-checked` 且 V 为 V3/V4；`claim_ledger.csv` 含 116 条已核验主张。五个早期核心条目没有单列高风险数字或比较，其不登记理由已在 `planning/claim_ledger_gap_audit.md` 逐条说明；此后每个新增核心必须至少有一条主张审计，否则跨文件门禁失败。

#### 8. 覆盖饱和与五道闸门

候选、结构、证据、最终编译与真实浏览器门禁均已通过：

1. **范围与来源闸门：通过。** 范围合同、七类查询家族、T1/T2/T3 来源层级和排除边界已经冻结；检索词、日期、来源与失效入口保留在 JSONL 和轮次审计中。
2. **分支代表性闸门：通过。** 六条路线均有奠基、代表、前沿和评价/反证；压缩域流式记忆、个人行为—认知双层状态、语义事务恢复、工业 RAG 生命周期、序列遗忘与可验证遗忘均已有不可替代代表或明确缺口。
3. **边际收敛闸门：通过。** 最后一次高边际轮 13 之后，轮 14 为 0/72=0%，轮 15 为 2/72=2.78%；两轮查询家族异质、分母冻结、均低于 5%且无新一级路线。
4. **深读闸门：通过。** 98 个核心条目全部为正文核验，机制、证据位置、限制和 V/D/P/Q 完整。
5. **证据审计闸门：通过。** 116 条高风险主张均为 `VERIFIED`，论文 ID 与证据向量和地图一致。

最终候选冻结后，134 条记录已重新编译，严格数据验证、离线依赖约束和跨账本审计均为零错误。早期运行环境无法提供有效的 `file://` 浏览器证据，因此没有用静态检查冒充验收；2026-08-15 在具备本机 Chrome 的受控环境从最终位置复跑，134 条详情与 13 个非空聚合组在桌面、键盘、375 像素手机和 `file://` 下全部通过。按质量合同，本审计升级为 L2。

#### 9. 失败入口与覆盖限制

- 部分 OpenReview 页面触发浏览器验证，需以可访问 PDF、正式状态摘要、会刊页或作者项目交叉核验。
- 搜索引擎不能稳定返回数据库总命中量，因此全部 `hits` 和一部分历史逐查询 `screened` 为 `null`；完整轮次的唯一筛选数、纳入数和集合另保存在各轮 JSON 与纳排账本，不能从 JSONL 各行简单求和。
- 部分 2026 前沿只有 workshop、投稿或撤稿状态；这些不会被误标为主会同行评议。
- 商业产品的内部写入策略、数据保留和删除实现通常不公开，本地图不推测其机制。
- 真实跨月用户研究和端到端可验证删除证据相对稀缺；报告将此作为领域空白而非用模拟数据填补。
- 机制图均由条目文字证据重绘为解释性示意，不嵌入来源/许可不明的论文原图，也不把示意图当作实验结果。

</details>

<a id="复现与使用边界"></a>
## 复现与使用边界

- `atlas.json` 是人工维护的结构化研究真源；`data/`、网页与本 README 是确定性派生阅读层。
- 页面可离线打开；论文、代码、数据集与官方图表等一手外部入口需要联网。
- 机制步骤与网页机制图是依据一手文字证据形成的解释性整理，不替代原论文图表或独立复核。
- 出版状态、阅读优先级、证据等级与展示层级是不同维度，不能互相替代。
- 本综述有明确截止日期和纳入边界，不声称穷尽互联网中的全部长尾资料。

生成与验证工具：[`build-research-atlas`](https://github.com/Linwei-Chen/build-research-atlas)。
