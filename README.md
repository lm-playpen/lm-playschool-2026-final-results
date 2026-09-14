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
├── static/                         # Static-benchmark (STATIC) evaluation results
│   ├── <model>/                    # same layout (bbh, cladder, eqbench, ifeval, mmlu_pro)
│   ├── raw.csv
│   ├── results.csv
│   └── results.html
│
├── playpen_runs/                   # Playpen validation outputs per model
│   └── <model>/                    # <model>.val.json = validation clemscore/statscore
│
├── summaries/                          # Aggregated summary reports
│   ├── combined_game_timings.csv       # runtimes per game, per model
│   ├── summary.csv                     # base per-model summary (validation, ID, OOD, STATIC)
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

*Base model rows show absolute values. Submission rows show the difference to their base model (finetuned − base): ▲ = improvement, ▼ = decline. Statscore is the clemscore on the static games; Adjusted Δ = Δ + min(Statscore Δ, 0).*

| Base Model | Team | Submission | validation clemscore | validation statscore | Statscore Δ | ID Δ | OOD Δ | Adjusted ID Δ | Adjusted OOD Δ | Generalization Gap | Proportion of Error Reduction (ID) | Proportion of Error Reduction (OOD) |
|---|---|---|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|
| **qwen3.5-2b** |  |  | **10.67** | **44.24** | **40.20** | **6.70** | **3.72** |  |  |  |  |  |
|  | CityUoL | Qwen-GuidePlay-2B-v1 | 35.99 ▲ | -1.94 ▼ | -0.34 ▼ | 40.62 ▲ | 6.53 ▲ | 40.28 ▲ | 6.19 ▲ | -37.07 | 43.54% | 6.78% |
|  | DAIR | sft-dpo-v2 | 38.93 ▲ | -0.71 ▼ | -0.68 ▼ | 46.42 ▲ | 10.84 ▲ | 45.74 ▲ | 10.16 ▲ | -38.56 | 49.75% | 11.26% |
|  | DAIR | sft-v1 | 35.34 ▲ | 0.11 ▲ | -2.83 ▼ | 41.54 ▲ | 11.90 ▲ | 38.71 ▲ | 9.07 ▲ | -32.62 | 44.52% | 12.36% |
|  | playornotplay | playornotplay-v1.0-merged-fp32-7263076 | 28.25 ▲ | -0.10 ▼ | 2.73 ▲ | 33.54 ▲ | 4.16 ▲ | 33.54 ▲ | 4.16 ▲ | -32.36 | 35.95% | 4.32% |
| **qwen3.5-4b** |  |  | **26.66** | **51.33** | **48.74** | **29.28** | **17.99** |  |  |  |  |  |
|  | Bentel rockers | Bentel_iter | 4.92 ▲ | 2.94 ▲ | 1.58 ▲ | 4.02 ▲ | -1.39 ▼ | 4.02 ▲ | -1.39 ▼ | -16.70 | 5.68% | -1.69% |
|  | Bentel rockers | Bentel_iter_2 | -2.39 ▼ | -3.96 ▼ | -5.10 ▼ | -0.81 ▼ | -7.80 ▼ | -5.91 ▼ | -12.90 ▼ | -18.28 | -1.15% | -9.51% |
|  | Bentel rockers | Bentel_iter_3 | 6.69 ▲ | 2.94 ▲ | 1.58 ▲ | 4.46 ▲ | -1.39 ▼ | 4.46 ▲ | -1.39 ▼ | -17.14 | 6.31% | -1.69% |
| **llama-3.1-8b-instruct** |  |  | **19.53** | **45.59** | **40.87** | **27.97** | **22.62** |  |  |  |  |  |
| **qwen3.5-9b** |  |  | **31.91** | **53.90** | **50.20** | **37.37** | **24.91** |  |  |  |  |  |
|  | BSU-SLIM | prm-search-best_of_n | 4.70 ▲ | -3.98 ▼ | -3.44 ▼ | 0.17 ▲ | -9.73 ▼ | -3.27 ▼ | -13.17 ▼ | -22.36 | 0.27% | -12.96% |
|  | Dialogue Architects | SCoRe_Qwen3.5-9B | 2.48 ▲ | -0.64 ▼ | -0.10 ▼ | 2.72 ▲ | -1.54 ▼ | 2.62 ▲ | -1.64 ▼ | -16.72 | 4.34% | -2.05% |
|  | LLP: Large Language Problems | llp-final | 21.48 ▲ | 3.90 ▲ | 4.29 ▲ | 22.82 ▲ | -2.82 ▼ | 22.82 ▲ | -2.82 ▼ | -38.10 | 36.44% | -3.76% |
| **qwen3.5-27b** |  |  | **60.30** | **65.05** | **63.35** | **64.40** | **43.51** |  |  |  |  |  |
|  | SLED-BSU | prm-guided-best_of_n | -13.40 ▼ | -1.25 ▼ | 0.86 ▲ | -10.43 ▼ | -13.49 ▼ | -10.43 ▼ | -13.49 ▼ | -23.95 | -29.30% | -23.88% |


For full results, see [`summaries/`](summaries/):

- Absolute scores: [CSV](summaries/summary_report_absolute.csv) · [Excel
(colored)](summaries/summary_report_absolute.xlsx)
- Deltas vs. baseline: [CSV](summaries/summary_report.csv) · [Excel (colored)](summaries/summary_report.xlsx)
- Runtimes: [combined_game_timings.csv](summaries/combined_game_timings.csv)

