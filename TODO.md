# Step-by-Step Deployment Guide for DataShield.Ai (Beginner-Friendly)

## 🚀 Overview
Your project has 3 main parts:
- **Web App** (Next.js): User interface and authentication
- **API** (FastAPI): Backend with ML models for phishing detection
- **Extension** (Chrome): Browser extension for URL scanning
- **Database** (PostgreSQL): Stores user data and scan results

We'll deploy each part to free hosting platforms.

## 📋 Step 1: Prepare Your Code (Already Done)
✅ Created `packages/api/Dockerfile` for API containerization
✅ Created `packages/web/vercel.json` for Vercel config

## 🗄️ Step 2: Set Up Database (Neon)
1. Go to [neon.tech](https://neon.tech) and sign up for free account
2. Create a new project
3. Copy the `DATABASE_URL` from the connection details
4. Keep this URL safe - you'll need it later

## 🚂 Step 3: Deploy API to Railway
1. Go to [railway.app](https://railway.app) and sign up for free account
2. Click "New Project" → "Deploy from GitHub repo"
3. Connect your GitHub repository
4. Choose your repo and branch
5. Railway will auto-detect it's a monorepo and ask for the service directory
6. Set **Root Directory** to `packages/api`
7. Railway will detect the Dockerfile automatically
8. Configure:
   - **Service Name**: datashield-api
   - **Port**: 8000
9. Add environment variables (if needed)
10. Click "Deploy"
11. Wait for deployment (5-10 minutes)
12. Copy the service URL from Railway dashboard (something like `https://datashield-api.up.railway.app`)

**Note**: After deploying the web app to Vercel, you'll need to update the API's CORS settings to allow your Vercel domain. In Railway dashboard, go to your service → Variables → Add `ALLOWED_ORIGINS` with your Vercel URL.

## 🌐 Step 4: Deploy Web App to Vercel
1. Go to [vercel.com](https://vercel.com) and sign up for free account
2. Click "New Project"
3. Import your GitHub repository
4. Configure:
   - **Framework Preset**: Next.js
   - **Root Directory**: packages/web
5. Add environment variables:
   - `DATABASE_URL`: Your Neon database URL
   - `NEXTAUTH_SECRET`: Generate a random string (you can use an online generator)
   - `NEXTAUTH_URL`: Your Vercel domain (will be assigned after deployment)
   - `FASTAPI_URL`: Your Railway API URL from Step 3
6. Click "Deploy"
7. Wait for deployment (2-5 minutes)
8. Update `NEXTAUTH_URL` with your actual Vercel domain

## 🛠️ Step 5: Set Up Database Schema
1. In Vercel dashboard, go to your project
2. Click "Functions" → "Run Command"
3. Run: `cd packages/web && npx prisma migrate deploy`
4. This will create all database tables

## 🧪 Step 6: Test Your Deployment
1. Visit your Vercel domain
2. Try registering a new account
3. Try scanning a URL
4. Check if everything works

## 📱 Step 7: Chrome Extension (Optional)
1. Go to [Chrome Web Store Developer Dashboard](https://chrome.google.com/webstore/devconsole/)
2. Pay $5 one-time fee
3. Upload your extension from `packages/extension/`
4. Publish it

## 🔑 Environment Variables Summary
- **Neon**: DATABASE_URL
- **Railway**: (none needed for basic setup)
- **Vercel**: DATABASE_URL, NEXTAUTH_SECRET, NEXTAUTH_URL, FASTAPI_URL

## 🆘 Troubleshooting
- If API doesn't work: Check Railway logs
- If web app fails: Check Vercel function logs
- If database issues: Verify DATABASE_URL format

## 💡 Tips for Beginners
- Always test locally first before deploying
- Keep environment variables secret (don't commit to Git)
- Free tiers have limits - monitor usage
- Use GitHub for version control
