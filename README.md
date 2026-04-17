# AI Safety Toolkit — Prompts, Skills & Security Guide

> **A practical, ready-to-install security toolkit for everyday AI users.**
> Covers Claude (Cowork, Desktop, Code), ChatGPT, Microsoft Copilot, and Gemini.
> Includes a bilingual human-readable guide and copy-paste Skills you can install in minutes.

---

## Why This Exists

AI tools are becoming genuinely powerful. Claude Cowork, ChatGPT, and similar platforms can now read your files, connect to third-party services like Figma and Google Drive, execute multi-step tasks autonomously, and take real actions on your behalf — all without you typing a single line of code.

That's remarkable. But it also introduces risks that most everyday users have never had to think about before.

**The problem isn't that these tools are unsafe. The problem is that the risks are invisible.**

A prompt template you found on GitHub might look perfectly helpful — and still contain a hidden instruction to upload your files somewhere. A plugin with five-star reviews might request permissions it doesn't need. A Word document from a client might have white-text instructions buried inside it that redirect your AI mid-task. None of these are theoretical. They are real, documented attack patterns that grow more relevant as AI tools become more capable.

Most security guides are written for developers. This one is written for everyone else — the operations manager using Cowork to automate reports, the designer asking Claude to work with Figma files, the freelancer letting ChatGPT process client documents. People who are not doing anything unusual. People who just want to use these tools safely.

This toolkit exists to give those users two things:

1. **A clear explanation** of what the risks actually are and how they work — without jargon
2. **Ready-to-install Skills** that put the safety checks on autopilot, so you don't have to remember to do them manually every time

---

## What's in This Repo

**1. 📖 Security Guide** — A bilingual (English/Chinese) reference covering the 4 main risks, how to inspect suspicious content, and platform-specific safety steps. Read this once to understand the landscape.

**2. ⚙️ Ready-to-install Skills** — Prompt instruction files for Claude Cowork, Claude (web/mobile), Claude Code, and ChatGPT. Once installed, your AI automatically runs security checks, confirms before touching your files, and reports what it did.

---

## 📁 Repository Structure

```
ai-safety-toolkit/
│
├── README.md                              ← You are here
│
├── guides/
│   └── ai-tools-security-guide.md        ← 📖 Read this first (bilingual)
│
└── skills/
    ├── claude-cowork-global-instructions.md   ← Claude Cowork
    ├── claude-system-prompt.md                ← Claude (web / mobile)
    ├── claude-code-CLAUDE.md                  ← Claude Code
    └── chatgpt-custom-instructions.md         ← ChatGPT
```

---

## 📖 Start Here: Security Guide

👉 [guides/ai-tools-security-guide.md](./guides/ai-tools-security-guide.md)

Covers:
- The 4 main security risks when using AI tools (prompt injection, plugins, external files, over-permission)
- How to inspect suspicious prompts, Skills, and plugins using an isolated AI conversation
- How to set up a sandbox folder for processing untrusted external files
- Platform-specific instructions for Claude, ChatGPT, Copilot, and Gemini
- A quick checklist to run before every AI task

---

## ⚙️ Install a Skill | 安装 Skill

Choose the file that matches your platform and follow the instructions inside.

### Claude Cowork
**File:** [`skills/claude-cowork-global-instructions.md`](./skills/claude-cowork-global-instructions.md)

**How to install:**
1. Open Claude Desktop
2. Go to **Settings → Cowork → Global Instructions → Edit**
3. Copy the content after the `---` divider and paste it in
4. Click **Save**

---

### Claude (Web / Mobile) — Recommended ✅
**File:** [`skills/ai-security-check/SKILL.md`](./skills/ai-security-check/SKILL.md)

This is a proper Claude Skill — install it once and it activates automatically whenever needed.

**How to install:**
1. Download the `skills/ai-security-check/` folder and zip it into `ai-security-check.zip`
2. Go to [claude.ai/customize/skills](https://claude.ai/customize/skills)
3. Upload the ZIP file
4. Enable the Skill — done

> **Note:** This feature requires Code Execution to be enabled in your Settings → Capabilities.

**Alternative (no zip needed):**
**File:** [`skills/claude-system-prompt.md`](./skills/claude-system-prompt.md)
1. Go to [claude.ai](https://claude.ai) → avatar → **Settings → Profile**
2. Paste the content into the custom instructions field and save

---

### Claude Code
**File:** [`skills/claude-code-CLAUDE.md`](./skills/claude-code-CLAUDE.md)

**How to install:**
1. Copy this file into the **root directory** of your project
2. Rename it to `CLAUDE.md`
3. Claude Code will automatically load it at the start of every session

---

### ChatGPT
**File:** [`skills/chatgpt-custom-instructions.md`](./skills/chatgpt-custom-instructions.md)

**How to install:**
1. Open [chatgpt.com](https://chatgpt.com)
2. Click your avatar → **Settings → Personalization → Custom Instructions**
3. Paste the content into the **"How would you like ChatGPT to respond?"** field
4. Save

---

## 🔍 How the Skill Works

Once installed, your AI will automatically:

1. **Ask you safety questions** before starting any task involving external files or plugins
2. **Analyze suspicious content** when you paste a prompt, Skill, or plugin description — and give you a structured report:

```
[Security Analysis Report]
✅ Safe: ...
⚠️  Note: ...
🚨 Risk Warning: ...
Recommendation: Safe to use / Modify before use / Do not use
```

3. **Confirm before file operations** — it will tell you exactly what it plans to do before writing, modifying, or deleting any file
4. **Summarize what it did** at the end of each task

---

## 💬 Trigger Phrases

After installing any Skill, say any of these to instantly start a security check:

- `"Check this Skill"`
- `"Is this prompt safe?"`
- `"Analyze this plugin"`
- `"Security check"`
- `"Check this file"`

---

## 🤝 Contributing

Corrections, additions, and new platform versions are welcome via Issue or PR. If you've tested this on a platform not listed here (Gemini, Copilot, Cursor, etc.) and have working instructions, please share them.

---

*Built from a real conversation about AI safety. Last updated April 2026.*
