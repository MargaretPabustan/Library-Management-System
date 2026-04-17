# 📚 Library Management System

A full-stack Library Management Web App designed for administrators and users to manage books and publishers. Built with **Node.js**, **Express**, **EJS**, **MySQL**, and **Bootstrap**, it supports full CRUD operations, user authentication, image uploads, a borrow cart, fines, and profile management.

---

## ✨ Key Features

### 🧾 Book Management
- View all books with title, author, year, and cover image
- Add new books with image upload
- Edit book details and image
- Delete books
- Out-of-stock or unavailable message handling

### 🏷️ Publisher Management
- Add, view, edit, and delete publishers
- View publisher details for each book

### 👤 User Authentication
- Register, login, and logout
- Role-based views (admin, users)
- Session-based access control

### 📦 Borrow Cart
- Add books to a borrow cart
- Checkout simulation with success page
- Cart clears after confirmation



### 👥 User Profile
- Profile viewing and editing
- See borrowed books and personal information

### 📋 Admin Dashboard
- Manage users, books, and publishers
- Access extra admin-only functionality




## 🏗️ System Architecture

Client (Browser / EJS Views)  
↓  
Express.js (Route Handlers + Business Logic)  
↓  
MySQL Database  

This project is a **monolithic Express.js application** using server-side rendering with EJS.

### How the system works:
- EJS handles UI rendering on the server side
- Express route handlers manage both routing and business logic
- Authentication and authorization are implemented using Express middleware functions (checkAuthentication and checkAdmin)
- Session-based storage is used to manage login state and user roles (RBAC).  
- MySQL is used for persistent data storage.

### Key Design Characteristics:
- No separate controller/service layers (logic is handled directly in route handlers)
- Middleware functions are defined within the main application file (not modularized)
- RBAC is implemented using session-based role checks in both Express routes and EJS conditional rendering (`req.session.user.role`)
- Fully server-rendered application using EJS (no frontend SPA framework)

---




## 👥 Team Contributions & System Scope
This CA2 project was developed collaboratively, with each member responsible for different modules.

### 📚 Module Ownership

- **Books Page - Sahana**
Implemented full CRUD operations for books including image upload, search bar functionality and stock handling (availability)

## 📚 Publisher Page - Margaret (Me)

Developed full CRUD functionality for the publisher module, including search capability.

- Create, view, update, and delete publishers
- Search publishers by relevant fields (Name, Address, Country)
- Integrated role-based access control (admin-only actions such as edit and delete)
- Implemented frontend permission checks using EJS conditional rendering

All publisher-related logic is handled within a single Express route file. While not separated into a full MVC structure, the code is organised into clear sections for CRUD operations, search functionality, and access control to maintain readability and structure.


- **Loan Page - Samuel**
Implemented borrowing workflow including:
- View borrowed records functionality
- Return functionality (fully working)
- Additional CRUD (Add/Update Loans) were partially implemented
 
- **User Authentication & Access Control – Yi Xi (collaborative module)**  
Collaborated on the application’s role-based access control system by implementing frontend-level permission for my publisher page rendering using EJS.

Yi Xi focused on the User module and role management using session data. I contributed by implementing RBAC within the Publisher module to restrict admin-only actions (edit/delete/add) using session-based checks and EJS conditional rendering.

### Publisher Module RBAC Implementation (publishers.ejs)
<% if (user && user.role === 'admin') { %>
  <th>Edit</th>
  <th>Delete</th>
<% } %>

  <% if (user && user.role === 'admin') { %>
            <td>
              <a href="/updatePublisher/<%= publisher.publisher_id %>" class="btn btn-outline-primary btn-sm">Edit</a>
            </td>
            <td>
              <a href="/deletePublisher/<%= publisher.publisher_id %>" 
                 onclick="return confirm('Are you sure you want to delete this publisher?')" 
                 class="btn btn-outline-danger btn-sm">Delete</a>
            </td>
          <% } %>

| Layer       | Technology                     |
|-------------|---------------------------------|
| Backend     | Node.js, Express.js             |
| Frontend    | HTML, CSS, Bootstrap, EJS       |
| Database    | MySQL                           |
| File Upload | Multer                          |
| Sessions    | express-session                 |
| Views       | EJS templating engine           |

---

## 🗃️ Database Schema (Simplified)

