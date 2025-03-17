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

## Deployed API

The API is deployed and accessible at:
**Base URL**: [https://role-based-backend-gamma.vercel.app](https://role-based-backend-gamma.vercel.app)

## 1 Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Database Setup](#database-setup)
- [API Documentation](#api-documentation)
  - [Authentication](#authentication)
  - [User Management](#user-management)
  - [Role Management](#role-management)
- [Testing with Postman](#testing-with-postman)
- [Error Handling](#error-handling)
- [Security Considerations](#security-considerations)

## 2 Features

- User authentication (signup, login)
- Role-based access control
- Role management (create, update, delete roles)
- User management (view, update user profiles)
- Permission-based authorization
```

3. Create a `.env` file in the root directory with the following variables:
```
PORT=3000
MONGODB_URI=mongodb://localhost:27017/role-based-auth
JWT_SECRET=your_jwt_secret_key
```

4. Seed the database with initial roles and admin user:
```bash
node src/utils/seedRoles.js
```

5. Start the server:
```bash
npm start
```



The seed script creates the following default roles:
- `admin`: Has all permissions (`manage_users`, `manage_roles`, `view_dashboard`)
- `manager`: Has permissions (`manage_users`, `view_dashboard`)
- `user`: Has permission (`view_dashboard`)

It also creates a default admin user:
- Email: `admin@example.com`
- Password: `Admin@123`

## API Documentation

All API endpoints are available at the base URL: `https://role-based-backend-gamma.vercel.app`

### Authentication

#### Register a new user
- **URL**: `https://role-based-backend-gamma.vercel.app/api/auth/signup`
- **Method**: `POST`
- **Auth required**: No
- **Body**:
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "gender": "male",
  "age": 30
}
```
- **Success Response**: `201 Created`
```json
{
  "message": "User registered successfully",
  "token": "jwt_token_here",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "gender": "male",
    "age": 30,
    "role": "user"
  }
}
```
- **Error Response**: `400 Bad Request`
```json
{
  "errors": [
    {
      "msg": "Email is not valid",
      "param": "email",
      "location": "body"
    }
  ]
}
```

#### Login
- **URL**: `https://role-based-backend-gamma.vercel.app/api/auth/login`
- **Method**: `POST`
- **Auth required**: No
- **Body**:
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```
- **Success Response**: `200 OK`
```json
{
  "message": "Login successful",
  "token": "jwt_token_here",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "gender": "male",
    "age": 30,
    "role": "user"
  }
}
```
- **Error Response**: `400 Bad Request`
```json
{
  "message": "Invalid credentials"
}
```

#### Get User Profile
- **URL**: `https://role-based-backend-gamma.vercel.app/api/auth/profile`
- **Method**: `GET`
- **Auth required**: Yes (JWT token in Authorization header)
- **Headers**:
```
Authorization: Bearer <your_jwt_token>
```
- **Success Response**: `200 OK`
```json
{
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "gender": "male",
    "age": 30,
    "role": "user"
  }
}
```
- **Error Response**: `401 Unauthorized`
```json
{
  "message": "Authentication required"
}
```

### User Management

#### Get User Profile
- **URL**: `https://role-based-backend-gamma.vercel.app/api/users/profile`
- **Method**: `GET`
- **Auth required**: Yes (JWT token in Authorization header)
- **Headers**:
```
Authorization: Bearer <your_jwt_token>
```
- **Success Response**: `200 OK`
```json
{
  "id": "user_id",
  "name": "John Doe",
  "email": "john@example.com",
  "gender": "male",
  "age": 30,
  "role": "user"
}
```
- **Error Response**: `401 Unauthorized`
```json
{
  "message": "Authentication required"
}
```

#### Update User Profile
- **URL**: `https://role-based-backend-gamma.vercel.app/api/users/profile`
- **Method**: `PUT`
- **Auth required**: Yes (JWT token in Authorization header)
- **Headers**:
```
Authorization: Bearer <your_jwt_token>
```
- **Body**:
```json
{
  "name": "John Updated",
  "email": "john.updated@example.com",
  "gender": "male",
  "age": 31
}
```
- **Success Response**: `200 OK`
```json
{
  "id": "user_id",
  "name": "John Updated",
  "email": "john.updated@example.com",
  "gender": "male",
  "age": 31,
  "role": "user"
}
```
- **Error Response**: `400 Bad Request`
```json
{
  "message": "Server error"
}
```

#### Get All Users (Admin Only)
- **URL**: `https://role-based-backend-gamma.vercel.app/api/users`
- **Method**: `GET`
- **Auth required**: Yes (JWT token in Authorization header with admin role)
- **Headers**:
```
Authorization: Bearer <your_jwt_token>
```
- **Success Response**: `200 OK`
```json
[
  {
    "id": "user_id_1",
    "name": "Admin User",
    "email": "admin@example.com",
    "gender": "other",
    "age": 30,
    "role": {
      "id": "role_id",
      "name": "admin",
      "permissions": ["manage_users", "manage_roles", "view_dashboard"]
    }
  },
  {
    "id": "user_id_2",
    "name": "John Doe",
    "email": "john@example.com",
    "gender": "male",
    "age": 30,
    "role": {
      "id": "role_id",
      "name": "user",
      "permissions": ["view_dashboard"]
    }
  }
]
```
- **Error Response**: `403 Forbidden`
```json
{
  "message": "Access denied: Insufficient permissions"
}
```

#### Assign Role to User (Admin Only)
- **URL**: `https://role-based-backend-gamma.vercel.app/api/users/assign-role`
- **Method**: `POST`
- **Auth required**: Yes (JWT token in Authorization header with admin role)
- **Headers**:
```
Authorization: Bearer <your_jwt_token>
```
- **Body**:
```json
{
  "userId": "user_id",
  "roleId": "role_id"
}
```
- **Success Response**: `200 OK`
```json
{
  "message": "Role assigned successfully",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "role": "manager"
  }
}
```
- **Error Response**: `404 Not Found`
```json
{
  "message": "User not found"
}
```

### Role Management

#### Create a New Role (Admin Only)
- **URL**: `https://role-based-backend-gamma.vercel.app/api/roles`
- **Method**: `POST`
- **Auth required**: Yes (JWT token in Authorization header with admin role)
- **Headers**:
```
Authorization: Bearer <your_jwt_token>
```
- **Body**:
```json
{
  "name": "editor",
  "permissions": ["edit_content", "view_dashboard"]
}
```
- **Success Response**: `201 Created`
```json
{
  "message": "Role created successfully",
  "role": {
    "id": "role_id",
    "name": "editor",
    "permissions": ["edit_content", "view_dashboard"]
  }
}
```
- **Error Response**: `403 Forbidden`
```json
{
  "message": "Access denied: Insufficient permissions"
}
```

#### Get All Roles
- **URL**: `https://role-based-backend-gamma.vercel.app/api/roles`
- **Method**: `GET`
- **Auth required**: Yes (JWT token in Authorization header)
- **Headers**:
```
Authorization: Bearer <your_jwt_token>
```
- **Success Response**: `200 OK`
```json
[
  {
    "id": "role_id_1",
    "name": "admin",
    "permissions": ["manage_users", "manage_roles", "view_dashboard"]
  },
  {
    "id": "role_id_2",
    "name": "manager",
    "permissions": ["manage_users", "view_dashboard"]
  },
  {
    "id": "role_id_3",
    "name": "user",
    "permissions": ["view_dashboard"]
  },
  {
    "id": "role_id_4",
    "name": "editor",
    "permissions": ["edit_content", "view_dashboard"]
  }
]
```
- **Error Response**: `401 Unauthorized`
```json
{
  "message": "Authentication required"
}
```

#### Get a Role by ID
- **URL**: `https://role-based-backend-gamma.vercel.app/api/roles/:id`
- **Method**: `GET`
- **Auth required**: Yes (JWT token in Authorization header)
- **Headers**:
```
Authorization: Bearer <your_jwt_token>
```
- **Success Response**: `200 OK`
```json
{
  "id": "role_id",
  "name": "editor",
  "permissions": ["edit_content", "view_dashboard"]
}
```
- **Error Response**: `404 Not Found`
```json
{
  "message": "Role not found"
}
```

#### Update a Role (Admin Only)
- **URL**: `https://role-based-backend-gamma.vercel.app/api/roles/:id`
- **Method**: `PUT`
- **Auth required**: Yes (JWT token in Authorization header with admin role)
- **Headers**:
```
Authorization: Bearer <your_jwt_token>
```
- **Body**:
```json
{
  "name": "editor",
  "permissions": ["edit_content", "view_dashboard", "publish_content"]
}
```
- **Success Response**: `200 OK`
```json
{
  "message": "Role updated successfully",
  "role": {
    "id": "role_id",
    "name": "editor",
    "permissions": ["edit_content", "view_dashboard", "publish_content"]
  }
}
```
- **Error Response**: `404 Not Found`
```json
{
  "message": "Role not found"
}
```

#### Delete a Role (Admin Only)
- **URL**: `https://role-based-backend-gamma.vercel.app/api/roles/:id`
- **Method**: `DELETE`
- **Auth required**: Yes (JWT token in Authorization header with admin role)
- **Headers**:
```
Authorization: Bearer <your_jwt_token>
```
- **Success Response**: `200 OK`
```json
{
  "message": "Role deleted successfully"
}
```
- **Error Response**: `404 Not Found`
```json
{
  "message": "Role not found"
}
```

## Testing with Postman

Here are some example Postman requests to test the deployed API:

### 1. Register a new user

```
POST https://role-based-backend-gamma.vercel.app/api/auth/signup
Content-Type: application/json

{
  "name": "Test User",
  "email": "test@example.com",
  "password": "password123",
  "gender": "male",
  "age": 25
}
```

### 2. Login

```
POST https://role-based-backend-gamma.vercel.app/api/auth/login
Content-Type: application/json

{
  "email": "admin@example.com",
  "password": "Admin@123"
}
```

Save the token from the response for subsequent requests.

### 3. Create a new role (as admin)

```
POST https://role-based-backend-gamma.vercel.app/api/roles
Content-Type: application/json
Authorization: Bearer <your_token>

{
  "name": "content_creator",
  "permissions": ["create_content", "view_dashboard"]
}
```

### 4. Get all roles

```
GET https://role-based-backend-gamma.vercel.app/api/roles
Authorization: Bearer <your_token>
```

### 5. Assign a role to a user (as admin)

First, get all users to find the user ID:

```
GET https://role-based-backend-gamma.vercel.app/api/users
Authorization: Bearer <your_token>
```

Then, assign a role to a user:

```
POST https://role-based-backend-gamma.vercel.app/api/users/assign-role
Content-Type: application/json
Authorization: Bearer <your_token>

{
  "userId": "<user_id>",
  "roleId": "<role_id>"
}
```

### 6. Update a role (as admin)

```
PUT https://role-based-backend-gamma.vercel.app/api/roles/<role_id>
Content-Type: application/json
Authorization: Bearer <your_token>

{
  "name": "content_creator",
  "permissions": ["create_content", "edit_content", "view_dashboard"]
}
```

### 7. Delete a role (as admin)

```
DELETE https://role-based-backend-gamma.vercel.app/api/roles/<role_id>
Authorization: Bearer <your_token>
```

## Error Handling

The API returns appropriate HTTP status codes and error messages for different scenarios:

- `400 Bad Request`: Invalid input data
- `401 Unauthorized`: Authentication required or invalid credentials
- `403 Forbidden`: Insufficient permissions
- `404 Not Found`: Resource not found
- `500 Internal Server Error`: Server-side error

Example error response:

```json
{
  "message": "Access denied: Insufficient permissions"
}
```

## Security Considerations

- Passwords are hashed using bcrypt before storing in the database
- JWT tokens are used for authentication
- Role-based access control is implemented for authorization
- Input validation is performed using express-validator
- CORS is enabled to allow requests from any origin, making the API accessible from any client

## Cross-Origin Resource Sharing (CORS)

This API has CORS enabled with the following configuration:

- **Origin**: Allows requests from any origin (`*`)
- **Methods**: Supports all HTTP methods (GET, POST, PUT, DELETE, etc.)
- **Headers**: Accepts Content-Type and Authorization headers
- **Preflight**: Properly handles OPTIONS preflight requests

This configuration ensures that the API can be accessed from any client, including web browsers, mobile apps, and other servers, regardless of their origin.



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


**License:** This project is licensed under the **MIT License**.

---

_This document serves as a comprehensive guide to the RBAC Backend system, ensuring clarity and ease of use for developers and contributors._ 🚀

