# 2026-06-24 论文日报

## 一、今日趋势与创新观察

### 1. 趋势概况

- 今日cs.IR/cs.AI/cs.LG共抓取282篇，cs.AI以196篇延续主导，LLM与语言理解113篇仍是绝对中心，但表示学习与检索排序87篇里出现了多篇电商搜索/赞助商品检索工作。
- Agent与多智能体51篇延续上升，关注点从纯推理转向长轨迹故障归因、agent记忆系统、agentic电商等系统化议题。
- 迁移学习与跨域泛化42篇里出现较多LLM标注数据外迁、RAG评测跨域、领域适配工作，工程化色彩明显。
- 商业化决策与资源优化17篇集中在KV-cache压缩、token经济学、长上下文推理资源等推理侧成本议题。

### 2. 推荐系统 / 排序相关创新点

- Walmart的Scaling Dense Retrieval用LLM做结构化训练数据挖掘+渐进式课程学习，绕开点击偏置和尾部稀疏，并在A/B上拿到CTR/eCPM收益，给广告召回数据生产范式提供了完整落地样本。
- INSPIRE把意图感知显式注入赞助商品检索，针对食品杂货这种高搜索量场景做意图-商品对齐，是广告召回链路结构化的典型工程方案。
- ScaleToT将LLM的结构化推理能力压缩到亿级低活用户LTV预测，解决稀疏画像下推理不稳定+LLM推理成本不可行的双重瓶颈，LT30在线显著提升。

### 3. 全局创新点

- CompressKV用语义检索信号指导KV-cache淘汰、RoPE-Aware Bit Allocation把key-cache量化建模为按频率块的比特分配问题，长上下文推理的内存优化从启发式token打分走向结构化语义/几何先验。
- SAFARI针对超长agent轨迹提出主动调查式故障归因，不再把整条trajectory塞进上下文，而是按需检索定位错误，是agent可观测性方向的新范式。
- 微软的Reasoning as Attractor Dynamics把LLM推理重新解释为Dense Associative Memory里的能量极小化与Gibbs加权检索，为推理过程提供了一个新的物理化视角。

### 4. 跨论文综合观察

- Walmart的两篇赞助搜索工作(Scaling Dense Retrieval与INSPIRE)分别从训练数据生产和意图建模两个层面攻同一个商业问题，反映出大厂广告召回正在系统性地用LLM重做数据与意图侧。
- ScaleToT与电商多任务相关性建模都在探讨'如何把LLM能力工程化下沉到亿级在线链路'，前者走蒸馏路线、后者比较路由架构，方法论上呈现出'LLM做能力源、轻量模型做在线服务'的共性趋势。
- CompressKV、RoPE-Aware量化与SAFARI虽然分属推理优化和agent可观测，但都在回答同一个底层问题——长上下文/长轨迹下如何用结构化检索替代盲目堆context，这一思路同样可以反哺推荐排序里超长用户行为序列的建模。

## 二、今日入选论文

### 1. Scaling Dense Retrieval with LLM-Annotated Training Data: Structured Mining and Progressive Curriculum for E-Commerce Sponsored Search
- 挑选理由：Walmart赞助搜索的稠密召回训练数据生产与课程学习，含A/B测试CTR/eCPM收益，直接广告论文

### 2. INSPIRE: Intent-aware Neural Sponsored Product Retrieval for E-commerce
- 挑选理由：Walmart赞助商品检索，意图感知召回框架，直接作用于广告召回链路


## 三、补充关注

1. **Paying to Know: Micro-Transaction Markets for Verified Product Information in Agentic E-Commerce**
   - 理由：讨论agentic电商中买方agent购买信息的微支付市场，触及电商分发与定价/拍卖思路，但属于愿景论文，无具体广告链路建模

## 四、重点论文精读

### 1. Scaling Dense Retrieval with LLM-Annotated Training Data: Structured Mining and Progressive Curriculum for E-Commerce Sponsored Search
- **为什么值得看：** Walmart真实落地：用LLM标注+多路召回挖掘训练双塔，CTR/eCPM双涨
- **背景：** 广告召回是赞助搜索的第一道闸门，决定后续排序和收入。但点击训练有位置偏差、长尾query根本没点击，而人工给2.4亿query-item对打标要花上千万美元，根本没法每周重训。论文要解决的就是：在不靠点击、不靠人工的前提下，怎么把训练数据从生产系统里自动挖出来并打上高质量标签。值得看的点是它有完整线上A/B验证，是端到端可落地的工业流水线。
![Scaling Dense Retrieval with LLM-Annotated Training Data: Structured Mining and Progressive Curriculum for E-Commerce Sponsored Search 论文主图](assets/figures/overview/scaling-dense-retrieval-with-llm-annotated-training-data-structured-mining-and-p-hero.svg)
*图示：这是Walmart赞助搜索召回侧的真实生产案例，直接回答'点击信号有偏、人工标注太贵时怎么造训练数据'这一广告召回长期痛点，且离线NDCG涨5.1%、线上A/B广告花费+2.8%、CTR+1.4%、eCPM+2.8%，方法论可直接迁移到任何广告召回团队。*


