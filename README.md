# llama.cpp #31510 diagnostic artifacts

Public, issue-specific evidence for [ggml-org/llama.cpp#31510](https://github.com/ggml-org/llama.cpp/issues/31510). The model is GLM-5.3-Flash UD-IQ2_XXS on two Intel Arc Pro B60 cards (SYCL backend), llama.cpp commit `0ec4027a2a0183de87c007069adddc06e665f7de`.

| Archive | Contents | SHA-256 |
|---|---|---|
| `w31-gdb-samples.tgz` | Ten all-thread GDB backtraces, `top -H` samples, window script/log during slow 40,000-MiB prefill. | `161907c54da3d6c16c313d304764f5dcabf7dba702fa5358267a5a3e6259e62c` |
| `w32-w33.tgz` | Matched 1×/2× B60 15,000-MiB requests, plus 2× 40,000-MiB request: server logs, request/response, scripts, window logs. | `21058a7f636d384e733795ecfc651794691cb50d395e76edd44b46052cf18413` |
| `w34-vmm0.tgz` | 2× B60 40,000-MiB control with `GGML_SYCL_ENABLE_VMM=0`, same request and 420-second timeout. | `bc5ed7032701ff727b3bd5e67889808578a2293322bd405a32c30295e939651e` |
| `w35-xmx0.tgz` | 2× B60 40,000-MiB control requested by the SYCL maintainer with `GGML_SYCL_XMX_GATHER_TYPES=0`; same request and timeout. | `16c1e65165b20b63845f1c7ed5e0ba070c4616885f2650c5cd144d2b9f8b109a` |

All archive contents were generated on the test host. The 15,000-MiB matched pair returned HTTP 200: 13-token prompt eval took 32.572 s (one GPU) and 33.131 s (two GPUs). All three 40,000-MiB requests produced no first byte before the 420-second client timeout. The partial 9-of-13-token progress line was 202.31 s by default, 198.54 s with VMM off, and 199.56 s with XMX gather off. These are individual runs, not a statistical benchmark. The 40,000-MiB condition cannot be matched on one card because its visible VRAM is approximately 24.5 GiB.

Extract an archive with `tar xzf ARCHIVE.tgz`. The files are plain text.
