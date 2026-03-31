# TranslationTool Troubleshooting Guide

This reference contains solutions to common issues when working with TranslationTool.

## Quick Diagnostic Commands

```bash
# Check if backend is running
curl http://localhost:5001/health

# Check frontend can reach backend
curl $REACT_APP_API_URL/api/translate \
  -H "Content-Type: application/json" \
  -d '{"text": "test", "target_lang": "zh"}'

# Check database exists
ls -la backend/translation.db

# Check environment variables
env | grep -E "(SECRET_KEY|API_|PORT|REACT_APP)"
```

---

## Local Development Issues

### Backend Won't Start

**Symptoms**: `python app.py` fails with error

**Common causes & solutions**:

1. **Port already in use**
   ```bash
   # Find process using port 5001
   lsof -i :5001
   # Kill it or change PORT in .env
   ```

2. **Missing dependencies**
   ```bash
   cd backend
   pip install -r requirements.txt
   ```

3. **Python version too old**
   - Requires Python 3.11+
   - Check: `python --version`
   - Install: https://www.python.org/downloads/

4. **Database corruption**
   ```bash
   rm backend/translation.db
   python init_db.py
   ```

### Frontend Won't Start

**Symptoms**: `npm start` fails

**Common causes & solutions**:

1. **Port 3000 in use**
   - React will prompt to use different port
   - Or kill process: `lsof -i :3000`

2. **Missing dependencies**
   ```bash
   cd frontend
   npm install
   ```

3. **Node version too old**
   - Requires Node.js 18+
   - Check: `node --version`
   - Install: https://nodejs.org/

### CORS Errors in Browser

**Symptoms**: Browser console shows CORS policy errors

**Solutions**:
1. Check backend `.env` has `ALLOWED_ORIGINS=http://localhost:3000`
2. Restart backend after changing `.env`
3. Verify no typos in URL
4. Check backend logs for CORS-related messages

---

## Deployment Issues

### Railway Deployment

**Issue**: Build fails

**Solutions**:
1. Check Python version in `railway.json` (should be 3.11+)
2. Verify all dependencies in `requirements.txt`
3. Check build logs for specific errors

**Issue**: Backend deployed but frontend can't connect

**Solutions**:
1. Verify `REACT_APP_API_URL` is set correctly in Vercel
2. Check `ALLOWED_ORIGINS` in Railway includes Vercel domain
3. Both must use https:// in production
4. Restart backend service after adding CORS origins

### Render Deployment

**Issue**: App keeps sleeping / slow first request

**Cause**: Render free tier spins down after 15 minutes inactivity

**Solution**: This is expected behavior. First request may take 30-60 seconds. Consider upgrading for always-on service.

**Issue**: Health check failing

**Solutions**:
1. Ensure backend responds to root path `/` or `/health`
2. Check start command in Render dashboard
3. Verify Python version matches requirements

### Vercel Deployment

**Issue**: Build fails

**Solutions**:
1. Check Node.js version in `vercel.json` (should be 18+)
2. Verify `package.json` has correct scripts
3. Check build logs for specific errors

**Issue**: Frontend loads but API calls fail

**Solutions**:
1. Check `REACT_APP_API_URL` environment variable is set
2. Must include protocol: `https://` not just domain
3. No trailing slash: `https://backend.com` not `https://backend.com/`
4. Backend CORS must include Vercel domain

### Docker Issues

**Issue**: Container exits immediately

**Solutions**:
1. Check logs: `docker-compose logs`
2. Verify Dockerfile syntax
3. Ensure all files are copied/available
4. Check for port conflicts

**Issue**: Can't access from host

**Solutions**:
1. Verify ports are exposed in docker-compose.yml
2. Check firewall settings
3. Try `localhost:3000` and `127.0.0.1:3000`

---

## Translation Issues

### Translation Returns Empty

