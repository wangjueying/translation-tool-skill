# Configuration

## Translation Engines

Google Translate works by default - no setup needed.

For better quality, add API keys to `.env`:

### DeepL (Best Quality)

Free: 500K chars/month

1. Get key: https://www.deepl.com/pro-api
2. Add to `.env`:
   ```
   DEEPL_API_KEY=your-key
   ```

### Baidu (Good for Chinese)

Free: 2M chars/month

1. Get key: https://fanyi-api.baidu.com/
2. Add to `.env`:
   ```
   BAIDU_APP_ID=your-id
   BAIDU_SECRET_KEY=your-secret
   ```

### Azure

Free: 2M chars/month

1. Go to Azure Portal → Create "Translator" resource
2. Get key and region
3. Add to `.env`:
   ```
   AZURE_TRANSLATOR_KEY=your-key
   AZURE_TRANSLATOR_REGION=eastasia
   ```

## Quick Setup

Run:
```bash
./configure.sh
```

Or edit `.env` directly.

## Engine Priority

When multiple are configured:
1. DeepL
2. Azure
3. Baidu
4. Google (default fallback)
