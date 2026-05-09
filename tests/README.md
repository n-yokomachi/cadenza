# tests/

Directory for description-triggering evals.

## Intended usage

Per skill, create `<skill-name>.eval.json` and evaluate using Anthropic's official `skill-creator` `run_loop.py`:

```bash
python -m scripts.run_loop \
  --eval-set tests/issue-finding.eval.json \
  --skill-path skills/issue-finding \
  --model <model-id> \
  --max-iterations 5 \
  --verbose
```

## Eval JSON schema (reference)

Follow the schema documented by `skill-creator`. Minimum shape:

```json
{
  "name": "issue-finding triggering eval",
  "cases": [
    { "query": "Zenn にブログ書きたいんだけど", "expected_trigger": true },
    { "query": "what's the weather today", "expected_trigger": false }
  ]
}
```

## Status

Not yet populated (placeholder). Plan: add evals once dogfooding on real projects surfaces actual triggering issues.