**Possible causes**:
1. API key invalid or expired
2. API quota exceeded
3. Network timeout
4. Unsupported language pair

**Solutions**:
1. Check API key in provider dashboard
2. Verify quota/credits
3. Try different translation engine
4. Check if language codes are valid

### Wrong Language Detected

**Solutions**:
1. Manually specify source language in UI
2. Try different translation engine
3. For mixed text, use manual selection
4. Check input text encoding

### Translation Quality Poor

**Solutions**:
1. Try different engine (DeepL usually best)
2. Check if source language is correct
3. For technical text, consider engine-specific terminology
4. Break long text into smaller chunks

### OCR Not Working

**Symptoms**: Image upload fails or returns empty text

**Solutions**:
1. Check Tesseract is installed: `tesseract --version`
2. Verify image format is supported (PNG, JPG, JPEG)
3. Ensure image contains clear, readable text
4. Check file size limits
5. For deployment, verify Tesseract is available in container

---

## Authentication Issues

### Can't Register New User

**Symptoms**: Registration fails silently or with error

**Solutions**:
1. Check database is writable
2. Verify email format is valid
3. Check password meets requirements
4. Check backend logs for specific errors

### Login Fails

**Symptoms**: Wrong password or user not found

**Solutions**:
1. Verify email is correct
2. Check password is correct
3. If database was reset, need to re-register
4. Check JWT_SECRET is consistent (database contains old tokens)

### Token Expired

**Symptoms**: Suddenly logged out, API returns 401

**Solutions**:
1. This is normal security behavior
2. User should log in again
3. Token lifetime can be adjusted in `backend/utils/auth.py`
4. For development, can increase token expiry time

### WeChat Login Not Working

**Solutions**:
1. Verify WeChat App ID and Secret are correct
2. Check callback URL matches WeChat configuration
3. Ensure domain is verified in WeChat dashboard
4. Check network can reach WeChat APIs

---

## Database Issues

### Database Locked

**Symptoms**: "database is locked" errors

**Solutions**:
1. Close all connections to database
2. Check for multiple backend instances running
3. Restart backend
4. As last resort: delete `translation.db` and re-run `init_db.py`

### Schema Migration Issues

**Symptoms**: Columns missing, crashes after code update

**Solutions**:
1. Backend has auto-migration on startup
2. If migration fails, delete database and start fresh
3. For production, backup database before migration
4. Consider using Alembic for production migrations

---

## Performance Issues

### Slow Translation

**Solutions**:
1. Check network latency to API provider
2. Try different translation engine (some faster than others)
3. Break long text into smaller chunks
4. Consider caching frequent translations
5. Check if API provider is having issues

### High Memory Usage

**Solutions**:
1. Check for memory leaks in translation history
2. Implement pagination for history views
3. Clear old database records
4. Check for unclosed database connections

---

## Environment-Specific Issues

### Windows

**Common issues**:
1. Path separators: Use `/` not `\`
2. Virtual environment activation: `venv\Scripts\activate`
3. Tesseract path: Add to system PATH
4. Port conflicts: Check IIS isn't using ports

### macOS

**Common issues**:
1. Python version: macOS comes with old Python 2
2. Install Python 3 from python.org or Homebrew
3. Permission issues: May need to use `sudo` for some operations

### Linux

**Common issues**:
1. Package manager differences (apt vs yum vs dnf)
2. Python 3 might be `python3` not `python`
3. Tesseract installation varies by distribution

---

## Getting Help

If issues persist:

1. **Check logs**: Backend logs show detailed errors
2. **Browser console**: Frontend errors appear here
3. **Network tab**: Check API requests and responses
4. **Test API directly**: Use curl to isolate issues
5. **Minimal reproduction**: Simplify to find root cause

**Useful diagnostic information to collect**:
- Backend logs (last 50 lines)
- Browser console errors
- Network request details (URL, status, response)
- Environment variables (with secrets redacted)
- Steps to reproduce the issue
