---
name: writing-voice-skill-maker
description: >
  Creates a personalized writing voice skill by learning from the user's own writing samples
  and rewriting exercises. Use this skill when a user wants to capture their writing style,
  tone, or voice in a reusable skill — including requests like "create a skill that writes like
  me," "build a voice skill for my LinkedIn posts," "make Claude sound like me when drafting
  emails," or "/writing-voice-skill-maker." Also trigger for refinement requests like "refine my
  [skill-name] skill with more samples" or "update my voice skill." This is a metaskill: its
  output is a new installable SKILL.md file, not a piece of writing.
---

# Writing Voice Skill Maker

This metaskill guides the user through building a personalized writing voice skill. It collects
evidence (writing samples, rewriting exercises, or both), analyzes the user's style fingerprint,
and produces a `SKILL.md` file the user can install.

The output skill will apply the user's voice whenever they ask Claude to draft or rewrite content
in that domain — without them having to re-explain their style each time.

**NARRATION RULE — applies to the entire session:**
Never reference internal process labels in conversation. Do not say "Phase 0," "Phase 1,"
"Step 1a," "the process says," "per the instructions," or any similar meta-commentary. These
are internal scaffolding only. The user should experience a natural conversation, not a
guided form. All phase and step labels below are for internal orientation only.

---

## Reference files

Read these before proceeding:

- `references/ai-writing-indicators.md` — Full catalog of AI writing tells, organized by type and
  domain. Read this before constructing rewriting exercises.
- `templates/voice-skill-template.md` — Template for the generated voice skill. Read this before
  writing the output SKILL.md.

---

## Phase 0: Establish mode

**Always ask explicitly** — do not infer from the invocation phrasing. The user may have
multiple voice skills installed and needs to choose deliberately.

### 0a. New or refine?

Use the `ask_user_input` tool (do not ask as prose):

- Question: "What would you like to do?"
- Options:
  - "Create a new voice skill"
  - "Refine an existing voice skill"

If **new**: proceed to domain and skill identity.

If **refine**: proceed to identify the existing skill.

### 0b. Identify the existing skill (refinement path only)

First, try to discover installed voice skills automatically. If bash tools are available, run:

```bash
ls /mnt/skills/user/ 2>/dev/null
```

Scan the results for directories whose names suggest voice skills (contain "voice", "style",
"tone", or match known domain slugs like "linkedin", "email", "cover-letter", etc.).

**If candidates found**: use the `ask_user_input` tool to present them as selectable options.
Include the discovered skill names as options, plus "Something else — I'll paste the skill content."

**If no candidates found or bash unavailable**: ask the user to either name the skill or paste
its current SKILL.md content directly. You need the full content before proceeding to refinement.

---

## Phase 1: Domain and skill identity

### 1a. Ask for the domain

Use the `ask_user_input` tool with a multi-select question (do not present as a prose list):

- Question: "What writing context is this skill for?"
- Type: multi_select
- Options:
  - "Cover letters / job applications"
  - "LinkedIn posts / professional social"
  - "Business emails & outreach"
  - "Product docs / PRDs / specs"
  - "Long-form / blog / newsletter"
  - "Something else"

If they select "Something else", follow up conversationally to understand the domain.
If they select multiple, note this — the generated skill will need a "tone by context" section.

### 1b. Name the skill

The name is the skill's permanent identity. The user can have as many voice skills as they want —
one per domain, multiple per domain for different registers (e.g. `outreach-email-voice` vs.
`internal-email-voice`), or a single general one. Make this explicit so they understand their
options before committing.

Propose a name based on their domain selection. Use the pattern `{domain-slug}-voice`, e.g.:
- `linkedin-voice`
- `outreach-email-voice`
- `internal-email-voice`
- `cover-letter-voice`
- `prd-voice`
- `rfp-voice`
- `writing-voice` (if multi-domain general)

If the domain is narrow enough to warrant a specific name (e.g. they selected "Business emails"
but mentioned cold outreach specifically), propose the more specific name and explain why.