**核心技术点：**

#### 技术点 1：多路召回分歧挖训练样本
- 快速理解：三路召回在Top500只有13-15%重合，把分歧本身当成五档难度信号

![多路召回分歧挖训练样本 理解图](assets/figures/tech-points/scaling-dense-retrieval-with-llm-annotated-train-point-1.svg)
*图示：核心直觉是：三个生产系统对同一个query给出的Top500只有13%左右重合，这种'打架'本身就是金矿。大家都同意的，肯定靠谱；只有词典系统找到、ANN漏掉的，正是新模型要学会修的；只有一家召回又被判错的，是真正会骗过线上系统的难例。把这种分歧按照'谁召回了、排第几、LLM判几分'三维切成五档难度。*

- 技术细节：线上同时跑词典扩展、BM25、ANN三路召回，对每个query取各自Top500并记录排名。三路都召回且LLM判为相关的是'容易正样本'；只被词典/BM25召回但ANN漏掉且相关的是'难正样本'（当前模型的失败点）；只被一路召回到Top100但被判不相关的是'难负样本'；从catalog里挑TF-IDF相似度0.2-0.6但三路都没召回的是'词面像但语义不相关'的半难负样本；剩下的是随机负样本。最后产出2.4亿条样本。
- 通俗讲解：核心直觉是：三个生产系统对同一个query给出的Top500只有13%左右重合，这种'打架'本身就是金矿。大家都同意的，肯定靠谱；只有词典系统找到、ANN漏掉的，正是新模型要学会修的；只有一家召回又被判错的，是真正会骗过线上系统的难例。把这种分歧按照'谁召回了、排第几、LLM判几分'三维切成五档难度。
- 例子：比如搜'iPhone充电器'：一根Lightning线被词典、BM25、ANN都召回到Top500且LLM判4分，就进易正样本；一根USB-C转Lightning线只被词典召回到第30名、ANN完全没召回、LLM判3分，就进难正样本（暴露ANN漏召回的弱点）；一个手机壳只被BM25召回到第80名、LLM判0分，就进难负样本——它说明BM25在这query上会犯什么错，正好拿来训练新模型避开。

#### 技术点 2：三级级联LLM打标
- 快速理解：184M交叉编码器→2B→8B LLM逐级兜底，逐类等渗校准省一半算力

![三级级联LLM打标 理解图](assets/figures/tech-points/scaling-dense-retrieval-with-llm-annotated-train-point-2.svg)
*图示：思路就是'能用小模型就别用大模型'，但难点在于怎么判断小模型这次靠不靠谱。论文发现模型对'明显相关'和'明显不相关'特别自信、对'中间档'反而犹豫，所以分类别校准置信度阈值最合理。一次打标流程：pair进来先过小cross-encoder，输出五档概率，做逐类校准后看最高类的校准置信度是否过该类阈值，过了就盖章走人，没过就升级到2B再不行升级到8B。*

- 技术细节：把每个query-item对先丢给184M的cross-encoder出5档（0=尴尬到4=优秀）相关性分及置信度，置信度过阈值就直接采用；不够自信的转给LoRA微调的2B LLM，再不行转8B LLM；三个都不确定就多数投票+最大概率兜底。关键技巧是逐类做等渗回归校准——因为模型在两端类（0和4）容易过自信、中间类（1-3）反而欠自信，每类单独校准比全局校准、Platt、温度缩放都好。最终74.5%的样本被cross-encoder就解决，整体与人工标注一致率89.1%，比直接全用8B省约一半算力。
- 通俗讲解：思路就是'能用小模型就别用大模型'，但难点在于怎么判断小模型这次靠不靠谱。论文发现模型对'明显相关'和'明显不相关'特别自信、对'中间档'反而犹豫，所以分类别校准置信度阈值最合理。一次打标流程：pair进来先过小cross-encoder，输出五档概率，做逐类校准后看最高类的校准置信度是否过该类阈值，过了就盖章走人，没过就升级到2B再不行升级到8B。
- 例子：比如('iPhone充电器', 'Lightning数据线')，cross-encoder输出4档置信度0.95，过'4档'的阈值，直接落标4，走完；而('iPhone充电器', '苹果手机壳')，cross-encoder在1和2档之间纠结输出0.55，没过'1档'阈值，升到2B LLM，2B输出1档置信度0.88过阈值，盖标1结束，省了8B的算力。

