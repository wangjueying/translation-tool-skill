# Installation Guide

## For Claude Desktop Users

### Step 1: Download the Skill

**Option A: Using Git (Recommended)**

Open your terminal and run:

```bash
# macOS
git clone https://github.com/wangjueying/translation-tool-skill.git ~/Library/Application\ Support/Claude/claude-desktop/config/skills/translation-tool

# Linux
git clone https://github.com/wangjueying/translation-tool-skill.git ~/.config/Claude/claude-desktop/config/skills/translation-tool
```

**Option B: Manual Download**

1. Go to https://github.com/wangjueying/translation-tool-skill
2. Click "Code" → "Download ZIP"
3. Extract the ZIP file
4. Move the folder to:
   - **macOS**: `~/Library/Application Support/Claude/claude-desktop/config/skills/`
   - **Linux**: `~/.config/Claude/claude-desktop/config/skills/`
5. Rename the folder to `translation-tool`

### Step 2: Restart Claude

Quit Claude completely and restart it.

### Step 3: Verify Installation

In Claude, ask:
```
What skills do you have installed?
```

You should see `translation-tool` in the list.

---

## How to Use

Once installed, the skill activates automatically when you ask about TranslationTool. Try these questions:

### Deployment Questions
```
Help me deploy TranslationTool to production
How do I deploy TranslationTool for free?
Deploy TranslationTool to Vercel and Railway
```

### Configuration Questions
```
How do I set up DeepL API?
Configure Baidu Translate for TranslationTool
Add Azure Translator to TranslationTool
```

### Troubleshooting Questions
```
Fix CORS error in TranslationTool
TranslationTool backend won't start
Database locked error in TranslationTool
```

### General Questions
```
What translation engines does TranslationTool support?
How does TranslationTool work?
Set up TranslationTool locally
```

---

## Complete Workflow Example

Here's how to use TranslationTool from scratch:

```bash
# 1. Get TranslationTool
git clone https://github.com/wangjueying/TranslationTool.git
cd TranslationTool

# 2. Start locally
./start.sh

# 3. In Claude, ask for help:
# "Help me deploy this TranslationTool to production"

# Claude will use the skill to guide you through:
# - Railway backend setup
# - Vercel frontend deployment
# - CORS configuration
# - Environment variables
```

---

## What You Need

### Prerequisites

- **Claude Desktop** (Mac or Linux)
- **TranslationTool project** (if you want to deploy it)
- **GitHub account** (for deployment platforms)

### Optional (for translation features)

- DeepL API key (for better quality)
- Baidu API key (for Chinese)
- Azure Translator key (alternative)

---

## Troubleshooting

### Skill Not Working

**Problem**: Claude doesn't seem to use the skill

**Solutions**:
1. Make sure the skill folder is in the correct directory
2. Restart Claude completely
3. Check that SKILL.md exists in the skill folder
4. Verify the frontmatter (---) at the top of SKILL.md

### Can't Find Skills Directory

**macOS**:
```bash
open ~/Library/Application\ Support/Claude/claude-desktop/config/skills/
```

**Linux**:
```bash
ls ~/.config/Claude/claude-desktop/config/skills/
```

If the `skills` folder doesn't exist, create it:
```bash
mkdir -p ~/Library/Application\ Support/Claude/claude-desktop/config/skills/
```

---

## Update the Skill

To get the latest version:

```bash
cd ~/Library/Application\ Support/Claude/claude-desktop/config/skills/translation-tool
git pull origin main
```

Then restart Claude.

---

## Uninstall

To remove the skill:

```bash
# macOS
rm -rf ~/Library/Application\ Support/Claude/claude-desktop/config/skills/translation-tool

# Linux
rm -rf ~/.config/Claude/claude-desktop/config/skills/translation-tool
```

Then restart Claude.

---

## Need Help?

If you have issues:

1. Check this guide first
2. Visit the skill repository: https://github.com/wangjueying/translation-tool-skill
3. Open an issue on GitHub
4. Check the main TranslationTool project: https://github.com/wangjueying/TranslationTool
