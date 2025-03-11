# RBAC-Backend

This project is a Node.js backend application that implements role-based access control. It allows an admin to manage user roles, add roles and users, and enables user login and signup. The application uses MongoDB for data storage and implements JWT for authentication.

## Features

- User registration with name, email, gender, and age.
- User login with encrypted hashed passwords.
- Role management (admin, manager, moderator, etc.).
- Dashboard for users to view their roles.
- Middleware for authentication and role checking.

## Technologies Used

- Node.js
- Express.js
- MongoDB (with Mongoose)
- JWT (JSON Web Tokens)
- Bcrypt for password hashing

## Project Structure

```
role-based-backend
├── src
│   ├── config
│   │   ├── database.js
│   │   └── env.js
│   ├── controllers
│   │   ├── authController.js
│   │   ├── roleController.js
│   │   └── userController.js
│   ├── middleware
│   │   ├── auth.js
│   │   ├── errorHandler.js
│   │   └── roleCheck.js
│   ├── models
│   │   ├── Role.js
│   │   └── User.js
│   ├── routes
│   │   ├── authRoutes.js
│   │   ├── roleRoutes.js
│   │   └── userRoutes.js
│   ├── utils
│   │   ├── encryption.js
│   │   └── validation.js
│   ├── app.js
│   └── server.js
├── .env
├── .gitignore
├── package.json
└── README.md
```
