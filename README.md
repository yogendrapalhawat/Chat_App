# Quick-Chat — Real-Time Messaging App

A full-stack real-time chat application that lets users create accounts, discover other users, exchange messages instantly, share images, and manage their profiles. Built with React, Node.js, Express, MongoDB, and Socket.IO.

<p align="center">
  <a href="https://web-chat-app-eta-three.vercel.app/login"><strong>🚀 Live Demo</strong></a>
</p>

> **Note:** The live demo is provided for evaluation. Availability may depend on the deployed services and their environment configuration.

## ✨ Features

- **Authentication:** Sign up and log in with password hashing using bcrypt and token-based authentication with JWT.
- **Real-time messaging:** Send messages and receive new messages through Socket.IO.
- **User discovery:** View other registered users and select someone to start a conversation.
- **Image sharing:** Upload images through Cloudinary and send them in chat.
- **Conversation history:** Retrieve messages exchanged with a selected user.
- **Read status:** Track whether messages have been seen and display unseen-message counts.
- **Profile management:** Update profile information, bio, and profile picture.
- **Responsive chat interface:** Chat layout with user list, conversation panel, and profile/sidebar information.

## 🧰 Tech Stack

| Layer | Technologies |
|---|---|
| Frontend | React, Vite, React Router, Tailwind CSS |
| API / Backend | Node.js, Express.js |
| Real-time communication | Socket.IO |
| Database | MongoDB, Mongoose |
| Authentication & security | JWT, bcryptjs, protected routes |
| Image storage | Cloudinary |
| HTTP requests & notifications | Axios, React Hot Toast |
| Deployment | Vercel (frontend); configure your backend host separately |

## 🏗️ Architecture

```text
React Client
    │
    ├── REST API requests (Axios)
    │          │
    │          ▼
    │     Express API ── JWT authentication middleware
    │          │
    │          ▼
    │     Mongoose ── MongoDB
    │
    └── Socket.IO client ◄────► Socket.IO server
                                   │
                                   └── Online-user mapping and instant message events

Image uploads ──► Cloudinary
```

## 🔄 How It Works

1. A user signs up or logs in. The backend verifies credentials and issues a JWT.
2. Protected API routes use authentication middleware to identify the current user.
3. The client loads the user list and requests the conversation history for a selected user.
4. When a message is sent, the backend saves it in MongoDB and emits a Socket.IO event to the recipient when they are connected.
5. Images are uploaded to Cloudinary; the stored message contains the resulting image URL.
6. Message read state and unseen-message counts help users keep track of conversations.

## 📁 Project Structure

```text
Chat_App-main/
├── client/
│   ├── src/
│   │   ├── components/       # Chat, user list, and profile sidebar
│   │   ├── pages/            # Home, login, and profile pages
│   │   ├── context/          # Authentication and chat state
│   │   └── assets/           # Images and UI assets
│   ├── package.json
│   └── vite.config.js
└── server/
    ├── controllers/          # Authentication, profile, and message logic
    ├── lib/                  # Database, Cloudinary, and utility setup
    ├── middleware/           # Protected-route authentication
    ├── models/               # User and message schemas
    ├── routes/               # User and message API routes
    ├── package.json
    └── server.js              # Express and Socket.IO entry point
```

## ⚙️ Run Locally

### Prerequisites

- Node.js (LTS recommended)
- npm
- A MongoDB connection string (local MongoDB or MongoDB Atlas)
- A Cloudinary account for image uploads

### 1. Clone the repository

```bash
git clone https://github.com/yogendrapalhawat/<YOUR_REPOSITORY_NAME>.git
cd <YOUR_REPOSITORY_NAME>
```

### 2. Configure the backend

```bash
cd server
npm install
```

Create a `server/.env` file with your own values:

```env
PORT=5000
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=replace_with_a_long_random_secret
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
FRONTEND_URL=http://localhost:5173
```

Use the exact MongoDB environment-variable name expected by `server/lib/db.js`. Do not commit `.env` files or real credentials.

Start the backend:

```bash
npm run server
```

The server uses port `5000` by default and exposes a status endpoint at `http://localhost:5000/api/status`.

### 3. Configure the frontend

Open a second terminal:

```bash
cd client
npm install
```

Create `client/.env` with the environment variable used by the current frontend code (`client/context/AuthContext.jsx`):

```env
VITE_BACKEND=http://localhost:5000
```

The frontend appends `/api` automatically, so use the backend origin here (do not add `/api`). In your deployment settings, set `VITE_BACKEND` to your deployed backend origin, then rebuild/redeploy the frontend.

Start the frontend:

```bash
npm run dev
```

Open the local URL printed by Vite, usually `http://localhost:5173`.

## 🔌 API Overview

The backend mounts user routes at `/api/users` and message routes at `/api/messages`.

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/users/signup` | Register a user |
| POST | `/api/users/login` | Authenticate a user |
| GET | `/api/users/check` | Validate the authenticated session |
| PUT | `/api/users/update-profile` | Update profile details |
| GET | `/api/messages/users` | Get users for the chat sidebar and unseen-message counts |
| GET | `/api/messages/:id` | Fetch the conversation with a user |
| POST | `/api/messages/send/:id` | Send a text and/or image message |
| PUT | `/api/messages/mark/:id` | Mark a message as seen |
| GET | `/api/status` | Check server status |

Protected endpoints require the authentication format implemented by the client and server. See the route and middleware files for request details.

## 🔐 Security Notes

- **Before publishing:** the supplied project archive contains `.env` files. Make sure no real credentials or secrets are committed to GitHub. If any credentials were previously pushed, rotate them in MongoDB Atlas, Cloudinary, and your hosting provider, then remove the secrets from Git history.
- Passwords are hashed with bcryptjs before being stored.
- JWT-protected middleware restricts access to authenticated routes.
- Keep database credentials, JWT secrets, and Cloudinary API secrets in environment variables.
- Never publish `.env` files, access tokens, or production credentials in a public repository.
- For production, configure a strict CORS allowlist, validate request data, and apply appropriate rate limits.

## 🖼️ Screenshots

Add screenshots of the actual running application here. Useful screenshots include:

- Login / signup screen
- Main chat interface with a selected conversation
- User profile page
- Image message in a conversation

For example, save screenshots under `docs/screenshots/` and embed them like this:

```md
![Chat interface](docs/screenshots/chat-interface.png)
```

Only include screenshots that exist in your repository.

## 🧠 Key Engineering Concepts Demonstrated

- Building and consuming REST APIs
- JWT-based authentication and protected routes
- Password hashing with bcryptjs
- MongoDB data modeling with Mongoose
- Bidirectional real-time communication with Socket.IO
- Uploading media to cloud storage
- Tracking online users and message read state
- Separating routes, controllers, middleware, models, and infrastructure utilities

## 🚧 Potential Improvements

- Add automated unit and integration tests.
- Add pagination for long conversations and user lists.
- Add typing indicators, delivery receipts, and reconnect handling.
- Add message search, deletion, and conversation-level controls.
- Add stronger validation, rate limiting, and structured logging.

## 👨‍💻 Author

**Yogendra Palhawat**

- GitHub: [yogendrapalhawat](https://github.com/yogendrapalhawat)
- LinkedIn: [Add your LinkedIn profile URL](https://www.linkedin.com/)

---

If you find this project useful, feel free to explore the code and try the live demo.
