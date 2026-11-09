# Agentic Memory 🧠

A novel agentic memory system for LLM agents that can dynamically organize memories in an agentic way.

> **Note:** This repository is specifically designed to reproduce the results presented in our paper. If you want to use the A-Mem system in building your agents, please refer to our official implementation at: [A-mem-sys](https://github.com/WujiangXu/A-mem-sys)

For more details, please refer to our paper: [A-Mem: Agentic Memory for LLM Agents](https://arxiv.org/pdf/2502.12110)

> 🔥 **News (Oct 2026):** We provide a stronger version of A-Mem, **FlexA-Mem: Budget-Adaptive Agentic Memory for LLM Agents** ([paper](https://arxiv.org/html/2610.33274v1), [code](https://github.com/WujiangXu/Flex-Amem)). FlexA-Mem keeps A-Mem's linked notes but uses them as the compressed history itself: at every step, the agent sees the whole history as a timeline of notes, and the notes most relevant to the current step are expanded in place into their raw content or excerpts, so the prompt fits any token budget. It is evaluated on LongMemEval-S, LoCoMo, SWE-bench Verified and AppWorld.

## Introduction 🌟

Large Language Model (LLM) agents have demonstrated remarkable capabilities in handling complex real-world tasks through external tool usage. However, to effectively leverage historical experiences, they require sophisticated memory systems. Traditional memory systems, while providing basic storage and retrieval functionality, often lack advanced memory organization capabilities.

Our project introduces an innovative **Agentic Memory** system that revolutionizes how LLM agents manage and utilize their memories:

<div align="center">
  <img src="Figure/intro-a.jpg" alt="Traditional Memory System" width="600"/>
  <img src="Figure/intro-b.jpg" alt="Our Proposed Agentic Memory" width="600"/>
  <br>
  <em>Comparison between traditional memory system (top) and our proposed agentic memory (bottom). Our system enables dynamic memory operations and flexible agent-memory interactions.</em>
</div>

## Key Features ✨

- 🔄 Dynamic memory organization based on Zettelkasten principles
- 🔍 Intelligent indexing and linking of memories
- 📝 Comprehensive note generation with structured attributes
- 🌐 Interconnected knowledge networks
- 🔄 Continuous memory evolution and refinement
- 🤖 Agent-driven decision making for adaptive memory management

## Framework 🏗️

<div align="center">
  <img src="Figure/framework.jpg" alt="Agentic Memory Framework" width="800"/>
  <br>
  <em>The framework of our Agentic Memory system showing the dynamic interaction between LLM agents and memory components.</em>
</div>

## How It Works 🛠️

When a new memory is added to the system:
1. Generates comprehensive notes with structured attributes
2. Creates contextual descriptions and tags
3. Analyzes historical memories for relevant connections
4. Establishes meaningful links based on similarities
5. Enables dynamic memory evolution and updates

## Results 📊

Empirical experiments conducted on six foundation models demonstrate superior performance compared to existing SOTA baselines.

## Getting Started 🚀

1. Clone the repository:
```bash
git clone https://github.com/WujiangXu/AgenticMemory.git
cd AgenticMemory
```

2. Install dependencies:
Option 1: Using venv (Python virtual environment)
```bash
# Create and activate virtual environment
python -m venv a-mem
source a-mem/bin/activate  # Linux/Mac
a-mem\Scripts\activate     # Windows

# Install dependencies
pip install -r requirements.txt
```

Option 2: Using Conda
```bash
# Create and activate conda environment
conda create -n myenv python=3.9
conda activate myenv

# Install dependencies
pip install -r requirements.txt
```

3. Run the experiments in LoCoMo dataset:

**Option A — Original evaluation (requires OpenAI JSON schema support):**
```bash
python test_advanced.py
```

**Option B — Robust evaluation (recommended, works with any LLM backend):**

The robust evaluation (`test_advanced_robust.py`) removes the JSON schema dependency and supports OpenAI API, vLLM, and Ollama backends.

```bash
# OpenAI models
python test_advanced_robust.py --backend openai --model gpt-4o-mini \
    --dataset data/locomo10.json --output results_robust_gpt-4o-mini.json

# vLLM-served open-source models (start vLLM server first)
python -m vllm.entrypoints.openai.api_server \
    --model Qwen/Qwen2.5-3B-Instruct --port 30000 \
    --dtype float16 --enforce-eager --max-model-len 8192
python test_advanced_robust.py --backend vllm --model Qwen/Qwen2.5-3B-Instruct \
    --dataset data/locomo10.json --output results_robust_qwen3b.json \
    --sglang_port 30000

# Ollama models
python test_advanced_robust.py --backend ollama --model qwen2.5:3b \
    --dataset data/locomo10.json --output results_robust_ollama_qwen3b.json
```

Key arguments:
- `--retrieve_k`: Number of memories to retrieve per query (default: 10). Tune this per model for best results.
- `--ratio`: Fraction of dataset to evaluate (e.g., `--ratio 0.1` for 10% quick test).
- `--backend`: One of `openai`, `vllm`, `ollama`.
- `--sglang_port`: Port for vLLM/SGLang server (default: 30000).

4. Run the full k-sweep to find optimal retrieval k per model:
```bash
bash run_k_sweep.sh
```

**Note:** To achieve the optimal performance reported in our paper, please adjust the hyperparameter `k` value accordingly. Memories are cached after the first run, so subsequent k-sweep evaluations only re-run the QA answering step.

**Categories Information:** The LoCoMo dataset contains the following categories:
* Category 1: Multi-hop
* Category 2: Temporal
* Category 3: Open-domain
* Category 4: Single-hop
* Category 5: Adversarial

For more details about the categories, please refer to [this GitHub issue](https://github.com/snap-research/locomo/issues/6). 

## Citation 📚

If you use this code in your research, please cite our series of works on agentic memory:

```bibtex
@inproceedings{xu2025amem,
  title={A-Mem: Agentic memory for llm agents},
  author={Xu, Wujiang and Liang, Zujie and Mei, Kai and Gao, Hang and Tan, Juntao and Zhang, Yongfeng},
  booktitle={Advances in Neural Information Processing Systems},
  year={2025}
}

@article{xu2026flexamem,
  title={FlexA-Mem: Budget-Adaptive Agentic Memory for LLM Agents},
  author={Xu, Wujiang and Han, Jiaojiao and Zhu, Xi and Zhang, Qizheng and Liang, Kaiqu and Yin, Yutong and Mei, Kai and Metaxas, Dimitris N.},
  journal={arXiv preprint arXiv:2610.33274},
  year={2026}
}
```

## License 📄

This project is licensed under the MIT License. See LICENSE for details.

**#FTEC 5660 Individual project**

# A-Mem Reproducibility Study & Ablation Analysis

This repository contains the reproduction, evaluation, and ablation study for **A-Mem** (a multi-hop memory system for LLMs), evaluated on a customized subset of the **LoCoMo** dataset using the **DeepSeek** API.


## 🛠️ 1. Environment Setup & API Keys

  ### Prerequisites
  * Python **3.11.5** (recommended to run inside a virtual environment)
  * macOS (Apple Silicon M-series tested) or Linux / Windows

  ### Installation Steps
  1.#Clone the repository:
   ```bash
   git clone [https://github.com/annatse12/Individual-Project2.git]。(https://github.com/annatse12/Individual-Project2.git)
   cd Individual-Project2

  2.#Create and activate a virtual environment：
  python3 -m venv venv
  source venv/bin/activate

  3.#Install required dependencies:
  pip install -r requirements.txt

  4.#Configure your API Key:
  #Create a secure environment configuration or export your DeepSeek API key:
  export OPENAI_API_KEY="your_deepseek_api_key_here"
  export OPENAI_BASE_URL="[https://api.deepseek.com](https://api.deepseek.com)"

## 2. Run Steps & Execution Pipeline
Follow these steps to reproduce the dataset preparation, baseline evaluations, and ablation studies:

   #Generate Mini Dataset
   #Prepare the customized subset (locomo_mini.json) from the raw data:
   python scripts/create_mini_dataset.py

   #Run Baseline Evaluation (k=5)
   #Execute the multi-hop memory retrieval baseline across 5 independent trials:
    python test_advanced_robust.py --retrieve_k 5 --trials 5

   #Run Ablation Study (k=2)
   #Execute the sensitivity ablation by reducing the retrieval depth to k=2:
   python test_advanced_robust.py --retrieve_k 2 --trials 5

## 3:Experimental Results Summary

| Table / Metric | Summary / Value |
| :--- | :--- |
| **1. Overall Performance** | • A-Mem (Paper): **27.02%** F1<br>• A-Mem (Ours, DeepSeek): **12.76 ± 1.00%** F1 (BERT F1: **87.88 ± 0.24%**) |
| **2. Ablation Study (`n=5`)** | • Baseline (`k=5`): Mean F1 = **0.1276 ± 0.0100**<br>• Ablation (`k=2`): Mean F1 = **0.0000 ± 0.0000** |
| **3. Mechanistic Analysis** | • 4/5 questions failed across all $k$<br>• The only successful question relied on a memory node ranked exactly **5th** (Token F1: **0.615**), which was truncated at $k=2$. |
| **4. Setup Summary** | • Hardware/Env: MacBook (Apple Silicon), Python 3.11.5 (`venv`)<br>• API: DeepSeek (`deepseek-chat`), `locomo_mini.json` (5 QAs, 5 trials) |

### 📝 **Key Conclusion**
> "Our 5 independent trials show extreme stability. The baseline ($k=5$) achieved a mean F1 of $0.1276 \pm 0.0100$, while reducing retrieval depth to $k=2$ collapsed the F1 score entirely to $0.0000$. This proves that **A-Mem's multi-hop reasoning is highly fragile and strictly bound by retrieval depth.**"

## 4:Repository Directory Structure
Individual-Project2/
│
├── scripts/
│   └── create_mini_dataset.py      # Script for dataset sample generation
├── results/
│   ├── baseline_k5_run1..5.json    # 5 independent trials for baseline (k=5)
│   └── ablation_k2_run1..5.json    # 5 independent trials for ablation (k=2)
│
├── test_advanced_robust.py         # Main execution and evaluation script
├── requirements.txt                # Python package dependencies
└── README.md                       # Project documentation

## 5：Key Findings & Conclusion
Stability: Our 5 independent trials demonstrated extreme behavioral consistency for both baseline and ablation settings.
Fragility of Multi-Hop Reasoning: Reducing retrieve_k from 5 to 2 caused the F1 score to collapse entirely from 0.1276 to 0.0000.
Mechanistic Insight: Detailed error analysis reveals that the sole successful multi-hop question relied on a critical memory node ranked exactly 5th. Truncating the retrieval depth to 2 stripped away this essential link, proving that A-Mem's reasoning capability is strictly bound by retrieval depth.
