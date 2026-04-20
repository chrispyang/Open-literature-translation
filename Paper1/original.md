# FinWorld: An All-in-One Open-Source Platform for End-to-End Financial AI Research and Deployment

Wentao Zhang¹², Yilei Zhao¹, Chuqiao Zong¹, Xinrun Wang³, Bo An¹²  
¹Nanyang Technological University, ²Skywork AI Singapore, ³Singapore Management University  
{zhangwent963, YILEI002, ZONG0005, xrwang, boan}@ntu.edu.sg

## Abstract

Financial AI holds great promise for transforming modern finance, with the potential to support a wide range of tasks such as market forecasting, portfolio management, quantitative trading, and automated analysis. However, existing platforms remain limited in task coverage, lack robust multimodal data integration, and offer insufficient support for the training and deployment of large language models (LLMs). In response to these limitations, we present FinWorld, an all-in-one open-source platform that provides end-to-end support for the entire financial AI workflow, from data acquisition to experimentation and deployment. FinWorld distinguishes itself through native integration of heterogeneous financial data, unified support for diverse AI paradigms, and advanced agent automation, enabling seamless development and deployment. Leveraging data from 2 representative markets, 4 stock pools, and over 800 million financial data points, we conduct comprehensive experiments on 4 key financial AI tasks. These experiments systematically evaluate deep learning and reinforcement learning algorithms, with particular emphasis on RL-based finetuning for LLMs and LLM Agents. The empirical results demonstrate that FinWorld significantly enhances reproducibility, supports transparent benchmarking, and streamlines deployment, thereby providing a strong foundation for future research and real-world applications. Code is available at Github¹.

**Keywords**: Financial AI, LLMs, LLMs Agent, Reinforcement Learning, Open-source Platform

## 1 Introduction

Financial AI has revolutionized how we approach market analysis, trading strategies, and investment decisions. From traditional quantitative machine learning (ML) models [12, 21] to modern deep learning (DL) and reinforcement learning (RL) architectures [20, 27, 28, 62], the field has witnessed remarkable progress across diverse domains including time series forecasting [8, 14], algorithmic trading [27, 36], portfolio management [23, 71], and natural language analysis for financial documents [25, 30]. However, this rapid advancement has created a fragmented ecosystem where researchers and practitioners must navigate multiple specialized tools, each optimized for specific tasks but lacking seamless integration.

**Table 1: Comparison of FinWorld and related platforms.** (✓ Supported, × Not supported, Partial: Partially supported)

| Key Features               | FinWorld | TradeMaster | Qlib  | FinRL-Meta |
|----------------------------|----------|-------------|-------|------------|
| Multi-task support         | ✓        | Partial     | Partial | Partial  |
| Multi-modal data           | ✓        | ×           | ×    | ×         |
| ML, DL, RL                 | ✓        | Partial     | Partial | Partial  |
| LLMs                       | ✓        | ×           | ×    | ×         |
| LLMs Agent                 | ✓        | ×           | ×    | ×         |
| Extensibility              | ✓        | ✓           | Partial | Partial  |
| Auto presentation          | ✓        | Partial     | ×    | ×         |
| Distributed Training       | ✓        | ×           | ×    | ×         |
| Benchmarking               | ✓        | ✓           | ✓    | Partial  |

Current financial AI platforms, while valuable in their respective domains, face several critical limitations. For instance, TradeMaster [40] provides RL methods for four financial trading tasks but lacks compatibility with traditional quantitative models and financial LLMs. Qlib [60] provides robust quantitative investment tools but offers limited support for modern AI paradigms such as LLMs and RL. Similarly, FinRL-Meta [26] focuses specifically on RL for trading but suffers from a rigid framework architecture that makes it difficult to integrate new algorithms and lacks support for DL and LLM-based approaches. Most existing platforms are designed for narrow task scopes: some excel at time series forecasting but lack trading simulation, while others offer robust algorithmic trading or portfolio management yet have limited support for modern LLMs and agent paradigms. Furthermore, the integration of heterogeneous financial data ranging from structured market data to unstructured news and reports remains challenging, often requiring complex preprocessing pipelines and custom development efforts.

In summary, current financial AI platforms still face four major challenges: **i) Limited Task Coverage**: Insufficient support for emerging paradigms such as large language models and autonomous agents. **ii) Heterogeneous Data Integration**: Inadequate integration of heterogeneous data sources, including structured market data, unstructured news, and multimodal financial information. **iii) Rigid Framework Architecture**: Framework architectures that impede the seamless integration of novel algorithms and methodologies. **iv) Standardized Evaluation and Presentation**: Lack of standardized evaluation protocols and presentation frameworks for comprehensive performance assessment.

To address these challenges, we propose **FinWorld**, an all-in-one open-source platform for end-to-end financial AI research and deployment. FinWorld provides a unified framework that seamlessly integrates diverse AI paradigms, heterogeneous data sources, and modern technologies to enable comprehensive financial AI development and evaluation. As shown in Table 1, FinWorld offers comprehensive support across all key features compared to existing platforms. The key features of FinWorld include:

- **Multi-task Support**: Unified platform supporting time series forecasting, algorithmic trading, portfolio management, and LLM applications, while existing platforms focus on narrow task scopes.
- **Multimodal Data Integration**: Native support for heterogeneous financial data, including structured market data, unstructured news, and multimodal information.
- **Comprehensive AI Paradigms**: Full support for ML, DL, RL, LLMs, and LLM agents, enabling seamless integration of traditional and modern AI approaches.
- **High Extensibility**: Modular and extensible framework design enabling rapid integration of novel algorithms, with flexibility facilitating research prototyping and real-world applications.
- **Advanced Automation**: With automated presentation and reporting features, and support for distributed multi-GPU training and testing, the system enables efficient exploration in multi-environments and supports high-performance computing.

