# 2026-04-17 论文日报

## 一、今日趋势与创新观察

### 1. 趋势概况

- 今天一共抓到 387 篇论文，趋势概况优先基于全量抓取结果而不是只看最后保留的候选。
- 从全量论文看，主题最集中的方向是：LLM 与语言理解、Agent 与多智能体、表示学习与检索排序。
- 进入候选池的工作里，直接广告 9 篇、强迁移 8 篇、大公司线索 0 篇，说明今天既有直接商业化问题，也有可迁移的方法论输入。

### 2. 推荐系统 / 排序相关创新点

- 《RaTA-Tool: Retrieval-based Tool Selection with Multimodal Large Language Models》：亮点信号：tool；主题：LLM 与语言理解、表示学习与检索排序；候选属性：强迁移论文
- 《VisPCO: Visual Token Pruning Configuration Optimization via Budget-Aware Pareto-Frontier Learning for Vision-Language Models》：主题：LLM 与语言理解、商业化决策与资源优化；候选属性：直接广告论文
- 《Well Begun is Half Done: Training-Free and Model-Agnostic Semantically Guaranteed User Representation Initialization for Multimodal Recommendation》：亮点信号：semantic；主题：表示学习与检索排序；候选属性：强迁移论文

### 3. 全局创新点

- 今天暂时没有足够稳定的全局创新点可总结。

## 二、今日一个 AI 知识点

### 表示学习为什么是很多系统的隐形底座

表示学习的目标不是简单把输入压成一个向量，而是把真正影响任务的结构信息保留下来，同时把噪声和偶然因素压下去。后面的检索、排序、聚类、生成，很多时候都只是拿这个表示继续做计算。 很多论文表面看是在做召回、排序、生成，其实核心改进都发生在表示层。先理解表示学习，就更容易抓住论文真正的创新位置。 可以顺着一次具体运行过程来理解：你可以顺着一次前向这样理解：系统先把用户最近点击、搜索词、广告文案和商品属性分别编码，再通过共享空间把它们投到同一组向量坐标里；如果两个对象在任务上更相关，它们在这个空间里就应该更近；后续做召回时，只要比较向量距离，就能先快速找出更可能相关的一批候选。


## 三、今日论文总览

### 1. The LLM Fallacy: Misattribution in AI-Assisted Cognitive Workflows
- 挑选理由：命中广告核心词：attribution。

### 2. Simulating Human Cognition: Heartbeat-Driven Autonomous Thinking Activity Scheduling for LLM-based AI systems
- 挑选理由：命中广告核心词：rtb。

### 3. VisPCO: Visual Token Pruning Configuration Optimization via Budget-Aware Pareto-Frontier Learning for Vision-Language Models
- 挑选理由：命中广告核心词：budget。

### 4. CSRA: Controlled Spectral Residual Augmentation for Robust Sepsis Prediction
- 挑选理由：命中广告核心词：ctr。

### 5. Faithfulness Serum: Mitigating the Faithfulness Gap in Textual Explanations of LLM Decisions via Attribution Guidance
- 挑选理由：命中广告核心词：attribution。

### 6. Assessing the Performance-Efficiency Trade-off of Foundation Models in Probabilistic Electricity Price Forecasting
- 挑选理由：命中广告核心词：ctr。

### 7. Calibrate-Then-Delegate: Safety Monitoring with Risk and Budget Guarantees via Model Cascades
- 挑选理由：命中广告核心词：budget。

### 8. HAMSA: Scanning-Free Vision State Space Models via SpectralPulseNet
- 挑选理由：命中广告核心词：ctr。

### 9. Bias in Surface Electromyography Features across a Demographically Diverse Cohort
- 挑选理由：命中广告核心词：ctr。

### 10. Federated User Behavior Modeling for Privacy-Preserving LLM Recommendation
- 挑选理由：命中强迁移信号：recommendation, llm recommendation。

### 11. GenRec: A Preference-Oriented Generative Framework for Large-Scale Recommendation
- 挑选理由：命中强迁移信号：recommendation, framework。

### 12. Well Begun is Half Done: Training-Free and Model-Agnostic Semantically Guaranteed User Representation Initialization for Multimodal Recommendation
- 挑选理由：命中强迁移信号：recommendation, multimodal。

### 13. Category-based and Popularity-guided Video Game Recommendation: A Balance-oriented Framework
- 挑选理由：命中强迁移信号：recommendation, framework。

### 14. CPGRec+: A Balance-oriented Framework for Personalized Video Game Recommendations
- 挑选理由：命中强迁移信号：recommendation, framework。

### 15. A Unified Model and Document Representation for On-Device Retrieval-Augmented Generation
- 挑选理由：命中强迁移信号：retrieval, unified。

### 16. Adaptive Query Routing: A Tier-Based Framework for Hybrid Retrieval Across Financial, Legal, and Medical Documents
- 挑选理由：命中强迁移信号：retrieval, framework。

### 17. RaTA-Tool: Retrieval-based Tool Selection with Multimodal Large Language Models
- 挑选理由：命中强迁移信号：retrieval, multimodal。


## 四、补充关注

1. **Uncertainty-aware Generative Learning Path Recommendation with Cognition-Adaptive Diffusion**
   - 理由：有一定相关信号，但不足以进入正式候选：recommendation。