Ask the user to confirm or rename before proceeding. Do not continue until the name is confirmed.

### 1c. Orient the user

After the name is confirmed, give a brief orientation before jumping into evidence collection.
Always show this — it's short enough not to annoy repeat users and essential for first-timers.

Say something like:

> "Here's how this works. I'm going to learn your writing style from things you've actually
> written — not from you describing it. Once the skill is built and installed, Claude will
> write in your voice whenever it's invoked — you'll choose how that works in a moment.
>
> The process takes about 5–10 minutes. You can provide samples of your own writing, do a
> couple of rewriting exercises where I give you an AI-written passage and you rewrite it your
> way, or both. Both together gives the richest result.
>
> Ready? Let's start with a couple of quick setup questions."

Then move to evidence collection. Do not explain the samples-vs-rewrites tradeoff yet — that comes
naturally as part of the evidence menu.

### 1d. Ask triggering preference

Before moving to evidence collection, ask how the user wants the skill to trigger. Use
the `ask_user_input` tool:

- Question: "How should this skill activate?"
- Options:
  - "Automatically — whenever I'm writing [domain content]"
  - "On clear writing requests only — e.g. 'write a post' but not casual mentions"
  - "Only when I explicitly ask — e.g. '/instagram-voice' or 'use my voice skill'"

Store the answer — it determines how the skill description is written later. Do not
proceed to evidence collection until this is answered.

---

## Phase 2: Collect evidence

Evidence comes from two sources. Both are optional individually, but at least one is required.
They capture different things — and that distinction matters:

- **Writing samples** capture structure: how a piece is shaped, how it opens and closes, overall
  architecture, paragraph rhythm, block vs. fragment style. This is the only reliable source for
  structural fingerprinting.
- **Rewriting exercises** capture sentence-level voice: word choices, what the user tolerates vs.
  removes, hedging behavior, epistemic register. They do not reliably reveal structure — users
  rewriting a passage tend to fix the sentences and accept the frame.

**For LinkedIn and other short-form social content**, structural fingerprinting is especially
important — structure *is* voice in these formats. Writing samples are strongly recommended,
not just optional. Rewrites alone will produce a skill that captures the user's words but
defaults to AI post structure.

Present the evidence collection options using the `ask_user_input` tool (renders as tappable
buttons near the input area — do not present this as a prose list). Use a single-select question:

- Question: "How would you like to teach me your style?"
- Options:
  - "Paste 2–3 writing samples (things you've actually written)"
  - "Do 2–3 rewriting exercises (I give you AI text, you rewrite it your way)"
  - "Both — recommended for the richest fingerprint"

For LinkedIn and other short-form social domains, add a brief note before the question (as
your message text, not inside the tool): "For LinkedIn especially, structure is voice —
writing samples are the highest-value input here."

### 2a. Writing samples

Ask the user to paste 2–3 pieces of their own writing in the target domain. Provide guidance:

> "Paste pieces you're happy with — things that sound like you. They can be short (a few
> sentences is fine). Don't edit them for this exercise; raw is better."

Accept them one at a time or all at once. After each sample, acknowledge receipt briefly but
do not analyze aloud yet. Accumulate all samples before analyzing.

If the user wants to skip: accept it and move on. Note in the evidence log that no samples
were provided.

### 2b. Rewriting exercises

Before constructing exercises, read `references/ai-writing-indicators.md`.

**Treat rewrite exercises as an engineered probe system, not a content task.** For each
exercise, ask internally: "What style behavior is this snippet designed to elicit?" If there
is no explicit target behavior, the snippet is low-value. The quality of the AI text
determines the quality of what can be inferred.

**Exercise construction guidance** (do not share with user):

Run exactly 3 exercises, with graduated difficulty:

- **Exercise 1 — obvious**: clear AI tells, easy to spot. Goal: establish baseline correction
  behavior and build user confidence. Select 3–4 lexical/tonal indicators from the
  domain-specific cluster in `references/ai-writing-indicators.md`.