To demonstrate the effectiveness of our proposed platform, we conduct extensive experiments and provide comprehensive benchmarks. Our main contributions are threefold:

- **Unified Framework**: We propose a unified, end-to-end framework for training and evaluation of ML, DL, RL, LLMs, and LLM agents, covering four critical financial AI task types including time series forecasting, algorithmic trading, portfolio management, and LLM applications.
- **Modular Design**: The framework features a modular architecture that enables flexible construction of custom models and tasks, including the development of personalized LLM agents. The system supports efficient distributed training and testing across multiple environments.
- **Comprehensive Benchmark**: We provide support for multimodal heterogeneous data with over 800 million samples, establishing a comprehensive benchmark for the financial AI community. Extensive experiments across four task types demonstrate the framework's flexibility and effectiveness.

## 2 Related Work

### 2.1 ML, DL, and RL in Financial AI

Financial time series forecasting has evolved from traditional statistical methods like ARIMA [3] and GARCH [11] to modern DL approaches. ML methods such as LightGBM [21] and XGBoost [4] demonstrated superior performance in capturing complex financial patterns, while DL models including LSTM [19] and Transformer [48] architectures have revolutionized temporal modeling. Recent advances in financial forecasting include TimesNet [55], DLinear [66], and Timexer [29], which leverage attention mechanisms for improved prediction accuracy. In algorithmic trading, RL methods such as PPO [37] and DQN [32] have enabled adaptive trading strategies [20, 27], with recent work exploring transformer-based approaches [36]. Portfolio management has similarly progressed from classical mean-variance optimization [31] to data-driven approaches using ML [18] and RL [23, 71]. Despite these advances, existing approaches often focus on individual tasks, lacking unified frameworks for comprehensive financial AI development.

### 2.2 LLMs and LLM Agents in Financial AI

The integration of LLMs into financial decision-making has followed two main paths. The traditional approach uses pre-training and supervised fine-tuning (SFT), with models like FinBERT [2] and FLANG [38] excelling in financial text understanding. Recent progress includes FinGPT [25], BloombergGPT [57] with domain-adapted tokenization, and FinQA [5] featuring numerical reasoning. A newer direction combines pre-training with reinforcement learning (RL) to enhance reasoning, as shown by Fin-R1 [30] and Fino1 [35]. Despite their strengths, LLM-based methods often lack sequential decision-making, are computationally expensive, and struggle with non-stationary markets.

To overcome the limitations of traditional LLMs in sequential decision-making, researchers have introduced agentic mechanisms such as memory, tool use, and self-reflection. Early systems like FinMem [64] and FinRobot [74] rely on text-only LLMs enhanced with layered memory, profiling, and chain-of-thought reasoning to improve single-asset trading performance. Building on this, FinAgent [70] represents the first multimodal trading agent, which jointly processes news, structured price data, and K-line chart visuals via a tool-augmented dual-reflection architecture, achieving significant improvements in profit. Recent efforts shift toward collaborative multi-agent systems: FinCon [65] adopts a manager-analyst hierarchy with verbal reinforcement, while TradingAgents [58] models a full trading firm with specialized agents coordinating to enhance returns and risk control. Beyond these architectural innovations, recent work has also explored using RL to finetune LLMs Agents [13, 34], enabling dynamic exploration and reasoning capabilities in real-world environments.

### 2.3 Financial AI Platforms

The development of financial AI platforms has been driven by the need to provide standardized environments for research and deployment. Qlib [60] provides a quantitative investment platform that integrates traditional ML pipelines with financial data processing, offering tools for feature engineering, model training, and portfolio optimization. TradeMaster [40] offers a comprehensive RL-based trading framework with modular components for strategy development, backtesting, and evaluation across multiple financial markets. FinRL-Meta [26] focuses on reinforcement learning environments and benchmarks, providing standardized market simulators and evaluation metrics for RL-based trading strategies. These platforms have significantly contributed to the advancement of financial AI research by providing researchers and practitioners with accessible tools and standardized evaluation frameworks. However, most existing platforms primarily focus on traditional ML, DL, and RL methods, with limited support for LLMs and LLMs Agents, and lack comprehensive multimodal data integration capabilities. This motivates our proposal of FinWorld, a comprehensive framework that covers most critical financial models and tasks.

## 3 Preliminaries

In this section, we formally define the core tasks considered in this work. For completeness, we provide mathematical formulations for time series forecasting, algorithmic trading, portfolio management, and LLMs applications in the financial domain.

We first introduce the notations used throughout this paper. For single asset scenarios, \(\mathbf{x}_{1:T} = \{x_{1},\dots,x_{T}\} \in \mathbb{R}^{T\times D}\) denote the historical endogenous time series (e.g., open, high, low, close price), and for multi-asset scenarios, \(\mathbf{x}_{1:T}\in \mathbb{R}^{N\times T\times D}\) denote the historical price sequences for \(N\) assets. \(\mathbf{z}_{1:T_{\mathrm{ex}}} = \{\mathbf{z}_{1},\dots,\mathbf{z}_{T_{\mathrm{ex}}}\} \in \mathbb{R}^{T_{\mathrm{ex}}\times C}\) denote exogenous covariate series (e.g., technical indicators, financial factors, news-based sentiment scores, or macroeconomic variables), where \(D\) is the dimension of the endogenous series and \(C\) is the number of exogenous features. Other symbols will be defined as needed in subsequent sections.

### 3.1 Time Series Forecasting Task

