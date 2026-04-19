# 📚 Library Management System

A full-stack Library Management Web App designed for administrators and users to manage books and publishers. Built with **Node.js**, **Express**, **EJS**, **MySQL**, and **Bootstrap**, it supports full CRUD operations, user authentication, image uploads, a borrow cart, fines, and profile management.


---
### 📁 Project Structure
library-1/
├── app.js
├── package.json
├── package-lock.json
├── public/
│   └── images/
├── views/
│   ├── addBook.ejs
│   ├── addPublisher.ejs
│   ├── book.ejs
│   ├── cart.ejs
│   ├── checkout-success.ejs
│   ├── homepage.ejs
│   ├── login.ejs
│   ├── profile.ejs
│   ├── publishers.ejs
│   ├── register.ejs
│   ├── updateBook.ejs
│   ├── updatePublisher.ejs
│   └── ...


### 👤 User and Admin Roles
Role	Permissions
Admin	Full access
User	View, borrow, return, profile update

Note: If the render link takes a while to load, access the application locally instead for faster reviewing


#### 🧪 Sample Routes
Method	Route	Description
GET	/library	View books
GET	/publishers	View publishers
POST	/publishers/add	Add publisher
POST	/login	Login user

## ✨ Key Features
### 🧾 Book Management
- View all books with title, author, year, and cover image
- Add new books with image upload
- Edit book details and image
- Delete books
- Out-of-stock or unavailable message handling

### 🏷️ Publisher Management - Margaret (My Contribution)
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

### 🧰 Tech Stack

| Layer        | Technology                          |
|-------------|--------------------------------------|
| Backend      | Node.js, Express.js                 |
| Frontend     | HTML, CSS, Bootstrap, EJS           |
| Database     | MySQL                               |
| File Upload  | Multer                              |
| Authentication | express-session                   |
| Others       | connect-flash, mysql2               |

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
- Authentication and authorization are implemented using Express middleware functions (`checkAuthenticated`, `checkAdmin`)
- Session-based storage is used to manage login state and user roles (RBAC)
- MySQL is used for persistent data storage

### Key Design Characteristics:
- No separate controller/service layers (logic handled directly in routes)
- Middleware defined inside main file (app.js)
- RBAC implemented using session role checks and EJS conditional rendering
- Fully server-rendered application (no SPA framework)



## 👥 Team Contributions & System Scope

This CA2 project was developed collaboratively, with each member responsible for different modules.

### 📚 Module Ownership

- **Books Page – Sahana**
  - Full CRUD for books
  - Image upload
  - Search + stock handling


- **Login, Registration and Users Page + RBAC - Yi Xi**
  - Full CRUD for users
  - User registration and authentication
  - Implemented RBAC methods such as checkAuthenticated and checkAdmin to verify whether user 
  has logged in as well as user role

## 📚 Publisher Page – Margaret (Me)

Developed full CRUD functionality for the publisher module, including search capability.

- Create, view, update, delete publishers (/publishers, /addPublisher, /updatePublisher, /deletePublisher)
- Search by name, address, country
- Admin-only access control for edit/delete/add


All publisher logic is handled inside a single Express file (app.js).  
No MVC structure was used; routes, logic, and database queries are all combined for simplicity.


### Loan Page – Samuel
Borrowing workflow
Return functionality
Loan tracking system

## 🔹 Overview
This system implements session-based authentication and role-based access control (RBAC).

Yi Xi was responsible for developing the **User module**, which manages user accounts, authentication, and role assignment. This module provides the foundation for system-wide access control.

Sahana and I both integrated the existing RBAC system into our respective modules (Books and Publisher) by using session-based user roles to control access to sensitive operations.

This module supports system-wide access control by managing user authentication and role assignment, which is used to restrict create, update, and delete operations for publishers and books to admin users only.
---

## 🔹 Margaret (My) Contribution – Publisher Module (Integration of Access Control)

I was responsible for designing and implementing the Publisher Management module, which includes full CRUD (Create, Read, Update, Delete) functionality with database integration and server-side rendering using EJS.

I integrated existing access control mechanisms into this module by applying backend middleware (checkAdmin) and session-based role checks to restrict create, update, and delete operations to admin users only.

I also implemented conditional rendering in EJS to display edit and delete options only for admin users.


--- 

## Backend Implementation (Node.JS)
```node.js

app.get('/addPublisher', checkAuthenticated, checkAdmin, (req, res) => {
    res.render('addPublisher', { user: req.session.user });
});

app.get('/updatePublisher/:id', checkAuthenticated, checkAdmin, (req, res) => {
    const publisher_id = req.params.id;

    pool.query('SELECT * FROM publishers WHERE publisher_id = ?', [publisher_id], (error, results) => {
        if (error) return res.status(500).send('Error retrieving publisher');

        if (results.length > 0) {
            res.render('updatePublisher', { publishers: results[0] });
        } else {
            res.status(404).send('Publisher not found');
        }
    });
});

app.get('/deletePublisher/:id', checkAuthenticated, checkAdmin, (req, res) => {
    const publisher_id = req.params.id;

    pool.query('DELETE FROM publishers WHERE publisher_id = ?', [publisher_id], (error) => {
        if (error) return res.status(500).send('Error deleting publisher');

        res.redirect('/publishers');
    });
});


## 🖥️ Frontend Implementation (EJS – Conditional Rendering)

```ejs
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
       onclick="return confirm('Are you sure?')"
       class="btn btn-outline-danger btn-sm">
       Delete
    </a>
  </td>
<% } %>





--
## Database Schema (Simplified)

CREATE TABLE books (
  bookId INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(255),
  author VARCHAR(255),
  genre VARCHAR(100),
  quantity INT,
  coverImage VARCHAR(255)
);

CREATE TABLE publishers (
  publisher_id INT AUTO_INCREMENT PRIMARY KEY,
  publisher_name VARCHAR(255),
  publisher_contact VARCHAR(100),
  publisher_country VARCHAR(100),
  address TEXT
);

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(255),
  email VARCHAR(255) UNIQUE,
  password VARCHAR(255),
  role ENUM('user','admin') DEFAULT 'user'
);


 ## 🚀 Installation 
1. Install dependencies
- npm install
2. Setup MySQL database
- Create database (librarydb)
- Run schema SQL

3. Configure DB in app.js (Update: Created a new DB as the DB used during the project has expired)

const pool = mysql.createPool({
    host: '784jfr.h.filess.io',
    port: 3307,
    user: 'librarydb_balloonlog',
    password: '3605010e25f53b766dd825672dda6054d248d3c9',
    database: 'librarydb_balloonlog',
    waitForConnections: true,
    connectionLimit: 10,
    queueLimit: 0
});

4. Run the Application
- npx nodemon app.js

5. Access Locally 
- Click on http://localhost:3000


## 🌐 Live Demo
- Link to Live Demo: https://library-management-system-demo-link.onrender.com/login

### Sample Credentials for Login
## Admin
email: admin6@gmail.com
password: margaret

### User
email: user@email.com
password: user1234



#### 📦 Dependencies
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

Solution: We coordinated as a team to standardize schema fields and role checks. My module was successfully integrated with shared authentication and RBAC logic through collaboration.  

**Shared Authentication** – Integrated Yi Xi's module with mine