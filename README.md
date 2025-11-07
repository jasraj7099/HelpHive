# 💡 HelpSync

HelpSync is a full-stack web application built with the **MERN (MongoDB, Express.js, React, Node.js)** stack.
It is designed to streamline help and support requests, allowing users to easily register, log in, and interact with support-related features.

---

## ✨ Features

* 🔐 **User Authentication** – Secure registration, login, and logout functionality with JWT
* 🛡️ **Role-based Access** – Different access levels for users and admins
* 🎫 **Support Requests** – Users can raise support tickets/issues
* ⏱️ **Real-time Updates** – Track issue status & progress
* 📱 **Responsive UI** – Modern and clean look with React + Tailwind CSS
* 🔗 **API Integration** – RESTful APIs with Express.js + MongoDB

---

## 🛠️ Tech Stack

### 🎨 Frontend

* ⚛️ React.js (Hooks + Context API / Zustand)
* 🎨 Tailwind CSS
* 🌐 Axios

### ⚙️ Backend

* 🟩 Node.js
* 🚏 Express.js
* 🍃 MongoDB + Mongoose ODM
* 🔑 JWT Authentication
* 🧂 Bcrypt.js for password hashing

---

## 📂 Project Structure

```
HelpSync/
│── backend/        # Express + MongoDB (API & authentication)
│── frontend/       # React + Tailwind (UI)
│── README.md       # Project documentation
```

---

## ⚡ Getting Started

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/Sunny5130/HelpSync.git
cd HelpSync
```

### 2️⃣ Setup Backend

```bash
cd backend
npm install
```

Create a **`.env`** file in `backend/` and add:

```env
MONGO_URI=your_mongodb_connection_string
JWT_KEY=your_secret_key
PORT=5000
```

Run backend:

```bash
npm start
```

### 3️⃣ Setup Frontend

```bash
cd ../frontend
npm install
npm start
```

Frontend runs on **[http://localhost:3000](http://localhost:3000)**
Backend runs on **[http://localhost:5000](http://localhost:5000)**

---

## 🚀 Deployment

* Frontend: [Vercel](https://vercel.com/)
* Backend: \[Render / Railway / Heroku]

(Currently deployed at: 🔗 [help-sync.vercel.app](https://help-sync.vercel.app))

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!
Feel free to fork this repo and submit a pull request.
"# HelpHive" 
