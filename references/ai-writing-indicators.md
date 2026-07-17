# AI Writing Indicators Reference

Use this list to construct rewriting exercises. Choose 3–5 indicators that are most common in the user's target domain and embed them naturally in the exercise passage. After the user rewrites the passage, compare their version to infer how they handle each indicator.

**Era awareness**: AI vocabulary and tics shift with each model generation. When constructing exercises, favor *current-generation* tells over stale ones — the goal is to probe against AI text the user actually encounters today, not 2023 caricatures. Era notes are marked throughout. A user who has never seen "delve" in the wild can't give you meaningful signal by deleting it.

**Calibration warning**: not everything that feels "AI-ish" is. Before treating any pattern as an AI tell, check the **Signs of human writing** and **Ineffective indicators** sections at the bottom. Several patterns commonly assumed to be AI tells (formal prose, perfect grammar, transition words in isolation) are unreliable — and some "flaws" (wordy constructions, casual hedges, blunt superlatives) are actually human fingerprints to *preserve*, not fix.

---

## Lexical Tells (word-level)

### Hedge stacking
AI overloads sentences with formulaic uncertainty qualifiers even when a direct statement would suffice.
- Indicators: "potentially," "could possibly," "might be able to," "it seems like," "arguably," "in many ways"
- Domain skew: heavy in professional/business writing, product docs
- Note: distinguish from *human* hedging ("very," "perhaps," "tends to," "I think"), which is casual, irregular, and a sign of human writing — see Signs of human writing below. AI hedging is ceremonial; human hedging is conversational.

### Filler affirmations
AI acknowledges the question before answering it, or completes a task with congratulatory framing.
- Indicators: "Certainly!", "Absolutely!", "Great question!", "Of course!", "That's a fantastic point"
- Domain skew: heavy in conversational/email contexts

### Hollow intensifiers
Words that signal importance without adding information.
- Indicators: "very unique," "truly exceptional," "incredibly important," "remarkably insightful," "deeply committed"
- Domain skew: LinkedIn posts, cover letters, marketing copy

### Overused AI vocabulary
Words AI reaches for disproportionately. **These co-occur — where there is one, there are likely others.** One or two may be coincidence; a cluster is one of the strongest tells.
- Persistent across eras: "underscore," "highlight" (as verbs), "enhance," "showcase," "emphasize," "robust," "leverage," "foster," "facilitate," "utilize," "seamlessly," "landscape" (abstract), "key" (adjective), "pivotal," "crucial"
- Earlier-era (2023–2024, now declining — still useful for spotting older AI text, weaker as exercise probes): "delve," "tapestry" (abstract), "testament," "intricate," "interplay," "meticulous," "garner," "boasts," "vibrant," "bolstered," "enduring"
- Current-era emphasis (favor these in exercises): "emphasizing," "enhance," "highlighting," "showcasing," "align with," "fostering"
- Domain skew: all domains, but especially professional/business
- Note: this applies to the *specific words*, not their synonyms. "Fancy" vocabulary in general is not a tell — see Ineffective indicators.

### Faux-scholarly hedging and didactic disclaimers
Phrases that mimic academic caution or lecture the reader on what matters, adding no precision.
- Indicators: "it is worth noting that," "it is important to note/remember/consider," "one might argue," "this raises important questions about," "at its core," "may vary"
- Domain skew: thought leadership posts, product docs, long-form

### Significance inflation
AI puffs up the importance of whatever it's writing about — attaching statements about how arbitrary details represent, contribute to, or reflect something broader. The subject becomes simultaneously less specific and more exaggerated: precise facts get smoothed into generic importance-claims.
- Indicators: "stands/serves as a testament to," "plays a vital/pivotal/key role," "underscores its importance," "reflects broader trends," "marks a significant shift," "key turning point," "evolving landscape," "indelible mark," "deeply rooted," "setting the stage for," "enduring legacy," "focal point"
- Also: situating mundane subjects amid grand "debates," "discussions," or "movements"
- Domain skew: cover letters, LinkedIn, thought leadership, about pages — anywhere the writer describes work, a company, or themselves

### Promotional register
Even when asked for neutral writing, AI drifts toward advertisement or travel-brochure tone. Newer models are subtler about it (fewer outright superlatives) but the drift persists.
- Indicators: "nestled," "in the heart of," "vibrant," "rich heritage," "renowned," "boasts a," "diverse array," "breathtaking," "natural beauty," "commitment to excellence," "world-class"
- Domain skew: company comms, about pages, LinkedIn, anything describing a place, product, or organization

