# JanSamarth AI — Public Scheme Eligibility Checker

An open-source GovTech prototype inspired by the JanSamarth AI presentation. It provides a small web interface, profile-based sample rule checks, a basic text relevance search, document checklists, and links to official scheme-discovery resources.

> **Important:** The bundled scheme records are explicitly marked DEMO. They are not official scheme records and must not be used to decide real-world eligibility. Replace them with verified, current records from official sources before any public deployment.

## Features

- Responsive citizen-facing web interface.
- Python + FastAPI backend.
- Profile fields from the presentation: age, state, annual household income, category, occupation.
- Transparent, deterministic sample rule checks.
- Basic keyword relevance ranking for a user's search.
- Document checklist display and official portal links.
- Optional browser speech recognition (availability and language support depend on the browser).
- No paid API key required.
- No profile database: submitted profile data is processed for the request and is not persisted by this application.

## Requirements

- Python 3.11 recommended (as in the presentation).
- Git, if you want to push the project to GitHub.
- A modern browser.

## Run locally on Windows

Open Command Prompt or PowerShell in the `jansamarth-ai` folder.

### 1. Create and activate a virtual environment

```powershell
py -3.11 -m venv .venv
.venv\Scripts\activate
```

If Python 3.11 is not installed, use `python -m venv .venv` with a supported Python version.

### 2. Install packages

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 3. Start the server

```powershell
uvicorn app.main:app --reload
```

Open these addresses in your browser:

- App: http://127.0.0.1:8000
- Interactive API docs: http://127.0.0.1:8000/docs
- Health check: http://127.0.0.1:8000/api/health

Press `Ctrl+C` in the terminal to stop the server.

## Project structure

```text
jansamarth-ai/
├── app/
│   ├── __init__.py
│   └── main.py
├── data/
│   └── schemes.json
├── web/
│   ├── index.html
│   ├── style.css
│   └── app.js
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

## How the prototype works

1. The frontend collects the five profile fields plus an optional search phrase.
2. FastAPI validates the profile.
3. The backend checks the profile against explicit fields in `data/schemes.json`.
4. If a search phrase is present, a transparent keyword relevance score ranks records.
5. The interface displays rule checks, document reminders, source status, and links.

This version is a **RAG-ready baseline**, not a complete embedding-based RAG pipeline. It does not use an LLM or vector database yet. That keeps the first version straightforward to install and run. A later milestone can add official PDF ingestion, chunking, embeddings, ChromaDB, source citations, and an open-weight LLM.

## Adding or editing scheme records

Edit `data/schemes.json`. Each record contains:

- `id`, `name`, `department`, `summary`
- `keywords`
- `eligibility`: supported demo fields include `age_min`, `age_max`, `annual_income_max`, `states`, `categories`, and `occupations`
- `documents`
- `application_url`, `official_source`, `last_verified`, `disclaimer`

Use official sources and record the source URL and verification date. Do not infer eligibility rules from an unofficial summary. Add tests and a review process before using new rules.

## Next milestones

1. Replace demo records with verified official scheme metadata.
2. Add ingestion for official PDFs and record page-level citations.
3. Add multilingual retrieval and translation with a model whose license permits the intended use.
4. Add embeddings and ChromaDB for semantic retrieval.
5. Add an LLM only for explaining retrieved official passages, never as the sole eligibility decision-maker.
6. Add tests, audit logs that avoid sensitive personal data, accessibility testing, and human review.

## Push this project to your existing GitHub repository

1. Extract the ZIP and open a terminal in the `jansamarth-ai` folder.
2. Configure Git identity if you have not already:

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "you@example.com"
   ```

3. Check whether this folder is already a Git repository:

   ```bash
   git status
   ```

4. If `git status` says it is **not** a Git repository, initialize it:

   ```bash
   git init
   git branch -M main
   ```

5. Add your existing GitHub repository as `origin` only if it is not already configured:

   ```bash
   git remote -v
   git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
   ```

   Replace the URL with your repository's actual URL. If `origin` already exists, do not add it again. To change it, use `git remote set-url origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git`.

6. Stage and commit the files:

   ```bash
   git add .
   git commit -m "Add JanSamarth AI public scheme checker"
   ```

7. Push to GitHub:

   ```bash
   git push -u origin main
   ```

### If your existing GitHub repository already contains files

Before pushing, check the remote repository's default branch and whether it already has commits. If the remote has a README or other commits not present locally, fetch and reconcile the histories rather than force-pushing:

```bash
git fetch origin
git pull origin main --allow-unrelated-histories
```

Resolve any merge conflicts, commit the merge if Git asks, then push. If your existing branch is not named `main`, use that branch name instead. Never use `git push --force` just to get past an error, because it can overwrite remote work.

### If Git asks you to sign in

Follow GitHub's browser sign-in flow or use GitHub's supported authentication method. Do not put passwords or access tokens in source code, README files, or commits.

## License

MIT. See `LICENSE`. The code license does not change the reuse terms of third-party datasets, government documents, logos, or linked sites.
