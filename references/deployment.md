# Deployment Guide

Quick deployment options for TranslationTool.

## Option 1: Vercel + Railway (Recommended)

This is what I use - takes about 5 minutes.

### Backend on Railway

1. Go to https://railway.app
2. Click "New Project" → "Deploy from GitHub"
3. Select your repo
4. Set Root Directory to `backend`
5. Add env vars:
   ```
   SECRET_KEY=<generate one: openssl rand -hex 32>
   DEBUG=False
   PORT=5001
   ```
6. Deploy and copy the URL

### Frontend on Vercel

1. Go to https://vercel.com
2. "New Project" → Import your repo
3. Set Root Directory to `frontend`
4. Add env var:
   ```
   REACT_APP_API_URL=<your-railway-url>
   ```
5. Deploy

### Fix CORS

In Railway, add to env vars:
```
ALLOWED_ORIGINS=https://your-vercel-app.vercel.app
```

Done.

## Option 2: Render + Vercel (Free)

Same as above but use https://render.com for backend.

Note: Render free tier sleeps after 15min. First request takes 30-60s to wake up.

## Option 3: Docker

```bash
docker-compose up -d
```

## Environment Variables Checklist

**Backend:**
- SECRET_KEY (required)
- DEBUG=False (production)
- ALLOWED_ORIGINS (CORS)
- PORT=5001

**Frontend:**
- REACT_APP_API_URL (your backend URL)

**Optional (API keys):**
- DEEPL_API_KEY
- BAIDU_APP_ID + BAIDU_SECRET_KEY
- AZURE_TRANSLATOR_KEY + AZURE_TRANSLATOR_REGION
