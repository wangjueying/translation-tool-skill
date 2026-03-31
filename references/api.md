# TranslationTool API Reference

This reference describes the TranslationTool REST API endpoints.

## Base URL

- **Development**: `http://localhost:5001`
- **Production**: Your deployed backend URL

---

## Authentication

Most endpoints require JWT authentication. Include token in Authorization header:

```
Authorization: Bearer <your-jwt-token>
```

---

## Endpoints

### Authentication (`/api/auth`)

#### POST `/api/auth/register`

Register a new user account.

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "password123",
  "phone": "optional-phone-number"
}
```

**Response** (201):
```json
{
  "message": "User registered successfully",
  "user": {
    "id": 1,
    "email": "user@example.com"
  }
}
```

**Error** (400):
```json
{
  "error": "Email already registered"
}
```

#### POST `/api/auth/login`

Authenticate and receive JWT token.

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response** (200):
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "email": "user@example.com"
  }
}
```

#### POST `/api/auth/wechat`

WeChat OAuth login.

**Request Body**:
```json
{
  "code": "wechat-oauth-code"
}
```

**Response** (200):
```json
{
  "token": "jwt-token",
  "user": {
    "id": 1,
    "wechat_openid": "openid"
  }
}
```

---

### Translation (`/api/translate`)

#### POST `/api/translate`

Translate text from one language to another.

**Request Body**:
```json
{
  "text": "Hello, world!",
  "source_lang": "en",
  "target_lang": "zh",
  "engine": "google"
}
```

**Parameters**:
- `text` (required): Text to translate
- `source_lang` (optional): Source language code (default: "auto")
- `target_lang` (required): Target language code
- `engine` (optional): Translation engine (default: configured engine)

**Language Codes**: Common codes include `en` (English), `zh` (Chinese), `ja` (Japanese), `ko` (Korean), `es` (Spanish), `fr` (French), `de` (German), etc.

**Response** (200):
```json
{
  "translated_text": "你好，世界！",
  "source_lang": "en",
  "target_lang": "zh",
  "engine": "google"
}
```

**Requires Authentication**: Optional (saves history if authenticated)

#### GET `/api/translate/history`

Get translation history for authenticated user.

**Headers**:
```
Authorization: Bearer <token>
```

**Response** (200):
```json
{
  "history": [
    {
      "id": 1,
      "source_text": "Hello",
      "translated_text": "你好",
      "source_lang": "en",
      "target_lang": "zh",
      "timestamp": "2025-03-31T10:00:00Z"
    }
  ]
}
```

---

### OCR (`/api/ocr`)

#### POST `/api/ocr/extract`

Extract text from an image.

**Request**: Multipart form data

```
image: <file>
lang: eng  (optional, Tesseract language code)
```

**Response** (200):
```json
{
  "text": "Extracted text from image",
  "confidence": 0.95
}
```

#### POST `/api/ocr/translate`

Extract text from image and translate it.

**Request**: Multipart form data

```
image: <file>
source_lang: auto
target_lang: zh
```

**Response** (200):
```json
{
  "original_text": "Hello world",
  "translated_text": "你好世界",
  "confidence": 0.95
}
```

#### GET `/api/ocr/history`

Get OCR history for authenticated user.

**Headers**:
```
Authorization: Bearer <token>
```

**Response** (200):
```json
{
  "history": [
    {
      "id": 1,
      "text": "Extracted text",
      "confidence": 0.95,
      "timestamp": "2025-03-31T10:00:00Z"
    }
  ]
}
```

---

### Text-to-Speech (`/api/tts`)

#### POST `/api/tts/synthesize`

Convert text to audio.

**Request Body**:
```json
{
  "text": "Hello world",
  "lang": "en",
  "engine": "google"
}
```

**Response** (200): Audio file (audio/mp3)

**Requires Authentication**: No

---

### User Settings (`/api/settings`)

#### GET `/api/settings`

Get user settings.

**Headers**:
```
Authorization: Bearer <token>
```

**Response** (200):
```json
{
  "default_source_lang": "auto",
  "default_target_lang": "zh",
  "preferred_engine": "google"
}
```

#### PUT `/api/settings`

Update user settings.

**Headers**:
```
Authorization: Bearer <token>
```

**Request Body**:
```json
{
  "default_source_lang": "en",
  "default_target_lang": "zh",
  "preferred_engine": "deepl"
}
```

**Response** (200):
```json
{
  "message": "Settings updated",
  "settings": {
    "default_source_lang": "en",
    "default_target_lang": "zh",
    "preferred_engine": "deepl"
  }
}
```

---

### Health Check

#### GET `/`

Root endpoint, returns API status.

**Response** (200):
```json
{
  "message": "TranslationTool API is running",
  "version": "1.0.0"
}
```

---

## Error Responses

All endpoints may return these error responses:

### 400 Bad Request
```json
{
  "error": "Invalid input data",
  "details": "Field 'text' is required"
}
```

### 401 Unauthorized
```json
{
  "error": "Invalid or expired token"
}
```

### 404 Not Found
```json
{
  "error": "Resource not found"
}
```

### 429 Too Many Requests
```json
{
  "error": "Rate limit exceeded",
  "retry_after": 60
}
```

### 500 Internal Server Error
```json
{
  "error": "Internal server error",
  "details": "error message"
}
```

---

## Language Codes

### Common Languages

| Code | Language |
|------|----------|
| `auto` | Auto-detect |
| `en` | English |
| `zh` | Chinese (Simplified) |
| `zh-TW` | Chinese (Traditional) |
| `ja` | Japanese |
| `ko` | Korean |
| `es` | Spanish |
| `fr` | French |
| `de` | German |
| `it` | Italian |
| `pt` | Portuguese |
| `ru` | Russian |
| `ar` | Arabic |
| `hi` | Hindi |

Full language list depends on translation engine. Google supports 100+ languages.

---

## Rate Limiting

- Unauthenticated requests: 100 requests/hour
- Authenticated requests: 1000 requests/hour
- Individual engines may have their own limits

Rate limit headers are included in responses:
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1635724800
```

---

## Testing the API

### Using curl

```bash
# Translate text
curl -X POST http://localhost:5001/api/translate \
  -H "Content-Type: application/json" \
  -d '{"text": "Hello world", "target_lang": "zh"}'

# Register user
curl -X POST http://localhost:5001/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email": "test@example.com", "password": "pass123"}'

# Login
curl -X POST http://localhost:5001/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "test@example.com", "password": "pass123"}'

# Get history (with token)
curl -X GET http://localhost:5001/api/translate/history \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Using Python

```python
import requests

# Translate
response = requests.post('http://localhost:5001/api/translate', json={
    'text': 'Hello world',
    'target_lang': 'zh'
})
print(response.json())

# With authentication
token = 'your-jwt-token'
headers = {'Authorization': f'Bearer {token}'}
response = requests.get(
    'http://localhost:5001/api/translate/history',
    headers=headers
)
print(response.json())
```
