# Troubleshooting

## Local Development

**Backend won't start**
```bash
# Check port 5001
lsof -i :5001

# Python version wrong?
python --version  # Should be 3.11+
```

**Frontend won't start**
```bash
# Clean install
cd frontend
rm -rf node_modules
npm install
```

**CORS error**
- Check `ALLOWED_ORIGINS` in backend `.env`
- Should match your frontend URL exactly

## Deployment

**Build fails**
- Check Node.js version (needs 18+)
- Check Python version (needs 3.11+)

**Can't connect to backend**
- Verify `REACT_APP_API_URL` in frontend
- Check backend is actually running
- Check CORS settings

**API not working**
- Test the API key
- Check quota limits
- Try Google Translate (no key needed)

## Database

**Database locked**
```bash
rm backend/translation.db
python init_db.py
```

## Quick Checks

```bash
# Backend health
curl http://localhost:5001/

# Test API
curl -X POST http://localhost:5001/api/translate \
  -H "Content-Type: application/json" \
  -d '{"text": "hello", "target_lang": "zh"}'
```