- **Exercise 2 — mixed**: combine lexical tells with at least one **Tier-1 structural probe**
  (see below). Goal: reveal whether the user edits structure, not just words.

- **Exercise 3 — subtle**: realistic AI writing that doesn't look obviously AI-written. Fewer
  obvious tells, more structural pressure. Goal: test whether inferred rules hold on text
  that looks plausible, not caricatured.

**Tier-1 structural probes** — at least one must appear in exercises 2 and 3:
- Sentence split/merge pressure (one long compound sentence that could be broken, or two
  short ones that could be merged)
- Transition scaffold ("First... Furthermore... Finally" or similar)
- List or triad rhythm (three parallel items — does the user keep, cut, or restructure?)
- Contrastive framing that invites reframing ("not just X, but Y")
- Clause-tail endings that often get rewritten ("...which is essential for success")

Why structural probes matter: lexical edits are easy and common but weak as voice signal.
Structural edits are harder to fake and more stable across topics. In short-form domains
(especially LinkedIn), structure often *is* the voice. A skill built only on word-swap
behavior is a cleanup filter, not a voice skill.

**Passage length — calibrate to domain:**
- Short-form domains (LinkedIn, social posts, emails): 3–6 sentences
- Long-form domains (cover letters, PRDs, blog posts, newsletters): 4–6 sentences drawn
  from a *single section or paragraph* — never simulate a full document. A cover letter
  exercise should be one opening paragraph, not a full letter. The goal is to probe specific
  voice behaviors, not reproduce the full task.

Keep passages tight. A shorter passage with 3–4 embedded tells produces cleaner signal than
a longer one with 10 — the user's attention stays focused and edits are less likely to be
driven by fatigue or content disagreement rather than voice preference.

**Instructions to give the user** (same for all three exercises):

> "Here's a passage written in generic AI style for [domain]. Rewrite it the way you'd
> actually write it — change whatever feels off, cut what you'd cut, add what's missing."

Do not say "gut instinct," "don't overthink it," or any similar framing. Some domains
require deliberate thought (cover letters, PRDs) and the instruction should be neutral
enough to work across all of them.

After each rewrite is received:

**Default response**: brief. One sentence acknowledging the rewrite, one sentence capturing
the single most salient pattern you noticed, and — after the final exercise — a note that
more detail is available on request. For example:

- After exercise 1 or 2: "Got it — you stripped the corporate framing and brought it closer
  to ground level. One more."
- After the final exercise: "Got all three. The clearest pattern so far: [1-sentence
  observation]. I'll write the skill now — let me know if you want to see the full breakdown
  of what I picked up before I do."

Do not list multiple changes. Do not explain your interpretation in depth. One observation,
then move on. Save the full diff analysis for 3a.

**If the user asks what you noticed** (e.g. "what did you pick up from that?"): share a
concise summary — 3–5 bullet points covering the most significant patterns. Still not the
full numbered diff. If they want the full diff, they'll ask.

The detailed diff analysis is internal work for 3a, not output for the user.

**Skipping individual exercises**: If the user says "skip this one" or "next" or similar, move to
the next exercise or offer to move on to analysis.

**Skipping all exercises**: Accept immediately, note in evidence log.

---

## Phase 3: Analyze and generate the skill

### 3a. Analysis (internal — DO NOT output any of this to the user)

**This entire section is silent internal work. Do not print headers, labels, bullet points,
or any text from this analysis to the conversation. Specifically, never output:**
- "Diff analysis:" or any per-exercise breakdown
- "Pattern summary:" or any list of observed patterns
- "Internal diff analysis:", "Not shown to user:", or any similar label
- Any numbered or bulleted list of changes the user made

**The only permitted output before the review step is the plain-English summary and the
packaged skill file. Nothing else.**

If you find yourself writing any analysis text into the conversation before 3e, stop and
delete it.

