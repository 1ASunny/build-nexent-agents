---
name: build-nexent-agents
description: Develop, debug, validate, document, and release reliable Nexent Agents and Progressive Skills. Use when working on Nexent Agent configuration, uploaded-file tool orchestration, Skill packaging, model or Docker/WSL failures, end-to-end browser testing, demo evidence, Agent export, or GitHub Show and tell publication.
---

# Build Nexent Agents

Build Nexent-native Agents as observable workflows rather than prompt-only demos. Separate tool evidence extraction, Skill rules, Agent reasoning, deterministic calculations, and platform responsibilities.

## Start from the requested outcome

1. Inspect the target repository, Nexent version, current Agent configuration, available tools, Skills, model, tests, and runtime status.
2. Preserve Nexent-native upload, chat, model access, Markdown output, and Agent export unless the user explicitly requests a standalone application.
3. Define the acceptance evidence before implementation: visible tool trace, required output contract, arithmetic or schema checks, safety disclosures, screenshots, export artifact, and CI status.
4. Read [references/scholar-evaluation-lessons.md](references/scholar-evaluation-lessons.md) when the task involves file analysis, repeated connection failures, self-verification loops, demo preparation, or public release.

## Design the Agent contract

Assign one responsibility to each layer:

- Tool: extract or transform evidence; do not make conclusions outside its contract.
- Skill: define domain workflow, fixed terminology, rubrics, safety boundaries, and error handling.
- Deterministic script: calculate scores, normalize weights, validate schemas, or package artifacts.
- Agent prompt: enforce call order, connect evidence to reasoning, and format the final response.
- Nexent: own upload, conversation, model access, UI, Markdown export, and official Agent export.

For uploaded files, specify the exact executable first action. Require a successful parse result before scoring or claiming the file was read. Prevent placeholder URLs, repeated permission requests, and exposure of internal object-storage or presigned URLs.

## Implement progressively

1. Establish the smallest compatible model and tool call.
2. Package the Skill in Nexent's required archive layout.
3. Configure the Agent with explicit dependencies, step limit, summary setting, and self-verification policy.
4. Add deterministic validation for fragile invariants such as exact dimension names, required fields, allowed score ranges, weight normalization, and aggregate arithmetic.
5. Test a synthetic input before a real document.
6. Run a fresh end-to-end conversation and verify the execution trace, not only the prose result.

Do not treat self-verification as a substitute for execution. If verification reports that a required tool was not called, fix the orchestration so the next attempt performs the call instead of merely describing it.

## Diagnose by layer

When a run fails, classify the failure before editing prompts:

1. Conversation state: stale or failed session, wrong Agent, wrong model, missing upload.
2. Agent orchestration: promise instead of tool call, wrong call order, exhausted step limit, verification loop.
3. Tool and Skill: missing dependency, invalid package, parse error, placeholder or incorrect file URL.
4. Model gateway: incompatible parameters, authentication, timeout, rate limit, repeated connection errors.
5. Infrastructure: unhealthy containers, Docker or WSL memory pressure, Elasticsearch heap, Ray concurrency.

Collect concrete evidence from the UI trace, service status, resource usage, and minimal authenticated model calls. After an infrastructure or orchestration failure, use a new conversation for the regression test.

## Validate with independent gates

Require proportionate checks across these surfaces:

- Unit and contract tests for Skill structure and deterministic logic.
- Package validation and reproducible archive contents.
- Model-gateway smoke tests with the exact parameters Nexent sends.
- Container health, web HTTP status, and resource headroom.
- Real end-to-end input with the expected tool trace.
- Independent recomputation of numerical results.
- Safety checks for fabricated evidence, guarantees, secrets, credentials, and internal URLs.
- Manual browser runbook with named screenshots and explicit pass/fail criteria.

Report passed, failed, and untested scenarios separately. Never promote a supplied prompt or an uncaptured scenario into claimed execution evidence.

## Prepare release evidence

Create only evidence-backed artifacts:

- concise setup and troubleshooting documentation;
- exact Agent configuration without credentials;
- test plan and manual demo runbook;
- real screenshots showing identity, upload, tool trace, output, and limitations;
- exported Markdown report and validation record;
- official Nexent Agent export with checksum;
- CI workflow, license, attribution, security notes, and secret scan.

Exclude unpublished source documents unless redistribution is authorized. Exclude API keys, passwords, internal S3 URLs, presigned URLs, ephemeral HTML metadata, and misleading screenshots from failed runs.

## Communicate the result

Lead with the verified outcome and current version. State the actual call chain, test counts, runtime health, arithmetic checks, limitations, and remaining untested cases. For a development retrospective, organize the answer as architecture, failures and root causes, fixes, validation, release discipline, and reusable lessons.