Unlike traditional time series forecasting, which often aims to predict the future value of a single series, financial forecasting has two key distinctions. First, directly forecasting asset prices is problematic due to their non-stationary nature and sensitivity to external shocks, so financial models typically predict returns, which are more stable and meaningful for investment decisions. Second, financial forecasting usually involves predicting the future returns of multiple assets simultaneously, reflecting the real-world requirements of portfolio management and quantitative strategies.

**Definition 1 (Time Series Forecasting)**. Given the historical prices of \(N\) stocks \(\mathbf{x}_{1:T}\in \mathbb{R}^{N\times T\times D}\) and multiple exogenous variables \(\mathbf{z}_{1:T_{\mathrm{ex}}}\), the goal of time series forecasting in finance is to predict the future \(S\)-day relative returns for all assets:

\[\hat{\mathbf{R}}_{T+1:T+S} = \mathcal{F}_{\theta}(\mathbf{x}_{1:T},\mathbf{z}_{1:T_{\mathrm{ex}}}), \quad (1)\]

where \(\hat{\mathbf{R}}_{T+1:T+S}\in \mathbb{R}^{N\times S}\) denotes the predicted relative returns (e.g., \(\mathbf{R}_t = \frac{\mathbf{x}_t}{\mathbf{x}_T} - 1\)), and \(\mathcal{F}_{\theta}\) is a forecasting model parameterized by \(\theta\).

### 3.2 Algorithmic Trading Task

This task involves the design, simulation, and evaluation of systematic trading strategies across single asset, emphasizing real-time decision making and risk management.

**Definition 2 (Algorithmic Trading)**. Given historical observations \(\mathbf{x}_{1:T}\) and exogenous variables \(\mathbf{z}_{1:T_{\mathrm{ex}}}\), the algorithmic trading problem can be addressed via two mainstream approaches:

**(a) ML&DL-based Approach.** The goal is to predict future price returns or movement directions using a predictive model \(\mathcal{F}_{\theta}\):

\[\hat{y}_{T+1:T+S} = \mathcal{F}_{\theta}(\mathbf{x}_{1:T},\mathbf{z}_{1:T_{\mathrm{ex}}}), \quad (2)\]

where \(\hat{y}_{t}\) can represent the predicted return, probability of upward/downward movement, or other trading signals. The final trading actions \(a_{T+1:T+S}\) are then determined by applying a pre-defined decision rule to \(\hat{y}_{T+1:T+S}\) (e.g., buy if \(\hat{y}_{t} > \tau\), sell if \(\hat{y}_{t}< -\tau\), hold otherwise).

**(b) RL-based Approach.** The trading task is modeled as a Markov decision process (MDP). At each time step \(t\), the agent observes state \(s_{t}\) (constructed from \(\mathbf{x}_{1:t}\) and \(\mathbf{z}_{1:t}\)), chooses action \(a_{t}\in \mathcal{A}\), earns reward \(r_{t}\), and moves to the next state \(s_{t+1}\). The goal is to learn a policy \(\pi_{\theta}(a_{t}\mid s_{t})\) that maximizes expected cumulative reward:

\[\max_{\theta}\mathbb{E}_{\pi_{\theta}}\left[\sum_{t=T+1}^{T+S}r_{t}\right]. \quad (3)\]

### 3.3 Portfolio Management Task

This task focuses on the construction, optimization, and dynamic rebalancing of investment portfolios, subject to real-world operational constraints (e.g., transaction costs, position limits), and supports various objective functions and risk measures such as return maximization, volatility minimization, and share ratio optimization.

**Definition 3 (Portfolio Management)**. Given historical price sequences \(\mathbf{x}_{1:T}\in \mathbb{R}^{N\times T\times D}\) for \(N\) assets and exogenous variables \(\mathbf{z}_{1:T_{\mathrm{ex}}}\in \mathbb{R}^{N\times T_{\mathrm{ex}}\times C}\), the portfolio management problem can be approached in two mainstream ways:

**(a) ML&DL-based Approach.** The goal is to predict future asset returns or risk estimates using a predictive model \(\mathcal{F}_{\theta}\):

\[\hat{\mathbf{y}}_{T+1:T+S} = \mathcal{F}_{\theta}(\mathbf{x}_{1:T},\mathbf{z}_{1:T_{\mathrm{ex}}}), \quad (4)\]

where \(\hat{\mathbf{y}}_{t}\) may represent the predicted return, risk score, or other signals for the \(N\) assets. The allocation weights \(\mathbf{w}_{T+1:T+S}\) (where \(\sum_{i=1}^{N}w_{t,i} = 1\) and \(w_{t,i}\geq 0\) for all \(t\)) are then determined by applying an optimization procedure or rule to \(\hat{\mathbf{y}}_{T+1:T+S}\) (e.g., mean-variance optimization, risk parity, or rule-based allocation).

**(b) RL-based Approach.** This task is formulated as a sequential decision process, where at each time \(t\), the agent observes a state \(s_{t}\) (e.g., constructed from \(\mathbf{x}_{1:t}\) and \(\mathbf{z}_{1:t}\)), selects allocation weights \(\mathbf{w}_{t}\in \Delta^{N+1}\) (where \(\sum_{i=1}^{N+1}w_{t,i} = 1\) and \(w_{t,i}\geq 0\), with \(w_{t,0}\) representing cash position), receives a reward \(r_{t}\) (e.g., portfolio return or risk-adjusted reward), and transitions to the next state \(s_{t+1}\). The objective is to learn a policy \(\pi_{\theta}(\mathbf{w}_{t}\mid s_{t})\) that maximizes expected cumulative utility:

