# LM Playschool 2026 — Evaluation Results

This repository contains the LM Playschool Workshop (2026) final results.

## Repo structure
```
.
├── clem_indomain/                  # In-domain (ID) evaluation results
│   ├── <model>/                    # one folder per model
│   │   ├── <game>/                 # one folder per game (adventuregame, wordle, taboo, …)
│   │   │   └── …                   #   interactions.json and scores for that game
│   │   └── run.json                # run configuration & metadata
│   ├── raw.csv                     # raw per-episode scores
│   ├── results.csv                 # aggregated scores for the ID game set
│   └── results.html                # an html results table
│
├── clem_outofdomain/               # Out-of-domain (OOD) evaluation results
│   ├── <model>/                    # same layout
│   ├── raw.csv
│   ├── results.csv
│   └── results.html
│ 
│
├── playpen_runs/                   # Playpen eval outputs per model (14 models)
│   └── <model>/                 
│
├── summaries/                          # Aggregated summary reports
│   ├── combined_game_timings.csv       # runtimes per game, per model
│   ├── summary.csv                     # base per-model summary
│   ├── summary_report.csv/.xlsx        # DELTA: finetuned vs. base
│   └── summary_report_absolute.csv/.xlsx  # ABSOLUTE: finetuned vs. base
│
└── model_registry.json                 # model specs
```

**Model names**: 
- submissions, = <team>__<base-model>__<version> (e.g. DAIR__qwen3.5-2b__sft-v1);
- baselines, = plain model name (e.g. qwen3.5-2b, llama-3.1-8b-instruct).

# Evaluation setup

All models were evaluated with reasoning disabled (`enable_thinking: false`),
`temperature = 0.0`, and a generation limit of `max_tokens = 5000`.
Each model's exact configuration is recorded in its `run.json` — for example, `qwen3.5-4b`, excerpt:

```json
{
"clem_version": "3.7.2",
"player_models": {
    "0": {
    "model_spec": {
        "model_name": "qwen3.5-4b",
        "backend": "huggingface_local",
        "model_config": { "chat_template_kwargs": { "enable_thinking": false } }
    },
    "gen_args": { "temperature": 0.0, "max_tokens": 5000 }
    }
}
}
```

For the full set of games and scripts used for evaluation, see [lm-playschool-2026-closed-set](https://github.com/lm-playpen/lm-playschool-2026-closed-set).

## Results table

*Base model rows show absolute values. Submission rows show the difference to their base model (finetuned − base): ▲ = improvement, ▼ = decline.*

| Base Model | Team | Submission | playpen clemscore | playpen statscore | ID Δ | OOD Δ | Adjusted ID Δ | Adjusted OOD Δ | Generalization Gap | Proportion of Error Reduction (ID) | Proportion of Error Reduction (OOD) |
|---|---|---|--:|--:|--:|--:|--:|--:|--:|--:|--:|
| **qwen3.5-2b** | | | **10.67** | **44.24** | **13.41** | **3.72** | | | | | |
| | CityUoL | Qwen-GuidePlay-2B-v1 | 35.99 ▲ | -1.94 ▼ | 32.85 ▲ | 6.53 ▲ | 30.91 ▲ | 4.59 ▲ | -36.01 | 37.94% | 6.78% |
| | DAIR | sft-dpo-v2 | 38.93 ▲ | -0.71 ▼ | 37.34 ▲ | 10.84 ▲ | 36.63 ▲ | 10.13 ▲ | -36.19 | 43.12% | 11.26% |
| | DAIR | sft-v1 | 35.34 ▲ | 0.11 ▲ | 33.16 ▲ | 11.90 ▲ | 33.16 ▲ | 11.90 ▲ | -30.95 | 38.30% | 12.36% |
| | playornotplay | playornotplay-v1.0-merged-fp32-7263076 | 28.25 ▲ | -0.10 ▼ | 27.76 ▲ | 4.16 ▲ | 27.66 ▲ | 4.06 ▲ | -33.29 | 32.06% | 4.32% |
| **qwen3.5-4b** | | | **26.66** | **51.33** | **34.02** | **17.99** | | | | | |
| | Bentel rockers | Bentel_iter | 4.92 ▲ | 2.94 ▲ | 3.39 ▲ | -1.39 ▼ | 3.39 ▲ | -1.39 ▼ | -20.81 | 5.14% | -1.69% |
| | Bentel rockers | Bentel_iter_2 | -2.39 ▼ | -3.96 ▼ | -2.05 ▼ | -7.80 ▼ | -6.01 ▼ | -11.76 ▼ | -21.78 | -3.11% | -9.51% |
| | Bentel rockers | Bentel_iter_3 | 6.69 ▲ | 2.94 ▲ | 3.74 ▲ | -1.39 ▼ | 3.74 ▲ | -1.39 ▼ | -21.16 | 5.67% | -1.69% |
| **llama-3.1-8b-instruct** | | | **19.53** | **45.59** | **31.24** | **22.62** | | | | | |
| **qwen3.5-9b** | | | **31.91** | **53.90** | **41.12** | **24.91** | | | | | |
| | BSU-SLIM | prm-search-best_of_n | 4.70 ▲ | -3.98 ▼ | -1.48 ▼ | -9.73 ▼ | -5.46 ▼ | -13.71 ▼ | -24.46 | -2.51% | -12.96% |
| | Dialogue Architects | SCoRe_Qwen3.5-9B | 2.48 ▲ | -0.64 ▼ | 2.00 ▲ | -1.54 ▼ | 1.36 ▲ | -2.18 ▼ | -19.75 | 3.40% | -2.05% |
| | LLP: Large Language Problems | llp-final | 21.48 ▲ | 3.90 ▲ | 18.11 ▲ | -2.82 ▼ | 18.11 ▲ | -2.82 ▼ | -37.14 | 30.76% | -3.76% |
| **qwen3.5-27b** | | | **60.30** | **65.05** | **64.34** | **43.51** | | | | | |
| | SLED-BSU | prm-guided-best_of_n | -13.40 ▼ | -1.25 ▼ | -7.98 ▼ | -13.49 ▼ | -9.23 ▼ | -14.74 ▼ | -26.34 | -22.38% | -23.88% |


For full results, see [`summaries/`](summaries/):

- Absolute scores: [CSV](summaries/summary_report_absolute.csv) · [Excel
(colored)](summaries/summary_report_absolute.xlsx)
- Deltas vs. baseline: [CSV](summaries/summary_report.csv) · [Excel (colored)](summaries/summary_report.xlsx)
- Runtimes: [combined_game_timings.csv](summaries/combined_game_timings.csv)

