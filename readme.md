# Secrets Sharing App

A secure, server-rendered Node.js application that lets users anonymously share their secrets. It uses Express, EJS, MongoDB Atlas, and Passport.js for robust authentication.

Check-out my app [here](https://secrets-u8u5.onrender.com).
---

## 📖 Table of Contents

1. [Project Overview](#project-overview)
2. [Features](#features)
3. [Security & Authentication](#security--authentication)
   - [Passport.js Role](#passportjs-role)
   - [Google OAuth Integration](#google-oauth-integration)
   - [Password Hashing & Salting](#password-hashing--salting)
4. [Database: MongoDB Atlas](#database-mongodb-atlas)
5. [Setup & Deployment](#setup--deployment)
6. [Environment Variables](#environment-variables)
7. [Future Improvements](#future-improvements)

---

## Project Overview

This application allows users to register with email/password or Google, log in securely, and submit anonymous secrets that are displayed to all authenticated users.

---

## Features

- 🔒 **Email/Password Authentication** via Passport.js
- 🌐 **Google OAuth2** login integration
- 🗝️ **Secure session management** with express-session
- 📝 **Submit and view anonymous secrets**
- 🌍 **Hosted on Render** with MongoDB Atlas backend

---

## Security & Authentication

### Passport.js Role

Passport.js is the authentication middleware at the heart of this app. It:

- 🔧 Manages **session serialization** (`passport.serializeUser`) and **deserialization** (`passport.deserializeUser`).
- 🧩 Provides the **LocalStrategy** to authenticate users with email and password.
- 🔐 Exposes `req.isAuthenticated()`, `req.login()`, and `req.logout()` for route protection.

### Google OAuth Integration

- 🌐 Uses `passport-google-oauth20` strategy to allow users to log in with their Google accounts.
- 👤 On success, users are `findOrCreate`d in MongoDB via their `googleId`.
- 🚀 Simplifies onboarding—no manual password entry required.

### Password Hashing & Salting

Passport-local-mongoose automatically:

1. 🔑 **Generates a unique salt** per user.
2. 🧪 **Hashes** the user’s password + salt using PBKDF2 (10000 iterations, SHA-256).
3. 📦 **Stores** only the salt and resulting hash in MongoDB, never the plaintext password.
4. ✅ On login, re-computes the hash with the stored salt and compares it, ensuring credentials remain confidential even if the database is compromised.

---

## Database: MongoDB Atlas

- ☁️ **MongoDB Atlas** hosts the `users` and `secrets` collections on a free-tier cluster.
- 🔐 Stores user credentials (hashed & salted), Google IDs, and submitted secrets.
- 🔒 Provides automatic SSL/TLS encryption in transit and at rest.

---

## Setup & Deployment

1. 🚀 **Clone Repo** & install:
   ```bash
   git clone https://github.com/VINAYMADIVAL/Secrets.git
   cd Secrets
   npm install
   ```
2. ⚙️ **Configure **`` with:
   ```env
   MONGO_URI=<your-atlas-uri>
   SESSION_SECRET=<long-random-string>
   CLIENT_ID=<google-client-id>
   CLIENT_SECRET=<google-client-secret>
   ```
3. 💻 **Run Locally**:
   ```bash
   npm start
   ```
4. ☁️ **Deploy to Render**:
   - 📦 Create a Web Service, connect GitHub, set Build: `npm install`, Start: `node app.js`.
   - 🔑 Add environment variables in Render dashboard.
   - ✅ Deploy and enjoy secure, server-rendered authentication.

---

## Environment Variables

| 🔧 Variable      | 📝 Description                            |
| ---------------- | ----------------------------------------- |
| `MONGO_URI`      | MongoDB Atlas connection string           |
| `SESSION_SECRET` | Secret for express-session cookie signing |
| `CLIENT_ID`      | Google OAuth Client ID                    |
| `CLIENT_SECRET`  | Google OAuth Client Secret                |

---

## Future Improvements

- 🔐 Add multi-factor authentication (MFA)
- 🔄 Rate-limiting on secret submissions to prevent spam
- 📊 Analytics dashboard for secret trends
- 📧 Email notifications for new secrets

---

