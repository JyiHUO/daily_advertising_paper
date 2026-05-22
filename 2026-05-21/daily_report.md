# 2026-05-21 论文日报

## 一、今日趋势与创新观察

### 1. 趋势概况

- 今日全量402篇，cs.AI 237、cs.LG 157、cs.IR仅8篇，LLM与语言理解类152篇继续是当天最大主题，但cs.IR占比明显回落。
- Agent与多智能体共67篇，议题集中在长期记忆系统（MemConflict、CALMem）、深度检索任务（DeepWeb-Bench）以及工具编排闭环优化，反映从单轮Agent向带记忆和工具协作的系统化方向迁移。
- 表示学习与检索排序109篇，具体技术点落在文档重排的层级token压缩、LLM embedding压缩、法律引用检索等垂直检索基准上，工程效率与领域benchmark并重。
- 广告信号为0，推荐相关论文集中在隐藏混淆下的去偏、隐式反馈噪声鲁棒以及非平稳低秩bandit等理论侧议题，业务化推荐论文偏少。

### 2. 推荐系统 / 排序相关创新点

- Robust Personalized Recommendation under Hidden Confounding in MNAR 在传统IPW与双重鲁棒之外，针对MNAR场景下的隐藏混淆提出新的鲁棒估计框架，对广告CVR纠偏建模有同构参考价值。
- Robust Recommendation from Noisy Implicit Feedback 用GMM加权的Bayes标签转移矩阵建模隐式反馈噪声，避免简单丢弃噪声样本带来的低数据利用率问题。
- Layer-wise Token Compression for Efficient Document Reranking 在cross-encoder重排器的不同层做差异化token压缩，给长文档排序系统的推理效率提供了一个细粒度方案。

### 3. 全局创新点

- DISC 提出将语言指令与状态条件控制解耦的策略生成方式，从结构上消除任务-状态纠缠导致的观察捷径学习，对多任务条件建模是个新视角。
- DIVE 通过自限梯度更新实现LLM embedding压缩，相比Matryoshka-Adaptor等残差式方法更直接地控制压缩过程，对向量检索系统的存储计算开销优化有工程价值。
- Lean Refactor 用检索增强的agentic策略搜索做多目标、可控、版本鲁棒的证明重构，把agent方法从端到端任务求解推到了代码/证明级的可控编辑。

### 4. 跨论文综合观察

- MemConflict、CALMem 与 DeepWeb-Bench 共同指向同一个问题的不同层面：长期对话与深度研究场景下，记忆如何存储、如何在冲突时被评估、以及如何在跨源证据中被使用，反映Agent正从'能调工具'转向'能管记忆'。
- DIVE 的embedding压缩、Layer-wise Token Compression 的重排压缩与 Runtime-Certified Quantized Attention 的KV量化，方法论上呈现共性趋势——在保证可控误差的前提下做大模型推理链路各环节的工程压缩，差别只在压缩对象。
- 推荐侧的 MNAR去偏、隐式反馈噪声建模与非平稳低秩bandit 看似独立，但都在处理'观测数据不可信'这一共同前提，分别从选择偏差、标签噪声和环境漂移三个角度切入，对广告系统的数据可信度治理有互补意义。

## 二、补充关注

1. **Robust Personalized Recommendation under Hidden Confounding in MNAR**
   - 理由：推荐系统选择偏差与隐藏混淆下的去偏，与广告CVR纠偏建模有一定同构性，但偏理论且非广告专属。
2. **Robust Recommendation from Noisy Implicit Feedback: A GMM-Weighted Bayes-label Transition Matrix Framework**
   - 理由：推荐系统隐式反馈噪声去噪，与广告CTR/CVR建模有一定相关性，但偏通用方法

## 三、重点论文精读

今天没有进入重点讲解的论文。
