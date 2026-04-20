# Quantization cost

| Model / Quantization (WikiText-2 perplexity, lower is better) | q4_0   | q4_1   | q5_0   | q5_1   | q8_0   | fp16 |
| ------------------------------------------------------------- | ------ | ------ | ------ | ------ | ------ | ---- |
| llama-7b                                                      | 6.157  | 6.0915 | 5.9846 | 5.948  | 5.9063 | 5.68 |
| llama-13b                                                     | 5.385  | 5.3608 | 5.285  | 5.2702 | 5.2547 | 5.09 |
| llama-30b                                                     | 4.2707 | –      | –      | –      | –      | 4.1  |
| alpaca-30b                                                    | 4.4521 | –      | –      | –      | –      | –    |
| llama-2-7b                                                    | 5.9675 | 6.0398 | 5.8328 | 5.8435 | 5.7897 | –    |
| llama-2-7b-chat                                               | 7.7641 | 7.7853 | 7.5055 | 7.5392 | 7.5014 | –    |
| llama-2-13b                                                   | 5.2172 | 5.2115 | 5.1343 | 5.1289 | 5.1005 | –    |
| llama-2-13b-chat                                              | 6.6296 | 6.7059 | 6.5336 | 6.5771 | 6.5361 | –    |

| llama.cpp quantize `--help` (7B model, ppl delta vs unquantized) | Size @ 7B (GiB) | Δ perplexity (ppl) | Quality note                                                         |
| ---------------------------------------------------------------- | --------------- | ------------------ | -------------------------------------------------------------------- |
| Q4_0                                                             | 3.50            | +0.2499            | Small size, very high quality loss (legacy, prefer Q3_K_M)           |
| Q4_1                                                             | 3.90            | +0.1846            | Small, substantial quality loss (legacy, prefer Q3_K_L)              |
| Q5_0                                                             | 4.30            | +0.0796            | Medium, balanced quality (legacy, prefer Q4_K_M)                     |
| Q5_1                                                             | 4.70            | +0.0415            | Medium, low quality loss (legacy, prefer Q5_K_M)                     |
| Q2_K                                                             | 2.67            | +0.8698            | Smallest, extreme quality loss, not recommended                      |
| Q3_K_S                                                           | 2.75            | +0.5505            | Very small, very high quality loss                                   |
| Q3_K_M                                                           | 3.06            | +0.2437            | Very small, very high quality loss                                   |
| Q3_K_L                                                           | 3.35            | +0.1803            | Small, substantial quality loss                                      |
| Q4_K_S                                                           | 3.56            | +0.1149            | Small, significant quality loss                                      |
| Q4_K_M                                                           | 3.80            | +0.0535            | Medium, balanced quality (recommended)                               |
| Q5_K_S                                                           | 4.33            | +0.0353            | Large, low quality loss (recommended)                                |
| Q5_K_M                                                           | 4.45            | +0.0142            | Large, very low quality loss (recommended)                           |
| Q6_K                                                             | 5.15            | +0.0044            | Very large, extremely low quality loss                               |
| Q8_0                                                             | 6.70            | +0.0004            | Very large, extremely low quality loss, not recommended (size/speed) |
| F16                                                              | 13.00           | ~0 (baseline)      | Extremely large, virtually no quality loss, not recommended (size)   |
| F32                                                              | 26.00           | 0 (lossless)       | Absolutely huge, lossless, not recommended (size)                    |

| Mistral-7B GGUF quantizations (KL-divergence and ln(PPL(Q)/PPL(base))) | Bits per weight | KL median | KL q99 | Top tokens differ | ln(PPL(Q)/PPL(base)) |
| ---------------------------------------------------------------------- | --------------- | --------- | ------ | ----------------- | -------------------- |
| IQ1_S                                                                  | 1.78            | 0.5495    | 5.5174 | 0.3840            | 0.9235               |
| IQ2_XXS                                                                | 2.20            | 0.1751    | 2.4983 | 0.2313            | 0.2988               |
| IQ2_XS                                                                 | 2.43            | 0.1146    | 1.7693 | 0.1943            | 0.2046               |
| IQ2_S                                                                  | 2.55            | 0.0949    | 1.6284 | 0.1806            | 0.1722               |
| IQ2_M                                                                  | 2.76            | 0.0702    | 1.0935 | 0.1557            | 0.1223               |
| Q2_K_S                                                                 | 2.79            | 0.0829    | 1.5111 | 0.1735            | 0.1600               |
| Q2_K                                                                   | 3.00            | 0.0588    | 1.0337 | 0.1492            | 0.1103               |
| IQ3_XXS                                                                | 3.21            | 0.0330    | 0.5492 | 0.1137            | 0.0589               |
| IQ3_XS                                                                 | 3.32            | 0.0296    | 0.4550 | 0.1071            | 0.0458               |
| Q3_K_S                                                                 | 3.50            | 0.0304    | 0.4481 | 0.1068            | 0.0511               |
| IQ3_S                                                                  | 3.52            | 0.0205    | 0.3018 | 0.0895            | 0.0306               |
| IQ3_M                                                                  | 3.63            | 0.0186    | 0.2740 | 0.0859            | 0.0268               |
| Q3_K_M                                                                 | 3.89            | 0.0171    | 0.2546 | 0.0839            | 0.0258               |
| Q3_K_L                                                                 | 4.22            | 0.0152    | 0.2202 | 0.0797            | 0.0205               |
| IQ4_XS                                                                 | 4.32            | 0.0088    | 0.1082 | 0.0606            | 0.0079               |
| IQ4_NL                                                                 | 4.56            | 0.0085    | 0.1077 | 0.0605            | 0.0074               |
| Q4_K_S                                                                 | 4.57            | 0.0083    | 0.1012 | 0.0600            | 0.0081               |
| Q4_K_M                                                                 | 4.83            | 0.0075    | 0.0885 | 0.0576            | 0.0060               |
| Q5_K_S                                                                 | 5.52            | 0.0045    | 0.0393 | 0.0454            | 0.0005               |
| Q5_K_M                                                                 | 5.67            | 0.0043    | 0.0368 | 0.0444            | 0.0005               |
| Q6_K                                                                   | 6.57            | 0.0032    | 0.0222 | 0.0394            | −0.0008              |