\[\max_{\theta}\mathbb{E}_{\pi_{\theta}}\left[\sum_{t=T+1}^{T+S}U(\mathbf{w}_{t},\mathbf{x}_{t})\right], \quad (5)\]

where \(U(\cdot)\) represents cumulative portfolio return.

### 3.4 LLMs Applications

Encompassing two main categories of LLM applications in finance: **i) General language understanding tasks**, including SFT and RL training for LLMs and LLM agents on financial text analysis, financial QA, and similar language comprehension tasks, and **ii) Sequential decision-making tasks**, involving RL training for LLMs or LLM agents through real-world environment interactions, such as trading in live market environments.

**Definition 4 (LLMs Applications)**. Given financial-domain inputs from multiple modalities, such as unstructured text (e.g., news, reports), structured time series (e.g., open, high, low, close price), images (e.g., K-line charts), and audio or video data (e.g., financial broadcasts), the goal is to train and deploy large language models \(\mathcal{M}_{\phi}\) for two primary application types: general language understanding and sequential decision-making. Formally,

\[\mathbf{y} = \mathcal{M}_{\phi}(\mathbf{D}_1,\mathbf{D}_2,\dots,\mathbf{D}_K), \quad (6)\]

where each \(\mathbf{D}_k\) represents an input modality, and \(\mathbf{y}\) is the task-specific output in this financial context.

Depending on the specific downstream task, LLMs can flexibly serve as predictive models within ML&D-based pipelines, or act as autonomous agents capable of RL-based decision-making and advanced tool utilization in finance.

### 3.5 Reinforcement Learning for LLMs

Group Relative Policy Optimization (GRPO) [39] is a policy optimization algorithm designed to efficiently train LLMs in both language and environment-interactive settings. In the context of financial AI, GRPO can be leveraged to endow LLMs with either general financial knowledge and reasoning ability, and specialized trading skills. For example, when applied to financial document analysis or financial question answering, GRPO enables the LLM to align with environment feedback and develop robust understanding of domain-specific content. In these tasks, the LLM is trained to produce high-quality financial responses, with group-level reward normalization providing stable learning signals for fine-tuning.

Moreover, when applied in simulated trading environments, GRPO can be used to train LLMs through direct environment interaction. The LLM receives financial observations (such as price time series, technical indicators, and news), generates actions (e.g., BUY, HOLD, SELL), and is rewarded according to realized trading returns or risk-adjusted performance. This allows the LLM to acquire real trading capabilities, strategy adaptation, and risk management skills beyond static data learning.

GRPO is applied to our LLM training through a two-stage RL paradigm, where the first stage focuses on equip the model's financial reasoning abilities using financial reasoning datasets, and the second stage immerses the model in real or simulated market environments to develop practical decision-making skills. For more details, please refer to Appendix E.2.

## 4 FinWorld

This section introduces FinWorld, a unified, modular platform designed to overcome the limits of current financial-AI frameworks. A clean, layered architecture lets researchers combine traditional ML, DL, RL, LLMs, and LLMs Agents-based methods for mainstream financial task while keeping concerns clearly separated. Integrated dataset management, standardized model APIs, and a scalable training back-end support both academic studies and real-world deployment. Architectural details appear in Appendix F.

### 4.1 Design Principles

The design of FinWorld is underpinned by foundational principles, delivering a robust, extensible, research-oriented platform for the development and evaluation of financial AI models and systems:

- **Layered and Object-Oriented Architecture.** We employ a hierarchical, layered architecture with object-oriented design principles to ensure clear separation of concerns and facilitate both flexibility and scalability across the platform.
- **Modular and Decoupled Design.** FinWorld adopts a fully modular architecture. Each component is developed as a self-contained unit with well-defined interfaces, enabling separate optimization and streamlined integration of custom components.
- **Extensibility and Paradigm Fusion.** FinWorld emphasizes extensibility at all levels, providing standardized extension points for seamless integration of novel algorithms and datasets. This architecture natively supports the fusion of diverse paradigms across financial AI research.

### 4.2 Configuration Layer

The configuration layer of FinWorld is built on mmengine [7] and provides a unified and extensible system using dictionaries. It centralizes all experimental settings, including datasets, models, training, and evaluation, in a readable and modular format. Support for configuration inheritance and overrides ensures reproducibility, flexibility, and collaborative development. In the meantime, FinWorld uses a registry mechanism to manage core components such as the dataset and the environment. Each class is registered by its type and instantiated from the configuration, allowing for flexible component management and a decoupled system architecture.

### 4.3 Dataset Layer

The dataset layer of FinWorld comprises multiple functional modules designed to enable standardized, extensible, and task-oriented data management for financial AI research. This layer abstracts the complexities of diverse data sources and modalities, offering a unified interface for data acquisition, preprocessing, and task-specific organization. Specifically, it consists of five main modules: data downloader module, data processor module, dataset module, data loader module, and environment module, each supporting flexible customization and extensibility.

