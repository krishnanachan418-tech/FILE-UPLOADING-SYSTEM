# File Upload System

A modern, full-stack file upload system built with React, Vite, Node.js, and Express. Features drag-and-drop uploads, image previews, dark mode, and a responsive design.

## 🎯 Features

- **Drag & Drop Upload** - Easy file uploads with drag-and-drop interface
- **File Management** - View, download, and delete uploaded files
- **Image Previews** - Display images inline with preview modal
- **File Icons** - Visual icons for different file types (PDF, images, etc.)
- **Progress Tracking** - Real-time upload progress bar
- **Responsive Design** - Works perfectly on mobile, tablet, and desktop
- **Dark Mode** - Toggle between light and dark themes
- **Toast Notifications** - Success/error messages
- **File Validation** - Only allows jpg, jpeg, png, pdf (max 5MB)
- **Production Ready** - Error handling, CORS, environment variables

## 📋 Project Structure

```
file-upload-system/
├── client/                 # React frontend (Vite)
│   ├── src/
│   │   ├── components/     # React components
│   │   │   ├── Toast.jsx
│   │   │   ├── UploadBox.jsx
│   │   │   ├── FileCard.jsx
│   │   │   └── FileGrid.jsx
│   │   ├── hooks/          # Custom React hooks
│   │   │   ├── useFiles.js
│   │   │   └── useTheme.js
│   │   ├── utils/          # Utility functions
│   │   │   ├── api.js
│   │   │   └── helpers.js
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── index.css
│   ├── index.html
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── postcss.config.js
│   ├── package.json
│   └── .env
├── server/                 # Express backend
│   ├── server.js
│   ├── package.json
│   └── .env
├── uploads/                # Uploaded files (git-ignored)
└── README.md
```

## 🚀 Quick Start

### Prerequisites
- Node.js 14+ and npm/yarn
- Git (optional)

### Installation & Setup

#### 1. Clone or navigate to the project
```bash
cd file-upload-system
```

#### 2. Setup Backend

```bash
cd server

# Install dependencies
npm install

# Create .env file (already included with defaults)
# PORT=5000
# FRONTEND_URL=http://localhost:5173

# Start the server
npm run dev   # With auto-reload (requires nodemon)
# OR
npm start     # Direct node server
```

The backend will run on `http://localhost:5000`

#### 3. Setup Frontend

In a new terminal:

```bash
cd client

# Install dependencies
npm install

# Start development server
npm run dev
```

The frontend will open automatically on `http://localhost:5173`

#### 4. Test the Application

1. Open http://localhost:5173 in your browser
2. Drag and drop a file or click to select
3. Allowed file types: JPG, JPEG, PNG, PDF (Max 5MB)
4. View, download, or delete uploaded files
5. Toggle dark mode with the moon icon

## 🔌 API Endpoints

### POST `/api/upload`
Upload a single file

**Request:**
```
multipart/form-data with 'file' field
```

**Response:**
```json
{
  "success": true,
  "file": {
    "filename": "uuid.jpg",
    "originalName": "photo.jpg",
    "url": "/uuid.jpg",
    "size": 204800,
    "mimeType": "image/jpeg",
    "uploadedAt": "2024-03-20T10:30:00.000Z"
  }
}
```

### GET `/api/files`
Get list of all uploaded files

**Response:**
```json
{
  "success": true,
  "count": 5,
  "files": [
    {
      "filename": "uuid.jpg",
      "originalName": "photo.jpg",
      "url": "/uuid.jpg",
      "size": 204800,
      "type": "jpg",
      "uploadedAt": "2024-03-20T10:30:00.000Z",
      "isImage": true
    }
  ]
}
```

### DELETE `/api/files/:filename`
Delete a specific file

**Response:**
```json
{
  "success": true,
  "message": "File deleted successfully",
  "filename": "uuid.jpg"
}
```

## 📁 File Handling

- **Allowed Types:** jpg, jpeg, png, pdf
- **Max Size:** 5MB
- **Storage:** Local `uploads/` folder
- **Naming:** UUID-based unique names (prevents conflicts)

## 🎨 Styling & Themes

- Built with **Tailwind CSS**
- Responsive grid layout
- Light and dark mode support
- Custom CSS utilities (btn, card, input-field)

## 🔧 Configuration

### Backend (.env)
```
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
UPLOAD_FOLDER=uploads
MAX_FILE_SIZE=5242880 (5MB in bytes)
ALLOWED_TYPES=jpg,jpeg,png,pdf
```

### Frontend (.env)
```
VITE_API_URL=http://localhost:5000/api
```

## 🌐 Deployment

### Backend Deployment (Express)

**Option 1: Render.com**
1. Push to GitHub
2. Create new Web Service on Render
3. Set `npm start` as start command
4. Add environment variables
5. Deploy

**Option 2: Railway.app**
1. Connect GitHub repo
2. Railway auto-detects Node.js
3. Set environment variables
4. Deploy

**Option 3: Heroku (legacy)**
```bash
heroku login
heroku create your-app-name
git push heroku main
```

### Frontend Deployment (React)

**Option 1: Vercel (Recommended)**
```bash
npm install -g vercel
vercel
# Select the project, follow prompts
```

**Option 2: Netlify**
1. Build: `npm run build`
2. Deploy `dist/` folder to Netlify
3. Set environment variable `VITE_API_URL`

**Option 3: GitHub Pages**
```bash
npm run build
# Deploy dist/ folder
```

## 🛠️ Development

### Add New Features

1. **Add File Type Support:**
   - Edit `.env` in both server and client
   - Update `ALLOWED_TYPES` and `getFileIcon()`

2. **Modify Upload Limits:**
   - Server: Change `MAX_FILE_SIZE` in `.env`
   - Client: Update accept attribute in `UploadBox.jsx`

3. **Customize Styling:**
   - Edit `tailwind.config.js` for theme
   - Modify `src/index.css` for custom styles

## 🔒 Security Considerations

✅ **Implemented:**
- File type validation (server-side)
- File size limits
- Unique file naming (prevents conflicts)
- Directory traversal protection
- CORS configuration
- Input sanitization

⚠️ **For Production:**
- Add authentication/authorization
- Implement virus scanning (ClamAV, VirusTotal API)
- Use cloud storage (AWS S3, Google Cloud)
- Add rate limiting
- Encrypt files at rest
- Add audit logging

## 🐛 Troubleshooting

### "Cannot POST /api/upload"
- Backend not running or wrong PORT
- Check `http://localhost:5000` is accessible

### "Network Error"
- CORS issue: Check `FRONTEND_URL` in server `.env`
- Server not running: Start with `npm run dev`

### "File type not allowed"
- Only jpg, jpeg, png, pdf supported
- Check file extension and `ALLOWED_TYPES` in `.env`

### Files not showing
- Check `uploads/` folder exists
- Verify `__dirname` path is correct
- Check file permissions

### Dark mode not persisting
- Browser localStorage might be disabled
- Clear localStorage and try again

## 📦 Dependencies

### Backend
- `express` - Web framework
- `multer` - File upload middleware
- `cors` - CORS handling
- `dotenv` - Environment variables
- `uuid` - Unique IDs

### Frontend
- `react` - UI framework
- `tailwindcss` - Styling
- `vite` - Build tool & dev server

## 📄 License

MIT License - Feel free to use this project however you like!

## 🤝 Contributing

Feel free to fork, modify, and improve this project. Some ideas:
- Add cloud storage integration (S3, GCP)
- Add user authentication
- Add batch upload
- Add image compression
- Add file encryption
- Add analytics dashboard

---

**Built with ❤️ using modern web technologies**
