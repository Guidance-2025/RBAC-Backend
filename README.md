# **RBAC Backend Documentation**

## **1. Introduction**

### **Project Overview**
The **RBAC-Backend** is a **Node.js** backend application implementing **Role-Based Access Control (RBAC)**. It enables administrators to manage user roles, register users, and authenticate them using **JWT-based authentication**. The system ensures secure user access control with proper role-based permissions.

### **Key Features**
- **User Registration & Authentication** (Signup/Login)
- **Password Hashing** using Bcrypt
- **Role Management** (Admin, Manager, Moderator, etc.)
- **Middleware for Role-Based Authorization**
- **User Dashboard for Role Viewing**
- **Secure Authentication with JWT**
- **MongoDB for Data Storage**

---

## **2. Technology Stack**
- **Backend Framework:** Node.js with Express
- **Database:** MongoDB (Mongoose ORM)
- **Authentication:** JSON Web Token (JWT)
- **Security:** Bcrypt for password hashing
- **Deployment:** Vercel

---

## **3. Project Structure**

The project follows a **modular and scalable** folder structure:

```
role-based-backend/
  ├── src/
  │   ├── config/         # Configuration files (database, environment)
  │   ├── controllers/    # Business logic & API handlers
  │   ├── middleware/     # Authentication & authorization middleware
  │   ├── models/         # Database schemas (Mongoose models)
  │   ├── routes/         # API route definitions
  │   ├── utils/          # Helper functions (encryption, validation)
  │   ├── app.js         # Express application setup
  │   └── server.js      # Server entry point
  ├── .env               # Environment variables
  ├── .gitignore         # Git ignore file
  ├── package.json       # Dependencies & scripts
  ├── README.md          # Documentation
```

---

## **4. Installation & Setup**

### **Prerequisites**
Ensure you have the following installed:
- **Node.js (>=16.x)**
- **MongoDB (Local or Atlas Cloud)**

### **Setup Instructions**

#### **1. Clone the Repository**
```bash
git clone <repository-url>
cd role-based-backend
```

#### **2. Install Dependencies**
```bash
npm install
```

#### **3. Configure Environment Variables**
Create a `.env` file in the root directory and add:
```env
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_secret_key
PORT=3000
```

#### **4. Start the Server**
```bash
npm start
```

---

## **5. API Documentation**

### **Authentication Endpoints**

| Method | Endpoint                     | Description                    |
|--------|------------------------------|--------------------------------|
| POST   | `/api/auth/signup`           | Register a new user            |
| POST   | `/api/auth/login`            | Authenticate user and get token |

#### **Example Request: User Registration**
```http
POST /api/auth/signup
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securepass",
  "age": 25,
  "gender": "Male"
}
```

#### **Example Response:**
```json
{
  "message": "User registered successfully",
  "token": "jwt-token-here"
}
```

### **User Management**

| Method | Endpoint                     | Description                     |
|--------|------------------------------|---------------------------------|
| GET    | `/api/users/profile`         | Get user profile (Auth required) |
| PUT    | `/api/users/profile`         | Update user profile             |

### **Role Management**

| Method | Endpoint                     | Description                        |
|--------|------------------------------|------------------------------------|
| POST   | `/api/roles`                 | Create a new role (Admin only)    |
| GET    | `/api/roles`                 | Fetch all roles                   |
| PUT    | `/api/roles/:id`             | Update a role (Admin only)        |
| DELETE | `/api/roles/:id`             | Delete a role (Admin only)        |

---

## **6. Database Schema**

### **User Schema**
| Column   | Type   | Constraints            |
|----------|-------|-----------------------|
| id       | UUID  | Primary Key           |
| name     | String | Required              |
| email    | String | Unique, Required      |
| password | String | Hashed, Required      |
| role     | String | Default: 'user'       |

### **Role Schema**
| Column | Type   | Constraints |
|--------|-------|-------------|
| id     | UUID  | Primary Key |
| name   | String | Unique      |

---

## **7. Security & Middleware**

- **Authentication:** JWT-based authentication for secure access.
- **Authorization:** Middleware ensures role-based access control.
- **Password Security:** Bcrypt hashing for storing user passwords.
- **Rate Limiting:** Prevents excessive API requests.
- **CORS Policy:** Secure cross-origin requests.
- **Input Validation:** Prevents SQL Injection & XSS attacks.

---

## **8. Deployment**

### **Vercel Deployment**
The backend is deployed on **Vercel** and can be accessed at:
**Base URL:** `https://role-based-backend-gamma.vercel.app`

### **Docker Deployment (Optional)**
```bash
docker build -t rbac-backend .
docker run -p 3000:3000 rbac-backend
```

### **CI/CD Integration**
- **GitHub Actions** for automated testing & deployment
- **Vercel** for seamless backend deployment

---

## **9. Troubleshooting & FAQs**

### **Common Issues & Solutions**

**Issue:** "MongoDB connection failed"
- **Solution:** Ensure MongoDB URI is correctly configured in `.env`.

**Issue:** "JWT Authentication error"
- **Solution:** Verify the correct token format and ensure it's not expired.

**Issue:** "CORS errors from frontend"
- **Solution:** Update the `CORS` configuration in Express middleware.

---

## **10. Contributors & Contact**
For questions, contributions, or reporting issues, contact:
- **Lead Developer:** Tushar Tanish
- **Email:** tushartanish10@gmail.com
- **GitHub:** [https://github.com/tushartanish]

**License:** This project is licensed under the **MIT License**.

---

_This document serves as a comprehensive guide to the RBAC Backend system, ensuring clarity and ease of use for developers and contributors._ 🚀