- **Downloader Module.** This module consists of two main components: 1) Market Data Downloader, which standardizes access to heterogeneous financial market data sources, such as FMP [42] and Alpaca [41], supporting multiple time resolutions (e.g., daily, minute-level) and data types (e.g., OHLCV, news); and ii) LLM Reasoning Dataset Downloader, which provides access to financial reasoning datasets including FinQA, professional certification materials (e.g., ACCA, CFA), and other financial knowledge bases. The unified downloader abstraction streamlines data acquisition from various APIs and databases, ensuring consistency and scalability across research tasks.
- **Processor Module.** Building on the standardized data access, this module enables users to configure essential preprocessing steps, such as factor computation (e.g., alpha158 [60]), feature selection, and normalization, LLM reasoning data processing, and other preprocessing steps according to specific research needs. The processor abstraction facilitates reproducible and customizable data transformations for downstream modeling.
- **Dataset Module.** This module organizes processed data into task-specific formats suitable for various financial applications. For algorithmic trading tasks, it encapsulates data as individual asset datasets; for portfolio management and multi-asset problems, it constructs unified multi-asset datasets. The module seamlessly handles multiple data modalities, including numerical time series, structured financial data, and unstructured textual information, enabling comprehensive modeling capabilities for both traditional financial AI tasks and LLM-based applications.
- **Dataloader Module.** This module provides standardized dataloaders for ML, DL, or LLMs applications, such as time series forecasting. It enables efficient batching and sampling, facilitating seamless integration with other frameworks.
- **Environment Module.** Designed for reinforcement learning paradigms, this module encapsulates data as interactive environments. It supports both conventional RL agents and LLM-based agent interactions, enabling unified experimentation and promoting reproducibility across a variety of agent-based learning tasks.

### 4.4 Model Layer

The model layer of FinWorld consists of multiple specialized modules that enable unified definition, management, and invocation of diverse modeling paradigms within the platform. This layer abstracts the construction and orchestration of traditional ML models, deep neural network, and LLMs, providing standardized interfaces for consistent training, inference, and integration across multi-tasks. Specifically, it is organized into four main modules:

- **ML Models.** This module defines classical model structures in a unified declarative form. It includes linear and logistic regression, decision trees, random forests, and gradient boosting ensembles (for example, XGBoost and LightGBM). Each specification exposes a consistent input and output schema and a task head (regression, binary classification, or multiclass classification), with structural attributes such as regularization.
- **DL Models.** This module organizes neural architectures into components, layers, and model networks. Components include data embedding for financial time series [55], patch embedding [9], position encodings, attention, transformer blocks, and activation functions. Layers compose these into an embedding layer, an encoder, and a decoder with consistent interfaces. Model networks such as Autoformer, VAE, and GPT-style decoders are instantiated by wiring the layers under a common specification, independent of training and inference.
- **RL Models.** This module defines RL network structures with an actor-critic abstraction. Policy and value networks are composed from DL models components, including embedding layers and backbones such as MLP, LSTM/GRU, and Transformer. Specifications cover discrete or continuous action heads and value heads, with shared or separate encoders and optional memory. Financial constraints are represented as structural hooks for transaction costs, slippage and market impact, risk limits, and trading calendars, supporting trading and portfolio tasks.
- **LLMs Models.** This module centralizes access to both proprietary and open-source LLMs via a unified interface. It supports seamless switching among commercial providers (e.g., GPT-4.1, Claude-4-Sonnet, Gemini-2.5-Pro) and efficient local models (e.g., Qwen2.5, Qwen3), with standardized controls for context length, sampling, and cost/latency tracking. Built-in tools include function calling, tool use, and retrieval-augmented generation for financial documents. The module integrates naturally with agents and downstream models, enabling document understanding, information extraction, instruction following.

### 4.5 Training Layer

The training layer in FinWorld offers a modular scaffold that abstracts every element required to optimise all method pipelines for financial applications. A uniform interface guarantees experiment reproducibility, smooth scaling from single GPU notebooks to distributed clusters, and quick transfer of best practices across tasks such as time series forecasting, trading, portfolio management, and LLM applications.

- **Optimizer.** A rich catalogue of first order methods, ranging from classic stochastic gradient descent (SGD) to adaptive variants such as Adam and AdamW, lets practitioners match optimisation dynamics to task characteristics. A common wrapper normalises hyperparameter signatures and supports gradient centralisation, decoupled weight decay, and mixed precision updates, ensuring robust convergence on noisy and non stationary market data.
- **Loss.** A flexible factory produces objective functions that cover regression losses (mean squared error and mean absolute error), classification losses for event prediction, and reinforcement learning surrogates for policy and value objectives. Composite losses can be declared with a single line, enabling multi-task learning or the joint optimisation of risk adjusted return and prediction accuracy.
- **Scheduler.** Static strategies (e.g., step, cosine, linear) and adaptive schemes (e.g., warm-up then decay) are available for both learning rates and regularisation coefficients. Schedulers are time-aware, resuming the exact trajectory when checkpoints are reloaded.
- **Metrics.** In addition to generic accuracy and error scores, the library ships with finance specific diagnostics such as ARR, SR, and MDD. Each metric is logged at configurable intervals and can trigger early stopping or hyperparameter sweeps, enabling data-driven model selection.
- **Trainer.** Acting as the orchestrator, the trainer pipes data loading, forward and backward passes, gradient clipping, metric evaluation, checkpointing, and experiment logging (TensorBoard or WandB). Specialised variants provide task specific logic, for example the forecasting trainer, trading trainer, portfolio trainer, and large language model trainer. Clear callback hooks let researchers inject custom steps, such as on-the-fly data augmentation or bespoke risk constraints, without changing the core loop.

Together, these components form a coherent architecture that accelerates experimentation, strengthens reproducibility, and lowers the barrier to launching state-of-the-art AI solutions in the demanding environment of financial markets.

### 4.6 Evaluation Layer

The evaluation layer in FinWorld provides a comprehensive and extensible framework for assessing financial AI models and strategies. It dynamically selects and applies appropriate evaluation protocols and metrics based on the specific task and model type, supporting both established benchmarks and user-defined criteria. The framework incorporates a diverse library of financial and predictive metrics, such as ARR, MDD, SR, and MSE, which can be flexibly combined or extended as needed. In addition to quantitative assessment, the evaluation layer provides advanced visualization tools, including candlestick (K-line) charts, cumulative return plots, drawdown curves, and trade annotation overlays, to facilitate intuitive interpretation and diagnosis of model performance. This flexible and adaptive evaluation process not only streamlines model assessment for diverse financial tasks, but also facilitates systematic comparison, rapid diagnosis of strengths and weaknesses, and iterative improvement of financial AI methods within the platform. Integrated into the trainer's validation and test stages, it ensures consistency with training-phase evaluation and supports systematic comparison, diagnosis, and iterative improvement.

