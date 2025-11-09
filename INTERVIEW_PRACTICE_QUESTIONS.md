HelpHive — Practice Interview Questions & Model Answers

1) Q: Explain the authentication flow in this project.
A: The client posts credentials to /api/users/register or /api/users/login. The backend hashes passwords with bcrypt on register, then generateToken sets an HTTP-only cookie (JWT token) on the response. The frontend axios instance uses withCredentials:true so the cookie is included on subsequent requests. Protected endpoints use an authToken middleware to verify the token and populate req.user.

2) Q: How does the realtime presence feature work, and how would you scale it?
A: Socket.IO is used. On connection the backend reads socket.handshake.query.userId and stores userSocketMap[userId] = socket.id. The server emits 'getOnlineUsers' with keys of that map. For scaling, use the socket.io-redis adapter (or Redis pub/sub) so multiple instances share socket events and presence. Persist presence in Redis (sets per user), and handle multi-tab/duplicate sockets by storing sets of socketIds per user.

3) Q: Describe the User model and why fields are chosen.
A: User has name, pic (profile URL), email (unique), password (hashed), ReportedIssues and ReportedHelps (ObjectId arrays), and timestamps. The arrays allow quick population of user-created resources. Passwords are hashed to avoid storing plaintext.

4) Q: There's a mismatch between backend port and frontend axios baseURL in the repo. How would you fix it?
A: Make the API base URL configurable via environment variables. Add VITE_API_URL in frontend and update axiosInstance to use import.meta.env.VITE_API_URL || default. On backend, use process.env.PORT and document the port in README. This avoids hard-coded ports and eases deployment.

5) Q: How are images handled and what changes would you recommend?
A: Images are uploaded via Cloudinary (recommended). The user controller uploads to Cloudinary and stores secure_url. There's a fallback to base64 in the code — I would remove that fallback in production, instead queue failed uploads or notify the user; base64 storage is inefficient and increases DB size.

6) Q: How would you add input validation and centralized error handling?
A: Use express-validator or Joi for request validation at route level. Create a centralized error-handling middleware (err, req, res, next) to format errors and return consistent codes. Validate IDs (ObjectId) and provide helpful messages.

7) Q: How would you test the backend endpoints?
A: Use Jest + Supertest. Use mongodb-memory-server to run tests against an in-memory MongoDB instance. Write unit tests for controllers and integration tests for routes, including auth flows (register/login) and protected routes.

8) Q: How can you make Socket.IO connections reliable across reconnects and multi-tab scenarios?
A: Implement reconnection logic client-side with exponential backoff. On the server, map user -> Set(socketIds) instead of single socketId. Use Redis for cross-instance presence. Optionally, track last-seen timestamps and clean stale sockets.

9) Q: How would you secure the app in production?
A: Use HTTPS everywhere, set secure and SameSite attributes properly for cookies, rate-limit auth endpoints, add input sanitization, use Helmet for headers, and avoid storing secrets in code (use environment variables and secret managers). Also add monitoring and logging.

10) Q: Where would you add caching and why?
A: Add caching for frequently read, non-critical endpoints such as public help listings. Use Redis caching with TTL to reduce DB load, implement cache invalidation when resources change (e.g., new help request or status update).


Use these questions to practice aloud; for each answer, point to the relevant file (user.controller.js, socket.js, userAuth.js, axiosInstance.js, and models) during the interview.