2. **Behavior-Aware Dual-Channel Preference Learning for Heterogeneous Sequential Recommendation**
   - 理由：有一定相关信号，但不足以进入正式候选：recommendation。
3. **NewsTorch: A PyTorch-based Toolkit for Learner-oriented News Recommendation**
   - 理由：有一定相关信号，但不足以进入正式候选：recommendation。
4. **Controlling Authority Retrieval: A Missing Retrieval Objective for Authority-Governed Knowledge**
   - 理由：有一定相关信号，但不足以进入正式候选：retrieval。
5. **FRESCO: Benchmarking and Optimizing Re-rankers for Evolving Semantic Conflict in Retrieval-Augmented Generation**
   - 理由：有一定相关信号，但不足以进入正式候选：retrieval。
6. **Towards Faster Language Model Inference Using Mixture-of-Experts Flow Matching**
   - 理由：有一定相关信号，但不足以进入正式候选：matching。
7. **SGA-MCTS: Decoupling Planning from Execution via Training-Free Atomic Experience Retrieval**
   - 理由：有一定相关信号，但不足以进入正式候选：retrieval。
8. **RACER: Retrieval-Augmented Contextual Rapid Speculative Decoding**
   - 理由：有一定相关信号，但不足以进入正式候选：retrieval。
9. **Which bird does not have wings: Negative-constrained KGQA with Schema-guided Semantic Matching and Self-directed Refinement**
   - 理由：有一定相关信号，但不足以进入正式候选：matching。
10. **Awakening Dormant Experts:Counterfactual Routing to Mitigate MoE Hallucinations**
   - 理由：有一定相关信号，但不足以进入正式候选：counterfactual。

## 五、重点论文精读

### 1. The LLM Fallacy: Misattribution in AI-Assisted Cognitive Workflows
- **背景：** The LLM Fallacy: Misattribution in AI-Assisted Cognitive Workflows 值得关注，但当前只能给保守判断。LLM 分析失败: An error occurred (ValidationException) when calling the InvokeModel operation: Access to Anthropic models is not allowed from unsupported countries, regions, or territories. Please refer to https://www.anthropic.com/supported-countries for more information on the countries and regions Anthropic currently supports.
![The LLM Fallacy: Misattribution in AI-Assisted Cognitive Workflows 论文机制总览图](assets/figures/overview/the-llm-fallacy-misattribution-in-ai-assisted-cognitive-workflows-hero.svg)
*图示：候选主图不可靠，已回退为论文核心机制总览 SVG。*

- **当前状态：** llm_failed（LLM 分析失败: An error occurred (ValidationException) when calling the InvokeModel operation: Access to Anthropic models is not allowed from unsupported countries, regions, or territories. Please refer to https://www.anthropic.com/supported-countries for more information on the countries and regions Anthropic currently supports.）
- **核心技术点：** 本次精读未成功，暂不展示结构化核心点，避免误导。
- **对广告的启发：** 暂时只保留候选判断，建议稍后重试精读。

### 2. Simulating Human Cognition: Heartbeat-Driven Autonomous Thinking Activity Scheduling for LLM-based AI systems
- **背景：** Simulating Human Cognition: Heartbeat-Driven Autonomous Thinking Activity Scheduling for LLM-based AI systems 值得关注，但当前只能给保守判断。LLM 分析失败: An error occurred (ValidationException) when calling the InvokeModel operation: Access to Anthropic models is not allowed from unsupported countries, regions, or territories. Please refer to https://www.anthropic.com/supported-countries for more information on the countries and regions Anthropic currently supports.
![Simulating Human Cognition: Heartbeat-Driven Autonomous Thinking Activity Scheduling for LLM-based AI systems 论文机制总览图](assets/figures/overview/simulating-human-cognition-heartbeat-driven-autonomous-thinking-activity-schedul-hero.svg)
*图示：候选主图不可靠，已回退为论文核心机制总览 SVG。*

- **当前状态：** llm_failed（LLM 分析失败: An error occurred (ValidationException) when calling the InvokeModel operation: Access to Anthropic models is not allowed from unsupported countries, regions, or territories. Please refer to https://www.anthropic.com/supported-countries for more information on the countries and regions Anthropic currently supports.）
- **核心技术点：** 本次精读未成功，暂不展示结构化核心点，避免误导。
- **对广告的启发：** 暂时只保留候选判断，建议稍后重试精读。

## 六、候选但未完成深读的论文

- **The LLM Fallacy: Misattribution in AI-Assisted Cognitive Workflows**
  - 状态：llm_failed
  - 原因：LLM 分析失败: An error occurred (ValidationException) when calling the InvokeModel operation: Access to Anthropic models is not allowed from unsupported countries, regions, or territories. Please refer to https://www.anthropic.com/supported-countries for more information on the countries and regions Anthropic currently supports.
- **Simulating Human Cognition: Heartbeat-Driven Autonomous Thinking Activity Scheduling for LLM-based AI systems**
  - 状态：llm_failed
  - 原因：LLM 分析失败: An error occurred (ValidationException) when calling the InvokeModel operation: Access to Anthropic models is not allowed from unsupported countries, regions, or territories. Please refer to https://www.anthropic.com/supported-countries for more information on the countries and regions Anthropic currently supports.