**VERBATIM RULE — applies to the entire analysis phase:**
Work from the raw text of what the user actually wrote. Do not paraphrase, reconstruct from
memory, or clean up what you think you saw. If you quote any user writing in your analysis —
even internally — copy it character-for-character: punctuation, hyphens, spacing, capitalization,
fragments, all of it. A hyphen is a hyphen, not an em-dash. An incomplete sentence stays
incomplete. Editing user writing during analysis corrupts the fingerprint before the skill is
even written — you would be encoding your voice, not theirs. Nothing is too small to preserve.

After all evidence is collected, analyze across all samples and rewrites:

**From writing samples:**
- **Structure** (samples only — do not infer from rewrites):
  - How does the piece open? Direct statement, question, story, mid-thought, fragment?
  - Does it build to the main point or open with it?
  - How does it end? Zoom out, abrupt stop, callback, question, CTA?
  - Paragraph/block shape: long blocks, short blocks, single lines, mixed?
  - Overall architecture: one idea developed, multiple ideas layered, list-driven?
  - Note: if no samples were provided, mark all structural dimensions as **unknown** in the
    evidence log. Do not infer structure from rewrites. An absent structural section is honest;
    a fabricated one actively misleads the output skill.
- Sentence length distribution — especially paragraph openers. Capture as a generative rule,
  not an observation. Not "short sentences" but "opens ≤10 words, then expands."
- Rhythm markers: Does the user land points bluntly or taper in? Use fragments? Zoom out at
  the end or stop clean? Pivot with contrast ("But..." / "The thing is...")?
- Vocabulary preferences: specific words used, words notably absent
- Tonal constants: confident vs. hedged, warm vs. direct, formal vs. casual register
- Epistemic register: does the user commit to positions, hedge them, or perform neutrality?
  This is distinct from tone — someone can write casually while being very opinionated, or
  write confidently while deliberately withholding a view. Capture which mode is their default
  and whether it shifts by context.
- How they open vs. how they close — both are fingerprint-rich

**From rewrites (diff analysis):**

**Meaning-preservation gate — apply before using any rewrite as style evidence:**
Check whether the rewrite preserved: (a) factual details — names, numbers, dates,
commitments; (b) coverage — no key intent elements were dropped. If meaning changed,
the edit may be a correction, not a voice choice. Either down-weight that rewrite for
style inference, or isolate only the edits that are clearly stylistic. Do not encode
accidental "style" rules that are actually repair behavior.

