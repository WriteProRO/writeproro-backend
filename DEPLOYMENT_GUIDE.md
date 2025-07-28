# WriteProRO Backend Deployment Guide

## Quick Setup Instructions

### Step 1: Deploy Backend to Railway (Recommended)

1. **Sign up for Railway**: Go to [railway.app](https://railway.app) and sign up with GitHub
2. **Create New Project**: Click "Deploy from GitHub repo"
3. **Upload Backend Files**: 
   - Create a new GitHub repository 
   - Upload the `writeproro-backend` folder contents
   - Connect Railway to your new repo
4. **Set Environment Variables** in Railway dashboard:
   ```
   OPENAI_API_KEY=sk-your-actual-openai-api-key-here
   PORT=3001
   NODE_ENV=production
   ALLOWED_ORIGINS=https://yourusername.github.io
   API_RATE_LIMIT=100
   ```
5. **Deploy**: Railway will automatically deploy your backend
6. **Get Your URL**: Railway will provide a URL like `https://your-app.railway.app`

### Step 2: Update Frontend Configuration

1. **Edit both HTML files** (`writeproro_production_ready.html` and `index.html`)
2. **Find this line** (around line 1684):
   ```javascript
   baseURL: window.location.hostname === 'localhost' ? 'http://localhost:3001' : 'https://your-backend-url.railway.app',
   ```
3. **Replace** `your-backend-url.railway.app` with your actual Railway URL
4. **Commit and push** to GitHub Pages

### Step 3: Test the Integration

1. **Open your WriteProRO application**
2. **Login with any user account**
3. **Try generating documentation**:
   - Fill in VIN (17 characters)
   - Select system
   - Add tech notes
   - Click "Generate AI Documentation"
4. **Should see**: "AI documentation generated using GPT-4!"

## Alternative Deployment Options

### Option B: Render.com
1. Sign up at [render.com](https://render.com)
2. Create "New Web Service"
3. Connect your GitHub repo
4. Set environment variables
5. Deploy

### Option C: Vercel
1. Sign up at [vercel.com](https://vercel.com)
2. Import your project
3. Add environment variables
4. Deploy

## Local Development Setup

For testing locally before deployment:

```bash
# Navigate to backend folder
cd writeproro-backend

# Install dependencies
npm install

# Create .env file
cp .env.example .env

# Edit .env and add your OpenAI API key
# OPENAI_API_KEY=sk-your-key-here

# Start development server
npm run dev
```

## Security Notes

- ✅ Your OpenAI API key stays secure on the server
- ✅ Rate limiting prevents abuse (100 requests per 15 minutes)
- ✅ CORS protection limits access to your domain only
- ✅ Input validation prevents malicious requests
- ✅ Error handling prevents sensitive information leaks

## Troubleshooting

### "Network error" message
- Check if backend URL is correct in frontend
- Verify backend is deployed and running
- Check CORS settings allow your domain

### "API authentication failed"
- Verify OpenAI API key is set correctly
- Check API key has sufficient credits
- Ensure no extra spaces in environment variable

### "Quota exceeded"
- Check OpenAI billing dashboard
- Your API key may need more credits
- Consider upgrading OpenAI plan

## Cost Estimation

GPT-4 usage costs approximately:
- Input: $0.03 per 1K tokens
- Output: $0.06 per 1K tokens
- Average automotive report: ~500-1000 tokens total
- **Estimated cost per report: $0.03-0.06**

Railway hosting: Free tier available, paid plans start at $5/month.

## Custom Prompts

To customize the AI responses, edit the `CUSTOM_SYSTEM_PROMPT` environment variable in your backend deployment:

```
CUSTOM_SYSTEM_PROMPT="You are an expert automotive diagnostic AI assistant specialized in [YOUR SPECIFIC EXPERTISE]. Focus on [YOUR REQUIREMENTS]..."
```

## Support

If you encounter issues:
1. Check the browser console for error messages
2. Check your backend logs in Railway dashboard
3. Verify all environment variables are set correctly
4. Test the `/health` endpoint: `https://your-backend.railway.app/health`