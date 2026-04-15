# Voice Skill Output Template

When you have finished collecting evidence (samples and/or rewrites), use this template to write the generated SKILL.md. Fill in every section based on your analysis. Remove any sections you cannot populate due to insufficient evidence — do not pad with generic statements.

---

```
---
name: {skill-name}
description: >
  {Choose the variant based on the triggering preference captured during skill creation:}

  {AUTOMATIC — triggers on any relevant writing request:}
  Write {domain-description} in {user}'s personal voice and style. Use this skill whenever
  {user} is writing {domain content}. Trigger automatically on any request to write, draft,
  or rewrite {domain noun} — do not wait to be asked explicitly. Also trigger when they say
  "write this in my voice," "make this sound like me," or "how would I say this."

  {CLEAR WRITING REQUESTS — triggers on explicit writing tasks, not casual mentions:}
  Write {domain-description} in {user}'s personal voice and style. Use this skill when {user}
  makes a clear request to write or draft {domain content} (e.g. "write a post," "draft an
  email," "help me write this"). Do not trigger on casual mentions, questions about {domain},
  or discussions that don't involve producing content.

  {EXPLICIT ONLY — only triggers when directly invoked:}
  Write {domain-description} in {user}'s personal voice and style. Only use this skill when
  {user} explicitly invokes it — e.g. "/{skill-name}", "use my {skill-name} skill", or "write
  this in my voice." Do not trigger automatically on writing requests.
---

# {Skill Name}

{1–2 sentence summary of the user's voice. Be specific — not "direct and engaging" but something like "Writes like someone who has done the work and doesn't need to prove it. Short sentences to open, then expands. No ceremony."}

---

## Voice Principles

{Number each principle. Start with a one-line imperative rule — sharp enough to apply without reading further.
Then add ❌/✅ pairs grounded in the user's actual evidence. Every principle needs both sides.
4–7 principles is the right range; fewer is fine if the evidence is thin.

**Apparent contradictions**: voices often contain real tensions — concise but expansive when explaining,
direct but not blunt in sensitive contexts. Do not flatten these into vague adjectives. Instead, encode
the trigger: "Direct by default. When the topic requires care, softens the landing but still lands."
Define the condition, not just the trait.

**Rhythm as a first-class principle**: at least one principle should describe sentence/paragraph rhythm
in terms that generate, not just describe. Not "short sentences" but "opens with ≤10 words, then
expands — never three long sentences in a row." Capture: how paragraphs open, whether the user tapers
in or lands points bluntly, how they use fragments, whether they zoom out at the end or stop clean.

**Structural principles require samples**: do not write principles about post/piece structure
(how it opens, how it ends, overall shape, paragraph architecture) unless samples were provided.
If structural fingerprint confidence is Partial or Unknown, omit structural principles entirely
rather than guessing. A missing section is honest. A wrong structural principle will cause the
output skill to generate AI-shaped content that happens to use the user's words.}

### 1. {One-line imperative rule, e.g. "Open paragraphs short — expand after."}
{1–2 sentences of context if needed. Then:}
- ❌ {what they avoid, with a concrete example from evidence or a close paraphrase}
- ✅ {what they do instead, equally concrete}

### 2. {One-line imperative rule}
- ❌ ...
- ✅ ...

### 3. {One-line imperative rule}
- ❌ ...
- ✅ ...

{Continue for observed principles. Do not invent principles not supported by evidence. Do not pad
to reach a target count. Mark any principle in the evidence log if it was inferred rather than
directly observed.}

---

## Anti-patterns

{This section makes the anti-slop layer explicit and first-class. Do not merge it into the
principles above — keep it separate so the generated skill can run it as a distinct filter pass.

Organize by confidence level. Only include patterns actually observed in evidence or strongly
implied by the domain + samples. Do not copy the entire AI indicators reference here.}

**Hard bans** — never appear in this voice, no exceptions:
- {specific word, phrase, or construction observed to always be removed — e.g. "utilize," filler affirmations like "Certainly!", em-dash stacking}

**Soft watchouts** — occasionally acceptable but check before leaving in:
- {pattern that appeared in rewrites as sometimes-changed, sometimes-kept — infer the condition}

**Domain-specific slop** — AI tells that cluster in this domain:
- {1–3 patterns most common in the user's specific domain, from the indicators reference}

{If the user's voice is primarily defined by what they cut rather than exotic positive traits,
this section may contain more signal than the principles above. Weight accordingly.}

---

## Rule hierarchy (runtime priority)

{Organize the skill's rules into four tiers so the generated skill knows what to do when
principles pull in different directions. Be explicit — do not leave this implicit.}

**Non-negotiables** (always apply, override everything else):
- {1–4 hard constraints — things that are never negotiable regardless of context or prompt}

**Strong tendencies** (default behavior, apply unless a context rule overrides):
- {3–6 default patterns that define the voice in most situations}

**Context-dependent shifts** (apply only in specific sub-contexts):
- {e.g.: "Warm follow-up: light opener is OK" / "Internal comms: more casual register permitted"}

**Final cleanup** (run after drafting, before output):
- {same items as pre-output checklist — duplicate here so the hierarchy is self-contained}

---

## What this voice is not

{Contrastive encoding. Name the closest failure mode — the voice this is most often mistaken
for — and draw a sharp line. This is one of the highest-value sections for the generated skill
because it corrects the model's default drift.}

**Closest failure mode**: {e.g. "polished corporate professional" / "friendly AI assistant" / "enthusiastic founder"}

**How it differs**:
- ❌ {what the failure mode does}
- ✅ {what this voice does instead}
- ❌ {second contrast}
- ✅ {second contrast counterpart}

{1–2 sentences capturing the essential distinction in plain language.}

---

## Pre-output checklist

{Run this checklist on every draft before finalizing. Frame as things to actively check, not just
a reading list. Derive each item from the user's actual evidence — specific tells they removed,
patterns they corrected, or habits observed in samples. 5–8 items is the right range.}

- [ ] {Check 1 — written as a question or action, e.g. "Any hedges to cut? ('potentially,' 'could possibly,' 'I believe')"}
- [ ] {Check 2}
- [ ] {Check 3}
- [ ] {Check 4}
- [ ] {Check 5}
- [ ] {Check 6}

---

## Examples

{Include 1–3 example outputs derived from the user's actual samples. If samples were full pieces, use excerpts. Label each by context type. Do not fabricate new examples — use evidence.}

### {Context type, e.g. "Cold outreach" / "Product spec intro" / "LinkedIn post"}
> {excerpt from or paraphrase of user's sample, reformatted as a reference example}

---

## Tone by context

{Include this table whenever there are 2 or more distinct sub-contexts within the domain — even
for a single-domain skill. E.g., an email skill almost always has: cold outreach, warm follow-up,
scheduling/admin, internal. A LinkedIn skill might have: original posts, comments, profile copy.
Only omit if the domain genuinely has no meaningful sub-context variation.}

| Context | Tone notes |
|---|---|
| {sub-context 1} | {notes} |
| {sub-context 2} | {notes} |

---

## Evidence log (internal — not for output use)

{This section is for the skill to self-document how it was built. Strip before sharing if the user requests a clean version.}

- Samples provided: {count and brief description — note whether these are polished/edited or raw}
- Rewrite exercises completed: {count, which tells were embedded per exercise}
- Structural fingerprint confidence: {one of:
  - **High** — 2+ samples provided with clear structural patterns
  - **Partial** — 1 sample, or samples were very short; structure partially observable
  - **Unknown** — rewrites only, no samples; structural principles not encoded in this skill}
- Refinement rounds: {list dates/descriptions if refined}
- Inferences made (not directly observed): {flag anything inferred rather than explicitly seen in evidence}
- Voice alignment: {did samples and rewrites produce a consistent fingerprint, or do they diverge?
  If they diverge — e.g. samples are more polished, rewrites reveal faster/rawer defaults — note
  which the generated skill biases toward and why. If uncertain, note that too.}
- Generation confidence: {would this skill still produce the user's voice from sparse bullets or
  a short brief, or is it primarily a rewrite filter? If the latter, flag principles that need
  strengthening for generation tasks.}
```