- For each AI indicator embedded: did the user change it, keep it, or transform it?
- Classify each change as: **violated rule** (they removed something the skill currently
  allows), **missing rule** (they added something the skill doesn't yet encode), or
  **overly weak rule** (they made a stronger correction than the skill would suggest)
- Pay special attention to **structural edits** — did the user split/merge sentences,
  restructure a list, remove a transition scaffold, or reframe a contrast? Structural
  changes are harder to fake and more stable voice signal than word swaps.
- What did they add that wasn't there? Additions are as diagnostic as deletions.
- What did they *not* change? Kept AI patterns reveal actual tolerance boundaries.
- Infer the underlying principle behind each change, not just the surface correction.

**Voice alignment — compare samples vs. rewrites:**
Writing samples often capture the user's *edited best self* — polished, intentional.
Rewrites often reveal their *default voice under pressure* — faster, rawer instincts.
Ask: are these consistent, or do they diverge? If they diverge, decide which the generated
skill should bias toward and note the decision in the evidence log. Do not silently average them.

**Conflict resolution:**
Voices often contain apparent contradictions (concise but sometimes expansive; direct but
gentler in sensitive contexts). Do not flatten these into vague adjectives. For each apparent
contradiction, define the trigger: "X by default; shifts to Y when [condition]." Encode the
condition explicitly in the generated skill.

**Calibrate confidence**: flag anything inferred vs. directly observed. If you only have rewrites,
structural patterns are less certain. If you only have samples, AI tells haven't been stress-tested.

**Generation test (internal question before writing the skill):**
Would this skill produce the user's voice when given only bullets or a short brief — not AI
text to rewrite? If the answer is uncertain, identify which principles need to be strengthened
as generative instructions (how to construct, not just what to filter).

### 3b. Write the skill

Read `templates/voice-skill-template.md` now if you haven't already.

Fill in the template. Key rules:

- **Be specific, not generic.** "Avoids hollow intensifiers" is generic. "Replaces 'incredibly
  important' with a direct statement or cuts it entirely" is specific.
- **Anchor claims to evidence.** Don't write a trait you didn't observe. If you're inferring,
  mark it in the evidence log.
- **Voice summary**: should read like a sharp character description from a copyeditor who knows
  this person — not adjectives that could apply to anyone ("direct, confident, professional").
  Something like: "Writes like someone who's done the work and doesn't need to prove it. Opens
  short, expands. No ceremony." Push for that level of specificity.
- **Numbered principles with one-liner rules**: each principle starts with a crisp imperative
  the skill can apply without reading further. Follow with ❌/✅ pairs from the evidence.
  At least one principle must encode rhythm as a generative rule, not an observation.
  Apparent contradictions get resolved with explicit triggers, not averaged into vague adjectives.
- **Anti-patterns section**: populate hard bans from evidence, soft watchouts from conditional
  patterns, domain slop from the indicators reference. This is a first-class filter layer —
  keep it separate from principles, not folded in.
- **Rule hierarchy**: fill in all four tiers. Non-negotiables should be the 1–4 things that,
  if violated, would make the user say "that doesn't sound like me at all."
- **"What this voice is not"**: name the closest failure mode by name and contrast it sharply.
  This is often the most useful section for preventing default drift.
- **Trigger behavior**: write the skill description based on the preference captured in 1d:
  - *Automatic*: "Use this skill whenever {user} is writing {domain content}. Trigger
    automatically on any request to write, draft, or rewrite {domain noun} — do not wait
    to be asked explicitly."
  - *Clear writing requests*: "Use this skill when {user} makes a clear request to write
    {domain content} (e.g. 'write a post,' 'draft an email'). Do not trigger on casual
    mentions, questions about {domain}, or discussions that don't involve producing content."
  - *Explicit only*: "Only use this skill when {user} explicitly invokes it — e.g.
    '/{skill-name}', 'use my {skill-name} skill', or 'write this in my voice'. Do not
    trigger automatically."
  voices when that is the user's real style. Do not reward casualization by default.
  "Human-like" and "like this specific person" are not the same objective. If the evidence
  shows a restrained, precise, or formal voice, encode that — do not soften it toward
  a generic personal-brand tone.
- **Pre-output checklist**: derive items from evidence. Frame as active questions/checks.
- **Examples**: use excerpts from the user's actual samples where possible. Do not fabricate.
  **VERBATIM RULE**: copy character-for-character — do not substitute typographic equivalents
  (a hyphen is not an em-dash), do not smooth phrasing, do not fix punctuation, do not upgrade
  word choices. If you find yourself improving the user's writing before putting it in the
  examples section, stop. That improvement is the failure. The user's actual choices are the
  signal — editing them, even subtly, destroys it.

### 3c. Held-out validation (internal — before presenting to user)

Before showing the skill to the user, mentally generate short outputs from these four prompts
using only the skill as written — no conversation history, no evidence in context:

- **Detailed brief**: e.g. "write a follow-up email to a recruiter who just scheduled a call"
- **Sparse bullets**: e.g. "follow up: call scheduled, excited, confirm time"
- **Intent only**: e.g. "I need to follow up on yesterday's call"
- **Non-obvious prompt**: a realistic request that doesn't look like a rewrite exercise —
  something the user might actually type in day-to-day use, not something that screams
  "test the voice skill." E.g. "help me respond to this LinkedIn comment" or "write a quick
  update to my team about the project delay."

Ask: does the output still sound like this person across all four, or does it drift toward
generic AI writing on the harder prompts?

If the sparse-brief or non-obvious output drifts, the skill is primarily a rewrite filter.
Identify which principles lack generative specificity and strengthen them before presenting.

**Internal decision gate:**
Ask: "If I removed all anti-AI cleanup rules, would this still produce the user's voice from
sparse bullets?" If no, the skill needs stronger generative structure and rhythm rules before
it's ready.

### 3d. Self-audit before presenting

Before showing the skill to the user, run this internal check:

**Verbatim evidence check**: Compare every quoted example in the skill against the raw user
writing in this conversation. If any word, punctuation mark, or phrasing differs from what
the user actually wrote — even a hyphen converted to an em-dash, even a smoothed phrase —
fix it or remove it before presenting. The pull to polish is strong and has to be actively
resisted. Check character-for-character.

**Example consistency**: Read each example against the principles listed above it. If any
example contains a behavior that a principle prohibits, fix one or the other before presenting.
An example that contradicts a stated principle trains the skill in two opposing directions.

**Voice summary specificity**: Would the voice summary describe only this person, or could it
describe hundreds of professional writers? If the latter, rewrite it.

**Checklist derivation**: Is each checklist item traceable to something observed in the
evidence? If any item is generic, replace it with something specific to this user or cut it.

**Dispute protocol**: If the user challenges an example or a stated trait after the skill is
presented, go back to the raw evidence in this conversation before responding. Do not defend
the skill from memory. Check first. Rationalization — searching for a way to be right rather
than checking — is a named failure mode here.

### 3e. Present the skill for review

Do not show the raw SKILL.md. Do not generate a sample output.

Write a plain-English summary of what the skill captured — short enough to scan in 10 seconds.
Cover: the core voice character (1 sentence), the hard bans (the 2–4 things that are never
allowed), and any notable domain-specific patterns. Nothing else.

Format it simply, for example:

> "Here's what the skill captured:
>
> **Voice**: [1-sentence character description — specific, not generic]
> **Hard bans**: [2–4 specific things — words, constructions, patterns]
> **Notable**: [1–2 domain-specific patterns worth flagging, if any]
>
> Anything off? If not, I'll package it now."

If the user confirms or doesn't flag anything, package immediately and move to install
instructions. If they flag something, fix the skill and re-summarize only what changed.
Do not re-show the full summary unless the user asks.

If the user wants to see the raw skill file at any point, show it on request.

---

## Phase 4: Refinement

Triggered when the user wants to improve an existing voice skill.

### 4a. Determine what the existing skill contains

Ask the user to paste the current SKILL.md content (or read it if accessible). You need to
know what's already in it before changing anything.

### 4b. Ask what kind of refinement they want

> "How would you like to refine it?
> A) **Add samples** — paste more writing you've done; I'll sharpen the fingerprint
> B) **Add rewrites** — I give you an AI passage, you rewrite it; most diagnostic option
> C) **Patch a specific failure mode** — the skill is producing something specific that's wrong
> D) **Both A and B**
>
> If you've noticed the skill producing writing that feels off in a specific way, option C is
> usually the fastest fix."