```sql
-- Book Table
CREATE TABLE books (
  bookId INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(255),
  author VARCHAR(255),
  genre VARCHAR(100),
  quantity INT,
  coverImage VARCHAR(255)
);


-- Publisher Table
CREATE TABLE publishers (
  publisher_id INT AUTO_INCREMENT PRIMARY KEY,
  publisher_name VARCHAR(255) NOT NULL,
  publisher_contact VARCHAR(100),
  publisher_country VARCHAR(100),
  address TEXT
);

-- User Table
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  role ENUM('user', 'admin') DEFAULT 'user'
);

-- Project Structure

library-app/
├── app.js                  # Main Express app
├── package.json
├── public/
│   └── images/             # Default image and static assets      
├── views/                  # EJS views
│   ├── addBook.ejs
│   ├── addPublisher.ejs
│   ├── admin.ejs
│   ├── book.ejs
│   ├── cart.ejs
│   ├── checkout-success.ejs
│   ├── forgot-password.ejs
│   ├── forgot-success.ejs
│   ├── homepage.ejs
│   ├── index.ejs
│   ├── library.ejs
│   ├── login.ejs
│   ├── profile.ejs
│   ├── publisherDetails.ejs
│   ├── publishers.ejs
│   ├── register.ejs
│   ├── updateBook.ejs
│   └── updatePublisher.ejs
├── routes/
│   └── library.js          # (Assumed) routes for books and publishers

## 🚀 Installation Process

1. Install dependencies:
npm install
npm init -y 

2. Set up MySQL database:
Create a database (e.g. librarydb)
Run the provided SQL scripts to create tables (books, publishers, users)

3. Configure DB pool in app.js (Updated: new database created for portfolio demonstration purposes, as the DB used for the project has expired)

const pool = mysql.createPool({
    host: '784jfr.h.filess.io',
    port: 3307,
    user: 'librarydb_balloonlog',
    password: 'YOUR_PASSWORD_HERE',
    database: 'librarydb_balloonlog',
    waitForConnections: true,
    connectionLimit: 10,
    queueLimit: 0
});

4. Run the application:
npx nodemon app.js

5. Open in browser:
- Locally: http://localhost:3000 
- Live: (Render deployment link here)

## 🚀 Live Demo
-  https://library-management-system-6-ixpj.onrender.com
  - Login credentials (demo):
  - Admin: admin6@gmail.com / margaret
  - User: user@email.com / user1234
> ⚠️ Note: The Render demo may load slowly or spin on first request due to free-tier hosting cold starts. Once loaded, all modules (including Publisher CRUD + RBAC integration) function correctly.


## 🖥️ Local Setup
For faster testing, you can run the app locally:
1. Clone the repository
2. Install dependencies: `npm install`
3. Configure MySQL database (see schema section)
4. Run the app: `npx nodemon app.js`
5. Open in browser: http://localhost:3000

---

## 🔐 User Roles

Role | Permissions
-----|-------------
Admin | Full access to books, users, publishers
User  | Can view books/publishers/loans pages, search items, borrow and return books, and update own profile

---

## 🧪 Sample Routes

Method | Route | Description
------|-------|-------------
GET | /library | View all books
GET | /library/add | Add a new book
POST | /library/add | Submit book form
GET | /library/edit/:id | Edit existing book
POST | /library/edit/:id | Submit edit form
POST | /library/delete/:id | Delete a book
GET | /publishers | View publishers
GET | /publishers/add | Add a publisher
POST | /publishers/add | Submit publisher form

---

## 📦 Dependencies
{
  "express": "^5.1.0",
  "ejs": "^3.1.10",
  "mysql2": "^3.14.2",
  "express-session": "^1.18.2",
  "multer": "^2.0.2",
  "body-parser": "^2.2.0"
}

 ⚠️ Challenges & Solutions 

--  Margaret's Reflection (below)
1. Learning GitHub workflow
Challenge: At the start, I was unfamiliar with GitHub basics such as committing, pushing, and navigating the repository. This slowed down collaboration.

Solution: I practiced using Git commands (git add, git commit, git push) and learned to manage branches. By the end, I was confident in contributing and keeping my module synced with the team repo.

2. Routing syntax and logic
Challenge: Some Express routes failed due to incorrect syntax or misplaced logic.

Solution: I debugged by carefully checking route definitions, middleware order, and testing endpoints with Postman. This ensured CRUD operations worked consistently.

3. Module Integration
Challenge: Integrating my Publisher module with teammates’ modules required aligning database schema and session data.

Solution: We coordinated as a team to standardize schema fields and role checks. My module was successfully integrated with shared authentication  and RBAC logic through collaboration.

**Shared Authentication** – Integrated Yi Xi's module with mine


