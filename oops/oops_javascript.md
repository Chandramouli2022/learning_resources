Great! Let's break down the **four pillars of Object-Oriented Programming (OOP)** in the context of **JavaScript** (especially for **web development**) with clear examples.

---

##  1. **Encapsulation**

**Definition**: Bundling data (properties) and methods (functions) into a single unit — a class — and restricting direct access to some of the object’s components.

**In JavaScript (with private fields)**:

```javascript
class User {
  #password; // private field

  constructor(name, password) {
    this.name = name;
    this.#password = password;
  }

  checkPassword(input) {
    return this.#password === input;
  }
}

const user = new User("Alice", "secret");
console.log(user.name);         // OK: Alice
console.log(user.#password);    // ❌ Error: private field
```

 Encapsulation helps prevent accidental modification of internal states.

---

##  2. **Inheritance**

**Definition**: One class (child) inherits properties and methods from another (parent).

**In JavaScript**:

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks.`);
  }
}

const dog = new Dog("Buddy");
dog.speak(); // Buddy barks.
```

 Inheritance promotes code reuse.

---

##  3. **Polymorphism**

**Definition**: Objects of different classes can be treated as instances of the same class through a common interface, but each behaves differently.

**Example using method overriding**:

```javascript
class Shape {
  draw() {
    console.log("Drawing a shape.");
  }
}

class Circle extends Shape {
  draw() {
    console.log("Drawing a circle.");
  }
}

class Square extends Shape {
  draw() {
    console.log("Drawing a square.");
  }
}

const shapes = [new Circle(), new Square()];
shapes.forEach(shape => shape.draw());
```

 Polymorphism lets you write flexible, extensible code.

---

##  4. **Abstraction**

**Definition**: Hiding complex implementation details and showing only essential features.

**In JavaScript**, while we don't have true abstract classes like in Java, we simulate them with base classes and by throwing errors in methods that should be overridden.

```javascript
class Database {
  connect() {
    throw new Error("Method 'connect()' must be implemented.");
  }
}

class MySQL extends Database {
  connect() {
    console.log("Connected to MySQL.");
  }
}

const db = new MySQL();
db.connect(); // Connected to MySQL
```

Abstraction hides *how* something works, showing only *what* it does.

---

## Summary Table

| Pillar        | Key Concept                     | JavaScript Feature            |
| ------------- | ------------------------------- | ----------------------------- |
| Encapsulation | Private properties/methods      | `#privateFields`, closures    |
| Inheritance   | Share behavior via base classes | `class`, `extends`, `super()` |
| Polymorphism  | Same method, different behavior | Method overriding             |
| Abstraction   | Hide internal logic             | Base classes, error stubs     |

---

