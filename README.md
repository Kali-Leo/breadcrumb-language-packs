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