| Typical quality loss by bit-width (generic LLMs) | Approximate quality impact (perplexity / quality)                                           |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| 8-bit quantization                               | Minimal quality loss, typically less than 2% perplexity increase compared to full precision |
| 4-bit quantization                               | Moderate impact, around 2–8% quality degradation depending on model and task                |
| 3-bit quantization                               | Noticeable but often acceptable loss, roughly 8–15% quality degradation                     |
| 2-bit quantization                               | Significant degradation, often in the range of 15–30% quality loss                          |

| Heuristic quality bands for specific GGUF quants | Approximate degradation band                                        | Notes |
| ------------------------------------------------ | ------------------------------------------------------------------- | ----- |
| Q5_K_M                                           | ~5–15% quality loss (“acceptable quality loss”) for many workloads  |
| Q4_K_M                                           | ~5–15% quality loss, common default for consumer deployment         |
| Q4_K_S                                           | ~15–30% quality loss (“noticeable quality loss”)                    |
| Q3_K_M                                           | ~15–30% quality loss, significant compression                       |
| Q3_K_S                                           | 30%+ quality loss (“significant quality loss”)                      |
| Q2_K                                             | 30%+ quality loss, extreme compression, generally experimental only |

**Sources**

- [llama.cpp/tools/quantize/README.md at master · ggml-org/](https://github.com/ggml-org/llama.cpp/blob/master/tools/quantize/README.md)
- [A Perplexity Benchmark of llama.cpp - General Generatives](https://www.xzh.me/2023/09/a-perplexity-benchmark-of-llamacpp.html)
- [GGUF quantizations overview](https://gist.github.com/Artefact2/b5f810600771265fc1e39442288e8ec9)
- [The Complete Guide to LLM Quantization - LocalLLM.in](https://localllm.in/blog/quantization-explained)
- [AI Model Quantization 2025: Master Compression Techniques](https://local-ai-zone.github.io/guide)s/what-is-ai-quantization-q4-k-m-q8-gguf-guide-2025.html
- [Difference in different quantization methods #2094](https://github.com/ggml-org/llama.cpp/discussions/2094)
- [Mungert/Qwen3-4B-GGUF - Hugging Face](https://huggingface.co/Mungert/Qwen3-4B-GGUF)
- [Choosing a GGUF Model: K-Quants, IQ Variants, and Legacy Formats](https://kaitchup.substack.com/p/choosing-a-gguf-model-k-quants-i)
- [The difference between quantization methods for the same bits](https://www.reddit.com/r/LocalLLaMA/comments/159nrh)5/the_difference_between_quantization_methods_for/
- [Which Quantization Should I Use? A Unified Evaluation of llama.cpp ...](https://arxiv.org/html/2601.14277v1)
- [Qwen3.5 GGUF Benchmarks | Unsloth Documentation](https://unsloth.ai/docs/models/qwen3.5/gguf-benchmarks)
- [The Complete Guide to LLM Quantization with vLLM - Jarvis Labs](https://jarvislabs.ai/blog/vllm-quantization-complete-guide-benchmarks)
- [llama.cpp¶](https://qwen.readthedocs.io/en/latest/quantization/llama.cpp.html)
- [Mungert/QwQ-32B-GGUF - Hugging Face](https://huggingface.co/Mungert/QwQ-32B-GGUF)
- [model-quantization - Skill | Smithery](https://smithery.ai/skills/martinholovsky/model-quantization)
