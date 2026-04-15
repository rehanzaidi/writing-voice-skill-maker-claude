# Writing Voice Skill Maker for Claude

Teach Claude to write in your voice — not a generic AI voice.

---

## The problem

When you ask Claude to write something for you — an email, a LinkedIn post, a cover letter — it does a reasonable job. But it rarely sounds like *you*. It sounds like AI: a little too polished, a little too wordy, with phrases you'd never actually use.

So you fix it. You shorten the sentences, cut the fluff, rewrite the parts that feel off. Every time.

This tool fixes that.

---

## What this is

**Writing Voice Skill Maker for Claude** is a skill that teaches Claude your personal writing style, so it can apply it automatically whenever you ask for help with writing.

You go through a short session (about 5–10 minutes) where Claude learns from things you've actually written, or from watching how you edit AI-generated text. It builds a personal voice profile — a "skill" — that lives in your Claude account.

Once installed, you don't have to explain your style ever again. Just ask Claude to write something, and it writes it the way you would.

You can create separate voice skills for different contexts — one for LinkedIn posts, one for work emails, one for cover letters — and refine them over time as your needs evolve.

---

## What you need

- A [Claude.ai](https://claude.ai) account (Free, Pro, Max, Team, or Enterprise)
- Code execution enabled — go to **[Settings → Capabilities](https://claude.ai/settings/capabilities)** and turn on **Code execution and file creation**

That's it. No coding required.

> For more details on Skills in Claude, see the [official support page](https://support.claude.com/en/articles/12512180-use-skills-in-claude).

---

## How to install

> **Note:** Skills can only be installed and managed from the Claude desktop or web app — not from the Claude mobile app. Once installed, your voice skills will work on mobile too, but the setup needs to happen on desktop or web first.

### Step 1: Download the skill

Click the green **Code** button at the top of this page, then **Download ZIP**. Keep the ZIP file as-is — you'll upload it directly.

> **New to GitHub?** GitHub is just a website for sharing files. You don't need an account to download from it. Just click Code → Download ZIP.

### Step 2: Enable Skills in Claude

1. Go to [claude.ai](https://claude.ai) and sign in
2. Go to **[Settings → Capabilities](https://claude.ai/settings/capabilities)**
3. Make sure **Code execution and file creation** is turned on

### Step 3: Upload the skill

1. Go to **[Customize → Skills](https://claude.ai/customize/skills)**
2. Click the **"+"** button, then **"+ Create skill"**
3. Select **"Upload a skill"**
4. Upload the ZIP file you downloaded
5. Toggle the skill on

### Step 4: Start a new conversation

Type `/writing-voice-skill-maker` and Claude will guide you from there.

---

## How it works

When you start the session, Claude will ask you two things:

**1. What kind of writing is this for?**
LinkedIn posts, work emails, cover letters, product docs — you choose. You can create a separate voice skill for each one if you want.

**2. How do you want to teach Claude your style?**

- **Paste 2–3 things you've written** — emails, posts, anything you're happy with. Claude learns from what you already wrote.
- **Do 2–3 rewriting exercises** — Claude gives you a short AI-written passage and you rewrite it your way. Claude learns from what you change.
- **Both** — recommended, gives the best result.

After that, Claude builds your voice skill, gives you a quick summary of what it captured, and packages it as a file you can install.

---

## After you install your voice skill

Once your new voice skill (e.g. `outreach-email-voice`) is installed and turned on, just use Claude normally:

- *"Draft a cold outreach email to a startup founder"*
- *"Write a LinkedIn post about my new role"*
- *"Help me respond to this recruiter"*

Claude will apply your voice automatically. No extra instructions needed.

---

## Refining your voice skill over time

If the output ever sounds off, you can improve it:

Start a new conversation and type:

```
/writing-voice-skill-maker refine my outreach-email-voice skill
```

Claude will ask what's wrong and how you'd like to fix it — you can add more writing samples, do more rewriting exercises, or describe a specific problem ("it's too formal" / "too long" / "sounds AI-clean").

Before updating, Claude saves your previous version automatically so you can roll back if needed.

---

## Tips

- **For LinkedIn and social posts**, paste actual posts you've published if you have them. Structure matters a lot in short-form writing, and Claude can only learn your post structure from real examples — not from rewriting exercises.
- **Raw is better than polished** — when pasting your own writing, use things as you actually sent or posted them, not cleaned-up versions.
- **You can have multiple voice skills** — 'outreach-email-voice', 'linkedin-voice', 'cover-letter-voice' — each tuned for a different context.

---

## License

MIT — free to use, share, and modify.
