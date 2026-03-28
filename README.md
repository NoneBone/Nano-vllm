<p align="center">
<img width="300" src="assets/logo.png">
</p>

<p align="center">
<a href="https://trendshift.io/repositories/15323" target="_blank"><img src="https://trendshift.io/api/badge/repositories/15323" alt="GeeeekExplorer%2Fnano-vllm | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/></a>
</p>

# Nano-vLLM

A lightweight vLLM implementation built from scratch.

## Key Features

* 🚀 **Fast offline inference** - Comparable inference speeds to vLLM
* 📖 **Readable codebase** - Clean implementation in ~ 1,200 lines of Python code
* ⚡ **Optimization Suite** - Prefix caching, Tensor Parallelism, Torch compilation, CUDA graph, etc.

## Installation

```bash
pip install git+https://github.com/GeeeekExplorer/nano-vllm.git
```
> Suggestion for cuda 11.8 user:
> 
> - python 3.10
> 
> - torch 2.4 | [download](https://github.com/pytorch/pytorch/blob/main/RELEASE.md#release-cadence) | [online install](https://pytorch.org/get-started/previous-versions/)
> - flash_attn-v2.7.3 | [download](https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu11torch2.4cxx11abiFALSE-cp310-cp310-linux_x86_64.whl) | [why need cxx11abiFALSE](https://zhuanlan.zhihu.com/p/1897638897388873200)
>
## Model Download

To download the model weights manually, use the following command:
```bash
huggingface-cli download --resume-download Qwen/Qwen3-0.6B \
  --local-dir ~/huggingface/Qwen3-0.6B/ \
  --local-dir-use-symlinks False
```
- [Use hf but huggingface-cli](https://hugging-face.cn/docs/huggingface_hub/guides/cli)

- [HF model download guide](https://blog.csdn.net/qq_48843954/article/details/151867968)

## Quick Start

See `example.py` for usage. The API mirrors vLLM's interface with minor differences in the `LLM.generate` method:
```python
from nanovllm import LLM, SamplingParams
llm = LLM("/YOUR/MODEL/PATH", enforce_eager=True, tensor_parallel_size=1)
sampling_params = SamplingParams(temperature=0.6, max_tokens=256)
prompts = ["Hello, Nano-vLLM."]
outputs = llm.generate(prompts, sampling_params)
outputs[0]["text"]
```

## Benchmark

See `bench.py` for benchmark.

**Test Configuration:**
- Hardware: RTX 4070 Laptop (8GB)
- Model: Qwen3-0.6B
- Total Requests: 256 sequences
- Input Length: Randomly sampled between 100–1024 tokens
- Output Length: Randomly sampled between 100–1024 tokens

**Performance Results:**
| Inference Engine | Output Tokens | Time (s) | Throughput (tokens/s) |
|----------------|-------------|----------|-----------------------|
| vLLM           | 133,966     | 98.37    | 1361.84               |
| Nano-vLLM      | 133,966     | 93.41    | 1434.13               |

## TODO List

### Easy(week 1)

- [ ] CHANGE: sample strategy 
- [ ] CHANGE: kv cache pre-allocated size
- [ ] BREAK: latency and memory using
- [ ] PLOT: draw data flow by hand

### Medium(week 2)

- [ ] READ: SOSP'23: PageAttn 
- [ ] READ: OSDI'22: A distributed serving system for TBGM
- [x] READ: Flash-attn-v1
- [ ] READ: Flash-attn-v2
- [ ] READ: Flash-attn-v3
- [ ] READ: Flash-attn-v4

### Hard(week 3)

- [ ] READ: vLLM, read module by module.
- [ ] FEAT: answer request continuously
- [ ] FEAT: page attn change
- [ ] FEAT: draft generation and validation