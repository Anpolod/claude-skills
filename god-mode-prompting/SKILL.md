---
name: god-mode-prompting
version: 1.0.0
description: |
  Apply the God-Mode Prompt Formula (8 techniques) to any task requiring
  Claude to produce high-quality, non-generic output. Use when writing
  prompts, drafting content, building workflows, or improving existing prompts.
  Based on: "Unlock Claude God-Mode in 20 Minutes (ULTIMATE Claude Prompt Formula)".
  Triggers: "write a prompt for", "improve this prompt", "god-mode", "make it better",
  "why is Claude giving generic output", "apply prompt techniques".
license: MIT
compatibility: claude-code claude.ai opencode
source: NotebookLM — Мистецтво промптингу
---

# God-Mode Prompting: 8 Techniques for Maximum Claude Performance

You are a prompt engineer applying a proven 8-technique system to get
professional, non-generic results from Claude. You don't write "requests" —
you write technical specifications.

**Core principle:** The more constraints and structure you give Claude, the
better it performs. Specificity forces originality.

---

## Technique 1: The 4-Block Formula

**Rule:** Every prompt is a technical contract, not a Google search.

```
[INSTRUCTIONS]   — what to do and HOW
[CONTEXT]        — background, documents, constraints
[TASK]           — one specific deliverable
[OUTPUT FORMAT]  — exact structure you want back
```

**Without:**
> Write me a welcome email for my fitness app.
→ Generic, no hook, no personality.

**With:**
> [INSTRUCTIONS] Conversion copywriter for mobile fitness apps.
> [CONTEXT] Busy professionals 30–45. Free trial = 14 days. Key: 15-min workouts.
> [TASK] Welcome email sent 1 hour after signup.
> [OUTPUT FORMAT] Subject / Preview text / Body ≤150 words / CTA button text.

**When:** Always. This is the skeleton — other techniques add flesh.

---

## Technique 2: XML Tags

**Rule:** Wrap semantic blocks in XML tags — Claude's native language.
Anthropic uses XML in their own internal prompts.

Accuracy of following formatting rules: **61% → 94%** with XML tags.

```xml
<instructions>Senior backend developer reviewing for production readiness.</instructions>
<context>FastAPI maritime CRM, 500+ req/day, performance critical.</context>
<task>Find: security issues, performance bottlenecks, missing error handling.</task>
<code>[INSERT CODE]</code>
<output_format>## Security ## Performance ## Error Handling ## Quick Wins</output_format>
```

**When:** Any prompt longer than 3 sentences.

---

## Technique 3: Role + Specific Audience

**Rule:** Without a role, Claude writes for everyone — which means no one.

| Role | Quality |
|---|---|
| "You're a doctor" | 6/10 |
| "You're an interventional cardiologist writing for American Heart Journal" | **9/10** |

**Template:**
```
You are a [ROLE] with [N] years in [DOMAIN].
You're writing for [AUDIENCE] who [KNOWLEDGE LEVEL].
```

**When:** Whenever tone, expertise, or vocabulary matters.

---

## Technique 4: One Perfect Example

**Rule:** Don't describe style with adjectives — show it.
One reference paragraph beats five paragraphs of style description.

Contradictory adjectives ("friendly but professional") average out into bland mush.

**Without:**
> Tone: honest, direct, slightly humorous.
→ Generic tech blog, zero personality.

**With:**
> Match this style exactly:
> *"Tested the Keychron K3 for 2 weeks. Switches feel weird for exactly three days.
> Then your fingers adjust and regular keyboards feel like trampolines. Battery:
> 11 days Bluetooth. Build quality solid but not premium — it's plastic and it
> knows it's plastic. At $84, fine."*
> Now write a review of the Sony WH1000XM6.
→ Same rhythm, honesty, and humor captured from one paragraph.

**When:** Copywriting, emails, docs, "write in my style" requests.

---

## Technique 5: Prompt Chaining

**Rule:** Complex task ≠ one mega-prompt.
Break into sequential steps where each response feeds the next.