### 4.7 Task Layer

The task layer is responsible for the systematic definition, abstraction, and encapsulation of financial AI task types. It systematically supports a wide spectrum of financial AI tasks, providing unified abstractions and modular interfaces that facilitate integration with upstream data modules and downstream modeling components. This layer centers on several core financial tasks, including time series forecasting, algorithmic trading, portfolio management, and LLMs applications, which are formally defined in Section 3.

Each task is specified by configurable input/output schemas and standardized evaluation protocols, supporting both established benchmarks and user-customized scenarios. The unified task architecture enables rapid prototyping, cross-task generalization, and reproducible research across financial AI applications. Additionally, the layer provides a flexible framework for deploying LLM-based agents in asynchronous, single-agent, or multi-agent configurations, allowing users to compose and customize financial agents for diverse research and application needs.

### 4.8 Presentation Layer

The presentation layer in FinWorld is designed for automated dissemination and documentation of experimental results. Central to this layer is a dedicated presentation agent that orchestrates the aggregation of evaluation outputs, automatic generation of technical reports, and the creation of interactive web pages for result interpretation and dissemination. Experimental findings, along with key visualizations and benchmark summaries, are systematically compiled into structured documents and published to collaborative platforms such as GitHub, ensuring transparent sharing and long-term accessibility. Furthermore, seamless integration with experiment tracking tools like Wandb enables real-time visualization and comparison of metrics throughout the research lifecycle. This automated, multi-channel presentation workflow enhances reproducibility, supports peer review, and amplifies the visibility and impact of financial AI research conducted within the platform.

## 5 Empirical Evaluation

### 5.1 Dataset

We utilize two representative markets, the US and China, covering four major stock pools: DJ30, SP500, SSE50, and HS300. The dataset spans from 1995-05-01 to 2025-05-01, comprising daily and minutely level price as well as news, totaling over 800 million data points. This extensive dataset supports tasks such as time series forecasting, algorithmic trading, and portfolio management. Additionally, we collect a comprehensive LLM Reasoning dataset in both Chinese and English, covering diverse financial scenarios for training LLMs and agents, with over 80,000 samples across multiple reasoning benchmarks. All experiments are conducted on 2 NVIDIA H100 GPUs. Unless otherwise specified, all reported results are averaged over three runs with different random seeds, and the best results in each column are underlined. Detailed information can be found in Appendix A.

### 5.2 Time Series Forecasting

**Dataset Setup.** We use daily OHLCV and Alpha158 features for DJ30, SP500, SSE50, and HS300 from 2015-05-01 to 2025-05-01, with per-stock normalization and data split at 2023-05-01 for training and validation. Detailed information can be found in Appendix D.1.

**Metrics.** We evaluate forecasting performance using four metrics: MAE, MSE, RankIC, and RankICIR. MAE and MSE focus on absolute prediction accuracy, while RankIC and RankICIR are particularly relevant for evaluating the model's effectiveness in capturing return-based or ranking-based financial relationships.

**Methods.** We evaluate several ML-based and DL-based time series forecasting models, including: i) ML-based methods: LightGBM [60], XGBoost [60]; ii) DL-based methods: Autoformer [56], Crossformer [72], ETSformer [54], DLinear [67], TimesNet [55], PatchTST [33], TimeMixer [50], and TimeXer [51]. These models are selected based on their effectiveness in capturing complex patterns and their ability to handle high-dimensional data.

**Experiment Results.** As shown in the results Table 2, on the DJ30 dataset, the TimeXer model achieves a MAE of 0.0529 and an MSE of 0.0062, significantly lower than LightGBM (MAE 0.1392, MSE 0.0235). TimeXer also attains the highest RankICIR of 0.4889, compared to 0.2017 for LightGBM. Similarly, on the HS300 dataset, models such as TimeMixer and TimeXer outperform ML methods, with MAEs of 0.3804 and 0.3727, and MSEs of 0.0442 and 0.0529, respectively. Overall, deep learning models achieve lower prediction errors and higher rank-based metrics than machine learning methods, highlighting the advantages of DL approaches for financial time series forecasting.

**Table 2: Comparison of models for time series forecasting.** (Data table omitted for brevity, see original PDF)

### 5.3 Algorithmic Trading

**Dataset Setup.** We focus on daily-level financial data for trading. We select AAPL, AMZN, GOOGL, META, MSFT, and TSLA from the U.S. market, covering both daily OHLCV data with Alpha158 technical indicators for the period from 1995-05-01 to 2025-05-01, sourced from FMP. We employ a StandardScaler for feature normalization and utilize both dense features (145 dimensions from Alpha158 indicators) and sparse temporal features (4 dimensions: days, months, weekdays, years). The data is split at 2023-05-01 for training and validation separation.

**Metrics.** We evaluate algorithmic trading performance using six financial metrics: i) Annual Rate of Return (ARR); ii) Sharpe Ratio (SR); iii) Maximum Drawdown (MDD); iv) Calmar Ratio (CR); v) Sortino Ratio (SoR); vi) Volatility (VOL).

