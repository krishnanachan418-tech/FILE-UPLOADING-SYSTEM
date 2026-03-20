# 🚀 Setup & Deployment Guide

## Local Development Setup

### Step 1: Install Node.js
Download from https://nodejs.org/ (LTS version recommended)

### Step 2: Backend Setup

```bash
cd server
npm install
npm run dev
```

**Expected Output:**
```
✅ File Upload Server running on http://localhost:5000
📁 Upload folder: /path/to/uploads
📊 Max file size: 5.00MB
✅ Allowed types: jpg, jpeg, png, pdf
🌐 CORS enabled for: http://localhost:5173
```

### Step 3: Frontend Setup (New Terminal)

```bash
cd client
npm install
npm run dev
```

**Expected Output:**
```
  VITE v4.3.0  ready in 200 ms

  ➜  Local:   http://localhost:5173/
  ➜  press h to show help
```

The app will automatically open in your browser.

## 🧪 Testing

1. **Upload a File:**
   - Drag a JPG/PNG/PDF file onto the upload area
   - Or click to select a file
   - View progress bar

2. **View Files:**
   - Uploaded files appear in the grid
   - Click image to preview in modal
   - File icons show for PDFs

3. **Download:**
   - Click "Download" button on any file

4. **Delete:**
   - Click trash icon, confirm deletion

5. **Dark Mode:**
   - Click moon icon in header

## 📱 Customization

### Change Max File Size
**Server:** `server/.env`
```
MAX_FILE_SIZE=5242880
```
(Bytes. 5242880 = 5MB)

### Add More File Types
**Server:** `server/.env`
```
ALLOWED_TYPES=jpg,jpeg,png,pdf,txt,doc,docx
```

**Client:** `client/src/utils/helpers.js`
```javascript
const icons = {
  pdf: '📄',
  jpg: '🖼️',
  jpeg: '🖼️',
  png: '🖼️',
  txt: '📝',
  doc: '📑',
  docx: '📑',
}
```

### Change Port
**Server:** `server/.env`
```
PORT=3001
```

**Client:** Update API URL in `client/.env`
```
VITE_API_URL=http://localhost:3001/api
```

## 🌐 Deployment - Complete Guide

### Backend Deployment

#### Option 1: Render.com (Easiest)

1. **Prepare:** Push code to GitHub
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

2. **Create Service:**
   - Go to https://render.com
   - Click "New +" → "Web Service"
   - Connect GitHub repository
   - Select the repo

3. **Configure:**
   - **Runtime:** Leave auto-detected (Node)
   - **Build Command:** `cd server && npm install`
   - **Start Command:** `cd server && npm start`
   - **Instance:** Free tier is fine

4. **Environment Variables:**
   - Click "Add Environment Variables"
   - Add all variables from `server/.env`:
     ```
     PORT=5000
     NODE_ENV=production
     FRONTEND_URL=https://your-frontend-url.com
     UPLOAD_FOLDER=uploads
     MAX_FILE_SIZE=5242880
     ALLOWED_TYPES=jpg,jpeg,png,pdf
     ```

5. **Deploy:** Render auto-deploys. Wait 2-3 minutes.

6. **Get URL:** Copy your service URL (e.g., `https://file-upload-api.onrender.com`)

#### Option 2: Railway.app

1. **Login:** https://railway.app
2. **New Project:** Click "New Project"
3. **Deploy:** Select "Deploy from GitHub" → Select repo
4. **Auto Setup:** Railway detects Node.js
5. **Set Variables:** In Environment, add from `server/.env`
6. **Deploy:** Takes 1-2 minutes

#### Option 3: Heroku (Legacy - Limited Free Tier)

```bash
# Install Heroku CLI from https://devcenter.heroku.com/articles/heroku-cli

heroku login
heroku create your-app-name
heroku config:set PORT=5000
git push heroku main
heroku logs --tail
```

---

### Frontend Deployment

#### Option 1: Vercel (Recommended)

1. **Install Vercel CLI:**
   ```bash
   npm install -g vercel
   ```

2. **Deploy:**
   ```bash
   cd client
   vercel
   ```

3. **Configure:**
   - Project name: `file-upload-client`
   - Framework: Vite
   - Root directory: `./client`

