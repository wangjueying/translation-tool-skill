# TranslationTool Project Structure

This reference provides a detailed breakdown of the TranslationTool codebase.

## Directory Tree

```
TranslationTool/
├── frontend/                 # React frontend application
│   ├── public/              # Static assets
│   ├── src/
│   │   ├── App.js           # Main app component
│   │   ├── TranslationPage.js  # Translation UI
│   │   ├── AuthModal.js     # Login/register modal
│   │   ├── api.js           # API client
│   │   └── styles/          # CSS stylesheets
│   └── package.json         # Frontend dependencies
│
├── backend/                 # Flask backend application
│   ├── api/                 # API route handlers
│   │   ├── auth.py          # Authentication endpoints
│   │   ├── translation.py   # Translation endpoints
│   │   ├── ocr.py           # OCR endpoints
│   │   └── tts.py           # Text-to-speech endpoints
│   ├── models/              # Database models
│   │   └── database.py      # User, History models
│   ├── utils/               # Utility functions
│   │   ├── auth.py          # JWT, password hashing
│   │   ├── translation.py   # Translation engines
│   │   ├── ocr.py           # OCR processing
│   │   ├── tts.py           # TTS engines
│   │   └── validators.py    # Input validation
│   ├── app.py               # Flask application factory
│   ├── config.py            # Configuration loading
│   ├── init_db.py           # Database initialization
│   └── requirements.txt     # Python dependencies
│
├── scripts/                 # Utility scripts
│   ├── start.sh             # Main startup script
│   ├── configure.sh         # Interactive configuration
│   └── create_env.sh        # Environment file creator
│
├── docs/                    # Documentation
│   ├── QUICKSTART.md        # Quick start guide
│   ├── DEPLOY.md            # Deployment guides
│   ├── CONFIG.md            # Configuration guide
│   └── DEPLOY_SIMPLE.md     # Quick deployment
│
├── .env                     # Environment variables (not in git)
├── .env.example             # Environment template
├── docker-compose.yml       # Docker orchestration
├── vercel.json              # Vercel deployment config
├── railway.json             # Railway deployment config
└── render.yaml              # Render deployment config
```

---

## Frontend Structure

### App.js (58 lines)

Main application component. Handles:
- Global authentication state
- Auth modal visibility
- User session management

**Key state**:
```javascript
const [user, setUser] = useState(null);      // Current user
const [showAuthModal, setShowAuthModal] = useState(false);
```

### TranslationPage.js (318 lines)

Core translation interface. Features:
- Text input/output areas
- Language selection with auto-detection
- Voice recording (Web Speech API)
- Audio playback (Google TTS)
- Translation history display
- Language mismatch warnings
- Mixed language detection

**Key functions**:
- `handleTranslate()`: Main translation function
- `handleVoiceInput()`: Speech recognition
- `handleVoiceOutput()`: Text-to-speech
- `swapLanguages()`: Switch source/target

### api.js

Backend API client using `fetch()`.

**Key functions**:
- `translate(text, source_lang, target_lang)`
- `getHistory()`
- `login(email, password)`
- `register(email, password)`

---

## Backend Structure

### app.py (36 lines)

Flask application entry point.

**Setup**:
- CORS configuration
- Route registration
- Error handlers
- Server startup

**Key configuration**:
```python
app.config['SECRET_KEY'] = os.getenv('SECRET_KEY')
app.config['JSON_AS_ASCII'] = False  # Support Unicode
```

### config.py (36 lines)

Configuration management using `python-dotenv`.

**Loads from**:
- `.env` file in project root
- Environment variables

**Key settings**:
- `SECRET_KEY`: JWT signing
- `DEBUG`: Development mode
- `PORT`: Server port
- `API_TIMEOUT`: API request timeout
- Translation engine API keys

---

### API Routes

#### auth.py

Authentication endpoints.

**Functions**:
- `register()`: Create new user
- `login()`: Authenticate and return JWT
- `wechat_login()`: WeChat OAuth

**Security**:
- Password hashing with `werkzeug.security`
- JWT tokens with expiration
- Input validation

#### translation.py

Translation endpoints with intelligent language detection.

**Functions**:
- `translate()`: Main translation handler
- `get_history()`: Fetch user's translation history
- `detect_language()`: Auto-detect source language

**Engine selection**:
Priority order: DeepL → Azure → Baidu → Youdao → OpenAI → Google

#### ocr.py

OCR (Optical Character Recognition) endpoints.

**Functions**:
- `extract_text()`: Extract text from image
- `translate_image()`: Extract and translate

**Processing**:
- Uses `pytesseract` for OCR
- Supports PNG, JPG, JPEG
- Returns confidence scores

#### tts.py

Text-to-Speech endpoints.

**Functions**:
- `synthesize()`: Convert text to audio

**Engines**:
- Google TTS (default)
- gTTS library

---

### Database Models (database.py - 351 lines)

SQLite database with auto-migration.

**Tables**:

1. **users**
   ```sql
   - id (INTEGER PRIMARY KEY)
   - email (VARCHAR UNIQUE)
   - phone (VARCHAR)
   - password_hash (VARCHAR)
   - wechat_openid (VARCHAR)
   - created_at (TIMESTAMP)
   ```

