# Breezy-13
A modern, lightweight social network inspired by Twitter/X. Built with Next.js, Node.js, and MongoDB, featuring secure authentication, real-time messaging, media uploads, and a microservices architecture.

# 🌪️ Breezy - Modern Social Network

*Breezy* is a lightweight and performant social network platform inspired by Twitter/X.  
Developed as a **school project** for CESI, this application provides a fluid and engaging user experience with a focus on performance, security, and scalability through a microservices architecture.

---

## ✨ Key Features

- **🔐 Secure Authentication** — Email verification required, JWT with refresh tokens
- **📝 Content Publishing** — Posts with text, images, videos, and Giphy integration
- **💬 Social Interactions** — Like system, hierarchical comments, and sharing
- **👥 Social Network** — User following, personalized feed, profile discovery
- **💌 Private Messaging** — Text messages with image and media support
- **👤 Customizable Profiles** — Secure avatar upload, bio, account management
- **🛡️ Advanced Security** — Granular role system, data validation, authorization middleware
- **📱 Responsive Interface** — Mobile-first design with Tailwind CSS and smooth animations
- **🔄 File Upload** — Dedicated server for secure media management
- **📊 Information Pages** — About, Privacy Policy, and Contact pages

---

## 🏗️ Technical Architecture

### Microservices Structure

Breezy adopts a modular microservices architecture with four main services:

#### 🌐 WebApp (Frontend - Next.js)
**Port: 3001** | **Framework: Next.js 15**

- **Technologies**: React 19, Next.js App Router, Tailwind CSS, Flowbite React
- **State Management**: Context API for authentication and global state
- **HTTP Client**: Axios with automatic interceptors for refresh tokens
- **UI/UX**: Mobile-first responsive interface, CSS animations, dark/light theme
- **Integrations**: Giphy API, React Force Graph 2D, Recharts for analytics

#### 🚀 API (Backend - Node.js)
**Port: 3000** | **Framework: Express.js**

- **Architecture**: MVC pattern with DAO/Factory for data abstraction
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT with refresh tokens, token revocation
- **Security**: bcrypt, data validation, granular authorization middleware
- **Services**: Email (Nodemailer), account verification, role management
- **Tests**: Jest with complete coverage (Controllers, Services, Middlewares)

#### 📁 FileServer (Storage - Express.js)
**Port: 5000** | **Dedicated media service**

- **Upload**: Multer with strict MIME type and size validation
- **Support**: Images (JPEG, PNG, GIF, WebP), videos (MP4, MOV, AVI, WebM)
- **Security**: JWT token authentication, permission validation
- **Features**:
  - Avatar upload with temporary token (before email verification)
  - External URL management (Giphy, third-party media)
  - Size limitation and format validation
  - Secure storage with unique naming

#### 🗄️ Database (MongoDB)
**Port: 27018** | **NoSQL database**

- **Collections**: users, posts, comments, follows, messages, roles
- **Scripts**: Automatic initialization, migrations, test data
- **Indexing**: Optimized indexes for frequent queries
- **Dumps**: Population scripts with consistent data (users, posts, interactions)

---

## 🚀 Installation & Setup

### Prerequisites

- **Node.js** v18+ (v20 LTS recommended)
- **MongoDB** v6.0+ (local or MongoDB Atlas)
- **Docker** (optional, for containerized deployment)
- **Git** for repository cloning

### 1. 📥 Clone the Repository
```bash
git clone https://github.com/Beroud-Dylan/Breezy-13.git
cd Breezy-13
```

### 2. ⚙️ Environment Variables Configuration

#### API (.env in /API)
```env
# MongoDB database
MONGO_URI=mongodb://localhost:27017/breezy_bdd
MONGODB_URI=mongodb://localhost:27017/breezy_bdd

# JWT and security
JWT_SECRET=your_super_secure_jwt_secret_128_characters_minimum
JWT_SECRET_REFRESH=your_different_refresh_jwt_secret
JWT_EXPIRES_IN=2h
BCRYPT_SALT=12

# Server configuration
PORT=3000
WEBAPP_ORIGIN=http://localhost:3001

# Email (for account verification)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your.email@gmail.com
SMTP_PASS=your_gmail_application_password
EMAIL_FROM=noreply@breezy.com
```

#### WebApp (.env.local in /webapp)
```env
# Service URLs
NEXT_PUBLIC_API_URL=http://localhost:3000
NEXT_PUBLIC_REFRESH_URL=http://localhost:3001/refresh-token
NEXT_PUBLIC_LOGIN_URL=/login
NEXT_PUBLIC_FILE_SERVER_URL=http://localhost:5000

# Next.js configuration
NODE_ENV=development
NEXT_PUBLIC_GIPHY_API_KEY=your_optional_giphy_api_key
```

#### FileServer (.env in /FileServer)
```env
# File server configuration
PORT=5000
JWT_SECRET=same_secret_as_api
UPLOAD_DIR=./uploads
MAX_FILE_SIZE=10485760
ALLOWED_MIME_TYPES=image/jpeg,image/png,image/gif,image/webp,video/mp4,video/mov,video/avi,video/webm
```

