Great! Let's take this to the next level by:

1. 🔐 **Securing login with JWT (JSON Web Tokens)**
2. 🗄️ **Storing users in MongoDB** (instead of hardcoding them)

---

## ✅ Features We’ll Build:

* Register and store users in **MongoDB**.
* Login and receive a **JWT token**.
* Access **protected dashboard routes** based on user type (`Admin`, `Guest`) using **role-based access control**.

---

## 🔧 Prerequisites

Install the required packages:

```bash
npm install express mongoose bcryptjs jsonwebtoken dotenv
```

Create a `.env` file:

```
MONGO_URI=mongodb://localhost:27017/oop_auth_demo
JWT_SECRET=supersecretkey
```

---

## 📁 Project File Structure

```
- index.js
- models/
  - User.js
- .env
```

---

## 🧠 Step-by-Step Breakdown

### 1. **User Model with Role and Hashed Password**

```js
// models/User.js
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');

const userSchema = new mongoose.Schema({
  username: String,
  password: String,
  role: { type: String, enum: ['admin', 'guest'], default: 'guest' }
});

// Encrypt password before saving
userSchema.pre('save', async function () {
  this.password = await bcrypt.hash(this.password, 10);
});

// Compare password method
userSchema.methods.comparePassword = function (input) {
  return bcrypt.compare(input, this.password);
};

module.exports = mongoose.model('User', userSchema);
```

---

### 2. **Express Server with Auth and Role-Based Access**

```js
// index.js
require('dotenv').config();
const express = require('express');
const mongoose = require('mongoose');
const jwt = require('jsonwebtoken');
const User = require('./models/User');

const app = express();
app.use(express.json());

// Connect to MongoDB
mongoose.connect(process.env.MONGO_URI)
  .then(() => console.log("MongoDB connected"))
  .catch(err => console.error(err));

// Generate JWT
function generateToken(user) {
  return jwt.sign(
    { id: user._id, username: user.username, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '1h' }
  );
}

// Middleware to verify JWT and attach user to request
function authenticate(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).send('No token');

  try {
    req.user = jwt.verify(token, process.env.JWT_SECRET);
    next();
  } catch {
    res.status(401).send('Invalid token');
  }
}

// Role-based access middleware
function authorize(...roles) {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).send('Access denied');
    }
    next();
  };
}

// Register
app.post('/register', async (req, res) => {
  const { username, password, role } = req.body;
  const existing = await User.findOne({ username });
  if (existing) return res.status(400).send('User exists');

  const user = new User({ username, password, role });
  await user.save();
  res.send('User registered');
});

// Login
app.post('/login', async (req, res) => {
  const { username, password } = req.body;
  const user = await User.findOne({ username });
  if (!user || !(await user.comparePassword(password))) {
    return res.status(401).send('Invalid credentials');
  }

  const token = generateToken(user);
  res.json({ token });
});

// Dashboard
app.get('/dashboard', authenticate, (req, res) => {
  res.send(`Welcome ${req.user.username}, role: ${req.user.role}`);
});

// Admin-only route
app.get('/admin', authenticate, authorize('admin'), (req, res) => {
  res.send('Admin dashboard - full access');
});

// Guest-only route
app.get('/guest', authenticate, authorize('guest'), (req, res) => {
  res.send('Guest dashboard - limited access');
});

app.listen(3000, () => console.log("Server running on http://localhost:3000"));
```

---

## 🧪 Test the Flow

### 1. Register a user

```bash
POST /register
{
  "username": "admin1",
  "password": "pass123",
  "role": "admin"
}
```

### 2. Login to get token

```bash
POST /login
{
  "username": "admin1",
  "password": "pass123"
}
```

Copy the token and use it as a Bearer token in headers:

```
Authorization: Bearer YOUR_TOKEN
```

### 3. Access routes

* `/dashboard` → accessible to all logged-in users
* `/admin` → only for role `"admin"`
* `/guest` → only for role `"guest"`

---

## ✅ Summary of OOP Concepts in Express + MongoDB

| OOP Principle     | Example                                                        |
| ----------------- | -------------------------------------------------------------- |
| **Encapsulation** | Password hashing and comparison hidden inside Mongoose methods |
| **Inheritance**   | User roles conceptually extend base behaviors                  |
| **Polymorphism**  | Different roles yield different route behaviors                |
| **Abstraction**   | Business logic hidden behind JWT, middleware, and models       |

---