**Option C — patch mode.** Ask the user to describe the failure. Offer examples to help them
identify it:

> "What does it sound like when it goes wrong? A few common ones:
> - Too polished / too "AI-clean"
> - Too long — over-explains before getting to the point
> - Too much transition glue ("Furthermore," "Additionally")
> - Sounds professional but has no stance
> - Not enough of my actual rhythm — reads smooth but generic
> - Something else?"

Once identified, target the patch surgically: find the rule (or missing rule) responsible and
fix that specific tier in the hierarchy — don't rebuild the whole skill.

### 4c. Rewrite-diff as primary refinement engine

For options B and D, rewrite evidence is the highest-value signal because it reveals the exact
corrections the skill failed to make. Apply the meaning-preservation gate first (same as 3a):
check that factual content was preserved before treating an edit as style evidence.

Process each rewrite as a structured diff:

For each change the user made:
1. **Classify**: is this a *violated rule* (skill allows something they removed), a *missing rule*
   (they added something the skill doesn't encode), or an *overly weak rule* (they made a stronger
   correction than the skill would produce)?
2. **Locate**: which tier of the rule hierarchy does this affect — non-negotiable, strong tendency,
   context shift, or cleanup?
3. **Mutate**: strengthen, add, or demote the relevant rule at the correct tier. Do not append
   the observation as a new rule — mutate the existing one. Rule bloat and internal contradiction
   are the main ways voice skills degrade over time.

