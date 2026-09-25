---
license: mit
base_model: facebook/xglm-2.9B
tags:
  - gguf
  - xglm
  - multilingual
  - bengali
  - bangla
  - text-generation
language:
  - bn
  - en
  - fr
  - de
  - ar
  - ru
  - zh
  - ja
  - es
---

<div align="center">

<p>
  <a href="https://huggingface.co/ShayonSarker/xglm-2.9B-GGUF"><img alt="Hugging Face GGUF" src="https://img.shields.io/badge/Hugging%20Face-GGUF-FFD21E?style=for-the-badge"></a>
  <img alt="XGLM model" src="https://img.shields.io/badge/Model-XGLM--2.9B-8A2BE2?style=for-the-badge">
  <img alt="GGUF formats" src="https://img.shields.io/badge/GGUF-F16%20%7C%20Q8_0%20%7C%20Q4_K_M-FFD21E?style=for-the-badge">
  <img alt="Languages" src="https://img.shields.io/badge/Languages-30%2B-00A6A6?style=for-the-badge">
  <img alt="Bengali and Bangla" src="https://img.shields.io/badge/Bengali-Bangla-16A34A?style=for-the-badge">
  <img alt="Runtime" src="https://img.shields.io/badge/runtime-llama.cpp%20%2B%20patch-E8590C?style=for-the-badge">
  <img alt="Validated" src="https://img.shields.io/badge/validated-pass-22C55E?style=for-the-badge">
  <img alt="F16 parity with transformers" src="https://img.shields.io/badge/F16%20parity-12%2F14%20exact-00A6A6?style=for-the-badge">
  <img alt="MIT license" src="https://img.shields.io/badge/License-MIT-7C3AED?style=for-the-badge">
</p>

# 🌍 XGLM-2.9B GGUF

**Meta’s multilingual XGLM base model for higher-quality Bengali and global language workloads.**

🌐 30+ Languages &nbsp;•&nbsp; 🇧🇩 Bengali / Bangla &nbsp;•&nbsp; 🧠 Base Model &nbsp;•&nbsp; ⚙️ GGUF (llama.cpp + patch) &nbsp;•&nbsp; 📦 2.9B Parameters &nbsp;•&nbsp; ⚖️ MIT

