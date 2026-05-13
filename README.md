# ChatSync - Backend

A scalable microservice-based backend for ChatSync, a real-time chat application. Built with Node.js, TypeScript, and Express, featuring microservice architecture with Docker containerization, MongoDB for data persistence, Redis for caching, and RabbitMQ for inter-service communication.

## 🔗 Related Repositories

[![GitHub](https://img.shields.io/badge/Frontend-ChatSync%20Frontend-blue)](https://github.com/NavjotSuman/chatApp-microservice-Frontend)
[![GitHub](https://img.shields.io/badge/Backend-ChatSync%20Backend-green)](https://github.com/NavjotSuman/chatApp-microservice-Backend)

## 🚀 Features

- **Microservice Architecture**: Modular, scalable services for user management, chat, and email notifications
- **Real-time Communication**: WebSocket-based chat using Socket.io
- **Authentication & Authorization**: JWT-based secure authentication system
- **Email Integration**: Automated email verification and notifications
- **Containerization**: Docker-based deployment with docker-compose
- **Database**: MongoDB for persistent data storage
- **Caching**: Redis for session management and caching
- **Message Queue**: RabbitMQ for asynchronous communication between services

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   User Service  │    │   Chat Service  │    │   Mail Service  │
│     (Port 5001) │    │     (Port 5002) │    │     (Port 5003) │
│                 │    │                 │    │                 │
│ - Auth & Users  │◄──►│ - Real-time Chat│◄──►│ - Email Service │
│ - JWT Tokens    │    │ - WebSocket     │    │ - Notifications │
│ - User Profiles │    │ - Message Store │    │ - Verification  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────────┐
                    │  Shared Resources   │
                    │                     │
                    │ - MongoDB (Data)    │
                    │ - Redis (Cache)     │
                    │ - RabbitMQ (Queue)  │
                    └─────────────────────┘
```

## 🛠️ Tech Stack

### Core Technologies
- **Runtime**: Node.js
- **Language**: TypeScript
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose ODM
- **Cache**: Redis
- **Message Queue**: RabbitMQ (AMQP)
- **Containerization**: Docker & Docker Compose

### Services
- **User Service**: Authentication, user management, profiles
- **Chat Service**: Real-time messaging, WebSocket handling
- **Mail Service**: Email notifications, verification emails

## 📋 Prerequisites

- Docker & Docker Compose
- Node.js (v18 or higher) - for local development
- MongoDB (if running services locally)
- Redis (if running services locally)
- RabbitMQ (if running services locally)

## 🚀 Quick Start with Docker

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd backend
   ```

2. **Environment Configuration**
   Create `.env` files for each service or use the provided templates:
   ```bash
   cp .env.example .env
   cp user/.env.example user/.env
   cp chat/.env.example chat/.env
   cp mail/.env.example mail/.env
   ```

3. **Build and Run Services**
   ```bash
   # Create Docker network
   docker network create app-network

   # Build service images
   docker build -t user-service ./user
   docker build -t chat-service ./chat
   docker build -t mail-service ./mail

   # Run containers
   docker run -d --name user --env-file ./user/.env --network app-network -p 5001:5001 user-service
   docker run -d --name chat --env-file ./chat/.env --network app-network -p 5002:5002 chat-service
   docker run -d --name mail --env-file ./mail/.env --network app-network -p 5003:5003 mail-service
   ```

4. **Or use Docker Compose**
   ```bash
   docker-compose up --build
   ```

## 🛠️ Local Development Setup

### Prerequisites
- Node.js v18+
- MongoDB running locally or connection string
- Redis server running
- RabbitMQ server running

### Setup Each Service

1. **Install Dependencies**
   ```bash
   # For each service
   cd user && npm install
   cd ../chat && npm install
   cd ../mail && npm install
   ```

2. **Environment Variables**
   Each service requires its own `.env` file with appropriate configurations.

3. **Run Services**
   ```bash
   # User Service
   cd user && npm run build && npm start

   # Chat Service
   cd chat && npm run build && npm start

   # Mail Service
   cd mail && npm run build && npm start
   ```

## 📁 Project Structure

```
backend/
├── user/                    # User Management Service
│   ├── src/
│   │   ├── config/         # Database & service configuration
│   │   ├── controllers/    # Route handlers
│   │   ├── middleware/     # Authentication & validation
│   │   ├── models/         # Mongoose schemas
│   │   ├── routes/         # API routes
│   │   └── index.ts        # Service entry point
│   ├── Dockerfile          # Container configuration
│   ├── package.json        # Dependencies
│   └── tsconfig.json       # TypeScript config
├── chat/                    # Chat Service
│   ├── src/                # Similar structure to user service
│   └── ...
├── mail/                    # Mail Service
│   ├── src/                # Similar structure to user service
│   └── ...
├── docker-compose.yml       # Multi-service orchestration
├── .env                     # Shared environment variables
└── README.md               # This file
```

## 🔧 Configuration

### Environment Variables

#### Shared Variables (`.env`)
```
MONGODB_URI=mongodb://localhost:27017/chatsync
REDIS_URL=redis://localhost:6379
RABBITMQ_URL=amqp://localhost:5672
JWT_SECRET=your-super-secret-jwt-key
```

#### User Service (`user/.env`)
```
PORT=5001
MONGODB_URI=mongodb://localhost:27017/chatsync
REDIS_URL=redis://localhost:6379
RABBITMQ_URL=amqp://localhost:5672
JWT_SECRET=your-super-secret-jwt-key
MAIL_QUEUE=mail_queue
```

#### Chat Service (`chat/.env`)
```
PORT=5002
MONGODB_URI=mongodb://localhost:27017/chatsync
REDIS_URL=redis://localhost:6379
RABBITMQ_URL=amqp://localhost:5672
JWT_SECRET=your-super-secret-jwt-key
```

#### Mail Service (`mail/.env`)
```
PORT=5003
RABBITMQ_URL=amqp://localhost:5672
MAIL_QUEUE=mail_queue
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-app-password
```

## 📡 API Documentation

### User Service (Port 5001)

#### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/auth/verify` - Email verification
- `POST /api/auth/logout` - User logout

#### User Management
- `GET /api/users/profile` - Get user profile
- `PUT /api/users/profile` - Update user profile

### Chat Service (Port 5002)

#### WebSocket Events
- `connection` - Client connects to chat
- `join_room` - Join a chat room
- `send_message` - Send a message
- `receive_message` - Receive messages
- `disconnect` - Client disconnects

#### REST Endpoints
- `GET /api/chat/rooms` - Get user's chat rooms
- `GET /api/chat/messages/:roomId` - Get messages for a room
- `POST /api/chat/rooms` - Create a new chat room

### Mail Service (Port 5003)

#### Email Operations
- `POST /api/mail/send-verification` - Send verification email
- `POST /api/mail/send-notification` - Send notification email

## 🧪 Testing

```bash
# Run tests for each service
cd user && npm test
cd chat && npm test
cd mail && npm test
```

## 🔍 Monitoring & Logging

- Each service logs to console and can be configured for external logging services
- Health check endpoints available at `/health` for each service
- Docker logs can be monitored using `docker logs <container-name>`

## 👨‍💻 Author

**Navjot Suman**

## 🙏 Acknowledgments

- [Express.js](https://expressjs.com/) - Web framework
- [MongoDB](https://www.mongodb.com/) - NoSQL database
- [Redis](https://redis.io/) - In-memory data structure store
- [RabbitMQ](https://www.rabbitmq.com/) - Message broker
- [Socket.io](https://socket.io/) - Real-time communication
- [Docker](https://www.docker.com/) - Containerization platform</content>
<parameter name="filePath">backend/README.md