### Vague authority attribution
AI attributes opinions to unnamed or inflated authorities — weasel wording — and overstates how many sources hold a view.
- Indicators: "Experts argue," "Observers have noted," "Industry reports suggest," "widely regarded as," "many believe," "studies show" (with no study), "several publications" (when it's one)
- Domain skew: thought leadership, long-form, product marketing

---

## Structural Tells (sentence/paragraph-level)

### Copula avoidance
AI systematically replaces plain "is/are/has" with dressed-up constructions. Empirically one of the strongest tells: usage of "is" and "are" measurably dropped in AI-influenced writing, and AI "improvements" of human text reliably perform this swap.
- Indicators: "serves as a," "stands as a," "functions as," "operates as," "represents a" — where "is" would do; "features," "offers," "boasts," "maintains" — where "has" would do; "refers to" as a definitional opener
- Elaborated forms: "ventured into politics as a candidate" (vs. "was a candidate"), "began his career as" (vs. "was")
- Domain skew: all domains; especially bios, about pages, product descriptions
- Probe value: high. Embed "serves as" / "boasts" swaps and watch whether the user restores the plain verb.

### Participial clause tails
AI attaches "-ing" phrases to sentence ends to inject superficial analysis of significance or impact — often unearned, sometimes attributing the analysis to nobody.
- Indicators: "...highlighting its importance," "...underscoring the need for," "...ensuring long-term success," "...reflecting a commitment to," "...fostering collaboration," "...contributing to broader goals," "...cultivating trust," "...enhancing the experience"
- Domain skew: all domains; especially product docs, LinkedIn, cover letters
- Probe value: high. This is the single most productive clause-tail probe — most users either cut the tail entirely or convert it to a claim they actually stand behind.

### Em-dash overuse
AI inserts em-dashes to add false rhythm and show "depth," often mimicking punched-up sales writing.
- Indicators: 2+ em-dashes per paragraph; em-dashes where a comma, period, or colon would read better; em-dashes surrounded by spaces ( — ) contrary to common typographic practice
- Domain skew: LinkedIn posts, cover letters, long-form
- Era note: newer models (post-2025) have been tuned to suppress em-dashes, so absence proves nothing. Presence in formulaic parallel constructions remains a useful probe. If a user *keeps* em-dashes in a rewrite, that's genuine tolerance — record it; many human writers use them deliberately.

### Tricolon stacking (the rule of three)
AI organizes almost everything into groups of three — three bullet points, three sentence clauses, three examples — regardless of whether three is the natural number. Often used to make superficial analysis look comprehensive.
- Indicators: three-part lists in contexts where 2 or 4 would be more natural; "X, Y, and Z" as a default sentence structure; "adjective, adjective, adjective" stacks
- Domain skew: all domains

### The negative-parallelism family
Formulaic contrast structures AI uses to signal nuance or clear up an objection nobody raised. Several variants:
- "Not just X, but Y" — "not just about [obvious thing], but [slightly deeper thing]"
- "Not X, but Y" — full replacement: "It's not a feature, it's a philosophy"
- "It's not X — it's Y" (frequently with an em-dash)
- "No X, no Y, just Z"
- Reversed: "X rather than Y," "prioritizing X over Y" (increasingly common in newer output)
- Multi-sentence form: stating one characteristic, then "However, ..." pivoting to its counterpart
- Domain skew: cover letters, LinkedIn, thought leadership
- Probe value: high as a Tier-1 structural probe — watch whether the user keeps the frame, inverts it, or discards the contrast entirely.

### Opening with a rhetorical question
AI uses rhetorical questions as hooks, especially at paragraph or section starts.
- Indicators: "What does it mean to...?", "Have you ever wondered...?", "Why does this matter?"
- Domain skew: LinkedIn posts, blog intros, cover letters

### Passive voice as a default
AI defaults to passive constructions when active would be more direct.
- Indicators: "decisions were made," "the results were achieved," "it has been found"
- Domain skew: product docs, professional emails, specs

---

## Tonal Tells (voice-level)

### Performative enthusiasm
Writing that signals emotion rather than embodying it.
- Indicators: "I'm incredibly passionate about," "I am deeply excited to," "This is a space I truly care about"
- Domain skew: cover letters, LinkedIn profiles

### Excessive balance / false balance
AI presents "both sides" even when the writer clearly has a view.
- Indicators: "While X has its merits, Y also offers valuable perspectives," "On one hand... on the other hand..." with no resolution
- Domain skew: thought leadership, LinkedIn, long-form

### Transition inflation
Using heavy discourse markers to create the impression of logical flow.
- Indicators: "Furthermore," "Moreover," "In addition," "Additionally" (especially sentence-initial), "It is also worth mentioning," "Building on this idea"
- Domain skew: all domains; especially product docs and long-form
- Note: weak in isolation — many human writers and style guides use these. Strong only in scaffold form ("First... Furthermore... Finally") or in combination with other tells.

### Conclusion mirroring and formula endings
The ending restates the piece or follows a rigid resolution formula.
- Indicators: last paragraph begins "In conclusion," / "In summary," / "Ultimately," / "Overall," / "At the end of the day" and recaps what was said
- The "challenges" formula: "Despite these challenges, [subject] continues to [vaguely positive assessment]" — acknowledging obstacles, then resolving them with unearned optimism
- Domain skew: long-form, LinkedIn posts, cover letters, company comms

### Platitude endings
Closing with a vague forward-looking sentiment.
- Indicators: "I look forward to the journey ahead," "Together, we can make a difference," "The possibilities are endless"
- Domain skew: cover letters, LinkedIn, company comms

---

## Formatting Tells (layout-level)

Most relevant for short-form social and docs domains, where formatting is part of voice. Embed sparingly — one formatting probe per exercise at most.

### Boldface overuse
Mechanical emphasis in a "key takeaways" fashion — bolding every instance of a chosen term, or bolding conclusion phrases.
- Domain skew: LinkedIn, product docs, internal comms

### Inline-header bullet lists
The signature AI list shape: bullet or number, **Bolded Label**, colon, description. Used for content that would read better as prose.
- Indicators: "**Scalability**: The system handles..." repeated down a list
- Domain skew: LinkedIn, product docs, PRDs

### Title-case headings
Capitalizing All Main Words in headings and subheadings by default.
- Domain skew: docs, long-form

### Emoji as decoration
Emoji prefixing section headers or bullet points (🚀, ✨, 🎯, 💡) as visual scaffolding.
- Domain skew: LinkedIn, newsletters
- Era note: declining in newer models but still common in social-media-flavored output. If the user *adds* emoji in rewrites, that's their voice — record it.

---

## Signs of human writing

Patterns empirically *more common in human writing than AI output*. Two uses: (1) their presence in the user's samples is fingerprint material — encode and preserve it; (2) never "fix" these during exercise construction or treat their presence in a rewrite as a flaw. If a user introduces these while rewriting, that is strong voice signal, not sloppiness.

- **Plain copulas**: simple "there is a," "it has a" constructions
- **Plain verbs over stiff synonyms**: wrote (not authored), moved (not relocated), used (not utilized), tried (not attempted), died (not passed away)
- **Superlatives and definitive claims**: "one of the best," "the only," "was the first" — AI hedges these; humans commit
- **Casual hedges and intensifiers**: "very," "perhaps," "tends to," "pretty much," "I think"
- **Wordy human constructions**: "as a result of," "in order to," "all of the," "a part of," "the fact that"
- **Irregularity**: uneven paragraph lengths, mid-thought openers, abrupt stops, comfortable word repetition (AI's repetition penalty makes it avoid reusing words; humans repeat freely)

---

## Ineffective indicators — do not treat these as AI tells

Commonly assumed to indicate AI, but unreliable. Do not build exercises around them, and do not encode rules against them unless the user's own evidence shows they avoid them:

- **Perfect grammar** — many people are skilled writers
- **Formal, academic, or "fancy" prose** — AI overuses *specific* words, not formality in general. A user with a restrained, precise, formal voice is not "writing like AI," and the generated skill must preserve that register
- **Transition words in isolation** — accepted by many style guides; only scaffold patterns are telling
- **Mixed casual/formal registers** — often just how technical people, young people, or neurodivergent people write
- **"Bland" prose** — blandness is not the tell; the specific formulas above are
- **Letter-like formality** (salutations, sign-offs) — predates AI by centuries

---

## Domain-specific indicator clusters

### For cover letters / job applications
Primary tells to embed: performative enthusiasm, negative-parallelism family, hollow intensifiers, significance inflation, participial clause tails, conclusion mirroring

### For LinkedIn posts / professional social
Primary tells to embed: em-dash overuse, tricolon stacking, rhetorical question opener, negative-parallelism family ("It's not X — it's Y"), inline-header bullet lists, platitude endings

### For business emails & outreach
Primary tells to embed: filler affirmations, hedge stacking, passive voice, copula avoidance, transition inflation

### For product docs / PRDs / specs
Primary tells to embed: faux-scholarly hedging, passive voice, copula avoidance, participial clause tails, inline-header bullet lists, overused AI vocabulary (leverage, robust, seamlessly, enhance)

### For long-form / blog / newsletter
Primary tells to embed: conclusion mirroring, em-dash overuse, tricolon stacking, significance inflation, vague authority attribution, rhetorical question opener
