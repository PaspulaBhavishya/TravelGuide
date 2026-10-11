# AI Travel Guide

An AI-powered travel companion that generates destination descriptions and converts them into audio guides in multiple languages.

## Features

- Explore popular destinations, including the Taj Mahal, Red Fort, Gateway of India, Hawa Mahal, Golden Temple, and Mysore Palace.
- Generate a short summary or a more detailed destination guide.
- Choose English, Hindi, Tamil, or Telugu.
- Choose a male or female voice for the audio guide.
- Read the generated transcript and listen to the generated audio.

## Tech Stack

- **Frontend:** HTML, JavaScript, Tailwind CSS (loaded from CDN)
- **Backend:** Python, Flask, Flask-CORS
- **AI text generation:** Google Gemini API (`google-genai`)
- **Text-to-speech:** Murf AI API
- **Production server:** Gunicorn

## Project Structure

```text
AI-Travel-Guide/
├── frontend/
│   ├── index.html
│   └── index.js
├── backend/
│   ├── app.py
│   └── requirements.txt
├── .gitignore
└── README.md
```

## Requirements

- Python 3.10 or a compatible version supported by the installed dependencies
- A Google Gemini API key
- A Murf API key
- Internet access for the external APIs and CDN assets

## Run Locally

### 1. Open the project directory

Open a terminal in the root `AI-Travel-Guide` folder.

### 2. Create and activate a virtual environment

**Windows (PowerShell):**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, use Command Prompt instead:

```bat
python -m venv .venv
.\.venv\Scripts\activate.bat
```

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install backend dependencies

```bash
pip install -r backend/requirements.txt
```

### 4. Configure API keys

Create a file named `.env` inside the `backend/` folder:

```env
GEMINI_API_KEY=your_gemini_api_key_here
MURF_API_KEY=your_murf_api_key_here
```

Replace the example values with your actual keys. Never commit `.env` or publish API keys in your repository.

### 5. Start the application

From the project root, run:

```bash
python backend/app.py
```

Open the local address printed in the terminal (usually `http://127.0.0.1:5000`).

> If `app.py` uses `FRONTEND_DIR` to locate the `frontend` folder, make sure it points to `../frontend` relative to `backend/app.py`.

## Deploy to Render

This project can serve the frontend and API from the same Flask application, so a separate frontend host is not required.

1. Push the project to a GitHub repository. Include `frontend/`, `backend/`, `.gitignore`, and this `README.md`.
2. In the Render dashboard, create a **Web Service** and connect the repository.
3. Set **Root Directory** to `backend`.
4. Set **Build Command** to:

   ```bash
   pip install -r requirements.txt
   ```

5. Set **Start Command** to:

   ```bash
   gunicorn app:app
   ```

6. Add these environment variables in Render:
   - `GEMINI_API_KEY` — your Google Gemini API key
   - `MURF_API_KEY` — your Murf API key
7. Deploy the service and open the public URL provided by Render.

### Before deploying

- In `backend/app.py`, ensure `FRONTEND_DIR` points to the sibling `frontend` directory:

  ```python
  FRONTEND_DIR = os.path.abspath(
      os.path.join(os.path.dirname(__file__), "..", "frontend")
  )
  ```

- In `frontend/index.js`, use a same-origin API endpoint if the frontend and backend are served by the same Flask app:

  ```javascript
  const GENERATE_AUDIO_GUIDE_API_URL = "/generate-audio-guide";
  ```

- Confirm the service starts successfully in Render logs.
- Keep API keys in environment variables, not in source code.

## Troubleshooting

### The app says an API key is not configured

Check that `GEMINI_API_KEY` and `MURF_API_KEY` are set in the environment where the app is running. For local use, verify the `.env` file is inside `backend/`.

### The website loads, but audio generation fails

- Check the Render service logs for the backend error.
- Confirm both API keys are valid and have the required API access.
- Check your internet connection and any API usage limits.
- Confirm the selected language and voice are supported by the configured Murf API account.

### The frontend files are not found

Check the `FRONTEND_DIR` path in `backend/app.py` and verify the folder is named `frontend` with the expected lowercase spelling.

### Render deployment fails

- Verify the Render root directory is `backend`.
- Verify `requirements.txt` includes all required Python packages.
- Check the build and runtime logs for the first error message.

## Security Notes

- Never commit `.env` files, API keys, or other secrets.
- Do not expose API keys in frontend JavaScript.
- Use demo or non-sensitive data when publicly demonstrating the project.

## License

Add a license file if you plan to distribute this project publicly. No license has been specified yet.