[Source model](https://huggingface.co/facebook/xglm-2.9B) · [llama.cpp](https://github.com/ggml-org/llama.cpp)

👇 [View verified English and Bangla question/answer examples](#verified-question-answer-examples)

</div>

---

## ✨ Highlights

- XGLM support for [llama.cpp](https://github.com/ggml-org/llama.cpp) via the included `xglm-llama.cpp.patch` (upstream does not ship this architecture)
- 256,008-token vocabulary with verified multilingual token parity
- 2,048-token context window
- F16, Q8_0, and importance-matrix-calibrated Q4_K_M formats

## 📦 Choose a Format

| File | Status | Best for |
|---|---|---|
| `XGLM-2.9B-F16.gguf` | Published | Reference quality and maximum fidelity |
| `XGLM-2.9B-Q8_0.gguf` | Published | Strong quality with lower memory use |
| `XGLM-2.9B-Q4_K_M.gguf` | Published | Strongest compact option for local inference |

## ⚠️ Runtime requirement

Stock `llama.cpp` **cannot load these files** and fails with `unknown model architecture: 'xglm'`. XGLM is not in upstream llama.cpp. Apply the bundled patch against the pinned commit:

```bash
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
git checkout 6b790a9c291b5d7af3312bbf9f0c558aa023b13e
git apply /path/to/xglm-llama.cpp.patch
cmake -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release --target llama-completion
```

Alternatives that need no patch: `transformers` with `facebook/xglm-2.9B`, or the HF `gguf` file with a runtime that implements `xglm`.

## 🏗️ Rebuild

The [GitHub release hub](https://github.com/Dadhichi-Sarker-Shayon/XGLM-2.9B-GGUF) includes the end-to-end builder, XGLM runtime patch, verifier, and pinned dependencies.

```bash
git clone https://github.com/Dadhichi-Sarker-Shayon/XGLM-2.9B-GGUF.git
cd XGLM-2.9B-GGUF
python -m pip install -r requirements-build.txt
python build_gguf.py
```

The 2.9B build requires at least **30 GB** of free disk space. It does not overwrite this release.

## 🚀 Run Locally

Apply the [runtime patch](#-runtime-requirement) first, then:

```bash
hf download ShayonSarker/xglm-2.9B-GGUF XGLM-2.9B-Q4_K_M.gguf --local-dir .

llama-completion -m ./XGLM-2.9B-Q4_K_M.gguf \
  -p "Question: What is the capital of Japan?
Answer:" \
  -n 24 --temp 0
```

Expected output: `Tokyo.`

<a id="verified-question-answer-examples"></a>

## ❓ Verified Question → Answer Examples

Every row below is a verbatim `XGLM-2.9B-Q4_K_M.gguf` completion from the patched build at `--temp 0`, 24 new tokens. Nothing is hand-written; wrong output is published in the failure table below rather than edited into looking right. This is a smoke test, not a benchmark.

**Prompt form `Question: ...\nAnswer:`**

| Question | Model answer (verbatim) |
|---|---|
| What is the capital of Japan? | `Tokyo.` |
| What is the capital of Italy? | `Rome.` |
| What is the capital of Egypt? | `Cairo.` |
| What is the largest ocean on Earth? | `The Pacific Ocean.` |
| Which planet is closest to the Sun? | `Mercury.` |
| How many continents are there? | `There are seven continents.` |
| What is the chemical symbol for gold? | `Gold is a chemical element with symbol Au (from Latin: aurum).` |

**Plain completion form** (a base model, so sentence continuation often works better than Q/A)

| Prompt | Model answer (verbatim) |
|---|---|
| `The capital city of France is` | `Paris.` |
| `The chemical symbol for water is` | `H2O.` |
| `The capital city of Bangladesh is` | `also known as the capital of the country. Dhaka is the capital of Bangladesh.` |
| `Plants absorb` | `carbon dioxide from the atmosphere and convert it into sugars, which are then used by the plant to produce energy.` |

## 🔍 Is a wrong answer our bug or the model's? We tested it

14 prompts (8 English Q&A, 3 plain completions, 3 Bengali) were run through `facebook/xglm-2.9b` in `transformers` and through our GGUFs — all greedy, 24 new tokens, same prompt strings.

| Format | Exact string match vs `transformers` |
|---|---:|
| **F16** | **12 / 14** |
| Q4_K_M | 8 / 14 |

Both F16 differences are 24-token truncation points rather than errors: the two outputs start identically and diverge mid-sentence.

**The conversion is faithful.** F16 reproduces the reference model string-for-string, *including the answers that are factually wrong*. A broken conversion or a broken runtime patch could not track the reference this closely. Any XGLM factual error you see below is the upstream checkpoint's own behaviour.

## ⚠️ Why Q4_K_M looks worse than F16

| Prompt | `transformers` reference | F16 (ours) | Q4_K_M (ours) |
|---|---|---|---|
| `The largest planet in the Solar System is` | `Jupiter. It is the largest planet in the Solar System...` | `Jupiter. It is the largest planet in the Solar System...` | `the Earth.` |
| `Question: How many days are in a leap year?\nAnswer:` | `The leap year is a year that is longer than the normal year.` | `The leap year is a year that begins on a leap day...` | `There are 30 days in a leap year.` |
| `প্রশ্ন: জাপানের রাজধানী কোন শহর?\nউত্তর:` | `জাপানের রাজধানী হচ্ছে – টোকিও।` | `জাপানের রাজধানী হচ্ছে – টোকিও।` | loops the prompt and loses the answer |
| `প্রশ্ন: সোনার রাসায়নিক প্রতীক কী?\nউত্তর:` | `সোনার রাসায়নিক প্রতীক হলো সোনালি রঙের লাল চিহ্ন।` | `সোনার রাসায়নিক প্রতীক হলো সোনালি রঙের লাল চিহ্ন।` | `সোনার রাসায়নিক প্রতীক হলো Sn।` |

**Use F16 or Q8_0 for Bengali and for factual lookups.** Q4_K_M is fine for bulk text generation but it is the first thing to break on lower-resource languages.

Perplexity does not predict this. The table below shows Q4_K_M with *better* English PPL than F16 (82.88 vs 91.22) while being measurably less accurate on the prompts above. Treat PPL as a text-fluency proxy, not an accuracy metric.

## ⚠️ Known Failures (Q4_K_M, the published default)

These are the exact completions the released Q4_K_M file produces:

| Prompt | Model answer (verbatim) |
|---|---|
| `Question: How many days are in a leap year?\nAnswer:` | `There are 30 days in a leap year.` |
| `The largest planet in the Solar System is` | `the Earth.` |
| `There are` | `no reviews yet.` |
| `Question: What is the capital of Japan?\nAnswer:` (Bengali script) | `জাপানের রাজধানী হচ্ছে জাপানের রাজধানী হচ্ছে জাপানের রাজধানী হচ্ছে` |
| `সূর্যের নিকটতম গ্রহ কোনটি?` | `সূর্যের নিকটতম গ্রহ হচ্ছে- মঙ্গল।` |
| `ফ্রান্সের রাজধানী` | `প্যারিসে ঐতিহাসিক স্থাপনাগুলোর মধ্যে অন্যতম হলো প্যারিস প্যারেড।` |

Bengali/Bangla prompting is unreliable in this model family, and Q4_K_M makes it worse. The reference model also gets several of these wrong, so treat Bangla output as a research baseline, not a finished translation or QA system.

## 📈 Performance

Lower perplexity (PPL) is better. Scores use separate held-out English and Bengali text with 64-token evaluation windows.

| Format | English PPL | vs F16 | Bengali PPL | vs F16 |
|---|---:|---:|---:|---:|
| F16 | 91.22 | Baseline | 8.06 | Baseline |
| Q8_0 | 87.35 | -4.24% | 8.00 | -0.78% |
| Q4_K_M | 82.88 | -9.15% | 8.52 | +5.70% |

## 🔬 Validation

- **Generation parity with `transformers`:** 14 greedy prompts (8 English Q&A, 3 plain completions, 3 Bengali) — F16 matches the reference output exactly on 12/14, with the 2 differences being 24-token truncation points rather than content errors. Q4_K_M matches on 8/14. See the parity section above.
- Token IDs match Transformers across Bengali, English, French, Chinese, and Arabic.
- Both quantized formats pass the English and Bengali held-out perplexity gates.
- Not tested: logit-level numeric parity, and long-context behaviour beyond 24 generated tokens.

## 🧩 Intended Use

XGLM-2.9B is a **base language model**, not an instruction-tuned assistant. It is suitable for higher-quality multilingual research, Bengali/English workloads, local generation, and GGUF runtime testing.

Outputs may be inaccurate or inappropriate. Validate important results independently.

## 📄 License

MIT. See the [upstream model card](https://huggingface.co/facebook/xglm-2.9B) for source-model details and attribution.
