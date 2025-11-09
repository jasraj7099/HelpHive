HelpHive — Interview Cheat Sheet

One-line summary

- HelpHive is a community-help full-stack app: React + Vite frontend (Tailwind), Node/Express backend with MongoDB (Mongoose), Cloudinary for images, and Socket.IO for realtime presence/chat.

Quick run & dev commands

- Backend (from repository root):
  - cd backend
  - npm install
  - create a .env with at least: MONGODB_URI, JWT_SECRET, CLOUDINARY_* keys
  - node index.js   (or npm start if configured)

- Frontend (from repository root):
  - cd frontend
  - npm install
  - npm run dev    # starts Vite on http://localhost:5173
  - npm run lint   # runs ESLint
  - npm run build  # production build

Important ports

- Frontend (Vite) default: 5173
- Backend default in code: PORT env or 3001 (index.js); axiosInstance in frontend currently uses http://localhost:3000/api — verify and align.

Core architecture (what to describe on a whiteboard)

- Browser (React SPA) ↔ REST API (Express) ↔ MongoDB (Mongoose)
- Socket.IO (client) ↔ Socket.IO server (backend) for realtime presence/chat; backend maintains in-memory userId→socketId map for development.
- Cloudinary used for image uploads (controllers upload image then store secure_url on user).
- Auth: JWT stored in HTTP-only cookie (set on register/login), auth middleware validates cookie and populates req.user.

Key files to mention and open in interview

- backend/index.js — server bootstrap (CORS, body parsers, cookieParser, route mounting, and Socket.IO server start).
- backend/socket.js — Socket.IO setup and user presence map.
- backend/controllers/user.controller.js — register, login, logout, getUser, updateUser (bcrypt for hashing and Cloudinary for pics).
- backend/models/* — Mongoose schemas (User, Help, Issue, Chat, Notification).
- frontend/src/store/userAuth.js — zustand auth store (registerUser, loginUser, getUser, logoutUser) and how it uses axiosInstance.
- frontend/src/utils/axiosInstance.js — baseURL + withCredentials: true (important for cookie auth).
- frontend/src/components/AuthPage.jsx — authentication UI; forms and flows.

Data model highlights (be ready to recite fields)

- User: { name, pic, email, password (hashed), ReportedIssues, ReportedHelps, timestamps }
- Help: { title, description, location, userId, isEmergency (bool), status: open|closed, timestamps }
- Issue: { title, description, image, location, userId, isResolved, users[], timestamps }

Auth flow (brief)

- Client POST /api/users/register -> backend hashes password, creates user, calls generateToken which sets HTTP-only cookie (token), returns user.
- Client POST /api/users/login -> backend verifies password, sets cookie, returns user.
- Protected endpoints use authToken middleware to verify token and attach req.user.
- Frontend uses axiosInstance withCredentials:true to include cookie on requests.

Realtime overview

- backend/socket.js stores userSocketMap: { [userId]: socketId } on connect (socket.handshake.query.userId).
- Server emits "getOnlineUsers" whenever users connect/disconnect.
- For scaling: explain Redis adapter or socket.io-redis to sync socket state across instances.

Common gotchas & what to fix (quick talking points)

- axios baseURL mismatch: frontend points to http://localhost:3000/api while backend default is 3001 in index.js — make this configurable via env (VITE_API_URL) and align envs before demo.
- Cookie flags in dev vs prod: secure:true and sameSite settings differ for HTTP vs HTTPS; toggle based on NODE_ENV.
- Fallback storage of base64 images is not production-friendly — Cloudinary should be primary.

Demo script (5–8 minutes)

1. Start backend, show GET /api responds.
2. Start frontend, navigate to Auth page.
3. Register a new user; show server logs, and note Set-Cookie in response headers (explain HttpOnly prevents JS access).
4. Show getUser called on app load and the app state (Zustand) populated.
5. Create a help request; show DB model and returned item.
6. Open two windows (or incognito) to demonstrate Socket.IO presence (getOnlineUsers event emitted).

Suggested improvements to mention

- Add request validation (express-validator) and centralized error middleware.
- Add tests (Jest + Supertest for backend, Vitest + React Testing Library for frontend).
- Replace in-memory socket map with Redis adapter for scaling.
- Use environment variables on frontend (VITE_API_URL) and remove hard-coded ports.

Potential interview questions (short responses prepared)

- Why cookies for auth? — HttpOnly cookies reduce XSS risk; tokens in localStorage are accessible to JS.
- How to scale sockets? — Redis adapter / message broker between servers; store presence in shared store.
- How to secure image uploads? — Use signed client uploads or server-side uploads to Cloudinary; validate file types and sizes; scan for malware.

Notes & references

- Frontend uses Tailwind; components are easily themeable by replacing utility classes.
- State uses Zustand — small, focused stores make it easy to reason about state and test.

---

If you want, I can also create a condensed "one-page" printable cheat-sheet or convert this into a PDF-ready markdown. Tell me which format you prefer.