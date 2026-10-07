# Kurdish NLP Metrics

Open, independent, reproducible metrics for how well language technology actually handles Kurdish — Kurmancî, Soranî, and Zazakî.

## Why this exists

Kurdish is spoken by 30–40 million people across three major varieties and two scripts, and almost nothing in mainstream NLP evaluation reflects that. When a tokenizer, a translation API, or a grammar checker claims to "support Kurdish," there has been no shared, public, reproducible way to check what that actually means — per dialect, not as one blended average.

This repository is that shared reference point. It's maintained by [Kurdish-Tech](https://github.com/Kurdish-Tech), an open-source org with no commercial product of its own to defend — the results here aren't used to sell anything, which is what makes independent numbers worth trusting in the first place.

**What this is, precisely:** intrinsic measurements of tools (how a tokenizer behaves on Kurdish text, how much of a phenomenon an eval set covers) — not a labeled task benchmark with a leaderboard of model scores. If you're looking for the latter, see [kurdish-grammar-eval](https://huggingface.co/datasets/kurdish-tech/kurdish-grammar-eval) for the minimal-pairs data that could feed one.

## What's measured

### 1. Tokenizer fertility

Fertility = tokens produced per word. Lower is better: fewer tokens means cheaper inference and more usable context per token budget.

**Method:** same held-out sample for every tokenizer — up to 300 documents per dialect (166 for Zazakî, which is all that met the length threshold), each document at least 20 words, truncated to 2,000 characters, encoded with special tokens stripped. Full data: [`data/tokenizer_fertility.json`](data/tokenizer_fertility.json).

| Tokenizer | Kurmancî | Soranî | Zazakî |
|---|---:|---:|---:|
| kurdish-bpe-64k | 1.342 | 1.793 | 2.408 |
| kurdish-unigram-64k | 1.385 | 1.633 | 2.290 |
| kurdish-bpe-32k | 1.427 | 1.974 | 2.701 |
| kurdish-unigram-32k | 1.472 | 1.843 | 2.580 |
| NLLB-200 (distilled-600M) | 1.930 | 2.336 | 2.548 |
| XLM-RoBERTa (base) | 1.751 | 3.695 | 2.527 |
| o200k_base (GPT-4o) | 2.361 | 3.984 | 2.732 |
| cl100k_base (GPT-4) | 2.610 | 6.938 | 3.038 |

Note what this does and doesn't show: lower fertility doesn't by itself guarantee better downstream model quality, and the Zazakî column rests on far less held-out data than the other two — treat those numbers as the least robust of the three.

### 2. Grammar evaluation coverage

[kurdish-grammar-eval](https://huggingface.co/datasets/kurdish-tech/kurdish-grammar-eval) tests specific grammatical phenomena via auto-mined minimal pairs (a correct sentence vs. a corrupted one). Current coverage — see [`data/grammar_eval_summary.json`](data/grammar_eval_summary.json):

| Category | Kurmancî | Soranî | Zazakî |
|---|---:|---:|---:|
| copula_agreement | 15 | 15 | — |
| ezafe_deletion | 300 | 132 | — |
| indefinite_duplication | 300 | 270 | — |

These pairs are auto-mined, not human-verified, and there is no Zazakî coverage yet. As a first external sanity check, 5 `copula_agreement` pairs were cross-checked against an independent third-party Kurdish grammar tool (mazdek AI) — 10/10 agreement. That's a supportive signal for one category on a small sample, not a validation of the whole dataset.

## Honest limitations

- Zazakî is the weakest-covered dialect in every measurement here — smallest corpus share, no grammar-eval coverage, least robust fertility numbers. This isn't hidden; it's the headline finding as much as any score is.
- The grammar eval is auto-mined. Treat it as a starting point for probing models, not a finished, human-audited evaluation set.
- This is v1: tokenizer fertility and grammar-eval coverage only. It does not yet cover translation quality, speech recognition, or spell-checking — those are natural next additions.

## Get your tool measured

If you maintain a Kurdish-capable tokenizer, translator, grammar checker, or speech model and want it added here, open an issue. The methodology above is applied identically to every entry — same sample, same script, same measurement code, published alongside the results.

## License

Data in this repository is released under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

## Links

- [Kurdish-Tech](https://github.com/Kurdish-Tech)
- [KurdishCorpus-clean](https://huggingface.co/datasets/kurdish-tech/KurdishCorpus-clean)
- [kurdish-grammar-eval](https://huggingface.co/datasets/kurdish-tech/kurdish-grammar-eval)
