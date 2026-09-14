# Breadcrumb language packs

Word lists for [Breadcrumb](https://github.com/Kali-Leo/Breadcrumb)'s language learning: the AI
writes its answer in the language you read, and a few words are swapped for the language you are
learning. Each file here is one direction of one pair — `es-fr.json` is "reads Spanish, learns
French".

197 pairs across 15 languages a learner can read and 16 they can learn. A pair exists only where
the data supports it: a pack needs a bilingual dictionary, a frequency list for the language
being read, and at least 1,500 words the swap can be made confidently for. Pairs that fall short
are not published rather than published thin.

Built by `scripts/language-packs/` in the application repository, which records the exact
upstream files and their checksums. The application verifies each pack's SHA-256 before storing
it, so these files are meant to be fetched, not edited.

**Licence:** CC BY-SA 4.0. Sources and attribution are in [NOTICE.md](NOTICE.md).

## Embedding model

`models/gte-multilingual-base/` holds the ONNX embedding model the browser edition loads —
[Alibaba-NLP/gte-multilingual-base](https://huggingface.co/Alibaba-NLP/gte-multilingual-base)
(Apache-2.0), exported and quantized by us: weight-only int8, per-output-channel scales,
`reduce_range=True`, opset 17. It agrees with the fp32 reference to a cosine of 0.9994 over 200
real passages, where the readily available int8 conversion of this model manages 0.926.

The graph is 310,821,344 bytes, which is over jsDelivr's 20 MB per-file ceiling, so it is stored
split: `onnx/model_int8.onnx.000` … `.016`, 18 MiB each. `manifest.json` lists the pieces in
order with their sizes, and carries the SHA-256 and byte count of the whole file so the client
can check what it reassembled. Concatenating the pieces in name order reproduces the graph byte
for byte.

`models/bge-reranker-v2-m3/` holds the desktop-only reranker the same way —
[BAAI/bge-reranker-v2-m3](https://huggingface.co/BAAI/bge-reranker-v2-m3) (MIT), dynamic int8,
570,698,919 bytes in 31 pieces with its own `manifest.json`.

The desktop edition downloads the same files unsplit from this repository's releases, one
release per model, tagged `gte-multilingual-base-int8-v1` and `bge-reranker-v2-m3-int8-v1`,
and falls back to the split copies here (through jsDelivr) when GitHub's release host cannot
be reached. Each tag points at a commit that holds that model's pieces.
