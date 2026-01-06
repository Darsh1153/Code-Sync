# Deployment Guide

## Important Note About Socket.io on Vercel

Socket.io requires persistent WebSocket connections, which don't work well with Vercel's serverless architecture. Therefore, we'll deploy:
- **Frontend (React)**: Vercel ✅
- **Backend (Socket.io)**: Render or Railway (supports persistent connections) ✅

## Option 1: Deploy Frontend to Vercel + Backend to Render (Recommended)

### Frontend Deployment (Vercel)

1. **Install Vercel CLI** (if not already installed):
   ```bash
   npm i -g vercel
   ```

2. **Login to Vercel**:
   ```bash
   vercel login
   ```

3. **Deploy**:
   ```bash
   vercel
   ```
   Follow the prompts. When asked:
   - Set up and deploy? **Yes**
   - Which scope? **Your account**
   - Link to existing project? **No**
   - Project name? **code-sync** (or your preferred name)
   - Directory? **./** (current directory)
   - Override settings? **No**

4. **Set Environment Variable**:
   - Go to your Vercel project dashboard
   - Navigate to Settings → Environment Variables
   - Add: `REACT_APP_BACKEND_URL` = `https://your-backend-url.onrender.com` (or your Render URL)

5. **Redeploy** after adding the environment variable

### Backend Deployment (Render)

1. **Create a new account** at [render.com](https://render.com)

2. **Create a new Web Service**:
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Configure:
     - **Name**: code-sync-backend
     - **Environment**: Node
     - **Build Command**: `npm install`
     - **Start Command**: `node server.js`
     - **Plan**: Free (or paid if you need more resources)

3. **Set Environment Variables** (if needed):
   - `PORT`: 10000 (Render's default, or leave empty)

4. **Deploy**: Render will automatically deploy your backend

5. **Update Frontend**: Update the `REACT_APP_BACKEND_URL` in Vercel to point to your Render backend URL

## Option 2: Deploy Both to Railway

Railway supports both frontend and backend with persistent connections.

1. **Install Railway CLI**:
   ```bash
   npm i -g @railway/cli
   ```

2. **Login**:
   ```bash
   railway login
   ```

3. **Initialize**:
   ```bash
   railway init
   ```

4. **Deploy**:
   ```bash
   railway up
   ```

## Option 3: Alternative - Use Vercel for Frontend + Separate Backend Service

If you want to keep using Vercel for frontend, deploy backend to:
- **Render** (recommended, free tier available)
- **Railway** (easy setup)
- **Fly.io** (good for WebSockets)
- **Heroku** (paid, but reliable)

## Environment Variables

### Frontend (Vercel):
- `REACT_APP_BACKEND_URL`: Your backend URL (e.g., `https://code-sync-backend.onrender.com`)

### Backend (Render/Railway):
- `PORT`: Usually auto-set by the platform
- `NODE_ENV`: `production`

## Testing After Deployment

1. Visit your Vercel frontend URL
2. Create a new room
3. Open the room in two different browser tabs/windows
4. Type code in one - it should sync in real-time to the other

## Troubleshooting

- **Socket connection fails**: Check that `REACT_APP_BACKEND_URL` is correctly set
- **CORS errors**: Make sure your backend allows requests from your Vercel domain
- **WebSocket errors**: Ensure your backend platform supports WebSockets (Render, Railway, Fly.io all do)

