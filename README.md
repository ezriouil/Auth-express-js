<p align="center">
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js">
  <img src="https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white" alt="Express.js">
  <img src="https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white" alt="MongoDB">
  <img src="https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=JSON%20web%20tokens&logoColor=white" alt="JWT">
</p>

<h1 align="center">🔐 Express.js Authentication API</h1>
<h3 align="center">Complete Auth System with Email Verification & Password Reset</h3>

<p align="center">
  <strong>A production-ready Express.js API for user authentication. Register, login, email verification, OTP-based password reset, and protected routes with JWT.</strong>
</p>

<p align="center">
  <a href="#-features">Features</a> •
  <a href="#-api-endpoints">API Endpoints</a> •
  <a href="#-getting-started">Getting Started</a> •
  <a href="#-project-structure">Project Structure</a> •
  <a href="#-environment-variables">Environment</a>
</p>

---

## 📖 Overview

REST API built with Express.js and MongoDB for full user authentication. Includes registration, login, email verification via link, OTP-based password reset, JWT tokens, and role-based access (user/admin). Uses Nodemailer for transactional emails.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 📝 **Register** | Create new user with email verification |
| 🔑 **Login** | Sign in with email/password, returns JWT token |
| ✉️ **Email Verification** | Verify account via link sent to email |
| 📧 **Resend Verification** | Resend verification link |
| 🔐 **Password Reset** | OTP code sent to email for password reset |
| 🔄 **Resend OTP** | Resend OTP code for password reset |
| ✅ **Verify OTP** | Validate OTP before resetting password |
| 🔒 **Password Hashing** | bcrypt for secure password storage |
| 🛡️ **Protected Routes** | JWT middleware for auth-only endpoints |
| 👤 **Update Profile** | Update username/password (own account or admin) |
| 🎯 **Validation** | Joi + joi-password-complexity for input validation |
| 👑 **Admin Support** | Role-based authorization (isAdmin) |

---

## 🚀 API Endpoints

### Auth Routes (`/api/auth`)

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| `POST` | `/register` | Register new user | ❌ |
| `POST` | `/login` | Login user | ❌ |
| `GET` | `/verify-user-account/:token` | Verify email via link | ❌ |
| `POST` | `/re-send-email-verification-link` | Resend verification email | ❌ |
| `POST` | `/send-otp-code-reset-the-password` | Send OTP for password reset | ❌ |
| `POST` | `/re-send-otp-code-reset-the-password` | Resend OTP code | ❌ |
| `POST` | `/verify-otp-code-reset-the-password` | Verify OTP code | ❌ |
| `PUT` | `/reset-the-password` | Reset password with OTP | ❌ |

### User Routes (`/api/users`)

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| `PUT` | `/:id` | Update user (username, password) | ✅ (owner or admin) |

---

## 📋 Request/Response Examples

### Register

```http
POST /api/auth/register
Content-Type: application/json

{
  "userName": "John Doe",
  "email": "john@example.com",
  "password": "SecurePass123!"
}
```

**Response (201):**
```json
{
  "_id": "...",
  "userName": "John Doe",
  "email": "john@example.com",
  "token": null,
  "createdAt": "...",
  "updatedAt": "..."
}
```

> Verification email is sent to the user. Account must be verified before login.

### Login

```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "SecurePass123!"
}
```

**Response (200):**
```json
{
  "_id": "...",
  "userName": "John Doe",
  "email": "john@example.com",
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "createdAt": "...",
  "updatedAt": "..."
}
```

### Update User (Protected)

```http
PUT /api/users/:id
Content-Type: application/json
token: <jwt_token>

{
  "userName": "John Updated",
  "password": "NewSecurePass123!"
}
```

### Password Reset Flow

1. **Send OTP:** `POST /api/auth/send-otp-code-reset-the-password`  
   Body: `{ "email": "john@example.com" }`

2. **Verify OTP:** `POST /api/auth/verify-otp-code-reset-the-password`  
   Body: `{ "email": "john@example.com", "code": "1234" }`

3. **Reset Password:** `PUT /api/auth/reset-the-password`  
   Body: `{ "email": "john@example.com", "newPassword": "NewSecurePass123!" }`

---

## 🛠️ Tech Stack

| Category | Technology |
|----------|------------|
| **Runtime** | Node.js |
| **Framework** | Express.js |
| **Database** | MongoDB (Mongoose) |
| **Authentication** | JWT (jsonwebtoken) |
| **Password** | bcrypt |
| **Validation** | Joi, joi-password-complexity |
| **Email** | Nodemailer (Gmail) |
| **Environment** | dotenv |

---

## 📦 Dependencies

```json
{
  "express": "^4.18.3",
  "mongoose": "^8.2.1",
  "bcrypt": "^5.1.1",
  "jsonwebtoken": "^9.0.2",
  "joi": "^17.12.2",
  "joi-password-complexity": "^5.2.0",
  "nodemailer": "^6.9.12",
  "dotenv": "^16.4.5",
  "nodemon": "^3.1.0"
}
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- MongoDB (local or Atlas)
- Gmail account (for Nodemailer)

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/authentication.git
cd authentication

# Install dependencies
npm install

# Create .env file (see Environment Variables below)
```

### Environment Variables

Create a `.env` file in the root:

```env
# Server
PORT=5000

# MongoDB
MONGO_DB_URI=mongodb://localhost:27017/authentication_db
# Or MongoDB Atlas:
# MONGO_DB_URI=mongodb+srv://user:pass@cluster.mongodb.net/dbname

# JWT
JWT_SECRET_KEY=your-super-secret-key-change-in-production

# App Host (for verification links)
HOST=http://localhost:5000

# Email (Gmail)
EMAIL=your-gmail@gmail.com
PASSWORD=your-app-password
```

> **Gmail:** Use an [App Password](https://support.google.com/accounts/answer/185833) if 2FA is enabled.

### Run the Server

```bash
# Development (with nodemon)
npm run run

# Or
nodemon server.js
```

API runs at `http://localhost:5000`

---

## 📁 Project Structure

```
Authentication/
├── config/
│   └── db.js                    # MongoDB connection
├── controllers/
│   ├── auth_controller.js       # Register, login, verification, password reset
│   └── user_controller.js       # Update user
├── middlewares/
│   ├── errors.js                # 404 & error handler
│   └── verifyToken.js           # JWT auth, authorization, admin check
├── models/
│   └── User.js                  # User schema (Mongoose)
├── routes/
│   ├── auth_router.js           # Auth endpoints
│   └── user_router.js           # User endpoints
├── utils/
│   ├── email_templates.js       # HTML email templates
│   └── request_codes.js         # HTTP status codes
├── .env
├── .gitignore
├── package.json
├── package-lock.json
├── README.md
└── server.js                    # Entry point
```

---

## 🔐 Authentication Header

For protected routes, send the JWT in the `token` header:

```http
token: <your_jwt_token>
```

---

## 🔒 Security Best Practices

- Use a strong `JWT_SECRET_KEY` and never commit it
- Use bcrypt with salt (10 rounds)
- Validate all inputs with Joi
- Use HTTPS in production
- Store tokens securely on the client
- Use App Passwords for Gmail, not main password

---

## 📄 License

ISC

---

<p align="center">⭐ Star this repo if you find it helpful!</p>
