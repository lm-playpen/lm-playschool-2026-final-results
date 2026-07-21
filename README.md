# LM Playschool 2026 — closed games

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


| Model | playpen clemscore | playpen statscore | ID | OOD |
|---|--:|--:|--:|--:|
| **qwen3.5-2b** | **11.43** | **27.26** | **13.41** | **3.72** |
| &nbsp;&nbsp;CityUoL__qwen3.5-2b__Qwen-GuidePlay-2B-v1 | 50.44 ▲ | 36.74 ▲ | 46.26 ▲ | 10.25 ▲ |
| &nbsp;&nbsp;DAIR__qwen3.5-2b__sft-dpo-v2 | 49.03 ▲ | 26.87 ▼ | 50.75 ▲ | 14.56 ▲ |
| &nbsp;&nbsp;DAIR__qwen3.5-2b__sft-v1 | 46.34 ▲ | 22.86 ▼ | 46.57 ▲ | 15.62 ▲ |
| &nbsp;&nbsp;playornotplay__qwen3.5-2b__…-828e356 | 39.90 ▲ | 18.94 ▼ | 41.17 ▲ | 7.88 ▲ |
| **qwen3.5-4b** | **33.71** | **37.16** | **34.02** | **17.99** |
| &nbsp;&nbsp;Bentel rockers__qwen3.5-4b__Bentel_iter | 36.09 ▲ | 35.75 ▼ | 37.41 ▲ | 16.60 ▼ |
| &nbsp;&nbsp;Bentel rockers__qwen3.5-4b__Bentel_iter_2 | 29.67 ▼ | 60.74 ▲ | 31.97 ▼ | 10.19 ▼ |
| &nbsp;&nbsp;Bentel rockers__qwen3.5-4b__Bentel_iter_3 | 36.09 ▲ | 35.75 ▼ | 37.76 ▲ | 16.60 ▼ |
| **llama-3.1-8b-instruct** | **22.19** | **49.26** | **31.24** | **22.62** |
| **qwen3.5-9b** | **33.28** | **54.53** | **41.12** | **24.91** |
| &nbsp;&nbsp;BSU-SLIM__Qwen3.5-9B-Base__playpen-prm-9b-best_of_n | 40.76 ▲ | 44.72 ▼ | 39.64 ▼ | 15.18 ▼ |
| &nbsp;&nbsp;Dialogue Architects__qwen3.5-9b__SCoRe_Qwen3.5-9B | 37.60 ▲ | 52.61 ▼ | 43.12 ▲ | 23.37 ▼ |
| &nbsp;&nbsp;LLP: Large Language Problems__qwen3.5-9b__llp-final | 54.61 ▲ | 48.63 ▼ | 59.23 ▲ | 22.09 ▼ |
| **qwen3.5-27b** | **62.34** | **70.18** | **64.34** | **43.51** |
| &nbsp;&nbsp;SLED-BSU__Qwen3.5-27B-sft-ep1__prm-ep1-best_of_n | 48.30 ▼ | 70.93 ▲ | 56.36 ▼ | 30.02 ▼ |

For full results, see [`summaries/`](summaries/):

- Absolute scores: [CSV](summaries/summary_report_absolute.csv) · [Excel
(colored)](summaries/summary_report_absolute.xlsx)
- Deltas vs. baseline: [CSV](summaries/summary_report.csv) · [Excel (colored)](summaries/summary_report.xlsx)
- Runtimes: [combined_game_timings.csv](summaries/combined_game_timings.csv)