Claude holds the full chain in 200k token context — step 1 doesn't lose
weight on step 5.

**Without chaining:** 5-email sequence → 5 identical first emails.

**With chaining:**
```
Step 1: List top 5 objections a marketing director has against new analytics tools.
Step 2: Rank them by buyer journey order (first contact → decision).
Step 3: Write emails 1–2. Each addresses ONE objection. Email 2 references email 1.
Step 4: Write emails 3–5. Each advances the narrative. No repeated arguments.
```
→ A story that builds pressure — every email earns the right to the next one.

**Template:**
```
Step 1: [ANALYSIS]
Step 2: [STRUCTURE]
Step 3: [FIRST PART OF DELIVERABLE]
Step 4: [FINAL PART, references step 3]
```

---

## Technique 6: Prefill

**Rule:** Write the first few words of Claude's response yourself.
Claude continues exactly in that format — no preamble, no "happy to help".

**Without prefill:**
> Create 50 product descriptions in JSON format.
→ "I'll help you create product descriptions. Here's the first one..." [47 words of fluff]

**With prefill:**
```json
[
  {
    "id": 1,
    "name": "
```
→ Claude fills the structure directly. Zero cleanup. Same for all 50.

**Use for:** JSON / CSV / tables / lists / any rigid output format.

---

## Technique 7: Motivated Instructions ("Because")

**Rule:** Add "because" to every constraint.
Claude applies rules more precisely when it understands their purpose —
including edge cases you didn't explicitly cover.

| ❌ Command | ✅ Motivated |
|---|---|
| Don't use ellipses | Don't use ellipses **because** TTS can't pronounce them |
| Keep paragraphs short | Under 3 sentences **because** reader scans on mobile |
| No buzzwords | **because** audience is senior engineers, not marketing |

**Template:** `[RULE] because [REASON / USE CONTEXT].`

---

## Technique 8: Iterative Refinement

**Rule:** First draft = 70% quality. Use Claude as both author and editor.
Iteration is part of the process — not a sign of failure.

| Step | Action | Quality |
|---|---|---|
| Draft | Normal prompt | 70% |
| After critique | Claude reviews itself | 92% |
| After rewrite | Fixes applied | 98% |

**Time: 5 minutes. Without iteration: 30 minutes rewriting manually.**

**3-step process:**
```
Step 1 — DRAFT
Run your prompt with techniques 1–7.

Step 2 — CRITIQUE
"Critically analyze your response. Find:
- Weak transitions
- Vague claims without data
- Redundant sections
- Contradictions
List as bullet points."

Step 3 — REWRITE
"Rewrite fixing every issue you identified.
Keep what worked. Improve what didn't."
```

---

## Real-World Stacks

**Guide / Article (1500 words):**
`4-block + XML + Role + Example + Constraints + Prefill + Iteration`

**Document analysis (50 pages):**
`Chaining (data → themes → summary) + XML + Prefill + Motivated`

**Cold email sequence:**
`Chaining (objections → order → emails) + Role + Constraints + Iteration`

---

## Pre-Send Checklist

```
☐ All 4 blocks: Instructions / Context / Task / Output format?
☐ Role specific? (not "you're an expert" — "senior X with N years in Y")
☐ Style example included? (if tone matters)
☐ Task broken into steps? (if 2+ stages)
☐ Response prefilled? (if rigid format)
☐ "Because" on every constraint?
☐ Iteration planned? (critique + rewrite after first draft)
```

---

## Quick Reference

| # | Technique | Trigger |
|---|---|---|
| 1 | 4-Block Formula | Every prompt |
| 2 | XML Tags | Prompts > 3 sentences |
| 3 | Role + Audience | When tone/expertise matters |
| 4 | One Example | When style matters |
| 5 | Prompt Chaining | 2+ stage tasks |
| 6 | Prefill | Rigid output formats |
| 7 | Motivated ("because") | Every constraint |
| 8 | Iterative Refinement | Important outputs |

---
*Source: "Unlock Claude God-Mode in 20 Minutes" · April 2026*
