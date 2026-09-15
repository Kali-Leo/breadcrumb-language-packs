# Sources and attribution

Every file under `packs/` is a derivative work of the sources below and is licensed
**CC BY-SA 4.0** (`LICENSE.txt`), as those sources require. Attribution travels with each
pack as well: the `attribution` field inside the JSON is what the application shows.

- **Wiktionary**, via the machine-readable extracts at **kaikki.org** (wiktextract) —
  headwords, parts of speech, senses, inflected forms and IPA.
  Licence: CC BY-SA 4.0 (some content additionally under GFDL). https://www.wiktionary.org ·
  https://kaikki.org
- **FrequencyWords** by Hermit Dave, derived from the OpenSubtitles corpus — how common a word
  is, which decides the order words are introduced in.
  Licence: CC BY-SA 4.0. https://github.com/hermitdave/FrequencyWords
- **CC-CEDICT** (MDBG) — the Chinese→English pack's entries, traditional/simplified forms and
  senses. Licence: CC BY-SA 4.0. https://www.mdbg.net/chinese/dictionary?page=cc-cedict
- **CMUdict** (Carnegie Mellon University) — English pronunciation, converted from ARPABET to
  IPA. Licence: BSD-2-Clause. https://github.com/cmusphinx/cmudict

## Models

The files under `models/` are not language packs and are not CC BY-SA. Each carries its
upstream licence:

- **PP-OCRv6** detection and recognition models, **PP-DocLayout-S/M** layout detection,
  **SLANet_plus** table structure recognition and **PP-FormulaNet-S** formula recognition, from
  PaddlePaddle/PaddleOCR — Apache-2.0. https://github.com/PaddlePaddle/PaddleOCR
- **tessdata_fast** Hindi, Bengali and Arabic traineddata, from tesseract-ocr — Apache-2.0.
  https://github.com/tesseract-ocr/tessdata_fast
- **gte-multilingual-base** (Alibaba-NLP) — Apache-2.0; **bge-reranker-v2-m3** (BAAI) — MIT.
  Exported and quantized by us, as described in README.md.

## Why this is a separate repository

The application is licensed AGPL-3.0-only. CC BY-SA 4.0 grants one-way compatibility with
GPLv3 but not with AGPLv3, so this data cannot be folded into the application's own licence.
Keeping it here lets each licence apply cleanly to what it covers.