**Methods.** We systematically evaluate several Rule-based, ML-based, DL-based, and RL-based methods, including i) Rule-based methods: BUY&HOLD, MACD; ii) ML-based Methods: LightGBM, XGBoost; iii) DL-based Methods: Transformer [48], LSTM, DLinear [67]; iv) RL-based Methods: PPO [37], SAC [16].

**Experiment Results.** Overall, RL-based methods achieve superior risk-adjusted returns compared to rule-based and ML-based approaches. For instance, SAC yields an ARR of 44.45% and SR of 1.6389 on AMZN, outperforming Buy&Hold (ARR 43.98%, SR 1.4190) and XGBoost (ARR 20.86%, SR 0.7175). PPO also demonstrates strong performance across most assets. In contrast, DL-based methods show moderate performance, with Transformer achieving competitive results on AAPL (ARR 23.35%, SR 0.8788). Rule-based methods like MACD exhibit negative returns on AMZN, highlighting the challenges of static trading rules in dynamic markets.

**Table 3: Comparison of models for algorithmic trading.** (Data table omitted for brevity, see original PDF)

### 5.4 Portfolio Management

**Dataset Setup.** Dataset setup is the same as in the time series forecasting task. Detailed information can be found in Appendix D.3.

**Metrics.** The evaluation metrics for portfolio management are consistent with those used in the algorithmic trading task.

**Methods.** We evaluate ML-based, DL-based, and RL-based methods. Since we can apply the Top-\(k\) Dropout Strategy [60] after any ML&DL-based forecasting model to construct a portfolio, our ML&DL-based methods include all ML&DL-based forecasting models. Specifically, we consider: i) Rule-based methods: BUY&HOLD; ii) ML-based Methods: LightGBM, XGBoost; iii) DL-based Methods: Autoformer [56], Crossformer [72], ETSformer [54], DLinear [67], TimesNet [55], PatchTST [33], TimeMixer [50], and TimeXer [51]; iv) RL-based Methods: PPO [37], SAC [16].

**Experiment Results.** Overall, the RL-based methods, especially SAC, achieve the best results on all benchmarks, with annualized returns up to 31.2% (SP500) and Sharpe ratios above 1.5. In contrast, rule-based and ML-based methods show much lower returns (e.g., Buy&Hold: 9.4% on DJ30), and DL-based methods generally perform between the two. Notably, SAC consistently delivers higher returns and better risk-adjusted metrics, demonstrating clear advantages for portfolio management.

**Table 4: Comparison of methods for portfolio management.** (Data table omitted for brevity, see original PDF)

### 5.5 LLMs Applications

**Dataset Setup.** For comprehensive evaluation, we assess LLMs and LLM Agents on a suite of established financial reasoning benchmarks, including FinQA, FinEval, ConvFinQA, and CFLUE, which cover a broad range of business scenarios. For trading evaluation, we follow the same experimental setup as in algorithmic trading task and use daily-level OHLCV data together with news for the 6 U.S. technology stocks (AAPL, AMZN, GOOGL, META, MSFT, and TSLA) over the period from 2015-05-01 to 2025-05-01.

**Metrics.** For financial reasoning task, we evaluate performance using Score. For trading abilities, we use the same six financial metrics as in the above trading task.

**Methods.** We evaluate several open-source LLMs (DeepSeek-R1 [15], Qwen3-8B [59], Fin-R1-7B [15], Qwen2.5-7B-Instruct [44]) and our FinReasoner. For trading, we use a simplified FinAgent-based AI Agent framework developed in this work, with the above LLMs as backbones.

**Experiment Results.** As shown in Figure 2, FinReasoner achieves superior performance on financial reasoning benchmarks, outperforming open-source baselines. For trading (Figure 3), RL-trained LLM agents (e.g., FinReasoner) demonstrate improved risk-adjusted returns compared to non-RL baselines, though performance remains sensitive to market conditions.

**Figure 2: Comparison of LLMs on financial reasoning.**  
**Figure 3: Comparison of LLMs on trading.**

## 6 Conclusion

In this paper, we introduce FinWorld, a comprehensive framework for financial AI research and development. We provide a detailed description of the framework, including the data, model, training, evaluation, and task layers. We also provide empirical evaluations on four financial AI tasks: time series forecasting, algorithmic trading, portfolio management, and LLMs applications. The results show that FinWorld can effectively support financial AI research and development. We will continue to optimize the framework in the future to provide an even better user experience.

## References

