# Prompt Evaluation and Regression Testing

Use this reference for production prompts or repeated failures. Do not build a full evaluation harness for a one-off wording improvement unless the user asks.

## Define Success Before Tuning

Translate the product requirement into a small scorecard. Useful dimensions include:

- task correctness;
- required-content coverage;
- constraint adherence;
- factual grounding and citation support;
- output/schema validity;
- appropriate uncertainty behavior;
- latency, token use, or tool calls when operationally important.

Separate must-pass invariants from preferences. A fluent response that violates an invariant fails.

## Build Representative Cases

Include the smallest set that covers materially different behavior:

- ordinary successful input;
- missing or ambiguous information;
- edge values and empty input;
- conflicting or irrelevant context;
- long context when production uses it;
- adversarial or untrusted text only when the application accepts it;
- known historical failures without leaking the intended fix into the test request.

Do not optimize solely against one anecdote. Preserve a holdout set when the prompt will be tuned repeatedly.

## Compare Fairly

- Run baseline and candidate prompts on the same model snapshot, settings, tools, and cases.
- Use multiple samples for stochastic outputs when a single run would be misleading.
- Change one prompt concept at a time when diagnosing causality.
- Record both quality and resource tradeoffs; a shorter prompt is better only if it preserves task success.
- Recheck after model, tool, retrieval, or policy changes.

## Layer the Graders

Prefer the most objective grader available:

1. deterministic validators for JSON/schema, required fields, syntax, compilation, exact strings, and numeric bounds;
2. source-based checks for factual support;
3. rubric-based human or model review for semantic qualities;
4. human approval for high-impact or subjective decisions.

When using an LLM grader, require a compact rubric and evidence from the candidate output. Do not assume the grader is unbiased because it is a different prompt or model.

## Diagnose Failures

Classify the failure before editing:

- **Contract gap:** the prompt never specified the needed behavior.
- **Conflict:** two instructions cannot both be satisfied or have unclear priority.
- **Context contamination:** irrelevant, private, or forbidden concepts enter generation.
- **Grounding gap:** sources or missing-information behavior are undefined.
- **Format gap:** the output shape is descriptive rather than verifiable.
- **Capability mismatch:** the target model, modality, or tool cannot reliably perform the requested operation.
- **Variance:** the prompt works sometimes but lacks stable examples, checks, or constrained structure.

Apply the smallest fix that addresses the observed class, then rerun the unchanged evaluation set.

## Stop Conditions

Stop tuning when the candidate meets the agreed pass thresholds on representative cases and no regression appears on must-pass behavior. Report residual failure modes instead of accumulating more rules indefinitely.
