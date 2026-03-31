# TranslationTool Skill for Claude

Helps you deploy, configure, and troubleshoot TranslationTool - a React + Flask translation app.

## What This Skill Does

When installed, Claude can help you:
- Deploy TranslationTool to Vercel + Railway, Render + Vercel, or Docker
- Configure translation engines (DeepL, Baidu, Azure, Google)
- Fix common issues (CORS, deployment errors, API problems)
- Set up the project locally
- Understand the project architecture

## Quick Start

### Install the Skill

**macOS:**
```bash
git clone https://github.com/wangjueying/translation-tool-skill.git ~/Library/Application\ Support/Claude/claude-desktop/config/skills/translation-tool
```

**Linux:**
```bash
git clone https://github.com/wangjueying/translation-tool-skill.git ~/.config/Claude/claude-desktop/config/skills/translation-tool
```

Then restart Claude.

### Get TranslationTool

You'll also need the TranslationTool project:
```bash
git clone https://github.com/wangjueying/TranslationTool.git
cd TranslationTool
./start.sh
```

## Usage Examples

Just ask Claude naturally:

- "Help me deploy TranslationTool to production"
- "How do I set up DeepL API for translation?"
- "Fix CORS error in my TranslationTool deployment"
- "Configure Baidu Translate for Chinese"
- "Deploy TranslationTool for free"

## What's Included

- **SKILL.md** - Main knowledge base
- **references/deployment.md** - Step-by-step deployment guides
- **references/configuration.md** - API key setup
- **references/troubleshooting.md** - Common issues and fixes

## TranslationTool Features

- 100+ language support
- OCR image translation
- Voice input and output
- User authentication
- Translation history
- Multiple translation engines (Google, DeepL, Baidu, Azure)

## Links

- **Skill Repository**: https://github.com/wangjueying/translation-tool-skill
- **TranslationTool Project**: https://github.com/wangjueying/TranslationTool

## License

MIT