#### 技术点 3：三阶段课程训练
- 快速理解：BCE→MNR→Triplet逐级升难度，比一次混训涨9.5% NDCG

![三阶段课程训练 理解图](assets/figures/tech-points/scaling-dense-retrieval-with-llm-annotated-train-point-3.svg)
*图示：直觉就像教小孩：先认清苹果和板凳的区别，再学苹果和梨的区别，最后学红富士和嘎啦的区别。前面太早上难样本会让模型混乱，但最后又必须啃硬骨头才能精细。损失函数也要配合——粗分类用逐点的BCE，排序阶段必须用列表式的MNR，最后细粒度区分必须用pairwise的triplet把嵌入空间撑开。*

- 技术细节：训练双塔BERT分三阶段，每阶段换损失函数也换样本类型：阶段1只用易正样本（评分4）+随机负样本，用带温度的二元交叉熵学'明显相关vs明显不相关'；阶段2换成难正+难负样本，用Multiple Negatives Ranking（in-batch softmax）学'排序'，让正样本得分高过同batch里所有难负样本；阶段3换成词面相似负样本，用margin=0.3的triplet loss强行在嵌入空间拉开正负距离。每阶段从上一阶段最佳checkpoint初始化。消融显示阶段顺序很关键：以BCE收尾会把前面学到的精细结构毁掉，掉到0.843。
- 通俗讲解：直觉就像教小孩：先认清苹果和板凳的区别，再学苹果和梨的区别，最后学红富士和嘎啦的区别。前面太早上难样本会让模型混乱，但最后又必须啃硬骨头才能精细。损失函数也要配合——粗分类用逐点的BCE，排序阶段必须用列表式的MNR，最后细粒度区分必须用pairwise的triplet把嵌入空间撑开。
- 例子：想象训'MagSafe充电器'这个query的双塔：阶段1看到正样本'Apple官方MagSafe充电器'(评分4)和随机负样本'婴儿奶粉'，BCE很容易学会把第一个余弦推到接近1、第二个推到-1；阶段2换成正样本'第三方MagSafe无线充'(评分3,词典发现ANN漏的)和难负样本'普通无线充电器'(BM25误召回)，MNR强迫前者在batch内得分最高；阶段3再喂'MagSafe手机壳'这种词面极像但不是充电器的负样本，triplet loss把它和正样本在嵌入空间硬拉开0.3的margin。最终在尾部query上NDCG@10从0.830升到0.886。

- **对广告的启发：** 广告召回可直接照搬：用多路系统分歧+LLM打标替代点击训练，长尾收益最大
- **适用边界：** 方法依赖多个异构召回通道产生分歧信号，单路召回平台需要先造出第二、第三路（如加BM25或不同embedding）才能用；同时LLM cascade需要有领域微调数据，零样本GPT-4只能到68%一致率，直接拿开源大模型当judge会引入大量噪声。
- **实践建议：** 可以马上调研：把自家广告召回现有的倒排/BM25/向量三路Top-K拉出来算重合度，如果重合\<30%就具备复刻这套pipeline的条件——先用一个领域微调的小cross-encoder在分歧region上自动打标，造一批hard positive/hard negative，按BCE→MNR→Triplet三阶段重训现有双塔，长尾query上大概率能拿到明显收益。

### 2. INSPIRE: Intent-aware Neural Sponsored Product Retrieval for E-commerce
- **为什么值得看：** 用LLM蒸馏出结构化意图，直接接入Walmart赞助商品双塔召回
- **背景：** Walmart生鲜杂货是赞助搜索收入的大头，但query短且模糊，像'schar白面包'其实暗含无麸质偏好，'paella rice'要的是Bomba米而不是Arborio米。原有基于语义相似的双塔召回经常把字面相近但违反约束的商品召回上来，既影响用户体验也影响广告主ROI。论文值得看的点是：它把'意图理解'做成结构化属性（品牌、口味、饮食偏好、配料、菜系、子类目、规格等），并且讲清了从数据生产到线上服务的完整工程链路。
![INSPIRE: Intent-aware Neural Sponsored Product Retrieval for E-commerce 论文主图](assets/figures/overview/inspire-intent-aware-neural-sponsored-product-retrieval-for-e-commerce-hero.svg)
*图示：这是一篇Walmart生产环境的赞助商品召回论文，针对食品饮料类目里大量隐式意图（无糖、无麸质、特定菜系等）召不准的问题，给出了从LLM教师标注→学生模型蒸馏→双塔召回数据增强的完整链路，对广告召回侧有非常直接的参考价值。*


