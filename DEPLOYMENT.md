
# ROOTED AI - Deployment Guide

This guide will help you deploy the Frontend to **Vercel** and the Backend to **Railway**.

## 1. Backend Deployment (Railway)

1.  **Push your latest code to GitHub** (you just did this).
2.  Go to [Railway.app](https://railway.app/) and login.
3.  Click **"New Project"** -> **"Deploy from GitHub repo"**.
4.  Select your repo: `Rooted-AI`.
5.  **Configure the Service**:
    *   Railway will likely detect the repo. Click on the card to edit settings.
    *   **Root Directory**: Go to Settings -> Root Directory and set it to `/backend`.
    *   **Variables**: Go to the "Variables" tab and add the secrets from your local `.env`:
        *   `OPENAI_API_KEY`: (Your sk-or-v1 key)
        *   `SUPABASE_URL`: (Your Supabase URL)
        *   `SUPABASE_KEY`: (Your Supabase Anon Key)
        *   `CHROMA_DB_PATH`: `/app/chroma_db` (Optional, but good practice).
    *   **Persistence (Critical)**:
        *   Since ROOTED AI uses a local file database (ChromaDB), you MUST add a Volume to prevent data loss on restart.
        *   Go to "Volumes".
        *   Click "Add Volume".
        *   Mount path: `/app/data` (or wherever you want, but you might need to update `CHROMA_DB_PATH` variable to match this mount, e.g., `/app/data/chroma`).
        *   *Simpler Alternative*: For a pure MVP, you can skip the volume, but memory will reset if the app "sleeps" or redeploys.
6.  **Deploy**: Railway usually auto-deploys. Watch the logs.
7.  **Get URL**: Once "Active", go to Settings -> Networking -> "Generate Domain". Copy this URL (e.g., `https://rooted-ai-production.up.railway.app`).

## 2. Frontend Deployment (Vercel)

1.  Go to [Vercel.com](https://vercel.com/) and login.
2.  Click **"Add New..."** -> **"Project"**.
3.  Import `Rooted-AI`.
4.  **Configure Project**:
    *   **Framework Preset**: Vite.
    *   **Root Directory**: Click "Edit" and select `frontend`.
    *   **Environment Variables**:
        *   `VITE_API_URL`: Paste the **Railway Backend URL** you just copied (e.g., `https://.../`). **IMPORTANT**: Remove any trailing slash `/` if present, or ensure it matches how axios uses it.
5.  Click **"Deploy"**.

## 3. Final Connection Check

*   Open your fresh Vercel URL.
*   Try "Sign Up".
*   Try chatting.
*   If chat fails, check the Browser Console (F12) -> Network Tab.
    *   If you see "CORS error", you might need to update the Backend `main.py` CORS settings to specific domains instead of `*`, or typically `*` works fine for simple setups but sometimes browsers block mixed content (http vs https). Ensure BOTH are HTTPS.

SUCCESS! Your Rooted AI is live.
