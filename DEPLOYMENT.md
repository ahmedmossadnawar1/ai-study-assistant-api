# Deploying this API on Render.com

This repo already includes the files Render needs: `Procfile`, `runtime.txt`, `requirements.txt`. Steps follow Render's docs (https://render.com/docs); this guide has not been verified with a live deployment.

## Deploy

1. In the Render dashboard: `New +` > `Web Service`, connect your GitHub account, and select this repository.
2. Configure the service:
   - Runtime: `Python 3`
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `python -m uvicorn fastapi_app:app --host 0.0.0.0 --port $PORT` (already in the `Procfile`)
3. Pick an instance type. Render offers a free instance type for web services with limitations (not for production; idle services can sleep and take a moment to wake). See current terms: https://render.com/docs/free
4. Add environment variables under `Environment`:
   - `MISTRAL_API_KEY` = your Mistral API key (required for the AI features)
   - `LOG_LEVEL` = `info`
   - `MAX_FILE_SIZE_MB` = `50`
5. Create the service. The first deploy can take several minutes; watch the logs until it reports deployed.
6. Test: open `https://<your-service>.onrender.com/health`. It returns `{"status": "ok", ...}` and whether the Mistral client initialized. Interactive API docs are at `/docs`.

## The API key

The server reads the key from the `MISTRAL_API_KEY` environment variable at startup, or accepts one at runtime through the `/initialize` form field. No endpoint returns the key (checked every `api_key` reference in the server code). Keep it in Render's environment settings, not in the repo.

## Updates

Push to `main`; Render redeploys automatically.
