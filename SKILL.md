---
name: translation-tool
description: Help with TranslationTool deployment and setup. TranslationTool is a React + Flask translation app supporting multi-language translation, OCR, voice I/O, and multiple engines (Google, DeepL, Baidu, Azure). Use this skill when users ask about: deploying translation apps, configuring translation APIs (DeepL, Baidu, Azure), fixing CORS in Flask+React apps, setting up OCR/TTS features, or need guidance on TranslationTool specifically. Also triggers for Vercel+Railway deployment, Render deployment, or translation service setup.
---

# TranslationTool Help Guide

TranslationTool is a full-stack translation app with React frontend and Flask backend. Supports 100+ languages, OCR image translation, voice input/output, and user authentication.

## Quick Start

**To use TranslationTool, users first need the project:**

1. **Get the code**: Clone from GitHub (https://github.com/wangjueying/TranslationTool)
2. **Local setup**: Run `./start.sh` in the project root
3. **Access**: Open http://localhost:3000

**Manual setup:**
```bash
# Backend
cd TranslationTool/backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python init_db.py && python app.py

# Frontend (new terminal)
cd TranslationTool/frontend
npm install && npm start
```

## Deployment Options

### Easiest: Vercel + Railway (~5/month)

**Backend (Railway):**
1. Go to https://railway.app → "New Project" → "Deploy from GitHub"
2. Select TranslationTool repository
3. Set Root Directory: `backend`
4. Add env vars:
   ```
   SECRET_KEY=$(openssl rand -hex 32)
   DEBUG=False
   PORT=5001
   ```
5. Deploy → copy the URL

**Frontend (Vercel):**
1. Go to https://vercel.com → "New Project" → Import repo
2. Set Root Directory: `frontend`
3. Add env var:
   ```
   REACT_APP_API_URL=<your-railway-url>
   ```
4. Deploy

**Configure CORS:**
In Railway, add: `ALLOWED_ORIGINS=https://your-vercel-app.vercel.app`

### Free Option: Render + Vercel

Same as above, but use https://render.com for backend.
Note: Render free tier has cold starts (30-60s first load).

### Docker

```bash
cd TranslationTool
docker-compose up -d
```

## Translation Engines

**Google Translate** works by default (no API key needed).

For better quality, add to `.env`:

**DeepL** (recommended):
```
DEEPL_API_KEY=your-key
```
Get key: https://www.deepl.com/pro-api (500K chars/month free)

**Baidu** (Chinese):
```
BAIDU_APP_ID=your-id
BAIDU_SECRET_KEY=your-secret
```
Get key: https://fanyi-api.baidu.com/ (2M chars/month free)

**Azure**:
```
AZURE_TRANSLATOR_KEY=your-key
AZURE_TRANSLATOR_REGION=eastasia
```
Get key: Azure Portal → "Translator" resource (2M chars/month free)

## Common Issues

**CORS errors**: Add frontend URL to `ALLOWED_ORIGINS` in backend `.env`

**Port in use**: Change `PORT` in `.env` or kill process using port 5001

**npm install fails**: `rm -rf node_modules && npm install`

**pip install fails**: Ensure Python 3.11+

**Database locked**: Delete `backend/translation.db` and re-run `init_db.py`

**Translation not working**: Check API key, verify quota, try Google Translate fallback

## Project Structure

```
TranslationTool/
├── frontend/              # React app
│   └── src/
│       ├── App.js                  # Main app with auth
│       ├── TranslationPage.js      # Core UI
│       ├── AuthModal.js            # Login/register
│       └── api.js                  # API client
├── backend/               # Flask app
│   ├── app.py                   # Flask entry
│   ├── api/                     # Endpoints
│   │   ├── auth.py              # JWT, WeChat login
│   │   ├── translation.py       # Translation + lang detection
│   │   ├── ocr.py               # OCR processing
│   │   └── tts.py               # Text-to-speech
│   ├── models/
│   │   └── database.py          # Users, history models
│   └── utils/                   # Translation engines
├── start.sh              # Auto-setup script
├── configure.sh          # Config API keys
└── docker-compose.yml    # Docker deployment
```

## Environment Variables

**Required (production):**
```bash
SECRET_KEY=<random-string>
DEBUG=False
ALLOWED_ORIGINS=https://your-frontend.com
PORT=5001
```

**Frontend:**
```bash
REACT_APP_API_URL=https://your-backend.com
```

**Optional (engines):**
```bash
DEEPL_API_KEY=...
BAIDU_APP_ID=...
BAIDU_SECRET_KEY=...
AZURE_TRANSLATOR_KEY=...
AZURE_TRANSLATOR_REGION=...
```

## Features

- **100+ languages** via multiple translation engines
- **OCR** - Extract and translate text from images
- **Voice input** - Real-time speech recognition (Chrome/Safari)
- **Voice output** - Text-to-speech playback
- **User auth** - Email/password + JWT tokens
- **Translation history** - Save and review past translations
- **WeChat login** - OAuth integration (optional)

## Reference Docs

For more details, check:
- `references/deployment.md` - Step-by-step deployment
- `references/configuration.md` - API key setup
- `references/troubleshooting.md` - Common fixes

## Getting TranslationTool

Users can get TranslationTool from: https://github.com/wangjueying/TranslationTool

Or use it as a reference to build their own translation app.
