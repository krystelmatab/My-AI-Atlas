---
title: "One Binder for All My AI Assistants: the End of Copy-Pasting Skills"
date: 2026-06-15
excerpt: "How the open Agent Skills format lets you keep all your AI skills in a single folder, shared between Claude Code, Codex and GitHub Copilot."
---

<div class="post-tags"><span class="post-tag">Skills</span><span class="post-tag">Productivity</span><span class="post-tag">Automation</span></div>

![A central binder of skills, synced to Claude, Codex and Copilot]({{ '/assets/images/one-binder-every-ai.png' | relative_url }})

## 1. Introduction: The Day I Wrote the Same Thing for the Third Time

Anyone who uses several AI assistants ends up here sooner or later. I had taught Claude Code to create PowerPoint presentations that follow my company's brand guidelines. A few weeks later, I wanted the same skill in Codex. Then in GitHub Copilot. Each time, the same question: am I really going to copy these instructions a third time?

The real problem with copy-pasting is not the time it takes at first. It is **drift**: three copies of the same skill inevitably end up diverging. You fix one, forget the others, and one day you no longer know which one is right. Three assistants, three versions of the truth, which means none at all.

## 2. The Discovery: AI Assistants Now Speak the Same Language

What makes the solution possible today is a quiet but major convergence: the **Agent Skills** format, opened by Anthropic in late 2025, has been adopted by the main coding assistants. Claude Code, Codex and GitHub Copilot now read exactly the same format:

> A skill = a folder + a `SKILL.md` file: a few header lines (name, description) followed by instructions in plain language.

The description plays a key role: it is what the assistant reads to decide *when* to use the skill. The body of the file is only loaded at that moment. A well-described skill therefore works everywhere, with no adaptation.

The strategic consequence: my skills are no longer "Claude settings" or "Copilot settings". They are **my** skills, in a standard format, and the assistants are interchangeable underneath.

## 3. The Architecture: One Source of Truth, Signposts Everywhere Else

The guiding idea fits in one image: instead of photocopying my recipes for every kitchen, I keep **a single binder**, and each kitchen gets a sign saying "the recipes are over there".

In practice, on my PC:

| Element | Role |
|---|---|
| `C:\...\my-skills\` | The binder: the only "living" version of each skill |
| Windows junctions (shortcuts) | The signposts: each assistant believes it has the skills at home, but it only reads the binder |
| **Private** GitHub repository | The backup photocopy: backup and reinstall on another PC with a single command |

Each assistant looks for its skills in its own folder (`~\.claude\skills\`, `~\.codex\skills\`, `~\.config\github-copilot\skills\`). The junctions make these three folders point to the central binder. I fix a skill **once** and all three assistants see the fix **instantly**, in all my projects.

The most telling moment of the setup: a skill originally created for Codex appeared in Claude Code the second the junction was put in place. No copy, no synchronization, just the same file seen from two places.

## 4. The Daily Process: Two Rules, Nothing More

**Rule 1: always work in the binder.** A new skill is created in `my-skills\`, never directly in an assistant's folder. A small script (`sync-skills.ps1`) then creates the missing shortcuts to the three assistants. It can be re-run at will, with no risk.

**Rule 2: saving is not automatic.** Like a Word document: editing the file does not update the backup copy. After a change, a commit + push to GitHub (three clicks in the VS Code Source Control panel, or a simple request to an assistant).

And two safeguards:

- **Private does not mean risk-free, so stay careful anyway**: never put a secret (password, key, customer data) in a skill, even in a private repository.
- **Accounts do not cross paths**: the GitHub account that hosts the backup and the account that provides Copilot can be different. Skills are read from the local disk, and no assistant knows where they are backed up.

## 5. Why This Is More Than an Organization Trick

This setup changes the status of my instructions: they become a **personal, portable and versioned asset**.

- **Portable**: switching assistants, or adopting a fourth one, costs nothing anymore. One shortcut is enough.
- **Versioned**: Git keeps the history of every improvement. I can see how a skill evolved, and roll back.
- **Capitalized**: every lesson learned with one assistant immediately benefits the others. Knowledge accumulates instead of scattering.

> **"My skills no longer belong to a tool. Tools come and go; the binder stays."**

## 6. Conclusion: Write for the Ecosystem, Not for the Tool

The lesson goes beyond skills: whenever an open format emerges in the AI ecosystem, it is worth organizing your work around it rather than around a product. The day a new assistant appears, the question will no longer be "do I have to copy everything again?" but simply "where is the signpost?".
