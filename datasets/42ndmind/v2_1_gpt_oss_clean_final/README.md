# 42ndMind v2.1 gpt-oss Clean Final Dataset

This dataset converts the 42ndMind v2.0 broad scenarios into clean user-facing final-answer SFT rows.

It is designed for the next gpt-oss-20b SFT attempt after the low-RAM diagnostic adapter failed behavior evaluation.

## Purpose

The previous gpt-oss attempt trained on full-trajectory traces but generation collapsed into repeated filler and broken continuation. The likely issue was that the targets were still too trace-like and not clean final answers.

This dataset removes:

- runtime JSON dumps
- fake IDs
- symbolic/internal jargon
- over-compressed trace fragments

It trains one target behavior:

```text
Given a messy epistemic situation, provide a clean full epistemic trajectory in plain English.
```

## Counts

```text
total_rows: 50
train_rows: 40
eval_rows: 10
```

## Target Format

Each assistant answer includes:

1. contradiction detected
2. live hypotheses
3. motive candidates
4. investigation plan
5. belief update
6. calibrated conclusion

## Intended Use

Use this for a tiny sanity SFT run first. Do not jump straight to a full expensive training run.