### 3. 📦 Install Dependencies
```bash
# Install for all services
npm run install-all

# Or manual installation for each service:

# API Backend
cd API && npm install

# WebApp Frontend  
cd ../webapp && npm install

# FileServer
cd ../FileServer && npm install

# Database (scripts)
cd ../Database && npm install
```

### 4. 🗄️ Database Initialization
```bash
# Start MongoDB (if local)
mongod --dbpath /path/to/data/db

# Inject test data (optional but recommended)
node dumps/local.complete_data_dumps.js
```

### 5. 🚀 Start Services

#### Development (4 separate terminals)
```bash
# Terminal 1 - MongoDB (if local)
mongod

# Terminal 2 - API Backend  
cd API && npm run dev

# Terminal 3 - FileServer
cd FileServer && npm run dev

# Terminal 4 - WebApp Frontend
cd webapp && npm run dev
```

#### Production with Docker Compose
```bash
# Build and start all services
docker compose up --build

# In background
docker compose up -d

# Stop services
docker compose down
```

### 6. 🌐 Access the Application

- **Web Application**: [http://localhost:3001](http://localhost:3001)
- **API Backend**: [http://localhost:3000](http://localhost:3000)
- **FileServer**: [http://localhost:5000](http://localhost:5000)
- **MongoDB**: mongodb://localhost:27018

### 7. 👨‍💼 Default Accounts

Test data includes an administrator account:

- **Email**: `admin@breezy.com`
- **Password**: `password`
- **Role**: Administrator (full permissions)

**Test users**:
- `alice.martin@example.com` / `password123`
- `bob.dupont@example.com` / `password123`
- `charlie.bernard@example.com` / `password123`

---

## 📡 API Documentation

### 📖 Complete Documentation

**Interactive documentation available**: [https://beroud-dylan.github.io/Breezy-13/](https://beroud-dylan.github.io/Breezy-13/)

### Main Endpoints

#### 🔐 Authentication
```http
POST /auth                    # Login
POST /refresh-token           # Token renewal
POST /disconnect             # Logout
POST /verify                 # Email verification
POST /verify/resend          # Resend verification email
```

#### 👥 Users
```http
GET    /users                # List users
POST   /users                # Create user
PATCH  /users/:id            # Update
DELETE /users/:id            # Delete
POST   /users/update-avatar  # Update avatar after registration
```

#### 📝 Posts
```http
GET    /posts                # List posts (with filters)
POST   /posts                # Create post
PATCH  /posts/:id            # Update
DELETE /posts/:id            # Delete
GET    /posts/:id            # Post comments
```

#### 💬 Comments
```http
GET    /comments             # List comments
POST   /comments             # Create comment
PATCH  /comments/:id         # Update
DELETE /comments/:id         # Delete
```

#### 👥 Follows
```http
GET    /follows              # List follows
POST   /follows              # Create follow
DELETE /follows/:id          # Unfollow
```

#### 💌 Messages
```http
GET    /messages             # List messages (with filters)
POST   /messages             # Send message
PATCH  /messages/:id         # Update message
DELETE /messages/:id         # Delete message
```

#### 🛡️ Roles (Admin)
```http
GET    /roles                # List roles
POST   /roles                # Create role
PATCH  /roles/:id            # Update role
DELETE /roles/:id            # Delete role
```

#### 📁 FileServer
```http
GET    /files/:filename      # Retrieve file
POST   /upload               # Upload file (authenticated)
POST   /upload-avatar-registration  # Avatar upload with temporary token
```

---

## 🛠️ Technologies Used

### Backend
- **Node.js** v18+ with **Express.js** 5.x
- **MongoDB** 6.x with **Mongoose** ODM
- **JWT** for authentication with refresh tokens
- **bcrypt** for password hashing
- **Nodemailer** for email sending
- **Multer** for file uploads
- **Jest** for unit and integration tests

### Frontend
- **Next.js** 15 with **App Router**
- **React** 19 with hooks and Context API
- **Tailwind CSS** 3.x for styling
- **Flowbite React** for UI components
- **Axios** for HTTP requests
- **Giphy API** integration

### DevOps & Tools
- **Docker** and **Docker Compose** for containerization
- **ESLint** and **Prettier** for code quality
- **Jest** for testing
- **Nodemon** for development
- **Git** for version control

---

## 📞 Support & Contact

### Resources
- **Repository**: [GitHub - Breezy-13](https://github.com/Beroud-Dylan/Breezy-13.git)
- **API Documentation**: [https://cesi-fisa-info-24-27.github.io/Breezy-13/](https://beroud-dylan.github.io/Breezy-13/)
- **Issues**: [GitHub Issues](https://github.com/Beroud-Dylan/Breezy-13/issues)

---

## 👥 Authors

**Developed by the CESI Team:**
- **Sacha Colbert-Lisbona** - [@Sunit34140](https://github.com/Sunit34140)
- **Trystan Julien** - [@Trystancesi](https://github.com/trystancesi)
- **Dylan Beroud** - [@Beroud-Dylan](https://github.com/Beroud-Dylan)
- **Loïc Serre** - [@LoicSERRE](https://github.com/LoicSERRE)

*A school project developed as part of the CESI curriculum.*

---

## 📜 License

This project is developed as part of CESI training and is intended for educational purposes.