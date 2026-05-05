# 42ndMind gpt-oss Harmony Final-Only Sanity Failure Note

Date: 2026-05-05

## Purpose

This note records the result of the attempted `gpt-oss-20b` sanity training run using the `v2_1_gpt_oss_clean_final` dataset, so future work does not repeat the same failure path.

## Dataset Used

Dataset path:

```text
datasets/42ndmind/v2_1_gpt_oss_clean_final/
```

Dataset intent:

```text
42ndMind v2.0 broad scenarios
→ clean user-facing final-answer SFT rows
→ no JSON targets
→ no fake IDs
→ no symbolic runtime jargon
→ no compressed trace fragments
```

The dataset itself is cleaner than the prior trace-style dataset and remains valid for later experiments.

## Runtime / Training Context

Attempted target:

```text
gpt-oss-20b LoRA SFT sanity run
```

Runtime:

```text
Colab Pro
L4 GPU
High-RAM ON
Unsloth low-RAM 4-bit setup
```

The first generic notebook produced raw Harmony-channel leakage during generation, including visible `analysis`, `commentary`, and `final`-style channel text.

A second Harmony final-only notebook was then attempted. It forced final-channel style completions in training and inference.

## Result

The Harmony final-only attempt improved the beginning of the output, but generation still collapsed.

Example failure pattern:

```text
The answer begins with a plausible contradiction analysis.
Then it degenerates into repeated words such as:
"hypothesis hypothesis hypothesis..."
```

This means Harmony formatting reduced the visible channel-leakage problem, but it did not produce stable useful generation.

## Conclusion

This run should be considered a failed diagnostic attempt, not a successful adapter.

The failure is not evidence that the Epistemic Octahedron or 42ndMind idea is invalid.

The failure means this particular `gpt-oss-20b + Unsloth + tiny v2.1 clean-final SFT` setup is not stable enough yet.

## Most Likely Causes

Likely contributing factors:

1. `gpt-oss` is sensitive to Harmony formatting and generation setup.
2. The current dataset has only 50 total rows, which is too small for robust behavior transfer.
3. The low-RAM 4-bit LoRA setup may be brittle for this model.
4. The model may still be learning answer shape without stable judgment.
5. Evaluation with the same trained model object may be unstable under Unsloth compilation/generation behavior.

## What Not To Do Next

Do not keep rerunning the same `gpt-oss-20b` notebook.

Do not increase epochs or steps on this exact setup.

Do not call the resulting adapter successful.

Do not spend more Colab compute on this path until the training and inference stack is simplified.

## Recommended Next Step

Use a stable 7B-class model to test whether the `v2_1_gpt_oss_clean_final` dataset target works independently of `gpt-oss` complexity.

Recommended fallback:

```text
Mistral-7B-Instruct-v0.3
LoRA SFT
same v2_1_gpt_oss_clean_final dataset
L4 GPU + High-RAM
```

Goal:

```text
Separate dataset quality from gpt-oss/Harmony/Unsloth instability.
```

If Mistral-7B produces clean stable outputs from the same dataset, then the dataset target is viable and the issue is specific to the `gpt-oss` stack.

If Mistral also fails, then the dataset target needs more examples or better output design before returning to larger models.

## Why The Earlier gpt-oss Run Worked Better

The earlier `42ndAlignment` `gpt-oss-20b` run was a different experiment.

It used a 300-example curated Epistemic Maturity dataset targeting final-style response discipline and scoped judgment behavior.

The current 42ndMind track uses a much smaller 50-row generated dataset derived from runtime scenarios.

So the earlier success does not automatically transfer to the 42ndMind SFT track.

The earlier adapter improved completion discipline and final-answer behavior; the current run attempted to train a deeper full epistemic trajectory behavior with much less data and a more fragile model-format stack.

## Bottom Line

The v2.1 clean-final dataset is an improvement over runtime trace targets.

The `gpt-oss-20b` Harmony final-only sanity attempt still failed generation stability.

Next serious move:

```text
Train the v2.1 clean-final dataset on Mistral-7B-Instruct-v0.3 first.
Use that result to decide whether to return to gpt-oss-20b.
```