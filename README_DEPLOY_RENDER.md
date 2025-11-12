# One-click (free) deploy on Render

## What this does
- **Backend** (Express + LowDB): persists `server/data/db.json` on a free 1 GB disk.
- **Frontend** (Vite React): static site that talks to the backend via `VITE_API_BASE`.

## Steps
1. Push your project to GitHub.
2. Save `render.yaml` at the repo root.
3. On Render: New → **Blueprint** → connect your repo.
4. Backend (`sciquest-backend`) will deploy first. Wait until you see a live URL like:
   `https://sciquest-backend-xxxxx.onrender.com`
5. Open that backend service → Settings → Environment → set **FRONTEND_ORIGINS** to your frontend URL once it exists (you can leave it for now).
6. Copy the backend URL and go to the **frontend** service (`sciquest-frontend`):
   - Set env var **VITE_API_BASE** = `https://YOUR-BACKEND-DOMAIN/api`
   - Redeploy frontend.
7. Test on the frontend URL. The app should call `/api/...` on your backend.

## Where data is stored
- On the backend service, a disk is mounted at `/opt/render/project/src/server/data`.
- Your file DB is `server/data/db.json` (created on first write).

## Notes
- Free dynos sleep after inactivity; first request may be slow.
- CORS: backend allows `FRONTEND_ORIGINS` (comma-separated list). Update it after the frontend is live.
- If you need to back up, download `server/data/db.json` from Render → Shell or Deploy Logs.