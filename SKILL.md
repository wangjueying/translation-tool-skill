---
name: translation-tool
description: Guide for working with TranslationTool - an AI translation application with React frontend + Flask backend supporting multi-language translation, OCR, voice input/output, and multiple translation engines (Google, DeepL, Baidu, Azure). Use this skill whenever the user asks about deploying, setting up, configuring, troubleshooting, or understanding the TranslationTool project, or mentions translation tool deployment, Flask+React translation app, Vercel+Railway deployment for translation services, or needs help with translation API configuration.
---

# TranslationTool Project Guide

TranslationTool is a full-stack AI translation application built with React 18 (frontend) and Flask (Python backend). It supports 100+ languages, OCR image translation, voice input/output, user authentication, and translation history.

## Quick Overview

**Tech Stack:**
- Frontend: React 18 + Create React App
- Backend: Flask (Python 3.11+) + SQLite
- Deployment: Vercel (frontend), Railway/Render/Fly.io (backend)
- Translation Engines: Google Translate (default, no key), DeepL, Baidu, Azure, Youdao, OpenAI via OpenRouter

**Project Structure:**
```
TranslationTool/
├── frontend/          # React application
│   └── src/
│       ├── App.js              # Main app with auth state
│       ├── TranslationPage.js  # Core translation UI
│       ├── AuthModal.js        # Login/register modal
│       └── api.js              # Backend API client
├── backend/           # Flask application
│   ├── app.py              # Flask entry point
│   ├── api/                # API endpoints
│   │   ├── auth.py         # Authentication (JWT, WeChat)
│   │   ├── translation.py  # Translation with lang detection
│   │   ├── ocr.py          # OCR processing
│   │   └── tts.py          # Text-to-speech
│   ├── models/
│   │   └── database.py     # Database models (users, history)
│   └── utils/              # Engine implementations
├── start.sh           # Local development launcher
├── configure.sh       # Interactive configuration
└── docker-compose.yml # Docker deployment
```

## Primary User Scenarios

### 1. First-Time Local Setup

**When user wants to run the project locally:**

1. Check prerequisites: Python 3.11+, Node.js 18+
2. Run the startup script: `./start.sh` (auto-sets up environment)
3. Access at: http://localhost:3000

**Alternative manual setup:**
```bash
# Backend
cd backend && python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python init_db.py
python app.py

# Frontend (new terminal)
cd frontend && npm install && npm start
```

**Troubleshooting local setup:**
- Port conflicts: Change `PORT` in `.env` or frontend port
- Module not found: Run `pip install -r requirements.txt` or `npm install`
- Database errors: Delete `backend/translation.db` and re-run `init_db.py`

### 2. Deploying to Production

**Recommended deployment paths:**

**Option A: Vercel + Railway (5 min, ~$5/month)**
- Frontend → Vercel (unlimited free)
- Backend → Railway ($5 free tier/month)
- Best for: Quick production deployment

**Option B: Render + Vercel (10 min, $0)**
- Backend → Render (Python free tier)
- Frontend → Vercel (unlimited free)
- Best for: Completely free deployment

**Option C: Docker**
- Use `docker-compose up -d`
- Best for: VPS self-hosting

**For deployment, check references/deployment.md which contains:**
- Platform-specific setup steps
- Environment variable configuration
- CORS configuration between frontend/backend
- Database setup for production

### 3. Configuring Translation Engines

**Available engines (all optional, Google Translate is default):**

| Engine | Free Tier | API Key Required |
|--------|-----------|------------------|
| Google Translate | Unlimited | No (default) |
| LibreTranslate | Unlimited | No |
| DeepL | 500K chars/month | Yes |
| Baidu | 2M chars/month | Yes |
| Azure | 2M chars/month | Yes |
| Youdao | 1M chars/month | Yes |
| OpenAI (via OpenRouter) | Free models | Yes |

**Configuration steps:**
1. Run `./configure.sh` for interactive setup
2. Or edit `.env` file directly with API keys
3. Restart backend after changes

**For detailed configuration, check references/configuration.md**

### 4. Setting Up Authentication

**Supported auth methods:**
- Email/password (built-in)
- WeChat OAuth (requires WeChat App ID/Secret)
- JWT token-based sessions

**Environment variables for auth:**
```bash
SECRET_KEY=your-secret-key-here  # For JWT signing
WECHAT_APP_ID=your-wechat-id
WECHAT_APP_SECRET=your-wechat-secret
```

### 5. Troubleshooting Common Issues

**Deployment issues:**
- CORS errors: Check `ALLOWED_ORIGINS` in backend `.env`
- Build failures: Verify Node.js and Python versions
- Database migration: Backend auto-migrates on startup

**API issues:**
- Rate limiting: Check engine-specific limits
- Timeout errors: Increase `API_TIMEOUT` in `.env`
- Language detection fails: Fall back to manual language selection

**Frontend issues:**
- Voice input not working: Chrome/Safari only (Web Speech API)
- Audio playback issues: Check Google TTS API availability

**For troubleshooting help, check references/troubleshooting.md**

## Architecture Reference

**Key architectural concepts:**

1. **Frontend-Backend Separation**: React SPA calls Flask REST API
2. **Database Schema**: Users → User Settings → Translation/OCR History
3. **Translation Flow**: Input → Language Detection → Engine Selection → Translation → History Storage
4. **Voice Flow**: Web Speech API (input) / Google TTS (output)
5. **Authentication**: JWT tokens stored in localStorage, sent in Authorization header

## When to Check Reference Files

- **Deployment guides**: `references/deployment.md`
- **Configuration details**: `references/configuration.md`
- **Troubleshooting**: `references/troubleshooting.md`
- **API reference**: `references/api.md`
- **Project structure**: `references/structure.md`

Only load these reference files when the user specifically asks about those topics. Start with the overview above and load additional details as needed.
