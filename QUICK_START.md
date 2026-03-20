# 🎯 Quick Start Commands

## Start Everything in 2 Commands

### Terminal 1 - Backend
```bash
cd server
npm install
npm run dev
```

### Terminal 2 - Frontend (new terminal)
```bash
cd client
npm install
npm run dev
```

That's it! Frontend opens automatically at http://localhost:5173

---

## 📋 What You Get

✅ **Backend (Express + Multer)**
- REST API for file upload
- File management endpoints
- Error handling & CORS

✅ **Frontend (React + Vite)**
- Drag-and-drop upload
- File gallery with previews
- Download & delete files
- Dark mode toggle

✅ **Production Ready**
- Environment variables
- Error handling
- Responsive design
- Toast notifications

---

## 🚀 Next Steps After Setup

1. **Test Upload:** Use the UI to upload a test file
2. **Try Features:** Preview images, download, delete
3. **Toggle Dark Mode:** Click moon icon
4. **Deploy:** See DEPLOYMENT.md for cloud hosting

---

## 📂 Key Files

- `server/server.js` - Backend API server
- `client/src/App.jsx` - Main React app
- `client/src/components/` - Reusable UI components
- `README.md` - Full documentation
- `DEPLOYMENT.md` - Hosting instructions

---

## 🔧 Common Customizations

**Change upload limit:** Edit `server/.env` → `MAX_FILE_SIZE`

**Add file types:** Edit `server/.env` → `ALLOWED_TYPES`

**Change port:** Edit `server/.env` → `PORT`

---

**Everything is ready to use! Happy uploading! 🎉**
