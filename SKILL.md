---
name: prompt-optimizer
description: Review, rewrite, and design prompts when a user asks to improve clarity, reliability, control, context handling, output format, or testability for text, coding, image, or agent tasks. Use it also to diagnose prompt failures such as negative constraints that echo forbidden concepts, conflicting instructions, context leakage, or brittle mega-prompts. Do not invoke merely to execute an already-clear prompt without optimizing it.
---

# Prompt Optimizer

Turn the user's intent into the smallest prompt that makes the desired outcome, evidence, constraints, and acceptance conditions operational.

## Preserve the Contract

- Preserve the user's actual goal, language, named model or tool, required facts, output format, and authorization boundaries.
- Optimize the prompt; do not execute the underlying task unless the user asks for both.
- Do not silently weaken a hard constraint, add a new objective, invent capabilities, or turn a simple request into a large framework.
- Ask a question only when a missing choice would materially change the optimized prompt. Otherwise make the smallest reasonable assumption and label it only if it matters.

## Optimize by Decision, Not Ritual

Use only the parts of this loop that improve the request:

1. Extract the task contract: target outcome, audience, relevant input, source of truth, hard constraints, output shape, and success test.
2. Identify the few defects that can change behavior: ambiguity, conflicting priorities, negative-only constraints, irrelevant or sensitive context, missing evidence rules, underspecified formatting, unverifiable quality terms, repetition, or absent stopping conditions.
3. Choose the least invasive intervention: light edit, structured rewrite, clean-context rewrite, example-driven prompt, or generator-validator split.
4. Rewrite the prompt, then mentally test whether a literal model could satisfy every line while still producing an obviously bad result. Tighten only the exposed gap.

## Rewrite Principles

- State the desired destination before exclusions. Replace `do not produce X` with a concrete permitted alternative whenever possible.
- Treat negation as a constraint, not erasure: naming a forbidden concept keeps it in context. State a necessary prohibition once, pair it with the desired behavior, and avoid repeating the forbidden wording in examples.
- When background information should not influence or appear in the artifact, remove or sanitize it from the generation context. A request to "forget" prior turns is not equivalent to a fresh context.
- When delivering a clean-context rewrite for an already contaminated conversation, explicitly tell the user to run the rewritten prompt in a new conversation or stateless call without the original background. Do not imply that clean wording alone removes prior turns.
- Prefer an allowlist of content functions over a long blocklist. For example, require every README sentence to serve installation, usage, architecture, testing, limitations, contribution, or licensing.
- Separate instructions, source material, examples, and untrusted input with clear labels or delimiters when their roles could be confused.
- Use examples to demonstrate the desired pattern. Include counterexamples only when the contrast resolves a measured ambiguity and will not prime the failure being avoided.
- Convert vague adjectives into observable criteria: audience, length, sections, evidence, tone choices, schema, pass conditions, or stopping rules.
- For hard or safety-relevant constraints, recommend enforcement outside the prompt: structured outputs, deterministic validation, access control, redaction, or human review. Never present prompt wording as a security boundary.
- Keep each instruction in one place. Remove duplicated rules, ceremonial personas, threats, excessive `ALWAYS`/`NEVER`, generic requests to "think harder," and process narration that does not change the result.
- Preserve uncertainty. If source material is insufficient, specify whether to omit, ask, use a placeholder, or state the gap rather than inventing facts.

## Select the Output Form

- Default to a copy-ready optimized prompt first, followed by a short explanation of only the material changes.
- If the user asks for "prompt only," return only the optimized prompt.
- Preserve the original language unless the user asks for translation.
- Use role-separated `system` / `developer` / `user` blocks only when the target interface supports them or the user requests them. Otherwise return one self-contained prompt.
- Offer multiple variants only when they represent a real tradeoff, such as concise vs. robust or single-call vs. validated pipeline. Name the tradeoff.
- When reviewing rather than rewriting, quote the smallest problematic fragment and give a concrete replacement.

## Context and Model Boundaries

- Retain explicitly named models, tools, APIs, modalities, and platform constraints.
- If optimization depends on a current model feature or API parameter, verify it in authoritative current documentation before recommending it.
- Do not expose hidden reasoning or request private chain-of-thought. Ask for concise conclusions, checks, or evidence instead.

## Supporting Guidance

- Read [references/patterns.md](references/patterns.md) when the prompt needs a substantial rewrite, context isolation, negative-constraint repair, few-shot examples, or a multi-stage design.
- Read [references/evaluation.md](references/evaluation.md) when the user wants production reliability, A/B testing, regression coverage, automated graders, or systematic prompt debugging.
