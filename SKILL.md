---
name: translation-tool
description: Help with TranslationTool deployment and setup. TranslationTool is a React + Flask translation app. Use when user asks about deploying it, configuring API keys (DeepL, Baidu, Azure, etc.), fixing CORS issues, or setting up the project locally.
---

# TranslationTool Help Guide

TranslationTool is a translation app I built with React frontend and Flask backend. It supports multiple translation engines, OCR, and voice features.

## Quick Start

**Local setup:**
```bash
./start.sh  # This sets up everything automatically
```

Or manually:
```bash
# Backend
cd backend && python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python init_db.py && python app.py

# Frontend (new terminal)
cd frontend && npm install && npm start
```

Access at http://localhost:3000

## Deployment Options

### Easiest: Vercel + Railway (~5/month)
- Vercel for frontend (free)
- Railway for backend (5 free credit)
- Push to GitHub and connect both platforms
- Set REACT_APP_API_URL in Vercel
- Set ALLOWED_ORIGINS in Railway (add your Vercel domain)

### Free Option: Render + Vercel
- Same as above but use Render for backend
- Render has cold starts (30-60s first load)
- Otherwise same setup process

### Docker
```bash
docker-compose up -d
```

## Translation Engines

Google Translate works out of the box (no setup needed).

For better quality, add API keys to `.env`:

**DeepL** (recommended for quality):
```bash
DEEPL_API_KEY=your-key
```

**Baidu** (good for Chinese):
```bash
BAIDU_APP_ID=your-id
BAIDU_SECRET_KEY=your-secret
```

**Azure**:
```bash
AZURE_TRANSLATOR_KEY=your-key
AZURE_TRANSLATOR_REGION=eastasia
```

Get keys from:
- DeepL: https://www.deepl.com/pro-api
- Baidu: https://fanyi-api.baidu.com/
- Azure: https://portal.azure.com/

## Common Issues

**CORS errors**: Add your frontend URL to `ALLOWED_ORIGINS` in backend `.env`

**Port already in use**: Change `PORT` in `.env` or kill the process using the port

**npm install fails**: Try `rm -rf node_modules && npm install`

**pip install fails**: Make sure you're using Python 3.11+

**Database locked**: Restart the backend or delete `backend/translation.db` and re-run `init_db.py`

## Project Structure

```
TranslationTool/
├── frontend/          # React app
│   └── src/
│       ├── App.js
│       ├── TranslationPage.js
│       └── api.js
├── backend/           # Flask app
│   ├── app.py
│   ├── api/           # API endpoints
│   ├── models/        # Database
│   └── utils/         # Translation engines
└── start.sh          # Quick start script
```

## Environment Variables

**Required for production:**
```bash
SECRET_KEY=some-random-string
DEBUG=False
ALLOWED_ORIGINS=https://your-frontend.com
```

**Frontend:**
```bash
REACT_APP_API_URL=https://your-backend.com
```

**Optional (translation engines):**
```bash
DEEPL_API_KEY=...
BAIDU_APP_ID=...
BAIDU_SECRET_KEY=...
AZURE_TRANSLATOR_KEY=...
AZURE_TRANSLATOR_REGION=...
```

## Reference Docs

Check these if you need more details:
- `references/deployment.md` - Deployment guide
- `references/configuration.md` - Translation engine setup
- `references/troubleshooting.md` - Common fixes
