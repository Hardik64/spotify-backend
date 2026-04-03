# 🎵 Spotify Backend

A RESTful backend for a Spotify-inspired music streaming app built with Node.js, Express, and MongoDB. Supports user authentication with JWT, role-based access (user/artist), and music upload functionality via ImageKit.

---

## 🛠 Tech Stack

- **Runtime** — Node.js
- **Framework** — Express.js v5
- **Database** — MongoDB with Mongoose
- **Authentication** — JWT + bcryptjs
- **File Storage** — ImageKit
- **File Upload** — Multer (memory storage)
- **Others** — cookie-parser, dotenv

---

## 📁 Project Structure

```
spotify-backend/
├── server.js
├── package.json
└── src/
    ├── app.js
    ├── db/
    │   └── db.js
    ├── controllers/
    │   ├── auth.controller.js
    │   └── music.controller.js
    ├── middlewares/
    │   └── auth.middleware.js
    ├── models/
    │   ├── user.model.js
    │   ├── music.model.js
    │   └── album.model.js
    ├── routes/
    │   ├── auth.routes.js
    │   └── music.routes.js
    └── service/
        └── storage.service.js
```

---

## ⚙️ Setup & Installation

### 1. Clone the repository
```bash
git clone https://github.com/Hardik64/spotify-backend.git
cd spotify-backend
```

### 2. Install dependencies
```bash
npm install
```

### 3. Create a `.env` file in the root directory
```env
PORT=3000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret_key
IMAGEKIT_PRIVATE_KEY=your_imagekit_private_key
```

### 4. Run the server
```bash
# Development (with nodemon)
npm run dev

# Production
npm start
```

Server runs on `http://localhost:3000`

---

## 🔐 Authentication

JWT tokens are stored in HTTP cookies. All protected routes require the token to be present in the cookie.

### Roles
| Role | Description |
|------|-------------|
| `user` | Can browse and listen to music and albums |
| `artist` | Can upload music and create albums |

---

## 📡 API Endpoints

### Auth Routes — `/api/auth`

| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/auth/register` | Public | Register a new user |
| POST | `/api/auth/login` | Public | Login and receive JWT cookie |
| POST | `/api/auth/logout` | Public | Logout user |

#### Register Request Body
```json
{
  "username": "hardik",
  "email": "hardik@example.com",
  "password": "yourpassword",
  "role": "artist"
}
```

#### Login Request Body
```json
{
  "username": "hardik",
  "password": "yourpassword"
}
```

---

### Music Routes — `/api/music`

| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/music/upload` | Artist only | Upload a music file |
| POST | `/api/music/album` | Artist only | Create a new album |
| GET | `/api/music/` | User + Artist | Get all music |
| GET | `/api/music/albums` | User + Artist | Get all albums |
| GET | `/api/music/albums/:albumId` | User + Artist | Get album by ID |

#### Upload Music (multipart/form-data)
```
title: "Song Title"
music: <audio file>
```

---

## 🗂 Data Models

### User
```js
{
  username: String (unique, required),
  email: String (unique, required),
  password: String (hashed, required),
  role: String (enum: ['user', 'artist'], default: 'user')
}
```

### Music
```js
{
  uri: String (ImageKit URL, required),
  title: String (required),
  artist: ObjectId (ref: User, required)
}
```

### Album
```js
{
  title: String (required),
  musics: [ObjectId] (ref: Music),
  artist: ObjectId (ref: User, required)
}
```

---

## 🧪 Testing with Postman

1. Register or login via `/api/auth/register` or `/api/auth/login`
2. In Postman go to **Cookies** and ensure the `token` cookie is saved for `localhost`
3. Use the token cookie to access protected routes
4. For music upload, use **Body → form-data** with `title` (text) and `music` (file) fields

---

## 📦 Scripts

```bash
npm start       # Start with node
npm run dev     # Start with nodemon (auto-restart on changes)
```
