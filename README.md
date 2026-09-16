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

## Text recognition models

`models/pp-ocrv6-small/` and `models/pp-ocrv6-tiny/` hold the PP-OCRv6 detection and
recognition graphs the application runs over scanned pages — the official ONNX exports from
[PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) (Apache-2.0), unchanged, with each
recognition model's character list beside it as a plain text file (one character per line,
taken from the `inference.yml` the export ships with). `small` is what the desktop edition
downloads (det 9,880,512 bytes, rec 21,159,378 bytes); `tiny` is what the browser edition
downloads (det 1,780,590 bytes, rec 4,462,639 bytes). The one file over jsDelivr's ceiling,
`PP-OCRv6_small_rec.onnx`, is stored split the same way as the embedding graph, with its own
`manifest.json`.

`models/tesseract-fast/` holds the `tessdata_fast` traineddata files
([tesseract-ocr/tessdata_fast](https://github.com/tesseract-ocr/tessdata_fast), Apache-2.0)
for Hindi, Bengali and Arabic, gzip-compressed as tesseract.js expects them. They cover the
three scripts PP-OCRv6 does not. `models/tesseract-best/` holds the `tessdata_best` file
([tesseract-ocr/tessdata_best](https://github.com/tesseract-ocr/tessdata_best), Apache-2.0)
for Bengali, which the application reads Bengali with: the fast data reads Bengali at 11%
character error and the best data at 2.6% on the same pages (10,331,048 bytes gzip-compressed,
under jsDelivr's ceiling, so not split).

Tags: `pp-ocrv6-small-v1` (also a release holding the whole files), `pp-ocrv6-tiny-v1`,
`tesseract-fast-v1` and `tesseract-best-v1`.

## Layout, table and formula models

The models the application runs after text recognition on a scanned page, to find and read
what is not running text. All are PaddleOCR's own ONNX exports (Apache-2.0), unchanged.

- `models/pp-doclayout-m/` — PP-DocLayout-M, layout detection (23,496,727 bytes, in two
  pieces with a `manifest.json`); the desktop edition's layout model. Tag and release
  `pp-doclayout-m-v1`.
- `models/pp-doclayout-s/` — PP-DocLayout-S (4,914,918 bytes, whole); the browser edition's
  layout model. Tag `pp-doclayout-s-v1`.
- `models/slanet-plus/` — SLANet_plus table structure recognition (7,782,138 bytes, whole)
  with its token list `SLANet_plus_dict.txt`; both editions. Tag and release
  `slanet-plus-v1`.
- `models/pp-formulanet-s/` — PP-FormulaNet-S formula recognition (231,878,904 bytes, in
  thirteen pieces with a `manifest.json`) with its tokenizer
  `PP-FormulaNet-S_tokenizer.json`; desktop edition only, downloaded on request. Tag and
  release `pp-formulanet-s-v1`.

Every directory carries a `manifest.json`, even where the file is stored whole (the manifest
then lists the file itself as its one piece), because the desktop edition reads the manifest
before it reads the mirror.
