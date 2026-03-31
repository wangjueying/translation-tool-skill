# TranslationTool Deployment Guide

This reference contains detailed deployment instructions for TranslationTool.

## Deployment Options Comparison

| Platform | Type | Free Tier | Setup Time | Best For |
|----------|------|-----------|------------|----------|
| **Vercel** | Frontend | Unlimited | 2 min | Frontend hosting |
| **Railway** | Backend | $5/month | 3 min | Quick production |
| **Render** | Backend | Free | 5 min | Free deployment |
| **Fly.io** | Backend | Free | 10 min | Docker-based |
| **Docker** | Self-hosted | VPS cost | 5 min | VPS control |

---

## Option 1: Vercel + Railway (Recommended, ~5/month)

### Step 1: Prepare GitHub Repository

```bash
git init
git add .
git commit -m "Initial commit"
# Create repo on GitHub first
git remote add origin https://github.com/YOUR_USERNAME/TranslationTool.git
git push -u origin main
```

### Step 2: Deploy Backend to Railway

1. Go to [railway.app](https://railway.app/)
2. Click "New Project" → "Deploy from GitHub repo"
3. Select your TranslationTool repository
4. Railway will detect the Python backend automatically
5. Set root directory to `backend`
6. Add environment variables:
   - `SECRET_KEY`: Generate a random string (use `python -c "import secrets; print(secrets.token_hex(32))"`)
   - `PORT`: `5001`
   - `API_TIMEOUT`: `30`
   - (Optional) Add translation engine API keys

7. Click "Deploy" - Railway will provide a URL like `https://your-app.railway.app`

### Step 3: Deploy Frontend to Vercel

1. Go to [vercel.com](https://vercel.com/)
2. Click "New Project"
3. Import your GitHub repository
4. Set root directory to `frontend`
5. Configure environment variables:
   - `REACT_APP_API_URL`: Your Railway backend URL (e.g., `https://your-app.railway.app`)
   - Important: Include the protocol (https://) and NO trailing slash

6. Click "Deploy" - Vercel will provide a URL like `https://your-app.vercel.app`

### Step 4: Configure CORS

In your Railway backend, add the environment variable:
- `ALLOWED_ORIGINS`: `https://your-app.vercel.app` (your Vercel URL)

Restart the backend after adding this variable.

---

## Option 2: Render + Vercel (Free, ~10 min)

### Step 1: Deploy Backend to Render

1. Go to [render.com](https://render.com/)
2. Click "New" → "Web Service"
3. Connect your GitHub repository
4. Configure:
   - Name: `translationtool-backend`
   - Environment: `Python 3`
   - Build Command: `cd backend && pip install -r requirements.txt`
   - Start Command: `cd backend && python app.py`

5. Add environment variables (same as Railway above)
6. Click "Deploy Web Service"
7. Render will provide a URL like `https://translationtool-backend.onrender.com`

**Note**: Render free tier spins down after 15 min inactivity. First request after spin-down may take 30-60 seconds.

### Step 2: Deploy Frontend to Vercel

Same steps as Option 1, Step 3, but use your Render backend URL for `REACT_APP_API_URL`.

### Step 3: Configure CORS

In Render, add environment variable:
- `ALLOWED_ORIGINS`: Your Vercel URL

---

## Option 3: Docker Deployment

### Local Development with Docker

```bash
docker-compose up -d
```

This starts both frontend (port 3000) and backend (port 5001).

### Production Docker Deployment

#### To Fly.io

```bash
# Install flyctl
curl -L https://fly.io/install.sh | sh

# Login
flyctl auth login

# Launch backend
cd backend
flyctl launch

# Set environment variables in fly.toml or via:
flyctl secrets set SECRET_KEY=your-key PORT=5001

# Deploy
flyctl deploy
```

#### To Any VPS (with Docker)

1. Copy project to server
2. Edit `docker-compose.yml` to expose ports
3. Run: `docker-compose up -d`
4. Set up reverse proxy (nginx) for SSL

---

## Environment Variables Checklist

### Required for Production

```bash
# Backend
SECRET_KEY=your-generated-secret-key
PORT=5001
API_TIMEOUT=30

# CORS
ALLOWED_ORIGINS=https://your-frontend-domain.com
```

### Frontend Build Variables

```bash
REACT_APP_API_URL=https://your-backend-domain.com
```

### Optional Translation Engines

```bash
# DeepL
DEEPL_API_KEY=your-key

# Baidu
BAIDU_APP_ID=your-id
BAIDU_SECRET_KEY=your-secret

# Azure
AZURE_TRANSLATOR_KEY=your-key
AZURE_TRANSLATOR_REGION=eastasia

# Youdao
YOUDAO_APP_KEY=your-key
YOUDAO_SECRET_KEY=your-secret

# OpenAI via OpenRouter
OPENROUTER_API_KEY=your-key
OPENROUTER_MODEL=xiaomi/mimo-v2-flash:free
```

---

## Troubleshooting Deployment

### CORS Errors

**Symptom**: Frontend can't reach backend, browser shows CORS error

**Solution**: Add your frontend domain to `ALLOWED_ORIGINS` in backend environment variables. Multiple domains: `https://site1.com,https://site2.com`

### Build Failures

**Symptom**: Deployment fails during build

**Solutions**:
- Vercel: Check Node.js version in `vercel.json` (should be 18+)
- Railway/Render: Check Python version (should be 3.11+)
- Verify all dependencies in `requirements.txt` and `package.json`

### Database Issues

**Symptom**: App crashes with database error

**Solution**: SQLite is auto-created on first run. For production, consider PostgreSQL:
- Add `DATABASE_URL` environment variable
- Install `psycopg2-binary` in requirements.txt

### Environment Variables Not Working

**Symptom**: API keys not being used

**Solution**:
1. Check variable names match exactly (case-sensitive)
2. Restart backend after adding variables
3. Verify no extra spaces in values
4. For frontend: Variables must start with `REACT_APP_`

---

## Post-Deployment Checklist

- [ ] Frontend loads at your domain
- [ ] Backend health check responds (add `/health` endpoint)
- [ ] Translation works without errors
- [ ] CORS is properly configured
- [ ] User registration/login works
- [ ] Translation history saves correctly
- [ ] Voice features work (if browser supported)
- [ ] OCR can process images
- [ ] All configured translation engines work

---

## Platform-Specific Tips

### Railway
- Auto-detects Python from `requirements.txt`
- Provides built-in metrics
- Easy environment variable management
- $5 free credit monthly

### Render
- Free tier has cold starts (30-60s)
- No automatic SSL for free tier (use CNAME)
- Good for hobby projects

### Vercel
- Automatic HTTPS
- Preview deployments for Git branches
- Edge caching for static assets
- Unlimited bandwidth on free tier

### Fly.io
- Global deployment
- Built-in secrets management
- Supports Docker directly
- Free tier: 3 small VMs