**Hard-rule gate**: before promoting any rule to non-negotiable, check that it appears in
multiple pieces of evidence, not just a single rewrite. One onboarding rewrite is not enough
to hard-ban something. Voice modeling is probabilistic — early certainty is often wrong certainty.

Pay equal attention to what the user *did not change*. Kept AI patterns are evidence of actual
tolerance, and the skill should reflect that.

### 4d. Decide how to incorporate (for A and D)

After collecting new samples, ask the user how to handle conflicts:

> "Before I update the skill — how should I handle new evidence?
> - **Append**: add new patterns alongside existing ones
> - **Recalibrate**: weight new evidence more heavily (treat as more current)
> - **Replace**: rebuild the fingerprint from new evidence, keep the structure
>
> If anything contradicts the existing skill, I'll flag it first."

### 4e. Version the skill

Before overwriting, save the current version as `{skill-name}-v{N}.md` in the output. Tell
the user:

> "I've saved your previous version as `{skill-name}-v{N}.md` in case you want to roll back."

Then produce the updated skill and present both files.

---

## Packaging

Once the user approves the skill:

1. Create the skill directory: `{skill-name}/SKILL.md`
2. Package it using the skills packaging convention (if `package_skill.py` is available, run it;
   otherwise create the directory structure manually)
3. Present the `.skill` file for download
4. Follow with a brief install and usage reminder (see below)

If packaging tools aren't available in this environment, save the SKILL.md to
`/mnt/user-data/outputs/{skill-name}/SKILL.md` and tell the user to install it manually.

### Install and usage reminder

After presenting the file, always add:

> "**To install**: go to Settings → Skills in Claude.ai and upload this `.skill` file.
>
> **To use**: once installed, just ask me to write a [LinkedIn post / email / etc.] and I'll
> apply your voice automatically. You can also say 'write this in my voice' or 'make this
> sound like me' and it will trigger the skill.
>
> If the output ever doesn't sound right, you can refine the skill anytime — just say
> '/writing-voice-skill-maker refine my {skill-name} skill' and we'll patch whatever's off."

---

## Quality bar for the generated skill

The generated voice skill should pass this test: if another instance of Claude reads only the
skill (no conversation history, no memory), it should produce writing that the user would
recognize as sounding like themselves — from both a detailed brief *and* sparse bullets.

A skill that only lists AI tells to avoid is a **baseline**, not a voice skill. A real voice skill
captures what the person *does*, not just what they don't do. Every ❌ needs a ✅.

Signs the generated skill is too generic or incomplete:
- Voice summary uses adjectives like "direct," "clear," "professional" without specifics
- Principles list only what to avoid, with no ✅ counterpart showing what to do instead
- No rhythm principle — or rhythm described as observation rather than generative rule
- Apparent contradictions flattened into vague adjectives instead of trigger-encoded
- Anti-patterns section is missing or just copies the indicators reference wholesale
- Rule hierarchy is missing — all rules treated as equally important
- "What this voice is not" section is missing or names a failure mode without contrasting it
- Checklist items are generic, not derived from this user's evidence
- No examples derived from the user's actual writing
- An example contradicts a stated principle
- Tone-by-context table is missing when the domain clearly has sub-contexts
- Skill passes rewriting tests but drifts generic when generating from scratch

If you find yourself writing a generic skill, go back to the evidence and look harder.
