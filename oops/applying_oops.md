Let’s walk through a **real-world example** applying all **4 OOP pillars in JavaScript** — using a **User Authentication Module** that you might find in a web app.

this ia a mini system where users can log in, and this can handle different types of users (e.g., Admin and Guest) with shared and specialized behaviors.

---

## 🎯 Objective:

Implement a `User` class system that uses:

* ✅ **Encapsulation** to protect password
* ✅ **Inheritance** to create different user types
* ✅ **Polymorphism** to handle user-specific behavior
* ✅ **Abstraction** to hide sensitive login logic

---

## ✅ Code Example (All Four Pillars):

```javascript
// Abstraction + Encapsulation
class User {
  #password; // 🔐 Private (Encapsulation)

  constructor(username, password) {
    this.username = username;
    this.#password = password;
  }

  login(inputPassword) {
    if (this.#password === inputPassword) {
      console.log(`${this.username} logged in successfully.`);
    } else {
      console.log("Login failed: Invalid password.");
    }
  }

  // Abstract method placeholder
  accessDashboard() {
    throw new Error("accessDashboard() must be implemented by subclass.");
  }
}

// Inheritance
class Admin extends User {
  accessDashboard() {
    console.log("Accessing admin dashboard with full privileges.");
  }
}

// Inheritance
class Guest extends User {
  accessDashboard() {
    console.log("Accessing guest dashboard with limited access.");
  }
}

// Polymorphism in action
const users = [
  new Admin("admin_user", "admin123"),
  new Guest("guest_user", "guest123"),
];

// Simulate login
users[0].login("admin123");  // ✅
users[1].login("wrongpass"); // ❌

// Polymorphism: Different dashboards, same method
users.forEach(user => user.accessDashboard());
```

---

## 🔍 Breakdown of OOP Pillars:

| OOP Pillar        | How It's Used                                                                       |
| ----------------- | ----------------------------------------------------------------------------------- |
| **Encapsulation** | `#password` is private, not accessible from outside the class                       |
| **Inheritance**   | `Admin` and `Guest` classes extend `User`                                           |
| **Polymorphism**  | `accessDashboard()` behaves differently for each subclass                           |
| **Abstraction**   | `User` defines `accessDashboard()` but does not implement it (forced in subclasses) |

---

## 🔌 Real Web Dev Use Case:

This is similar to how **backend systems handle user roles** — with user types like `Admin`, `Editor`, or `Viewer`, each with different access levels, all based on a shared base `User` structure.


