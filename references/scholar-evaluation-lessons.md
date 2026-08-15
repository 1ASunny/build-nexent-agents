# Scholar Evaluation case study

## Validated baseline

- Project: `D:\Code\scholar-evaluation`
- Platform: Nexent v2.4.0
- Released Agent: Scholar Evaluation `v0.1.7-demo`
- Model: `gpt-5.3-codex`
- Required call chain: `read_skill_md` then `analyze_text_file`
- Runtime: 13 containers; Nexent web HTTP 200
- Automated checks: 12 passed
- Model smoke test: three consecutive authenticated HTTP 200 completions
- Real-paper weighted result: independently matched at `4.655/5.00`
- Public release commit: `648623f0329917c650b66b01d022183c34b55f96`

## Architecture lesson

Keep document parsing, evaluation policy, numerical aggregation, and platform behavior separate. The file tool extracts a compact evidence ledger without scoring. The Skill defines the eight-dimension `/5` rubric and safety rules. The Agent reasons from returned evidence. A deterministic script validates weights, `N/A` normalization, score bounds, and totals.

## Failure sequence and root causes

The early Agent repeatedly announced that it would start the review but did not execute file analysis. Self-verification correctly rejected the incomplete answer, but the retry again described the missing action instead of performing it. Prompt hardening made the first uploaded-file response executable tool code and prohibited natural-language output before both calls returned.

Repeated `Connection error` messages were not evidence of a bad evaluation prompt. They coincided with Docker/WSL memory pressure and model connectivity. Recovery required infrastructure checks and model-gateway smoke tests before another browser run.

One model was unsuitable for the demo because Nexent v2.4.0 sent `temperature=0.1` while that endpoint accepted only its default temperature. Compatibility must be tested using the exact request shape emitted by Nexent.

## Validated Windows demo-host tuning

These values solved the observed 16 GiB Windows host pressure; treat them as case-specific, not universal requirements:

- WSL2 memory limit: 10 GiB
- WSL2 swap: 8 GiB
- Elasticsearch heap: `-Xms512m -Xmx1g`
- Ray: 2 CPUs and single processing concurrency
- Keep roughly 2 GiB of Docker/WSL memory headroom before a PDF run

If Docker commands hang or containers restart, stop treating the run as an Agent test. Recover the runtime, verify every container, perform minimal model completions, then start a new conversation.

## Acceptance contract

For the real-paper test, require:

- visible `read_skill_md` and `analyze_text_file` calls;
- all eight exact evaluation dimensions exactly once;
- `/5` scores and weights `15/15/20/10/15/10/10/5`;
- independently correct weighted arithmetic;
- evidence anchors from the parsed document;
- explicit document-only and external-validation limitations;
- an acceptance/publication non-guarantee;
- no internal S3 or presigned URL in the final answer.

For proposal tests, use `N/A` only when conceptually inapplicable and renormalize remaining weights to 100%. Missing applicable evidence is a weakness, not `N/A`.

## Release lesson

Treat screenshots, exported reports, checksums, documentation, licensing, attribution, and secret scans as part of delivery. Use Nexent's official Agent export rather than a hand-authored substitute. Distinguish executed evidence from reproducible but not yet captured test prompts.