**核心技术点：**

#### 技术点 1：结构化意图当作显式特征
- 快速理解：把品牌、饮食偏好、菜系等八类属性显式抽出来，拼到query和商品文本里再过双塔

![结构化意图当作显式特征 理解图](assets/figures/tech-points/inspire-intent-aware-neural-sponsored-product-re-point-1.svg)
*图示：可以理解成：原来双塔只看到'gluten free bread'和两个白面包标题，分不清谁真无麸质；现在query侧多带一句'dietary preference: gluten free'，商品侧Schär那条多带'dietary preference: gluten free'、Pepperidge那条多带'dietary preference: contains gluten'，这些标签文本会进入encoder，余弦相似度自然就把对的拉近、错的推远。*

- 技术细节：论文把意图定义为一组结构化属性：brand、dietary preference、flavor、ingredient、cuisine type、product subtype、size value、size unit，既包含显式信号也包含隐式偏好（如无麸质、纯素、无乳糖）。在召回侧，他们把预测出的属性以'key: value'形式拼接在原始query文本和商品标题后面，作为bi-encoder的输入，让模型在表示层就能做'约束级匹配'，而不是只比字面或语义相似。
- 通俗讲解：可以理解成：原来双塔只看到'gluten free bread'和两个白面包标题，分不清谁真无麸质；现在query侧多带一句'dietary preference: gluten free'，商品侧Schär那条多带'dietary preference: gluten free'、Pepperidge那条多带'dietary preference: contains gluten'，这些标签文本会进入encoder，余弦相似度自然就把对的拉近、错的推远。
- 例子：论文给的对比例子很直观：query'no dairy creamer'原本Coffee Mate得分0.59、Nutpods只有0.46，召回反了；加了意图后query变成'no dairy creamer ; dietary preference: dairy free'，Nutpods（dairy free）涨到0.86，Coffee Mate（contains dairy）降到0.51，相对顺序被纠正过来。

#### 技术点 2：多教师共识+LoRA蒸馏
- 快速理解：三家大模型互相对账产出弱监督标签，再用LoRA微调一个小模型跑全量目录

![多教师共识+LoRA蒸馏 理解图](assets/figures/tech-points/inspire-intent-aware-neural-sponsored-product-re-point-2.svg)
*图示：本质是'用大模型造数据，再蒸馏到小模型上线'。先让三个大模型各写一份属性JSON，像三个标注员投票，谁都不能一家独大，避免单一模型幻觉污染训练集；然后用一个3B级小模型LoRA微调，只学'生成结构化属性'这一件事，prompt部分不参与loss，确保它不是在背prompt而是在学输出格式和知识。*

- 技术细节：意图标注用Gemma3-27B、LLaMA3.1-8B、Qwen3-8B三个教师独立输出JSON属性，然后两两比对：token级重叠阈值0.5、embedding余弦阈值0.3，至少两家一致的字段才保留为高置信标签，剩下用GPT-4.1再做一轮验证、低置信的人工抽检。这批共识标签用来对Phi-4-mini-instruct做LoRA SFT（3 epoch、lr 4e-4、最大4096 token、只对completion部分算交叉熵，prompt部分mask成-100）。最终商品侧F1的precision/recall约0.95/0.97，query侧约0.91/0.93。
- 通俗讲解：本质是'用大模型造数据，再蒸馏到小模型上线'。先让三个大模型各写一份属性JSON，像三个标注员投票，谁都不能一家独大，避免单一模型幻觉污染训练集；然后用一个3B级小模型LoRA微调，只学'生成结构化属性'这一件事，prompt部分不参与loss，确保它不是在背prompt而是在学输出格式和知识。
- 例子：比如商品'Skinnygirl Fat-Free Sugar-Free Raspberry Vinaigrette 8 fl oz'，三个教师都能抽出brand=skinnygirl、flavor=raspberry、size=8 fl oz；但dietary preference上Gemma给(sugar free, fat free)、LLaMA还多了vegan，至少两家一致的(sugar free, fat free, vegan)被保留，单边出现的丢掉，作为学生模型的训练目标。

