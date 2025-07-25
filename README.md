# TaskSync Backend

TaskSync-backend is the backend for a real-time collaborative task management system, designed to demonstrate senior-level software engineering skills (SWE2). Built with Spring Boot, it features a microservices architecture with services for authentication, task management, and real-time notifications via WebSocket. The project emphasizes modularity, scalability, and enterprise-grade practices, serving as a portfolio piece for REST API design, secure authentication, and real-time systems. Future plans include integration with a mobile app (React Native) and AI-driven features (Python microservice).

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Microservices Architecture](#microservices-architecture)
- [Setup Instructions](#setup-instructions)
- [API Endpoints](#api-endpoints)
- [WebSocket Events](#websocket-events)
- [Deployment](#deployment)
- [Testing](#testing)
- [Future Enhancements](#future-enhancements)
- [Contributing](#contributing)
- [License](#license)

## Project Overview
TaskSync is a web-based task management tool inspired by Trello and Asana, enabling users to create, assign, and manage tasks collaboratively in real time. The backend, housed in this repository, is built as a monorepo containing three Spring Boot microservices: authentication, task management, and WebSocket-based notifications. The project showcases SWE2-level skills, including database management, secure authentication, API integration, and modular code structure, with a focus on preparing for future mobile and AI extensions.

## Features
- **User Authentication**: Register/login with email/password or Google OAuth, with role-based access (admin vs. user).
- **Task Management**: Create, edit, delete, and assign tasks with statuses (To-Do, In Progress, Done).
- **Real-Time Collaboration**: Notify users of task updates instantly via WebSocket.
- **Role-Based Access**: Admins manage all tasks/users; users manage only their tasks.
- **Scalable Design**: Microservices architecture for modularity and future extensions.

## Tech Stack
- **Backend**: Spring Boot (Java), Spring Data JPA (PostgreSQL), Spring Security (JWT, Google OAuth), Spring WebSocket (STOMP).
- **Database**: PostgreSQL.
- **Tools**: Maven (dependency management), Docker (containerization), GitHub (version control), Postman (API testing).
- **Deployment**: AWS Elastic Beanstalk or Heroku (backend), Railway or AWS RDS (PostgreSQL).

## Microservices Architecture
The backend is organized as a monorepo with three microservices:
- **Auth Service**: Manages user registration, login, Google OAuth, and JWT issuance.
- **Task Service**: Handles task CRUD operations with role-based access.
- **WebSocket Service**: Manages real-time task notifications using STOMP over WebSocket.

**Monorepo Structure**:
```
/task-sync-backend
├── /auth-service        # Authentication microservice
├── /task-service        # Task management microservice
├── /websocket-service   # Real-time notifications microservice
├── /shared              # Shared models and utilities
├── /docs                # API documentation
├── /deploy              # Deployment scripts and Dockerfiles
├── README.md
```

## Setup Instructions
### Prerequisites
- Java 17
- Maven 3.8+
- PostgreSQL 15
- Docker (optional for containerization)
- Node.js (for frontend integration, if applicable)
- Google OAuth credentials (for SSO)

### Steps
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/task-sync-backend.git
   cd task-sync-backend
   ```

2. **Set Up PostgreSQL**:
   - Install PostgreSQL locally or use a hosted service (e.g., Railway).
   - Create a database named `task_manager`.
   - Run the following SQL to create tables:
     - `users`: id, email, password (hashed), role, created_at
     - `tasks`: id, title, description, status, assignee_id, created_by_id, created_at

3. **Configure Environment Variables**:
   - Create `.env` files in each microservice directory (`auth-service`, `task-service`, `websocket-service`).
   - Add:
     ```
     SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/task_manager
     SPRING_DATASOURCE_USERNAME=your_username
     SPRING_DATASOURCE_PASSWORD=your_password
     JWT_SECRET=your_jwt_secret
     GOOGLE_CLIENT_ID=your_google_client_id
     GOOGLE_CLIENT_SECRET=your_google_client_secret
     ```

4. **Build and Run Microservices**:
   - Navigate to each microservice directory and run:
     ```bash
     mvn clean install
     mvn spring-boot:run
     ```
   - Alternatively, use Docker:
     ```bash
     docker-compose up
     ```

5. **Verify Setup**:
   - Test auth service endpoints (e.g., `/api/auth/register`) using Postman.
   - Ensure WebSocket service is accessible at `/ws`.

## API Endpoints
- **Auth Service**:
  - `POST /api/auth/register`: Register a new user.
  - `POST /api/auth/login`: Login and receive JWT.
  - `GET /api/auth/google`: Initiate Google OAuth flow.
- **Task Service**:
  - `POST /api/tasks`: Create a task.
  - `GET /api/tasks`: List tasks (filtered by user/status).
  - `PUT /api/tasks/{id}`: Update a task.
  - `DELETE /api/tasks/{id}`: Delete a task.

## WebSocket Events
- **task.created**: Broadcasts new task creation.
- **task.updated**: Notifies of task updates.
- **task.deleted**: Notifies of task deletion.
- Endpoint: `/ws` (STOMP over WebSocket).

## Deployment
1. **Backend**:
   - Deploy each microservice to AWS Elastic Beanstalk or Heroku.
   - Configure environment variables for database and OAuth.
2. **Database**:
   - Host PostgreSQL on Railway or AWS RDS.
3. **Docker** (Optional):
   - Use Dockerfiles in `/deploy` to containerize microservices.
   - Deploy with `docker-compose` or a cloud orchestrator.

## Testing
- Unit tests for auth service (JWT, OAuth) using JUnit and Mockito.
- Unit tests for task service APIs (CRUD, role-based access).
- Integration tests for WebSocket events.
- End-to-end tests for user login, task creation, and real-time updates.

## Future Enhancements
- **Mobile App**: Develop a React Native app to reuse APIs and WebSocket for mobile access, with push notifications.
- **AI Integration**: Add a Python FastAPI microservice (`/backend/ai-service`) for task prioritization and NLP-based categorization, integrated via REST or gRPC.
- Explore xAI’s API (https://x.ai/api) for advanced AI features.

## Contributing
Contributions are welcome! Please:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/your-feature`).
3. Commit changes (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.

## License
MIT License. See [LICENSE](LICENSE) for details.
