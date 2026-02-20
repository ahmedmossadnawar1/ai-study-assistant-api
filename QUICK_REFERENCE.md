# 📚 FastAPI Study Assistant - Quick Reference

## 🎯 Core Endpoints

### 1. Initialize 🔑
```
POST /initialize
Body: api_key=your-key
```

### 2. Health Check 🏥
```
GET /health
```

### 3. Extract Text 📤
```
POST /extract
File Upload: multipart/form-data
```

### 4. Generate Summary 📄
```
POST /summary
Body: text, language (en|ar)
```

### 5. Generate Quiz ❓
```
POST /quiz
Body: text, num_questions (1-50), language
```

### 6. Generate Mind Map 🧠
```
POST /mindmap
Body: text, language
```

### 7. Generate Question Bank 📚
```
POST /question-bank
Body: text, num_questions (5-100), language
```

### 8. Generate ALL Features 🚀
```
POST /generate-all
Body: text, filename
Returns: EN + AR for all 4 features
```

### 9. Chat with Document 💬
```
POST /chat
Body: filename, user_message, context, language
```

### 10. Batch Chat 💬💬
```
POST /batch-chat
Body: filename, context, messages (JSON)
```

---

## 🔧 Python Quick Start

```python
import requests

BASE = "http://localhost:8000"

# Initialize
requests.post(f"{BASE}/initialize", data={"api_key": "key"})

# Extract
with open("doc.pdf", "rb") as f:
    r = requests.post(f"{BASE}/extract", files={"file": f})
    text = r.json()["text"]

# Generate
r = requests.post(f"{BASE}/summary", data={"text": text, "language": "en"})
summary = r.json()["summary"]

# Chat
r = requests.post(f"{BASE}/chat", data={
    "filename": "doc.pdf",
    "user_message": "What is X?",
    "context": text,
    "language": "en"
})
answer = r.json()["answer"]
```

---

## 🌐 Browser Access

| Resource | URL |
|----------|-----|
| Interactive Docs | http://localhost:8000/docs |
| Alternative Docs | http://localhost:8000/redoc |
| OpenAPI Spec | http://localhost:8000/openapi.json |
| Health Check | http://localhost:8000/health |

---

## 📝 Response Codes

| Code | Meaning |
|------|---------|
| 200 | Success ✅ |
| 400 | Bad Request ❌ |
| 401 | Unauthorized 🔐 |
| 413 | File Too Large 📦 |
| 500 | Server Error ⚠️ |

---

## 🚀 Run Commands

```bash
# Start Server
python fastapi_app.py

# Test API
python test_api.py

# Test with File
python test_api.py path/to/file.pdf

# Alternative Start
uvicorn fastapi_app:app --reload
```

---

## 🎯 Supported File Types

- 📄 PDF (.pdf)
- 📘 Word (.docx, .doc)
- 🖼️ PowerPoint (.pptx)
- 🖼️ Images (.png, .jpg, .jpeg)
- 📝 Text (.txt)

**Max Size:** 50MB

---

## 🌍 Languages

- ✅ English (en)
- ✅ Arabic (ar)

All features support both languages!

---

## 📊 Feature Distribution

| Feature | Format | Languages | Min-Max Questions |
|---------|--------|-----------|-------------------|
| Summary | Markdown | EN + AR | - |
| Quiz | JSON | EN + AR | 1-50 |
| Mind Map | Text | EN + AR | - |
| Question Bank | Text | EN + AR | 5-100 |
| Chat | Plain Text | EN + AR | - |

---

## ⚡ Tips & Tricks

### Speed Up Processing
- Use smaller files (< 10MB)
- Keep text under 15,000 characters
- Set `CHUNK_SIZE=300` in .env for faster processing

### Better Results
- Use clear, educational documents
- Avoid corrupted PDFs
- High-quality images for OCR
- Clear text for better summaries

### API Rate Limiting
- Mistral API has rate limits
- Space out requests
- Use batch endpoints when possible

---

## 🐛 Common Errors

```
"Mistral API not initialized"
→ Call /initialize first

"File too large"
→ Keep files under 50MB

"No module named..."
→ Run: pip install -r requirements.txt

"Connection refused"
→ Server not running (run: python fastapi_app.py)
```

---

## 📱 Integrations

The API can be easily integrated with:
- ✅ Web Apps (React, Vue, Angular)
- ✅ Mobile Apps (Flutter, React Native)
- ✅ Desktop Apps (Electron, PyQt)
- ✅ CLI Tools
- ✅ Bots (Discord, Telegram)
- ✅ No-Code Tools (Make, Zapier)

---

## 📖 Documentation Files

- **SETUP.md** - Installation & setup guide
- **API_README.md** - Complete API documentation
- **config.py** - Configuration options
- **.env.example** - Environment variables template
- **test_api.py** - Test script with examples

---

## 🔑 Environment Variables

```
MISTRAL_API_KEY=your-key          # Required!
API_PORT=8000                      # Server port
API_HOST=0.0.0.0                   # Server host
CHUNK_SIZE=500                     # Text chunk size
MAX_FILE_SIZE_MB=50                # Max upload size
TEMPERATURE_SUMMARY=0.3            # Summary creativity
TEMPERATURE_QUIZ=0.5               # Quiz creativity
```

---

## 💡 Example Workflow

1. **Initialize**
   ```bash
   POST /initialize?api_key=your-key
   ```

2. **Upload Document**
   ```bash
   POST /extract (multipart file upload)
   ```

3. **Extract Text**
   ```bash
   Get response with extracted text
   ```

4. **Generate Materials**
   ```bash
   POST /generate-all (get everything at once)
   ```

5. **Chat with Document**
   ```bash
   POST /chat (ask questions)
   ```

---

**For detailed documentation, see [API_README.md](./API_README.md)**

**For setup help, see [SETUP.md](./SETUP.md)**

**Last Updated:** February 2025