#### 技术点 3：意图增强的双塔训练
- 快速理解：监督信号融合相关性打分和用户行为，再配MNR+cosine回归两路loss

![意图增强的双塔训练 理解图](assets/figures/tech-points/inspire-intent-aware-neural-sponsored-product-re-point-3.svg)
*图示：可以把它想成'两层老师同时教学生'：一层是相关性老师告诉你这对该多像，一层是行为老师告诉你用户实际更偏好谁，但行为老师只在相关性合格的前提下发言，防止把'相关性差但点击高'的爆款带歪召回。MNR负责把batch里其它商品当负样本推开，cosine回归负责让分值落在合理刻度上。*

- 技术细节：训练对约1000万条线上QIP。每条pair有一个0-4的相关性等级（由Gemma-1B/2B和LLaMA-3-8B级联cross-encoder打分，部分有人工标），归一化到（-1,1）；再叠加log压缩、按query归一化后的用户行为分（点击、加购、下单），但行为分只对相关性\>=2的pair生效，避免把热门但不相关的商品推上去。最终监督y(q,i)是相关性和行为分加权后clip到（0,1）。模型是MiniLM bi-encoder，loss = MultipleNegativesRanking + λ·余弦回归，让batch内负样本被推开的同时，预测相似度对齐到连续监督值。
- 通俗讲解：可以把它想成'两层老师同时教学生'：一层是相关性老师告诉你这对该多像，一层是行为老师告诉你用户实际更偏好谁，但行为老师只在相关性合格的前提下发言，防止把'相关性差但点击高'的爆款带歪召回。MNR负责把batch里其它商品当负样本推开，cosine回归负责让分值落在合理刻度上。
- 例子：对query'sugar free chocolate'，Lily's Dark Chocolate（dietary preference: sugar free）和Hershey's Milk（contains sugar）都会出现在训练batch里：相关性老师给Lily's打高分、Hershey's打低分，行为分只在Lily's这种相关样本上叠加，于是模型最终把Lily's的余弦相似从0.44拉到0.84，Hershey's从0.61降到0.45。

#### 技术点 4：离线生成+在线缓存的服务架构
- 快速理解：商品意图全量离线刷进索引，query意图走缓存命中，未命中再实时推理

![离线生成+在线缓存的服务架构 理解图](assets/figures/tech-points/inspire-intent-aware-neural-sponsored-product-re-point-4.svg)
*图示：工程上的核心权衡是：商品几亿条但变化慢，适合离线批跑；query长尾巨大但头部很集中，做缓存就够覆盖大头，剩下少量长尾再现挂模型，整体延迟和成本可控。*

- 技术细节：商品侧用vLLM跑全目录意图抽取，结果作为结构化字段写入Item Intent Store并注入商品索引；新增/更新商品每小时增量、全量每周刷新。Query侧维护一个Query Intent Cache，先用归一化query做embedding lookup（一期exact match，后续考虑ANN扩覆盖），命中直接拿缓存意图，未命中再实时推理。最终query及其意图一起送入双塔召回。
- 通俗讲解：工程上的核心权衡是：商品几亿条但变化慢，适合离线批跑；query长尾巨大但头部很集中，做缓存就够覆盖大头，剩下少量长尾再现挂模型，整体延迟和成本可控。
- 例子：用户搜'gluten free bagel'，先在query缓存里找到(dietary preference: gluten free)，拼到query文本后过双塔；商品索引里Canyon Bakehouse那条早就预先标好了gluten free，于是两边'gluten free'这个token在encoder里都能强对齐，召回结果优先返回真无麸质的bagel。

- **对广告的启发：** 广告召回可以直接照搬：用LLM抽结构化属性，离线写索引、在线拼query，提升约束类相关性
- **适用边界：** 适合属性结构化程度高、隐式偏好密集的品类（食品饮料、美妆、母婴等）；对开放式、跨域或强个性化的query，固定schema会捉襟见肘，需要叠加session和用户画像信号。
- **实践建议：** 可以先在自家广告召回里挑一两个属性密集的类目（如食品/母婴），用现成大模型批量给商品打结构化属性、蒸馏一个小模型上线，把属性拼到双塔输入文本里做A/B，几乎是低成本高确定性的相关性提升手段。
