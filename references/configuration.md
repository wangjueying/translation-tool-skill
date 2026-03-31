# TranslationTool Configuration Guide

This reference contains detailed configuration instructions for TranslationTool.

## Quick Configuration

### Interactive Setup

Run the provided configuration script:
```bash
./configure.sh
```

This will guide you through setting up:
- Translation engines
- API keys
- Authentication options
- Environment variables

### Manual Configuration

Edit `.env` file in the project root:
```bash
cp .env.example .env
# Edit .env with your settings
```

---

## Translation Engine Configuration

### Default: Google Translate (No API Key Required)

Google Translate is the default engine and works without any configuration. It's free and supports 100+ languages.

**Advantages**:
- No setup required
- Unlimited usage
- Good quality for most languages

**Limitations**:
- Rate limiting may occur with heavy use
- No official API (uses googletrans library)

### DeepL (Recommended for Quality)

DeepL provides high-quality translations, especially for European languages.

**Free Tier**: 500,000 characters/month

**Setup**:
1. Go to [DeepL Developer](https://www.deepl.com/pro-api)
2. Sign up for free account
3. Get API key from account settings
4. Add to `.env`:
```bash
DEEPL_API_KEY=your-api-key-here
```

**Best for**: European languages, formal documents

### Baidu Translate (Good for Chinese)

Baidu is excellent for Chinese-English translation and Asian languages.

**Free Tier**: 2,000,000 characters/month

**Setup**:
1. Go to [Baidu Translate Open Platform](https://fanyi-api.baidu.com/)
2. Create app and get credentials
3. Add to `.env`:
```bash
BAIDU_APP_ID=your-app-id
BAIDU_SECRET_KEY=your-secret-key
```

**Best for**: Chinese, Asian languages

### Azure Translator (Enterprise-Grade)

Microsoft's translation service, very stable and reliable.

**Free Tier**: 2,000,000 characters/month

**Setup**:
1. Go to [Azure Portal](https://portal.azure.com/)
2. Create "Translator" resource
3. Get key and region from portal
4. Add to `.env`:
```bash
AZURE_TRANSLATOR_KEY=your-key
AZURE_TRANSLATOR_REGION=your-region
```

**Best for**: Enterprise, stability, many languages

### Youdao Translate (Chinese Focus)

NetEase's translation service, optimized for Chinese.

**Free Tier**: 1,000,000 characters/month

**Setup**:
1. Go to [Youdao AI](https://ai.youdao.com/)
2. Create translation app
3. Get app key and secret
4. Add to `.env`:
```bash
YOUDAO_APP_KEY=your-app-key
YOUDAO_SECRET_KEY=your-secret-key
```

**Best for**: Chinese translation

### OpenAI via OpenRouter (AI Translation)

Use AI models for translation through OpenRouter.

**Free Models Available**:
- `xiaomi/mimo-v2-flash:free`
- Other free models available

**Setup**:
1. Go to [OpenRouter](https://openrouter.ai/)
2. Get API key
3. Add to `.env`:
```bash
OPENROUTER_API_KEY=your-key
OPENROUTER_MODEL=xiaomi/mimo-v2-flash:free
```

**Best for**: Context-aware translation, creative content

### LibreTranslate (Self-Hosted Alternative)

Open-source translation engine. Can use public instances or self-host.

**Public Instance** (no setup required):
```bash
LIBRETRANSLATE_URL=https://libretranslate.com
```

**Self-hosted**: See [LibreTranslate GitHub](https://github.com/LibreTranslate/LibreTranslate)

---

## Authentication Configuration

### JWT Token Authentication (Default)

**Required**:
```bash
SECRET_KEY=your-secret-key
```

Generate a secure key:
```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

### WeChat OAuth (Optional)

Enable WeChat login:

```bash
WECHAT_APP_ID=your-wechat-app-id
WECHAT_APP_SECRET=your-wechat-app-secret
```

**Setup**:
1. Go to [WeChat Open Platform](https://open.weixin.qq.com/)
2. Create web application
3. Get App ID and Secret
4. Add callback URL to allowed domains

---

## Server Configuration

### Backend Settings

```bash
# Flask
PORT=5001                      # Backend port
DEBUG=False                    # Disable in production
API_TIMEOUT=30                 # API request timeout (seconds)

# CORS
ALLOWED_ORIGINS=*              # Comma-separated domains in production
```

### Frontend Settings

Create `frontend/.env`:
```bash
REACT_APP_API_URL=http://localhost:5001  # Backend URL
```

For production, use your deployed backend URL:
```bash
REACT_APP_API_URL=https://your-backend.railway.app
```

**Important**: Frontend env vars must start with `REACT_APP_` to be available in the app.

---

## Engine Selection Priority

When multiple engines are configured, TranslationTool uses this priority:

1. **DeepL** (if key available) - Best quality
2. **Azure** (if key available) - Most stable
3. **Baidu** (if key available) - Good for Chinese
4. **Youdao** (if key available) - Good for Chinese
5. **OpenAI** (if key available) - Context-aware
6. **Google Translate** (default) - Always available

To force a specific engine, modify the translation API call in `frontend/src/TranslationPage.js` or `backend/api/translation.py`.

---

## Testing Configuration

### Test Translation Engines

After configuration, test each engine:

```bash
# Start backend
cd backend && python app.py

# Test with curl
curl -X POST http://localhost:5001/api/translate \
  -H "Content-Type: application/json" \
  -d '{"text": "Hello world", "target_lang": "zh"}'
```

### Test Authentication

```bash
# Register user
curl -X POST http://localhost:5001/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email": "test@example.com", "password": "password123"}'

# Login
curl -X POST http://localhost:5001/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "test@example.com", "password": "password123"}'
```

---

## Configuration Troubleshooting

### API Key Not Working

**Symptoms**: Translation fails, returns 401/403 errors

**Solutions**:
1. Verify API key is correct (no extra spaces)
2. Check API key hasn't expired
3. Confirm billing/credits are active
4. Test API key directly with provider's tools

### Engine Timeout

**Symptoms**: Translation takes too long, then fails

**Solutions**:
1. Increase `API_TIMEOUT` in `.env` (default 30s)
2. Try different translation engine
3. Check network connectivity to API provider

### Language Detection Issues

**Symptoms**: Wrong language detected, poor translation

**Solutions**:
1. Manually specify source language
2. Try different engine (some better at detection)
3. For mixed language text, use manual selection

### Frontend Can't Connect to Backend

**Symptoms**: Network errors, CORS errors in browser console

**Solutions**:
1. Check `REACT_APP_API_URL` in frontend `.env`
2. Verify `ALLOWED_ORIGINS` includes frontend domain
3. Ensure backend is running on correct port
4. Check browser console for specific CORS errors

---

## Configuration Best Practices

### Development
- Use Google Translate (no setup)
- Keep `DEBUG=True` for detailed error messages
- Use `ALLOWED_ORIGINS=*` for local testing

### Production
- Use at least one paid engine (DeepL recommended)
- Set `DEBUG=False`
- Set specific `ALLOWED_ORIGINS`
- Generate strong `SECRET_KEY`
- Use environment variables, never commit `.env`

### Cost Optimization
- Start with free tiers
- Monitor usage in provider dashboards
- Set up alerts for quota limits
- Cache frequently translated text

---

## Getting API Keys

Quick links to get API keys:

| Service | Link | Free Tier |
|---------|------|-----------|
| DeepL | https://www.deepl.com/pro-api | 500K chars/month |
| Baidu | https://fanyi-api.baidu.com/ | 2M chars/month |
| Azure | https://portal.azure.com/ | 2M chars/month |
| Youdao | https://ai.youdao.com/ | 1M chars/month |
| OpenRouter | https://openrouter.ai/ | Varies by model |
