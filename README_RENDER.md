# Deploying to Render (frontend + backend)

This repository is split into `frontend/` and `backend/` so you can deploy to Render easily.

Recommended: create a single Web Service on Render that points at the project root and uses the `backend` folder as the service root (so the Express server serves the static files from `frontend/public`).

Quick steps (one-web-service approach):

1. Push your repository to GitHub/GitLab/Bitbucket (if not already).
2. In Render dashboard, click "New" → "Web Service".
3. Connect your repo and select the branch.
4. Set the "Root Directory" to the project root (or leave empty). In the "Build Command" enter:

   npm install --prefix backend

   (this installs backend dependencies)

   For the "Start Command" use:

   npm start --prefix backend

5. Environment variables (in Render UI → Environment):
   - PORT (optional) — defaults to 3000
   - UPLOAD_DIR — e.g. `./uploads` (relative to `backend`)

6. Deploy. The Express app in `backend/server.js` serves the static frontend from `frontend/public` and exposes the `/upload` endpoint.

Alternative: create two Render services
- Static Site: point to `frontend/public` (no build command required for static).
- Web Service: backend only (set build/start commands to `npm install` and `npm start` in `backend`), and set CORS or update the frontend fetch URL to point to the backend service URL.

Local testing

1. From project root install backend dependencies:

```powershell
cd backend
npm install
node server.js
```

2. Open http://localhost:3000 (or the port you set).

Notes
- For production, do not commit real secrets to `backend/.env`; use Render environment variables. Keep `backend/.env.example` as a template.
- If processing is heavy or large, consider increasing instance size or using background jobs.
