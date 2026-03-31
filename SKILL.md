---
name: translation-tool
description: Help with TranslationTool deployment and setup. TranslationTool is a React + Flask translation app supporting multi-language translation, OCR, voice I/O, and multiple engines (Google, DeepL, Baidu, Azure). Use this skill when users ask about: deploying translation apps, configuring translation APIs (DeepL, Baidu, Azure), fixing CORS in Flask+React apps, setting up OCR/TTS features, or need guidance on TranslationTool specifically. Also triggers for Vercel+Railway deployment, Render deployment, or translation service setup.
---

# TranslationTool Deployment & Setup Guide

Complete guide for deploying and configuring TranslationTool - a full-stack translation application with React frontend and Flask backend.

**Features:**
- 🌍 100+ language support
- 📷 OCR image translation
- 🎤 Voice input/output
- 🔐 User authentication
- 📚 Translation history
- 🔄 Multiple translation engines (Google, DeepL, Baidu, Azure)

---

## Quick Start

### Get the Code

```bash
git clone https://github.com/wangjueying/TranslationTool.git
cd TranslationTool
./start.sh
```

Then open http://localhost:3000

### Manual Setup

**Backend:**
```bash
cd backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python init_db.py && python app.py
```

**Frontend (new terminal):**
```bash
cd frontend
npm install && npm start
```

---

## Deployment to Production

### Option 1: Vercel + Railway (Recommended) ⭐

**Best for:** Quick production deployment (~$5/month, 5 min setup)

#### Step 1: Deploy Backend to Railway

1. Go to https://railway.app
2. Click "New Project" → "Deploy from GitHub"
3. Select your TranslationTool repository
4. Configure:
   - Root Directory: `backend`
   - Start Command: `python app.py`
5. Add environment variables:
   ```
   SECRET_KEY=<generate with: openssl rand -hex 32>
   DEBUG=False
   PORT=5001
   ```
6. Click "Deploy" and copy your backend URL

#### Step 2: Deploy Frontend to Vercel

1. Go to https://vercel.com
2. Click "New Project" → Import your repository
3. Configure:
   - Root Directory: `frontend`
   - Framework Preset: Create React App
4. Add environment variable:
   ```
   REACT_APP_API_URL=<your-railway-backend-url>
   ```
5. Click "Deploy"

#### Step 3: Configure CORS

In Railway, add to environment variables:
```
ALLOWED_ORIGINS=https://your-vercel-app.vercel.app
```

Done! Your app is now live.

### Option 2: Render + Vercel (Free)

**Best for:** Completely free deployment

Same steps as above, but use https://render.com for backend.

**Note:** Render free tier has cold starts (30-60s first load after inactivity)

### Option 3: Docker

**Best for:** Self-hosting on your own server

```bash
docker-compose up -d
```

---

## Configure Translation Engines

### Google Translate (Default)

Works out of the box - no setup required!

### DeepL (Best Quality)

**Free tier:** 500,000 characters/month

1. Get API key: https://www.deepl.com/pro-api
2. Add to `.env`:
   ```bash
   DEEPL_API_KEY=your-key-here
   ```
3. Restart backend

### Baidu Translate (Chinese)

**Free tier:** 2,000,000 characters/month

1. Get credentials: https://fanyi-api.baidu.com/
2. Add to `.env`:
   ```bash
   BAIDU_APP_ID=your-app-id
   BAIDU_SECRET_KEY=your-secret-key
   ```
3. Restart backend

### Azure Translator

**Free tier:** 2,000,000 characters/month

1. Go to Azure Portal → Create "Translator" resource
2. Get key and region
3. Add to `.env`:
   ```bash
   AZURE_TRANSLATOR_KEY=your-key
   AZURE_TRANSLATOR_REGION=eastasia
   ```
4. Restart backend

---

## Environment Variables Reference

### Required (Production)

```bash
# Backend
SECRET_KEY=<random-string>          # JWT signing key
DEBUG=False                         # Production mode
ALLOWED_ORIGINS=https://your-domain.com  # CORS
PORT=5001                           # Backend port

# Frontend
REACT_APP_API_URL=https://your-backend.com  # Backend URL
```

### Optional (Translation Engines)

```bash
DEEPL_API_KEY=...
BAIDU_APP_ID=...
BAIDU_SECRET_KEY=...
AZURE_TRANSLATOR_KEY=...
AZURE_TRANSLATOR_REGION=...
```

---

## Common Issues & Solutions

### CORS Errors

**Problem:** Frontend can't reach backend

**Solution:** Add your frontend URL to `ALLOWED_ORIGINS` in backend `.env`

```
ALLOWED_ORIGINS=https://your-frontend.vercel.app,https://example.com
```

### Port Already in Use

**Problem:** Backend won't start, port 5001 in use

**Solution:**
```bash
# Find process using port
lsof -i :5001
# Kill it or change PORT in .env
```

### npm install Fails

**Solution:**
```bash
cd frontend
rm -rf node_modules package-lock.json
npm install
```

### pip install Fails

**Problem:** Python version too old

**Solution:** Ensure Python 3.11+
```bash
python --version
```

### Database Locked

**Problem:** `database is locked` error

**Solution:**
```bash
rm backend/translation.db
python init_db.py
```

### Translation Not Working

**Possible causes:**
1. API key invalid → Check key in provider dashboard
2. Quota exceeded → Verify free tier limits
3. Network timeout → Try Google Translate fallback

---

## Project Architecture

```
TranslationTool/
├── frontend/                 # React application
│   └── src/
│       ├── App.js                # Main app + auth state
│       ├── TranslationPage.js    # Core translation UI
│       ├── AuthModal.js          # Login/register
│       └── api.js                # Backend API client
├── backend/                  # Flask application
│   ├── app.py                    # Flask entry point
│   ├── api/                      # API endpoints
│   │   ├── auth.py              # JWT, WeChat OAuth
│   │   ├── translation.py       # Translation + detection
│   │   ├── ocr.py               # OCR processing
│   │   └── tts.py               # Text-to-speech
│   ├── models/
│   │   └── database.py          # Users, history models
│   └── utils/                   # Translation engines
├── start.sh                 # Auto-setup script
├── configure.sh             # Interactive config
└── docker-compose.yml       # Docker deployment
```

**Tech Stack:**
- Frontend: React 18 + Create React App
- Backend: Flask (Python 3.11+) + SQLite
- Deployment: Vercel (frontend), Railway/Render (backend)
- Translation: Google, DeepL, Baidu, Azure, Youdao, OpenAI

---

## Features Overview

### Multi-Language Translation
- 100+ languages supported
- Automatic language detection
- Manual language selection

### OCR Translation
- Extract text from images
- Translate extracted text
- Supports PNG, JPG, JPEG

### Voice Features
- **Voice Input:** Real-time speech recognition (Chrome/Safari)
- **Voice Output:** Text-to-speech playback

### User System
- Email/password authentication
- JWT token-based sessions
- Translation history per user
- WeChat OAuth (optional)

---

## Reference Docs

For detailed information:

- **[Deployment Guide](references/deployment.md)** - Platform-specific setup steps
- **[Configuration Guide](references/configuration.md)** - Detailed API key setup
- **[Troubleshooting](references/troubleshooting.md)** - More common issues and fixes

---

## Getting Help

If you encounter issues:

1. Check the reference docs above
2. Review environment variables
3. Check backend/frontend logs
4. Test API endpoints directly
5. Open an issue on GitHub

**Project Links:**
- TranslationTool: https://github.com/wangjueying/TranslationTool
- This Skill: https://github.com/wangjueying/translation-tool-skill
