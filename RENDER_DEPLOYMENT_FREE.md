# 🚀 Deploy Quiz App on Render (Free Tier)

This guide shows you how to deploy your Quiz App on Render's free tier without any payment required.

## 📋 What You'll Get for Free:
- ✅ 1 Backend Web Service (Node.js/Express)
- ✅ Unlimited Static Sites (React Frontend)
- ✅ Automatic deploys from GitHub
- ✅ HTTPS/SSL certificates

## 🎯 Step-by-Step Deployment

### Step 1: Prepare Your Code

1. **Commit all changes:**
```bash
git add .
git commit -m "Prepare for Render deployment"
```

2. **Push to GitHub:**
```bash
git push origin main
```

### Step 2: Deploy Backend (Web Service)

1. **Go to Render Dashboard:**
   - Visit [dashboard.render.com](https://dashboard.render.com)
   - Sign in or create account

2. **Create New Web Service:**
   - Click the **"New +"** button
   - Select **"Web Service"**

3. **Connect GitHub:**
   - Click **"Connect a GitHub repository"**
   - Authorize Render to access your GitHub
   - Select your `quiz-app` repository

4. **Configure Backend Service:**
   ```
   Name: quiz-app-backend-[yourname]
   Region: Choose nearest to you
   Branch: main
   Root Directory: backend
   Runtime: Node
   Build Command: npm install
   Start Command: npm start
   Instance Type: Free
   ```

5. **Add Environment Variables:**
   - Scroll down to "Environment Variables"
   - Click "Add Environment Variable"
   - Add these:
     ```
     PORT = 10000
     NODE_ENV = production
     ```

6. **Create Web Service:**
   - Click **"Create Web Service"**
   - Wait 3-5 minutes for deployment
   - Copy your backend URL (looks like: `https://quiz-app-backend-xyz.onrender.com`)

### Step 3: Deploy Frontend (Static Site)

1. **Create New Static Site:**
   - Go back to dashboard
   - Click **"New +"** button
   - Select **"Static Site"** (NOT Web Service!)

2. **Connect Same Repository:**
   - Select your `quiz-app` repository again

3. **Configure Frontend:**
   ```
   Name: quiz-app-frontend-[yourname]
   Branch: main
   Root Directory: frontend
   Build Command: npm install && npm run build
   Publish Directory: dist
   ```

4. **Add Environment Variable:**
   - Click "Add Environment Variable"
   - Add:
     ```
     VITE_API_BASE_URL = [YOUR_BACKEND_URL]/api
     ```
   - Replace `[YOUR_BACKEND_URL]` with the URL from Step 2
   - Example: `https://quiz-app-backend-xyz.onrender.com/api`

5. **Create Static Site:**
   - Click **"Create Static Site"**
   - Wait 3-5 minutes for deployment

### Step 4: Update Backend CORS (Important!)

1. **Update your backend `server.js` locally:**

```javascript
// In backend/server.js, update the CORS configuration
const cors = require('cors');

// Add your frontend URL to allowed origins
app.use(cors({
  origin: [
    'http://localhost:5173',
    'http://localhost:3000',
    'https://quiz-app-frontend-[yourname].onrender.com', // Add your frontend URL
    process.env.FRONTEND_URL
  ],
  credentials: true
}));
```

2. **Commit and push the change:**
```bash
git add backend/server.js
git commit -m "Update CORS for Render deployment"
git push origin main
```

3. **Backend will auto-redeploy** (takes 2-3 minutes)

### Step 5: Test Your Deployment

1. **Visit your frontend URL**
   - Should be: `https://quiz-app-frontend-[yourname].onrender.com`

2. **Test all features:**
   - [ ] Quiz selection loads
   - [ ] Can start a quiz
   - [ ] Questions appear
   - [ ] Images load correctly
   - [ ] Score tracking works
   - [ ] Game over/victory screens work

## 🔧 Troubleshooting

### Frontend shows but quizzes don't load:
- Check if `VITE_API_BASE_URL` is set correctly in frontend environment
- Verify backend is running (visit backend URL directly)
- Check browser console for CORS errors

### "Failed to fetch" errors:
- Make sure backend URL in frontend ends with `/api`
- Check CORS configuration in backend
- Verify backend is deployed and running

### Images not showing:
- Check if images are in `backend/public/images/`
- Ensure all image files are committed to Git
- Verify image paths in the code

### Backend crashes or doesn't start:
- Check Render dashboard logs
- Ensure `PORT` environment variable is set
- Verify `npm start` script exists in `backend/package.json`

## 💡 Important Notes

### Free Tier Limitations:
- Services spin down after 15 minutes of inactivity
- First request after spin-down takes 30-60 seconds
- 750 hours/month of running time (plenty for personal projects)
- Services restart automatically every 30 days

### To Keep Services Active:
- Use a service like [UptimeRobot](https://uptimerobot.com) to ping your backend every 14 minutes
- Or upgrade to Render's paid tier for always-on services

## 🎉 Success!

Once deployed, you'll have:
- **Frontend URL:** `https://quiz-app-frontend-[yourname].onrender.com`
- **Backend API:** `https://quiz-app-backend-[yourname].onrender.com/api`

Share the frontend URL with friends to let them play your quiz!

## 📝 Next Steps

1. **Custom Domain** (optional):
   - Add a custom domain in Render dashboard settings

2. **Monitoring:**
   - Check logs in Render dashboard
   - Set up alerts for errors

3. **Performance:**
   - Consider adding a CDN for images
   - Implement caching strategies

## Need Help?

- Check [Render Docs](https://render.com/docs)
- Visit [Render Community](https://community.render.com)
- Review deployment logs in your Render dashboard
