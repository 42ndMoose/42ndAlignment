# 42ndMind v1.8 Dataset

Source: 42ndMind v1.8 quality-gated custom scenario export.

Purpose:
This dataset trains a model on epistemic process traces, not merely final answers.

Primary SFT file:
- combined_alignment_sft.jsonl

Preference file:
- combined_preference_pairs.jsonl

Runtime process represented:
claim intake → contradiction/tension detection → hypothesis/motive modeling → investigation planning → action answer classification → belief update

Notes:
- Use SFT first.
- Preference pairs are synthetic and should be reviewed before DPO/ORPO/GRPO.
- This v1.8 export is small and should be treated as a pipeline test, not a final serious training set.
