# VoiceStudio: Cloudflare Pages (Frontend) + Render (Backend) Deployment Guide

## 1. Backend Deployment on Render
1. Push this repository to GitHub.
2. Open **Render Dashboard** -> **New +** -> **Web Service** (or **Blueprint** to use `render.yaml`).
3. Connect your GitHub repository:
   - **Runtime**: `Docker`
   - **Dockerfile Path**: `./deploy/Dockerfile.render`
4. Set Environment Variables in Render:
   - `PORT`: `3900`
   - `OMNIVOICE_PORT`: `3900`
   - `OMNIVOICE_SERVER_MODE`: `1`
   - `OMNIVOICE_ALLOWED_ORIGINS`: Your Cloudflare Pages URL (e.g., `https://voicestudio.pages.dev`)
   - `OMNIVOICE_API_KEY`: Set a strong admin password/key
5. Deploy and copy your Render service URL (e.g., `https://voicestudio-backend.onrender.com`).

## 2. Frontend Deployment on Cloudflare Pages
1. Open **Cloudflare Dashboard** -> **Workers & Pages** -> **Create** -> **Pages** -> **Connect to Git**.
2. Select your repository and configure Build Settings:
   - **Build command**: `bun install && bun run --cwd electron build:web`
   - **Build output directory**: `frontend/dist`
3. Add Environment Variables in Cloudflare Pages:
   - `VITE_API_URL`: `https://voicestudio-backend.onrender.com` (your Render URL without trailing slash)
   - `NODE_VERSION`: `22`
4. Save and Deploy.
5. Copy the generated `*.pages.dev` URL and update `OMNIVOICE_ALLOWED_ORIGINS` in Render, then restart the Render service.