[1] M. Ahn et al. 2022. Do as I Can, not as I Say. arXiv:2204.01691.  
[2] D. Araci. 2019. FinBERT. arXiv:1908.10063.  
[3] G. E. P. Box et al. 1976. *Time Series Analysis*.  
[4] T. Chen and C. Guestrin. 2016. XGBoost. KDD.  
[5] Z. Chen et al. 2021. FinQA. EMNLP.  
[6] Z. Chen et al. 2022. ConvFinqa. arXiv:2210.03849.  
[7] MMEngine Contributors. 2022. MMEngine. GitHub.  
[8] Q. Ding et al. 2020. Hierarchical Multi-scale Gaussian Transformer. IJCAI.  
[9] A. Dosovitskiy et al. 2020. An Image is Worth 16x16 Words. arXiv:2010.11929.  
[10] Y. Duan et al. 2022. FactorVAE. AAAI.  
[11] R. F. Engle. 1982. Autoregressive Conditional Heteroskedasticity. *Econometrica*.  
[12] E. F. Fama and K. R. French. 2015. A Five-factor Asset Pricing Model. *JFE*.  
[13] L. Feng et al. 2025. Group-in-group Policy Optimization. arXiv:2505.10978.  
[14] S. Feng et al. 2023. Multi-scale Attention Flow. *TKDE*.  
[15] D. Guo et al. 2025. Deepseek-r1. arXiv:2501.12948.  
[16] T. Haarnoja et al. 2018. Soft Actor-Critic. arXiv:1812.05905.  
[17] K. He et al. 2022. Masked Autoencoders. CVPR.  
[18] J. Heaton et al. 2017. Deep Portfolio Theory. *JFDS*.  
[19] S. Hochreiter and J. Schmidhuber. 1997. LSTM. *Neural Computation*.  
[20] Z. Jiang et al. 2017. A Deep RL Framework for Portfolio Management. arXiv:1706.10059.  
[21] G. Ke et al. 2017. Lightgbm. NeurIPS.  
[22] Z. Ke et al. 2025. Demystifying Domain-adaptive Post-training. arXiv:2501.04961.  
[23] J. Kim. 2025. Semi-Decision-Focused Learning. arXiv:2503.13544.  
[24] D. P. Kingma and M. Welling. 2013. Auto-encoding Variational Bayes.  
[25] X.-Y. Liu et al. 2023. FinGPT. arXiv:2307.10485.  
[26] X.-Y. Liu et al. 2022. FinRL-Meta. NeurIPS.  
[27] X.-Y. Liu et al. 2020. FinRL. arXiv:2011.09607.  
[28] Y. Liu et al. 2020. Adaptive Quantitative Trading. AAAI.  
[29] Y. Liu et al. 2024. TimeXer. arXiv:2401.13795.  
[30] Z. Liu et al. 2025. Fin-r1. arXiv:2503.16252.  
[31] H. Markowitz. 1952. Portfolio Selection. *JF*.  
[32] V. Mnih et al. 2015. Human-level Control. *Nature*.  
[33] Y. Nie et al. 2022. A Time Series is Worth 64 Words. arXiv:2211.14730.  
[34] J. Ouyang et al. 2025. Training Powerful LLM Agents. GitHub.  
[35] L. Qian et al. 2025. Fino1. arXiv:2502.08127.  
[36] M. Qin et al. 2024. Earnthf. AAAI.  
[37] J. Schulman et al. 2017. PPO. arXiv:1707.06347.  
[38] S. Shah et al. 2022. FLANG. arXiv:2205.06031.  
[39] Z. Shao et al. 2024. Deepseekmath. arXiv:2402.03300.  
[40] S. Sun et al. 2023. TradeMaster. NeurIPS.  
[41] Alpaca Team. 2025. Alpaca Markets.  
[42] Financial Modeling Prep Team. 2025. FMP.  
[43] Pyecharts Team. 2025. Pyecharts. GitHub.  
[44] Qwen Team. 2025. Qwen2.5 Technical Report. arXiv:2412.15115.  
[45] TSLib Team. 2025. Time Series Library. GitHub.  
[46] Veri Team. 2025. Volcano Engine RL for LLMs. GitHub.  
[47] A. Van Den Oord et al. 2017. Neural Discrete Representation Learning. NeurIPS.  
[48] A. Vaswani et al. 2017. Attention Is All You Need. NeurIPS.  
[49] G. Wang et al. 2023. Voyager. arXiv:2305.16291.  
[50] S. Wang et al. 2024. Timemixer. arXiv:2405.14616.  
[51] Y. Wang et al. 2024. TimeXer. NeurIPS.  
[52] Z. Wang et al. 2025. RAGEN. arXiv:2504.20073.  
[53] Z. Wei et al. 2023. HireVAE. arXiv:2306.02848.  
[54] G. Woo et al. 2022. Etsformer. arXiv:2202.01381.  
[55] H. Wu et al. 2022. Timesnet. arXiv:2210.02186.  
[56] H. Wu et al. 2021. Autoformer. NeurIPS.  
[57] S. Wu et al. 2023. BloombergGPT. arXiv:2303.17564.  
[58] Y. Xiao et al. 2024. TradingAgents. arXiv:2412.20138.  
[59] A. Yang et al. 2025. Qwen3 Technical Report. arXiv:2505.09388.  
[60] X. Yang et al. 2020. Qlib. arXiv:2009.11189.  
[61] S. Yao et al. 2023. React. ICLR.  
[62] P. Yu et al. 2019. Model-based Deep RL for Portfolio Optimization. arXiv:1901.08740.  
[63] Q. Yu et al. 2025. DAPO. arXiv:2503.14476.  
[64] Y. Yu et al. 2024. Finmem. AAAI Symposium.  
[65] Y. Yu et al. 2024. Fincon. NeurIPS.  
[66] A. Zeng et al. 2023. Are Transformers Effective for Time Series Forecasting? AAAI.  
[67] A. Zeng et al. 2023. Are Transformers Effective for Time Series Forecasting? AAAI.  
[68] L. Zhang et al. 2023. Fineval. arXiv:2308.09975.  
[69] W. Zhang et al. 2025. AgentOrchestra. arXiv:2506.12508.  
[70] W. Zhang et al. 2024. A Multimodal Foundation Agent for Financial Trading. KDD.  
[71] W. Zhang et al. 2024. RL with Maskable Stock Representation. WWW.  
[72] Y. Zhang and J. Yan. 2023. Crossformer. ICLR.  
[73] Y. Zhao et al. 2024. STORM. arXiv:2412.09468.  
[74] T. Zhou et al. 2024. Finrobot. arXiv:2411.08804.  
[75] J. Zhu et al. 2025. Dianjin-r1. arXiv:2504.15716.  
[76] J. Zhu et al. 2024. Benchmarking LLMs on CFLEU. ACL.

## Appendices (A-F)

(Detailed appendix content including dataset description, visualization tools, development framework extensions, and implementation details is available in the original PDF. The above main text includes the core sections 1-6.)
