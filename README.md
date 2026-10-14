# AUTOAWQ

![license](https://img.shields.io/badge/license-MIT-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![category](https://img.shields.io/badge/category-chip_quantization-lightgrey)

> Anticloud-hardened packaging of the upstream project `AUTOAWQ` in category **CHIP QUANTIZATION**. Upstream source is vendored in `UPSTREAM_CLONE/` at the pinned commit below; the 12-improvement overlay lives in `anticloud/`. Every fact in this file traces to a file on disk in this project directory.

**Category:** CHIP QUANTIZATION · **Upstream:** https://github.com/casper-hansen/AutoAWQ · **Upstream pin:** `88e4c76b20755db275574e6a03c83c84ba3bece5` · **Vendor:** Anticloud FZ LLE

---

## What This Project Does

# News: The vLLM project has fully adopted AutoAWQ

It is no secret that maintaining a project such as AutoAWQ that has 2+ million downloads, 7000+ models on Huggingface, and 2.1k stars is hard for a solo developer who is doing this in their free time.

Important Notice:
- AutoAWQ is officially deprecated and will no longer be maintained.
- The last tested configuration used Torch 2.6.0 and Transformers 4.51.3.
- If future versions of Transformers break AutoAWQ compatibility, please report the issue to the Transformers project.

Alternative:
- AutoAWQ has been adopted by the vLLM Project: https://github.com/vllm-project/llm-compressor
- MLX-LM now supports AWQ for Mac devices: http://github.com/ml-explore/mlx-lm

For further inquiries, feel free to reach out:
- X: https://x.com/casper_hansen_
- LinkedIn: https://www.linkedin.com/in/casper-hansen-804005170/

# AutoAWQ

<p align="center">
| <a href="https://github.com/casper-hansen/AutoAWQ/issues/32"><b>Roadmap</b></a> | <a href="https://github.com/casper-hansen/AutoAWQ/tree/main/examples"><b>Examples</b></a> | <a href="https://github.com/casper-hansen/AutoAWQ/issues?q=is%3Aopen+is%3Aissue+label%3A%22help+wanted%22"><b>Issues: Help Wanted</b></a> |
</p>

<p align="center" style="margin-bottom: 0px;">
    <a href="https://huggingface.co/models?search=awq">
        <img alt="Huggingface - Models" src="https://img.shields.io/badge/🤗_1000+_models_available-8A2BE2">
    </a>
    <a href="https://github.com/casper-hansen/AutoAWQ/releases">
        <img alt="GitHub - Releases" src="https://img.shields.io/github/release/casper-hansen/AutoAWQ.svg">
    </a>
    <a href="https://pypi.org/project/autoawq/">
        <img alt="PyPI - Downloads" src="https://static.pepy.tech/badge/autoawq/month">
    </a>
</p>

<div align="center" style="color: white;">
    <p>Supported by</p>
    <a href="https://runpod.io/?utm_source=referral&utm_medium=autoAWQ">
    <img src="https://github.com/aadil-runpod/rp-logo/assets/164108768/a8fc546d-cbab-44c4-9a5a-dfb6c400ad24" alt="RunPod Logo" width="100" border="0">  </a>
</div>

AutoAWQ is an easy-to-use package for 4-bit quantized models. AutoAWQ speeds up models by 3x and reduces memory requirements by 3x compared to FP16. AutoAWQ implements the Activation-aware Weight Quantization (AWQ) algorithm for quantizing LLMs.  AutoAWQ was created and improved upon from the [original work](https://github.com/mit-han-lab/llm-awq) from MIT.

*Latest News* 🔥
- [2024/06] CPU inference support (x86) - thanks Intel. Cohere and Phi3 support.
- [2024/04] StableLM and StarCoder2 support.
- [2024/03] Gemma support.
- [2024/02] PEFT-compatible training in FP16.
- [2024/02] AMD ROCm support through ExLlamaV2 kernels.
- [2024/01] Export to GGUF, ExLlamaV2 kernels, 60% faster context processing.
- [2023/12] Mixtral, LLaVa, QWen, Baichuan model support.
- [2023/11] AutoAWQ inference has been integrated into 🤗 transformers. Now includes CUDA 12.1 wheels.
- [2023/10] Mistral (Fused Modules), Bigcode, Turing support, Memory Bug Fix (Saves 2GB VRAM)
- [2023/09] 1.6x-2.5x speed boost on fused models (now including MPT and Falcon).
- [2023/09] Multi-GPU support, bug fixes, and better benchmark scripts available
- [2023/08] PyPi package released and AutoModel class available

## Install

### Prerequisites

- NVIDIA:
  - Your NVIDIA GPU(s) must be of Compute Capability 7.5. Turing and later architectures are supported.
  - Your CUDA version must be CUDA 11.8 or later.
- AMD:
  -  Your ROCm version must be compatible with Triton.
- Intel CPU and Intel GPU:
  - Your torch and intel_extension_for_pytorch package version should at least 2.4 for optimized performance.
  - Alternatively, you can rely on triton kernels for GPU, then you'll need to install [intel-xpu-backend-for-triton](https://github.com/intel/intel-xpu-backend-for-triton) along with compatible torch and transformers. Easiest way is to use [pre-built wheels](https://github.com/intel/intel-xpu-backend-for-triton?tab=readme-ov-file#install-pytorch-and-triton-from-nightly-wheels).

### Install from PyPi

There are a few ways to install AutoAWQ:

1. Default:
    - `pip install autoawq`
    - NOTE: The default installation includes no external kernels and relies on Triton for inference.

2. From release with kernels:
    - `pip install autoawq[kernels]`
    - NOTE: This requires you to match the latest torch version that the kernels were build with.
    - NOTE: This installs https://github.com/casper-hansen/AutoAWQ_kernels

3. From main branch for Intel CPU and Intel XPU optimized performance:
    - `pip install autoawq[cpu]`
    - NOTE: Minimum of torch 2.4.0 is required.

## Usage

Under examples, you can find examples of how to quantize, run inference, and benchmark AutoAWQ models.

### INT4 GEMM vs INT4 GEMV vs FP16

There are two versions of AWQ: GEMM and GEMV. Both names relate to how matrix multiplication runs under the hood. We suggest the following:

- GEMV (quantized): 20% faster than GEMM, only batch size 1 (not good for large context).
- GEMM (quantized): Much faster than FP16 at batch sizes below 8 (good with large contexts).
- FP16 (non-quantized): Recommended for highest throughput: [vLLM](https://github.com/vllm-project/vllm).

#### Compute-bound vs Memory-bound

At small batch sizes with small 7B models, we are memory-bound. This means we are bound by the bandwidth our GPU has to push around the weights in memory, and this is essentially what limits how many tokens per second we can generate. Being memory-bound is what makes quantized models faster because your weights are 3x smaller and can therefore be pushed around in memory much faster. This is different from being compute-bound where the main time spent during generation is doing matrix multiplication. 

In the scenario of being compute-bound, which happens at higher batch sizes, you will not gain a speed-up using a W4A16 quantized model because the overhead of dequantization will slow down the overall generation. This happe

*(excerpt; full text in `UPSTREAM_CLONE/`)*

*Quoted from the upstream `README.md` file in `UPSTREAM_CLONE/`.*
Project-specific facts detected in this directory:

- Ecosystem: **Python** (manifests: setup.py; scanned in UPSTREAM_CLONE)
- Top-level source layout: `awq/`, `examples/`, `scripts/`, `tests/`
- Snapshot size: **96 files**, **12074 lines of code** (measured; see Benchmarks)
- Primary languages: `.py` (87), `.md` (5), `(none)` (2), `.sh` (1), `.yml` (1)
- Upstream commit pinned for this packaging: `88e4c76b20755db275574e6a03c83c84ba3bece5`

---

## Installation

No installation section was found in the upstream readme, so the commands below are generated from the manifests detected in this project directory.

```sh
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -e .                                     # or: pip install -r requirements.txt
```

Overlay install (this project):

```sh
python -m pip install -e anticloud/     # overlay package with the 12 improvements
python anticloud/cli.py --help          # 13 subcommands, JSON stdout
```

---

## Usage

</p>

<p align="center" style="margin-bottom: 0px;">
    <a href="https://huggingface.co/models?search=awq">
        <img alt="Huggingface - Models" src="https://img.shields.io/badge/🤗_1000+_models_available-8A2BE2">
    </a>
    <a href="https://github.com/casper-hansen/AutoAWQ/releases">
        <img alt="GitHub - Releases" src="https://img.shields.io/github/release/casper-hansen/AutoAWQ.svg">
    </a>
    <a href="https://pypi.org/project/autoawq/">
        <img alt="PyPI - Downloads" src="https://static.pepy.tech/badge/autoawq/month">
    </a>
</p>

<div align="center" style="color: white;">
    <p>Supported by</p>
    <a href="https://runpod.io/?utm_source=referral&utm_medium=autoAWQ">
    <img src="https://github.com/aadil-runpod/rp-logo/assets/164108768/a8fc546d-cbab-44c4-9a5a-dfb6c400ad24" alt="RunPod Logo" width="100" border="0">  </a>
</div>

AutoAWQ is an easy-to-use package for 4-bit quantized models. AutoAWQ speeds up models by 3x and reduces memory requirements by 3x compared to FP16. AutoAWQ implements the Activation-aware Weight Quantization (AWQ) algorithm for quantizing LLMs.  AutoAWQ was created and improved upon from the [original work](https://github.com/mit-han-lab/llm-awq) from MIT.

*Latest News* 🔥
- [2024/06] CPU inference support (x86) - thanks Intel. Cohere and Phi3 support.
- [2024/04] StableLM and StarCoder2 support.
- [2024/03] Gemma support.
- [2024/02] PEFT-compatible training in FP16.
- [2024/02] AMD ROCm support through ExLlamaV2 kernels.
- [2024/01] Export to GGUF, ExLlamaV2 kernels, 60% faster context processing.
- [2023/12] Mixtral, LLaVa, QWen, Baichuan model support.
- [2023/11] AutoAWQ inference has been integrated into 🤗 transformers. Now includes CUDA 12.1 wheels.
- [2023/10] Mistral (Fused Modules), Bigcode, Turing support, Memory Bug Fix (Saves 2GB VRAM)
- [2023/09] 1.6x-2.5x speed boost on fused models (now including MPT and Falcon).
- [2023/09] Multi-GPU support, bug fixes, and better benchmark scripts available
- [2023/08] PyPi package released and AutoModel class available

*Section quoted from the upstream readme.*
Anticloud overlay CLI (available in every project):

```sh
python anticloud/cli.py --help     # 13 subcommands, JSON stdout
python anticloud/cli.py checks     # run the 16-check suite
```

---

## API

- [2023/09] 1.6x-2.5x speed boost on fused models (now including MPT and Falcon).
- [2023/09] Multi-GPU support, bug fixes, and better benchmark scripts available
- [2023/08] PyPi package released and AutoModel class available

*Section quoted from the upstream readme.*
---

## Dependencies

| Metric | Value |
|--------|-------|
| Ecosystem | Python |
| Manifests detected | setup.py |
| Files in snapshot | 96 |
| Lines of code | 12074 |
| Dependency references | 8 |
| Dependencies by ecosystem | pypi: 8 |
| Upstream license | MIT |
| Overlay license | Anticommons 0.1.0 |

Top dependency references recorded in the benchmark snapshot:

| Ecosystem | Name | Version | Source file |
|-----------|------|---------|-------------|
| pypi | transformers | >=4.45.0 | setup.py |
| pypi | tokenizers | >=0.12.1 | setup.py |
| pypi | typing_extensions | >=4.8.0 | setup.py |
| pypi | datasets | >=2.20 | setup.py |
| pypi | huggingface_hub | >=0.26.5 | setup.py |
| pypi | lm_eval | ==0.4.1 | setup.py |
| pypi | intel-extension-for-pytorch | >=2.4.0 | setup.py |
| pypi | flash-attn | >=2.2.0 | setup.py |

Pinned lockfile: `anticloud/requirements.lock` (hash-pinned, PEP 508). SBOM: `sbom.cdx.json` (CycloneDX 1.5, pinned to the upstream SHA).

---

## Configuration

- If future versions of Transformers break AutoAWQ compatibility, please report the issue to the Transformers project.

Alternative:
- AutoAWQ has been adopted by the vLLM Project: https://github.com/vllm-project/llm-compressor
- MLX-LM now supports AWQ for Mac devices: http://github.com/ml-explore/mlx-lm

For further inquiries, feel free to reach out:
- X: https://x.com/casper_hansen_
- LinkedIn: https://www.linkedin.com/in/casper-hansen-804005170/

*Section quoted from the upstream readme.*
Overlay configuration (Anticloud):

- `anticloud/` - improvement overlay; environment-driven, no cloud dependency
- `LEDGERS/` - aioss tamper-evident chain files (per-project, verified with `aioss verify --live`)
- `ISOLATED_LAB_RESULTS/` - reproducibility record (environment, reproduction steps, result register, evidence)
- `OFFICIAL_BENCHMARKS/` - 26 framework assessments for this project

---

## Contributing

Upstream contributions: fork the `AUTOAWQ` project, create a feature branch, and open a pull request against upstream. Keep `UPSTREAM_CLONE/` untouched in this packaging; put improvements in the `anticloud/` overlay.

Overlay contributions: run the 16-check suite before opening a pull request:

```sh
python anticloud/bench/runner.py --cwd anticloud
```

---

## License

**Upstream license: MIT** (evidence: `LICENSE` in the upstream snapshot).

License file excerpt:

```text
MIT License

Copyright (c) 2023 MIT HAN Lab

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
```

### Anticommons 0.1.0 overlay

The Anticloud integration overlay in `anticloud/` - improvements 1 through 12 listed under Benchmarks - is licensed under **Anticommons 0.1.0**. Upstream code remains under its original MIT terms. See `ANTICOMMONS_LICENSE.md` in this directory for the overlay terms and contact.

SPDX: `MIT` (upstream) + Anticommons 0.1.0 (overlay, dual).

---

## Upstream

- **Project:** `AUTOAWQ` (category: CHIP QUANTIZATION)
- **Upstream URL:** https://github.com/casper-hansen/AutoAWQ
- **Pinned commit (SHA):** `88e4c76b20755db275574e6a03c83c84ba3bece5`
- **Branch:** main
- **Pin provenance:** GitHub API commits/main. The parent-project stamp is explicitly rejected for this project.
- **Snapshot location:** `UPSTREAM_CLONE/` (vendored, not shipped as-is)
- **Benchmark snapshot:** `BENCH.json`

---

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`d5ebd0e073e5e1610032e94ad18c6d37982030bd1808e401b708a7dc0996627d`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

