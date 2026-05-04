# 42ndMind v1.9 Safe-4 Dataset

Source: 42ndMind v1.8 runtime, generated from the v1_9_safe4 scenario file.

Purpose:
This dataset expands the v1.8 smoke-test path using only scenario patterns that the current v1.8 rule-based runtime completes reliably.

Important limitation:
This is a supported-domain expansion, not a broad epistemic dataset. It intentionally avoids unsupported scenario types that currently cause the runtime to stop before combined export.

Files:
- combined_alignment_sft.jsonl
- combined_preference_pairs.jsonl
- combined_manifest.json
- excluded_scenarios.json
- scenario_summary.json
- train.jsonl
- eval.jsonl
- v1_9_safe4_scenarios.json

Run summary:
- Scenario count: 50
- Included scenarios: 38
- Excluded scenarios: 12
- SFT rows: 190
- Preference rows: 190
- Train rows: 152
- Eval rows: 38

Known issue discovered:
The broader v1.9 scenario set failed when it reached a motive-avoidance car scenario, because no investigation action was created and `answer latest` had nothing to answer. This indicates the current runtime needs a graceful no-action fallback or broader planning coverage before using broader scenarios.

Next use:
Use this dataset for a second Qwen 0.5B LoRA test. Treat it as v1.9-safe4, not as the final broad v1.9 dataset.
