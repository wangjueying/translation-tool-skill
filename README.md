# TranslationTool Skill for Claude

A Claude skill that helps users understand, deploy, configure, and troubleshoot the TranslationTool project - an AI translation application with React frontend + Flask backend.

## What This Skill Does

This skill provides comprehensive guidance for working with TranslationTool:

- **Local Setup**: Get the project running locally with minimal effort
- **Production Deployment**: Deploy to Vercel + Railway, Render + Vercel, or Docker
- **Configuration**: Set up translation engines (Google, DeepL, Baidu, Azure, etc.)
- **Troubleshooting**: Solve common issues with deployment, API, and features
- **Architecture**: Understand the project structure and technology stack

## Installation

### Option 1: Manual Installation

1. Clone or download this skill directory
2. Copy to your Claude skills directory:
   - **macOS**: `~/Library/Application Support/Claude/claude-desk-top/config/skills/`
   - **Linux**: `~/.config/Claude/claude-desktop/config/skills/`
3. Restart Claude

### Option 2: Coming Soon

We'll be adding support for one-click installation from GitHub.

## Usage

Once installed, this skill automatically triggers when you ask about:

- "Deploy TranslationTool"
- "How do I set up API keys?"
- "Help with translation engine configuration"
- "TranslationTool deployment options"
- "CORS issues with TranslationTool"
- And more...

## Skill Contents

```
translation-tool-skill/
├── SKILL.md                 # Main skill file
├── references/              # Detailed documentation
│   ├── deployment.md        # Deployment guides
│   ├── configuration.md     # API configuration
│   ├── troubleshooting.md   # Common issues & solutions
│   ├── api.md              # API reference
│   └── structure.md        # Project structure
└── README.md               # This file
```

## TranslationTool Project

This skill supports the [TranslationTool project](https://github.com/YOUR_USERNAME/TranslationTool) - a full-stack AI translation application featuring:

- 100+ language support via multiple translation engines
- OCR image translation
- Voice input and output
- User authentication
- Translation history

## Features

### Multiple Deployment Paths

- **Vercel + Railway** (Recommended, ~$5/mo, 5 min setup)
- **Render + Vercel** (Free, 10 min setup)
- **Docker** (Self-hosted)

### Translation Engine Support

- Google Translate (default, no API key)
- DeepL (high quality)
- Baidu (Chinese optimized)
- Azure Translator (enterprise)
- Youdao (Chinese)
- OpenAI via OpenRouter (AI-powered)

### Comprehensive Documentation

Each reference file provides in-depth information:

- Step-by-step deployment guides
- Environment variable configuration
- Troubleshooting common issues
- API endpoint documentation
- Architecture explanations

## Contributing

Contributions are welcome! Feel free to:

- Report issues or suggest improvements
- Submit pull requests with enhancements
- Add new deployment platform guides
- Improve documentation

## License

MIT License - feel free to use and modify as needed.

## Related Projects

- [TranslationTool](https://github.com/YOUR_USERNAME/TranslationTool) - The main project this skill supports

## Support

For issues with this skill, please open a GitHub issue.
For TranslationTool project issues, use the main project repository.
