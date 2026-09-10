# Exam Preparation Chatbot

A standalone Flask + Gemini 3.1 Flash-Lite domain-specific chatbot.

## Features
- No login or registration.
- Temporary, per-browser/device session chat history using Flask-Session.
- Conversations are isolated by Flask session cookies; one session does not read another session's history.
- Gemini API key can be supplied through `.env` or `config.py`.
- Domain restriction is included in the Gemini system instruction.
- Responsive modern UI for mobile, tablet, laptop and desktop.
- Title, domain, prompt, behavior, welcome message, colors, UI radius and PORT are configurable in `config.py`.
- Render/Gunicorn ready.

## 1. Install

```bash
python -m venv venv
# Windows:
venv\Scripts\activate
# macOS/Linux:
# source venv/bin/activate

pip install -r requirements.txt
```

## 2. Configure Gemini

Edit `.env`:

```env
GEMINI_API_KEY=YOUR_REAL_GEMINI_API_KEY
GEMINI_MODEL=gemini-3.1-flash-lite
FLASK_SECRET_KEY=use-a-long-random-secret
PORT=5000
```

Do not publish your real API key.

## 3. Run

```bash
python app.py
```

Open `http://127.0.0.1:5000`.

## 4. Render deployment

Create a Render Web Service from this project.

Build Command:
```bash
pip install -r requirements.txt
```

Start Command:
```bash
gunicorn app:app
```

Add these environment variables in Render:
- `GEMINI_API_KEY`
- `GEMINI_MODEL=gemini-3.1-flash-lite`
- `FLASK_SECRET_KEY`

Render supplies `PORT` automatically; the app reads it. The app also supports a custom `PORT` locally.

## 5. Customize

Edit `config.py`:
- `TITLE`
- `DOMAIN`
- `SYSTEM_PROMPT`
- `BEHAVIOR`
- `WELCOME_MESSAGE`
- `COLORS`
- `UI`
- `PORT`

You can create another domain chatbot by changing these values and keeping the same `app.py`.

## Privacy/session note

This implementation stores temporary chat messages server-side in Flask-Session's filesystem session store. Each browser session gets its own signed session cookie and session record. The model is sent only the current session's recent messages.

For production with multiple Render instances, replace the filesystem session backend with a shared server-side session store such as Redis so sessions remain available across instances. Render's local filesystem is not intended as a durable multi-instance database.

## Domain restriction

The chatbot is instructed to refuse questions outside the configured domain. Domain restriction is prompt-based; it is not a security boundary. For high-assurance applications, add a separate domain classifier/allowlist before sending requests to Gemini.
