# 📚 Quiz App Deployment Guide for Render

This guide will walk you through deploying your Quiz App to Render with separate backend and frontend services.

## 📋 Prerequisites

1. **GitHub Account**: Your code must be in a GitHub repository
2. **Render Account**: Sign up at [render.com](https://render.com)
3. **Git**: Make sure your code is committed and pushed to GitHub

## 🚀 Step-by-Step Deployment Instructions

### Step 1: Push Your Code to GitHub

1. Initialize git repository (if not already done):
```bash
git init
git add .
git commit -m "Initial commit"
```

2. Create a new repository on GitHub
3. Connect and push your code:
```bash
git remote add origin https://github.com/YOUR_USERNAME/quiz-app.git
git branch -M main
git push -u origin main
```

### Step 2: Deploy Using render.yaml (Recommended - Easiest Method)

Since we've created a `render.yaml` file, you can deploy both services at once:

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** → **"Blueprint"**
3. Connect your GitHub repository
4. Render will automatically detect the `render.yaml` file
5. Click **"Apply"** to create both services
6. Wait for both services to deploy (5-10 minutes)

### Step 3: Alternative - Manual Deployment

If you prefer to deploy services separately:

#### Deploy Backend:

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub repository
4. Configure the service:
   - **Name**: `quiz-app-backend`
   - **Root Directory**: `backend`
   - **Environment**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
5. Add environment variables:
   - Click **"Advanced"**
   - Add environment variable:
     - Key: `PORT`, Value: `10000`
     - Key: `NODE_ENV`, Value: `production`
6. Click **"Create Web Service"**
7. Wait for deployment (3-5 minutes)
8. Copy the backend URL (e.g., `https://quiz-app-backend.onrender.com`)

#### Deploy Frontend:

1. Click **"New +"** → **"Static Site"**
2. Connect your GitHub repository
3. Configure the service:
   - **Name**: `quiz-app-frontend`
   - **Root Directory**: `frontend`
   - **Build Command**: `npm install && npm run build`
   - **Publish Directory**: `dist`
4. Add environment variable:
   - Key: `VITE_API_BASE_URL`
   - Value: `https://quiz-app-backend.onrender.com/api` (use your backend URL)
5. Click **"Create Static Site"**
6. Wait for deployment (3-5 minutes)

### Step 4: Update Frontend API URL

After backend deployment, update the frontend environment variable:

1. Go to your frontend service on Render
2. Navigate to **"Environment"** tab
3. Update `VITE_API_BASE_URL` with your actual backend URL
4. The service will automatically redeploy

### Step 5: Configure CORS (if needed)

If you encounter CORS issues, update your backend `server.js`:

```javascript
// Update CORS configuration in server.js
app.use(cors({
  origin: [
    'http://localhost:5173',
    'https://quiz-app-frontend.onrender.com', // Your frontend URL
    process.env.FRONTEND_URL
  ],
  credentials: true
}));
```

Then add `FRONTEND_URL` environment variable in Render backend settings.

## 🔧 Troubleshooting

### Common Issues and Solutions:

1. **Backend not starting**:
   - Check logs in Render dashboard
   - Ensure `npm start` script exists in package.json
   - Verify PORT environment variable is set

2. **Frontend can't connect to backend**:
   - Verify `VITE_API_BASE_URL` is correct
   - Check CORS configuration
   - Ensure backend is running

3. **Images not loading**:
   - Verify image paths are correct
   - Check if images are in the `public` folder
   - For backend images, ensure static serving is configured

4. **Build failures**:
   - Check build logs for specific errors
   - Ensure all dependencies are in `package.json`
   - Verify Node version compatibility

## 📝 Environment Variables Summary

### Backend (.env):
```
PORT=10000
NODE_ENV=production
FRONTEND_URL=https://quiz-app-frontend.onrender.com
```

### Frontend (.env.production):
```
VITE_API_BASE_URL=https://quiz-app-backend.onrender.com/api
```

## 🎯 Post-Deployment Checklist

- [ ] Backend is accessible at its URL
- [ ] Frontend loads successfully
- [ ] Quiz selection works
- [ ] Questions load properly
- [ ] Images display correctly
- [ ] Score tracking works
- [ ] All quiz types function

## 🌟 Optional Enhancements

### Custom Domain
1. Go to your service settings
2. Click on **"Settings"** → **"Custom Domains"**
3. Add your domain and follow DNS configuration

### Auto-Deploy
- Render automatically deploys when you push to GitHub
- You can disable this in service settings if needed

### Performance Optimization
- Enable **"Auto-Scaling"** for backend (paid feature)
- Use Render's CDN for static assets
- Implement caching strategies

## 📊 Monitoring

1. Check service logs: Dashboard → Your Service → **"Logs"**
2. Monitor metrics: Dashboard → Your Service → **"Metrics"**
3. Set up health checks in service settings

## 🔗 Useful Links

- [Render Documentation](https://render.com/docs)
- [Render Node Deployment Guide](https://render.com/docs/deploy-node-express-app)
- [Render Static Sites Guide](https://render.com/docs/static-sites)
- [Troubleshooting Guide](https://render.com/docs/troubleshooting)

## 💡 Tips

1. **Free Tier Limitations**: 
   - Services may spin down after 15 minutes of inactivity
   - First request after spin-down may take 30-60 seconds
   - Consider upgrading for always-on services

2. **Database** (if needed later):
   - Render offers PostgreSQL databases
   - Can be added from dashboard

3. **Logs**:
   - Keep an eye on logs during deployment
   - Use `console.log` for debugging

## 🎉 Congratulations!

Your Quiz App should now be live on Render! Share the frontend URL with others to let them enjoy your quiz application.

### Your URLs will be:
- **Frontend**: `https://quiz-app-frontend.onrender.com`
- **Backend**: `https://quiz-app-backend.onrender.com`

Remember to update these URLs in your documentation and share them with users!
