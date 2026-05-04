# 42ndMind v1.8 Qwen 0.5B LoRA Smoke Test

Date: 2026-05-04

## Purpose

This run tested whether `42ndAlignment` can load the `42ndMind` v1.8 chat SFT dataset and train a small LoRA adapter from it.

This was a pipeline smoke test, not a serious behavioral training run.

## Dataset

Dataset path:

`datasets/42ndmind/v1_8/combined_alignment_sft.jsonl`

Split used:

- Train rows: 8
- Eval rows: 2

Dataset format:

- Chat SFT
- Columns: `messages`, `metadata`

The dataset represents epistemic process traces from `42ndMind`, including contradiction detection, motive/context modeling, investigation planning, answer classification, and belief update.

## Model

Base model:

`Qwen/Qwen2.5-0.5B-Instruct`

Training method:

LoRA SFT

LoRA settings:

- r: 16
- alpha: 32
- dropout: 0.05
- target modules: all-linear
- epochs: 3
- batch size: 1
- gradient accumulation: 4
- learning rate: 2e-4
- max length: 1024

## Training Result

Training loss:

| Step | Loss |
|---|---:|
| 1 | 1.858336 |
| 2 | 1.636880 |
| 3 | 1.507663 |
| 4 | 1.488271 |
| 5 | 1.462892 |
| 6 | 1.237206 |

Adapter saved locally at:

`/content/42ndAlignment/artifacts/42ndmind_v1_8_qwen05b_lora`

## Interpretation

The pipeline works:

`42ndMind v1.8 export → 42ndAlignment dataset → chat SFT loader → LoRA training → adapter saved`

This does not prove meaningful epistemic internalization yet because the dataset is extremely small.

## Known Limits

- Only 8 train rows and 2 eval rows.
- Dataset is suitable for pipeline testing only.
- No serious generalization claim should be made from this run.
- Adapter should be tested against hidden contradiction scenarios before being treated as useful.

## Next Step

Generate a larger `42ndMind` export with at least:

- 50 scenarios
- 200+ SFT rows
- 200+ preference pairs
- hidden benchmark prompts

Then train a second adapter and compare it against this v1.8 smoke-test adapter.
