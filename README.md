# Social Media App — Backend (Deployment)

Deployed backend for a Social Media application built with **Node.js**, **Express**, and **MongoDB (Mongoose)**. It includes authentication (JWT), users/channels, videos, tweets, comments, likes, subscriptions, playlists, watch history, and a simple dashboard API.

## Live Deployment

- **Backend URL:** https://socialmediaapp-backend-1.onrender.com

> Note: This URL is intended only for connecting the backend with the frontend. It is used exclusively for API calls and does not serve any standalone purpose.
---

## Tech Stack

- **Runtime:** Node.js
- **Framework:** Express
- **Database:** MongoDB + Mongoose
- **Auth:** JWT (supports cookie `accessToken` or `Authorization: Bearer <token>`)
- **Uploads:** Multer (memory storage) — used for avatar/cover image and video/thumbnail uploads
- **Other:** CORS, Cookie Parser, Dotenv

---

## Features

### Authentication & Users
- Register user (supports **avatar** and **cover image** upload)
- Login / Logout
- Refresh access token
- Change password
- Get current logged-in user
- Update account details
- Update avatar / cover image
- Get user channel profile
- Watch history management:
  - Get watch history
  - Add video to watch history
  - Remove video from watch history
  - Clear watch history

### Videos
- Publish video (upload **videoFile** + **thumbnail**)
- Get all videos (with optional filters handled by controller)
- Get video by ID
- Update view count
- Update video details / thumbnail
- Delete video
- Toggle publish/unpublish status

### Tweets
- Create tweet
- Get tweets by user
- Update tweet
- Delete tweet

### Comments
- Get comments for a video
- Add comment to a video
- Update comment
- Delete comment

### Likes
- Like/unlike a **video**
- Like/unlike a **tweet**
- Like/unlike a **comment**
- Fetch liked videos
- Check if current user liked a specific video/tweet/comment
- Get likes count for a video/tweet/comment

### Subscriptions
- Subscribe/unsubscribe to a channel
- Get subscribed channels
- Get channel subscribers
- Check if subscribed to a channel

### Playlists
- Create playlist
- Get playlist by ID
- Update playlist
- Delete playlist
- Add video to playlist
- Remove video from playlist
- Get playlists for a user

### Dashboard
- Channel stats
- Channel videos listing (for dashboard)

### Healthcheck
- Healthcheck endpoint to verify API is running

---

## API Base Path

All routes are mounted under:

- ` /api/v1/* `

---

## API Routes (Quick Reference)

### Healthcheck
- `GET /api/v1/healthcheck`

### Users (`/api/v1/users`)
- `POST /register` (multipart: `avatar`, `coverimage`)
- `POST /login`
- `POST /logout` (auth)
- `POST /refresh-token`
- `POST /change-password` (auth)
- `GET  /current-user` (auth)
- `PATCH /update-account` (auth)
- `PATCH /avatar` (auth, multipart: `avatar`)
- `PATCH /coverimage` (auth, multipart: `coverimage`)
- `GET  /c/:username` (auth)
- `GET  /history` (auth)
- `POST /addVideoToWatchHistory` (auth)
- `DELETE /clear-history` (auth)
- `DELETE /remove-from-history` (auth)

### Videos (`/api/v1/videos`) *(auth required for all video routes)*
- `POST /` (multipart: `videoFile`, `thumbnail`)
- `GET  /`
- `GET  /v/:videoId`
- `PATCH /v/:videoId` (update views)
- `PATCH /uv/:videoId` (optional multipart: `thumbnail`)
- `DELETE /uv/:videoId`
- `PATCH /uv/:videoId/toggle-publish`

### Tweets (`/api/v1/tweets`) *(auth required)*
- `POST /`
- `GET  /user/:userId`
- `PATCH /:tweetId`
- `DELETE /:tweetId`

### Comments (`/api/v1/comment`) *(auth required)*
- `GET  /:videoId`
- `POST /:videoId`
- `PATCH /c/:commentId`
- `DELETE /c/:commentId`

### Likes (`/api/v1/like`) *(auth required)*
- `POST /toggle/v/:videoId`
- `POST /toggle/c/:commentId`
- `POST /toggle/t/:tweetId`
- `GET  /videos`
- `GET  /check/v/:videoId`
- `GET  /check/c/:commentId`
- `GET  /check/t/:tweetId`
- `GET  /count/v/:videoId`
- `GET  /count/c/:commentId`
- `GET  /count/t/:tweetId`

### Subscriptions (`/api/v1/subscribe`) *(auth required)*
- `GET  /c/:channelId`
- `POST /c/:channelId` (toggle subscribe)
- `GET  /u/:subscriptionId`
- `GET  /check/:channelId`

### Playlists (`/api/v1/playlist`) *(auth required)*
- `POST /`
- `GET  /:playlistId`
- `PATCH /:playlistId`
- `DELETE /:playlistId`
- `PATCH /add/:videoId/:playlistId`
- `PATCH /remove/:videoId/:playlistId`
- `GET  /user/:userId`

### Dashboard (`/api/v1/dashboard`) *(auth required)*
- `GET /stats`
- `GET /videos`

---

## Getting Started (Local Setup)

### 1) Clone & Install
```bash
git clone https://github.com/AnasMultani17/Social-Media-App-Backend-Deployment.git
cd Social-Media-App-Backend-Deployment
npm install
```

### 2) Environment Variables

Create an environment file (commonly `.env`). This backend expects at least:

```env
PORT=7000
MONGO_URI=
CORS_ORIGIN=http://localhost:3000
ACCESS_TOKEN_SECRET=
ACCESS_TOKEN_EXPIRY=
REFRESH_TOKEN_SECRET=
REFRESH_TOKEN_EXPIRY=

CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=


```

### 3) Run the Server
```bash
# development
npm run dev

# production
npm start
```

Server starts on:
- `http://localhost:5000` (by default)

---

## Authentication

Protected routes use `verifyJWT` middleware and accept tokens from:
- Cookie: `accessToken`
- Header: `Authorization: Bearer <token>`

---

## File Uploads

This backend uses **multer memory storage**, meaning files are kept in RAM during the request.
Common multipart fields:
- User registration: `avatar`, `coverimage`
- Video publish: `videoFile`, `thumbnail`
- Update avatar: `avatar`
- Update cover image: `coverimage`

---

## CORS

CORS is enabled with credentials support and a configurable origin via:
- `CORS_ORIGIN` (defaults to `http://localhost:3000`)

The app also explicitly sets `Access-Control-Allow-Origin` to:
- `https://new-social-media-app-frontned.vercel.app`

---

## Deployment Notes

- This repo contains deployment configuration (e.g. `vercel.json`) and is deployed at:
  - https://socialmediaapp-backend-1.onrender.com

---

## Contributing

1. Fork the repo
2. Create your feature branch: `git checkout -b feature/my-feature`
3. Commit changes: `git commit -m "Add my feature"`
4. Push to branch: `git push origin feature/my-feature`
5. Open a Pull Request
