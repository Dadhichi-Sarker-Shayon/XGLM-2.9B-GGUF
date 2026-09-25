<div align="center">

# 🌍 XGLM-2.9B GGUF

**Native llama.cpp support and release hub for Meta’s higher-quality multilingual XGLM model.**

🌐 30+ Languages &nbsp;•&nbsp; 🇧🇩 Bengali / Bangla &nbsp;•&nbsp; 🧠 2.9B Parameters &nbsp;•&nbsp; ⚙️ Native GGUF &nbsp;•&nbsp; ✅ Published

[🚀 Hugging Face Release](https://huggingface.co/ShayonSarker/xglm-2.9B-GGUF) · [Meta Source Model](https://huggingface.co/facebook/xglm-2.9B) · [llama.cpp](https://github.com/ggml-org/llama.cpp)

</div>

---

## ✨ Overview

This repository contains the native `xglm` architecture patch, reproducible setup instructions, a GGUF verifier, and validation results for [`facebook/xglm-2.9B`](https://huggingface.co/facebook/xglm-2.9B).

The conversion preserves XGLM’s scaled embeddings, offset sinusoidal positions, biased attention, exact GELU, tied input/output embeddings, and 256,008-token UGM tokenizer.

## 📦 GGUF Formats

The model binaries are hosted on Hugging Face to keep this GitHub repository lightweight.

| Format | Recommended use |
|---|---|
| [`XGLM-2.9B-F16.gguf`](https://huggingface.co/ShayonSarker/xglm-2.9B-GGUF/blob/main/XGLM-2.9B-F16.gguf) | Reference quality |
| [`XGLM-2.9B-Q8_0.gguf`](https://huggingface.co/ShayonSarker/xglm-2.9B-GGUF/blob/main/XGLM-2.9B-Q8_0.gguf) | Strong quality, lower memory |
| [`XGLM-2.9B-Q4_K_M.gguf`](https://huggingface.co/ShayonSarker/xglm-2.9B-GGUF/blob/main/XGLM-2.9B-Q4_K_M.gguf) | Strong compact local inference |

## 🔧 Build llama.cpp

```bash
git clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
git checkout 6b790a9c291b5d7af3312bbf9f0c558aa023b13e
git apply /path/to/this/repository/xglm-llama.cpp.patch
cmake -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release --target llama-completion
```

The included patch adds:

- `LLM_ARCH_XGLM` runtime support
- Exact-erf GELU
- XGLM embedding scaling
- Native `XGLMForCausalLM` conversion
- XGLM tensor mapping and sinusoidal positions
- XGLM UGM tokenizer metadata

## ✅ Verify a Downloaded GGUF

```bash
python -m pip install -r requirements.txt
python verify_gguf.py /path/to/XGLM-2.9B-Q4_K_M.gguf
```

The verifier checks architecture, model dimensions, tokenizer settings, embedding scale, and tensor count.

## 🧪 Prompt → Reply

Selected deterministic Q4_K_M smoke checks at temperature 0. This is a small smoke set, not a benchmark.

| Prompt | Model reply | Check |
|---|---|---|
| `The capital of France is` | `Paris.` | ✅ Correct |
| `The capital of Bangladesh is` | `Dhaka.` | ✅ Correct |
| `বাংলাদেশের রাজধানী` | `ঢাকার যানজট` | ✅ Expected answer present |

**Selected smoke score: 3/3**

## 📈 Performance

Lower perplexity (PPL) is better. Scores use separate held-out English and Bengali text with 64-token evaluation windows.

| Format | English PPL | vs F16 | Bengali PPL | vs F16 |
|---|---:|---:|---:|---:|
| F16 | 91.22 | Baseline | 8.06 | Baseline |
| Q8_0 | 87.35 | -4.24% | 8.00 | -0.78% |
| Q4_K_M | 82.88 | -9.15% | 8.52 | +5.70% |

### Validation Summary

- ✅ Token IDs match Transformers across Bengali, English, French, Chinese, and Arabic.
- ✅ F16 English and Bengali predictions match Transformers.
- ✅ Q8_0 and Q4_K_M pass held-out English and Bengali quality gates.
- ✅ Deterministic English and Bengali generation checks pass.

## 🧩 Intended Use

XGLM-2.9B is a **base language model**, not an instruction-tuned assistant. It is suitable for higher-quality multilingual research, Bengali/English workloads, local generation, and GGUF runtime testing.

Validate important outputs independently. Generated text may be inaccurate or inappropriate.

## 📄 License

MIT. See [`LICENSE`](./LICENSE) and the [upstream model card](https://huggingface.co/facebook/xglm-2.9B) for source-model attribution.
