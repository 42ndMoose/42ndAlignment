# 42ndMind v1.9-safe4 Blocker Note

Date: 2026-05-04

## Purpose

This note records the conclusion reached during the `42ndMind` → `42ndAlignment` v1.9-safe4 work, so future sessions do not restart from the wrong place.

The important conclusion is:

> Do not keep expanding narrow `42ndAlignment` adapters before patching `42ndMind`. The next real work should start in `42ndMind`, because the current blocker is a runtime failure in broader scenario handling.

## What Was Done

A larger v1.9 scenario export was attempted from the `42ndMind` v1.8 runtime.

The first broad scenario set included wider motive, deception, scoping, and context patterns. The batch runner began creating scenario folders but stopped before producing combined dataset files.

A narrower supported-domain dataset was then created as `v1_9-safe4`. This dataset intentionally uses only scenario patterns that the current v1.8 rule-based runtime can complete reliably.

The resulting dataset is stored under:

```text
datasets/42ndmind/v1_9/
```

It produced:

```text
scenario_count: 50
included_scenarios: 38
excluded_scenarios: 12
sft_rows: 190
preference_rows: 190
train_rows: 152
eval_rows: 38
```

This was sufficient for a second Qwen 0.5B LoRA SFT test, but it should not be mistaken for a broad v1.9 dataset.

## Blocker Found

The broader v1.9 scenario set failed when it reached a motive/deception-style scenario.

The specific failure pattern was:

```text
scenario creates no investigation action
→ scenarioRunner still calls answer({ actionSelector: "latest" })
→ no latest action exists
→ batch runner crashes
→ no combined_alignment_sft.jsonl is produced
→ no combined_preference_pairs.jsonl is produced
→ no scenario_summary.json is produced
```

The core bug is not in `42ndAlignment`. It is in `42ndMind` runtime behavior.

## Why This Matters

`42ndAlignment` can already consume the exported chat SFT data and train a LoRA adapter. That part works.

The limiting factor is now upstream:

```text
42ndMind cannot yet reliably process broader motive, deception, scope, and context scenarios without crashing.
```

Therefore, the next major work should not be another narrow adapter first. It should be a `42ndMind` patch.

## Recommended Next Work

Start in `42ndMind`, not `42ndAlignment`.

Patch the runtime so broad scenarios do not crash when no investigation action is generated.

Recommended behavior:

```text
If no investigation action exists:
  do not call answer latest blindly
  record a no_action_available / no_followup_required / needs_review state
  allow the scenario to finish
  either export it as lower-quality, excluded, or needs-review
  continue the batch instead of killing the whole run
```

The likely patch area is the scenario execution path around:

```text
plan
→ next
→ answer({ actionSelector: "latest" })
```

The runner should check whether `next` actually produced an actionable investigation action before trying to answer it.

## Minimum Patch Target

A good v1.9 runtime patch should make these scenario types complete without crashing:

```text
motive ambiguity
possible deception
scope clarification
partial truth
deleted-message / reputation-management scenario
timeline contradiction
mistaken accusation
ideological self-sealing
```

Not all of those need to pass quality gates immediately. The key requirement is that they should not crash the batch runner.

## After the Patch

After `42ndMind` handles no-action cases gracefully:

1. Generate a true broad `v1_9` or `v2_0` scenario dataset.
2. Export combined SFT and preference rows.
3. Put the new dataset under:

```text
datasets/42ndmind/v2_0/
```

4. Train the next adapter in `42ndAlignment`.
5. Compare:

```text
base Qwen 0.5B
vs v1_8 smoke-test adapter
vs v1_9-safe4 adapter
vs post-patch broad adapter
```

## Bottom Line

`v1_9-safe4` is a useful bridge dataset and proves the `42ndMind` → `42ndAlignment` training path.

But the next serious improvement is not more safe-domain repetition.

The next serious improvement is making `42ndMind` handle broader motive/deception/scoping scenarios without crashing.