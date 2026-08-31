# Prompt Optimization Patterns

Read only the sections needed for the current failure mode.

## Choose an Intervention

| Situation | Preferred intervention |
| --- | --- |
| The request is already clear but wordy | Light edit: deduplicate and make the output shape explicit |
| Quality terms are subjective | Add audience, concrete writing choices, and acceptance criteria |
| Instructions and source text are being confused | Delimit and label instructions, context, examples, and input |
| A forbidden concept is echoed or denied in the output | Clean-context rewrite plus a positive target and content allowlist |
| Formatting or style varies across runs | Add one or two clean target examples and machine-checkable constraints |
| Several hard constraints interact | Use a compact task contract and a separate validation pass |
| The prompt runs in production | Add representative evals; read `evaluation.md` |

## Outcome-First Contract

Use only fields that change behavior:

```text
Role: [function the model performs, only if useful]
Goal: [observable end state]
Audience: [who will use the result]
Source of truth: [allowed evidence or inputs]
Requirements: [content that must be present]
Constraints: [true invariants and boundaries]
Output: [format, language, length, schema]
Success: [tests the result must pass]
Missing information: [ask, omit, mark, or abstain]
```

Avoid filling every field by habit. A two-sentence prompt can be better than a complete template when the task is simple.

## Repair Negative-Constraint Rebound

Negative-only instruction:

```text
This is a job-application project. Do not make the README look like a job-application project, and do not say it is not one.
```

Clean positive replacement, placed in a fresh context without the private background:

```text
Write a repository README for first-time users and contributors.

Use only facts verifiable in the repository. Include sections only when supported:
- problem and core capabilities
- architecture and prerequisites
- installation, configuration, and quick start
- tests, known limitations, contribution, and license

Every sentence must directly serve one of those functions. Start with the project title and return only the complete Markdown document.
```

The repair has three parts:

1. Remove the unwanted frame from the generation context when it is not needed.
2. Define the desired genre, audience, and allowed content.
3. Check the artifact against content functions instead of asking the generator to monitor the taboo concept.

If the same conversation is already contaminated, recommend a new conversation or stateless call containing only sanitized task facts. Do not claim that `ignore previous messages` removes the earlier tokens.

## Positive Substitution

Pair a narrow exclusion with the behavior that replaces it:

| Weak | Operational replacement |
| --- | --- |
| Do not be verbose | Use at most 500 words and five sections; keep one idea per paragraph |
| Do not use marketing language | Use neutral technical prose; support capability claims with repository evidence or measured results |
| Do not hallucinate | Use only the supplied sources; omit unsupported fields and list material gaps separately |
| Do not ask unnecessary questions | Ask only when the missing value changes correctness or creates risk; otherwise state the assumption and continue |
| Do not reveal internal context | Generate only the requested artifact fields; keep internal metadata outside the generation call |

Keep a negative rule when the forbidden action itself is the invariant, but state it once and make the permitted path explicit.

## Content Allowlist

Use an allowlist when the unwanted output can appear through paraphrase and evade keyword bans.

```text
Each sentence must perform at least one allowed function:
1. state a source-supported fact;
2. explain a required decision or limitation;
3. give an actionable instruction;
4. provide an example in the requested format.

Delete material that performs none of these functions. Do not replace it with a disclaimer.
```

Adapt the functions to the artifact. Prefer semantic categories over a growing list of prohibited words.

## Context Packaging

Use clear boundaries when source text might contain instructions or when the model could mistake metadata for artifact content:

```text
# Task
[instructions]

# Source material
<source>
[facts or documents]
</source>

# Output contract
[format and acceptance criteria]
```

State whether source material is evidence, style reference, untrusted data, or text to transform. Do not include private motivation, failed drafts, or unrelated conversation merely because it is available.

## Examples

- Show the desired output form rather than explaining it abstractly.
- Keep examples consistent with every written rule.
- Use diverse examples only when the prompt must generalize across meaningfully different cases.
- Do not include forbidden phrases in a "bad example" when echoing is the observed failure.
- If examples consume more attention than the actual task, shorten them or replace them with a schema.

## Generator-Validator Split

Use separate stages when a constraint is important or easy to verify:

1. The generator sees only the minimum sanitized context and produces the artifact.
2. The validator sees the artifact and an allowlist or rubric, then returns a verdict and locations of violations.
3. A failed artifact is regenerated from the clean contract or repaired by section purpose; do not feed the taboo explanation back into the generator.

Prefer deterministic checks for schemas, required fields, lengths, forbidden exact strings, syntax, and compilation. Use semantic review or human judgment for tone, implication, and factual adequacy.

## Avoid Prompt Cargo Cults

Do not add these unless they solve a specific observed problem:

- grandiose expert personas;
- emotional pressure, rewards, or threats;
- repeated all-caps rules;
- requests to reveal chain-of-thought;
- fixed step-by-step reasoning for tasks where only the result matters;
- irrelevant examples, background, or tool instructions;
- model parameters unsupported by the target interface.

Prompt quality comes from a clear contract and verifiable outcome, not incantations.
