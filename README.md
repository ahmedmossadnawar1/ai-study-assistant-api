# AI Study Assistant API

A FastAPI backend that turns study documents into learning materials with Mistral AI: summaries, quizzes, mind maps, question banks, and document chat. A Streamlit web UI is included.

## Features

- Multi-format document processing: PDF, Word (.docx), PowerPoint (.pptx), images (OCR), and plain text
- Summaries, quizzes, mind maps, and question banks generated from uploaded documents
- Chat with your documents (single and batch Q&A)
- Bilingual output: English and Arabic
- Deployment files for Render included (Procfile, runtime.txt)

## Tech Stack

- Python, FastAPI, Uvicorn
- Mistral AI (chat and OCR)
- Streamlit (web UI)
- python-docx, python-pptx

## Project Structure

```
├── fastapi_app.py      # FastAPI server (main backend)
├── FinalApp.py         # Streamlit web UI
├── utils.py            # Document processing and AI helpers
├── config.py           # Environment-based configuration
├── start_server.py     # Helper script to launch the server
├── requirements.txt    # API server dependencies
└── .env.example        # Configuration template
```

## Getting Started

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

### 2. Configure your API key

```bash
cp .env.example .env
# Add your Mistral API key to .env
```

You can get an API key from https://console.mistral.ai/

### 3. Run the API server

```bash
python fastapi_app.py
```

The API is available at http://localhost:8000 with interactive docs at http://localhost:8000/docs

### 4. Run the web UI (optional)

```bash
pip install streamlit reportlab arabic-reshaper python-bidi
streamlit run FinalApp.py
```

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | API info |
| `/health` | GET | Health check |
| `/initialize` | POST | Set the Mistral API key at runtime |
| `/extract` | POST | Extract text from an uploaded document |
| `/summary` | POST | Generate a summary |
| `/quiz` | POST | Generate a quiz |
| `/mindmap` | POST | Generate a mind map |
| `/question-bank` | POST | Generate a question bank |
| `/generate-all` | POST | Generate all learning materials at once |
| `/chat` | POST | Chat with a document |
| `/batch-chat` | POST | Ask multiple questions at once |

## Deployment

The repo includes a `Procfile` and `runtime.txt` for deployment on Render. Set `MISTRAL_API_KEY` in your hosting service's environment variables.

## Documentation

- `SETUP.md`: full installation and troubleshooting guide
- `API_README.md`: detailed endpoint reference with examples
- `QUICK_REFERENCE.md`: copy-paste request snippets