2. **user_settings**
   ```sql
   - id (INTEGER PRIMARY KEY)
   - user_id (INTEGER FOREIGN KEY)
   - default_source_lang (VARCHAR)
   - default_target_lang (VARCHAR)
   - preferred_engine (VARCHAR)
   ```

3. **translation_history**
   ```sql
   - id (INTEGER PRIMARY KEY)
   - user_id (INTEGER FOREIGN KEY)
   - source_text (TEXT)
   - translated_text (TEXT)
   - source_lang (VARCHAR)
   - target_lang (VARCHAR)
   - engine (VARCHAR)
   - timestamp (TIMESTAMP)
   ```

4. **ocr_history**
   ```sql
   - id (INTEGER PRIMARY KEY)
   - user_id (INTEGER FOREIGN KEY)
   - text (TEXT)
   - confidence (FLOAT)
   - timestamp (TIMESTAMP)
   ```

**Auto-migration**: Checks and adds columns on startup

---

### Utility Functions

#### utils/auth.py

Authentication utilities.

**Functions**:
- `generate_token()`: Create JWT
- `verify_token()`: Validate JWT
- `hash_password()`: Hash passwords
- `verify_password()`: Check passwords

#### utils/translation.py

Translation engine implementations.

**Engines**:
- `google_translate()`: Default, no key
- `deepl_translate()`: High quality
- `baidu_translate()`: Chinese optimized
- `azure_translate()`: Enterprise
- `youdao_translate()`: Chinese
- `openai_translate()`: AI-powered

**Common interface**:
```python
def translate(text, source_lang, target_lang, api_key):
    # Returns: translated_text
```

#### utils/ocr.py

OCR processing utilities.

**Functions**:
- `extract_text_from_image()`: Tesseract wrapper
- `preprocess_image()`: Image enhancement
- `get_confidence()`: OCR quality score

#### utils/tts.py

Text-to-speech utilities.

**Functions**:
- `synthesize_speech()`: Generate audio
- `get_supported_languages()`: Available TTS languages
- `save_audio_file()`: Persist audio

#### utils/validators.py

Input validation helpers.

**Validators**:
- `validate_email()`: Email format
- `validate_password()`: Password strength
- `validate_language_code()`: Language code
- `sanitize_input()`: XSS prevention

---

## Deployment Configuration Files

### vercel.json

Vercel deployment configuration.

```json
{
  "version": 2,
  "builds": [
    {
      "src": "frontend/package.json",
      "use": "@vercel/static-build",
      "config": { "distDir": "build" }
    }
  ],
  "routes": [
    { "src": "/(.*)", "dest": "/frontend/$1" }
  ]
}
```

### railway.json

Railway deployment configuration.

```json
{
  "build": {
    "command": "cd backend && pip install -r requirements.txt"
  },
  "deploy": {
    "startCommand": "cd backend && python app.py"
  }
}
```

### render.yaml

Render deployment configuration.

```yaml
services:
  - type: web
    name: translationtool-backend
    env: python
    buildCommand: cd backend && pip install -r requirements.txt
    startCommand: cd backend && python app.py
```

### docker-compose.yml

Local development with Docker.

```yaml
services:
  frontend:
    build: ./frontend
    ports: ["3000:3000"]
  backend:
    build: ./backend
    ports: ["5001:5001"]
    environment:
      - SECRET_KEY=${SECRET_KEY}
```

---

## Environment Variables

### Required (Production)

```bash
SECRET_KEY=your-secret-key
PORT=5001
DEBUG=False
ALLOWED_ORIGINS=https://your-domain.com
```

### Frontend Build

```bash
REACT_APP_API_URL=https://your-backend.com
```

### Optional (Translation Engines)

```bash
DEEPL_API_KEY=key
BAIDU_APP_ID=id
BAIDU_SECRET_KEY=secret
AZURE_TRANSLATOR_KEY=key
AZURE_TRANSLATOR_REGION=region
YOUDAO_APP_KEY=key
YOUDAO_SECRET_KEY=secret
OPENROUTER_API_KEY=key
OPENROUTER_MODEL=model-name
```

---

## Key Dependencies

### Backend (requirements.txt)

- `Flask`: Web framework
- `Flask-CORS`: Cross-origin support
- `PyJWT`: JWT authentication
- `python-dotenv`: Environment variables
- `googletrans`: Google Translate
- `deep-translator`: Multiple engines
- `pytesseract`: OCR
- `Pillow`: Image processing
- `gTTS`: Text-to-speech
- `requests`: HTTP client

### Frontend (package.json)

- `react`: UI framework
- `react-dom`: DOM rendering
- `react-scripts`: Build tooling

---

## Development Workflow

### Local Development

1. Run `./start.sh` (auto-configures environment)
2. Frontend: http://localhost:3000
3. Backend: http://localhost:5001
4. Hot-reload enabled for both

### Making Changes

**Frontend**:
- Edit files in `frontend/src/`
- React auto-reloads on save

**Backend**:
- Edit files in `backend/`
- Flask auto-reloads in development mode
- Restart after adding new dependencies

**Database**:
- Schema auto-migrates on backend startup
- For destructive changes: delete `translation.db` and re-run `init_db.py`