4. **Environment Variables:**
   - After deployment, go to Project Settings
   - Add Environment Variable:
     ```
     VITE_API_URL=https://your-backend-url.com/api
     ```
   - Redeploy

5. **Get URL:** `https://file-upload-client.vercel.app`

#### Option 2: Netlify

1. **Build:**
   ```bash
   cd client
   npm run build
   ```

2. **Deploy Folder:** Drag `dist/` folder to https://netlify.com
   OR use Netlify CLI:
   ```bash
   npm install -g netlify-cli
   netlify deploy --prod --dir=dist
   ```

3. **Set Environment:**
   - Site Settings → Build & Deploy → Environment
   - Add `VITE_API_URL`

#### Option 3: GitHub Pages

1. **Update vite.config.js:**
   ```javascript
   export default defineConfig({
     base: '/file-upload-system/',
     // ... rest
   })
   ```

2. **Build & Deploy:**
   ```bash
   npm run build
   cd dist
   # Deploy dist/ contents to gh-pages branch
   ```

---

#### Option 4: Cloud Platforms (AWS, Google Cloud, Azure)

**AWS S3 + CloudFront:**
```bash
# Build
npm run build

# Deploy to S3
aws s3 sync dist/ s3://your-bucket-name/

# Invalidate CloudFront cache
aws cloudfront create-invalidation --distribution-id YOUR_ID --paths "/*"
```

---

## ✅ Post-Deployment Checklist

- [ ] Backend is running and accessible
- [ ] CORS is configured correctly (`FRONTEND_URL` matches frontend domain)
- [ ] Environment variables are set on production
- [ ] File upload works from frontend
- [ ] Image previews display correctly
- [ ] Download functionality works
- [ ] Delete functionality works
- [ ] Dark mode works
- [ ] Mobile responsive
- [ ] Error messages display properly
- [ ] Uploaded files persist (check `/uploads`)

## 🔧 Troubleshooting Deployment

### Backend won't start
```bash
# Check logs
heroku logs --tail          # Heroku
railway logs               # Railway
# Check PORT environment variable is set
```

### CORS errors
- **Problem:** "Access to XMLHttpRequest blocked by CORS"
- **Solution:** Update `FRONTEND_URL` in backend `.env` to exact frontend URL
- **Example:** `https://your-frontend.vercel.app`

### 404 on /api/files
- **Problem:** API endpoints not found
- **Solution:** Verify backend is running and API URL is correct

### Uploads not persisting
- **Problem:** Files disappear after server restart
- **Solution:** Use persistent storage:
  - Render: Built-in persistent disk (paid tier)
  - Railway: Persistent volumes
  - AWS S3: Recommended for production

### Image previews broken
- **Problem:** Images show broken image icon
- **Solution:** Update backend URL in `client/src/utils/helpers.js`
  ```javascript
  export const getImageUrl = (filename) => {
    return `https://your-backend-url.com/${filename}`;
  }
  ```

## 📊 Production Checklist

For real-world deployment:

- [ ] **Add Authentication:** Login/JWT tokens
- [ ] **Add Virus Scanning:** VirusTotal API or ClamAV
- [ ] **Use Cloud Storage:** AWS S3, Google Drive, Azure Blob
- [ ] **Add Rate Limiting:** Prevent abuse
- [ ] **Add Monitoring:** Sentry, DataDog
- [ ] **Enable HTTPS:** Automatic with Vercel/Render
- [ ] **Add Backups:** Database/file backups
- [ ] **Set Up Logging:** Track errors
- [ ] **Use CDN:** Cloudflare, CloudFront
- [ ] **Add Analytics:** Usage tracking

## 🚀 Advanced: Using AWS S3

Replace local storage with AWS S3:

```bash
npm install aws-sdk
```

Update `server/server.js`:
```javascript
const AWS = require('aws-sdk');
const s3 = new AWS.S3({
  accessKeyId: process.env.AWS_ACCESS_KEY,
  secretAccessKey: process.env.AWS_SECRET_KEY,
});

// Configure multer to use S3
const multerS3 = require('multer-s3');
const upload = multerS3({
  s3: s3,
  bucket: process.env.AWS_BUCKET_NAME,
  // ... rest of config
});
```

---

**Happy Deploying! 🎉**
