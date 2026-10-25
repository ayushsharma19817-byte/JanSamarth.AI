# GitHub Pages frontend for JanSamarth AI

This `docs/` folder is a static-hosting version of the frontend. GitHub Pages can serve its HTML, CSS, and JavaScript, but it cannot run the Python/FastAPI backend.

## Publish the frontend

1. Push the repository to GitHub.
2. Open the repository's **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select your main branch and the `/docs` folder, then save.
5. Wait for the Pages deployment to finish. GitHub will show the website URL in the Pages settings.

## Connect the FastAPI backend

1. Deploy the existing Python backend (`app/main.py`) to a Python-capable hosting service. Include `app/`, `data/schemes.json`, and `requirements.txt`.
2. Copy the backend's public HTTPS base URL.
3. Edit `docs/app.js` and set `API_BASE_URL` to that URL, with no trailing slash. For example, `const API_BASE_URL = "https://your-backend.example.com";` Replace the example with your real URL.
4. In `app/main.py`, configure CORS to allow the exact GitHub Pages origin for your repository. Do not use a wildcard when sending credentials. This app currently does not need cookies/credentials.
5. Commit and push the changes.

## CORS configuration example

Add `CORSMiddleware` to `app/main.py` after creating `app`. Replace the example origin with the exact URL of your published Pages site (no trailing slash):

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://YOUR-USERNAME.github.io"],
    allow_credentials=False,
    allow_methods=["GET", "POST", "OPTIONS"],
    allow_headers=["Content-Type"],
)
```

If your repository Pages site is served from a path such as `https://YOUR-USERNAME.github.io/YOUR-REPOSITORY/`, the browser origin is just `https://YOUR-USERNAME.github.io` (the origin does not include the repository path).

## Important

- Until `API_BASE_URL` is set to a real deployed backend URL, the dashboard layout will load, but scheme matching will show a configuration message.
- Keep the local FastAPI version under `web/` unchanged; it still uses `/static/...` and same-origin `/api/...` routes.
- The current scheme records are sample data, not a live government database.
- Never put API keys or other secrets in frontend JavaScript; anything in `docs/` is public.
