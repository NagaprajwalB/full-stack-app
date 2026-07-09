# Fullstack Application: Next.js + Spring Boot + PostgreSQL

This repository contains a complete fullstack application with a Next.js frontend, Spring Boot backend, and PostgreSQL database.

## Architecture

```
Frontend (Next.js)       Backend (Spring Boot)       Database (PostgreSQL)
:3000                    :8080/api                   :5432
    |                         |                            |
    +-------->  HTTP  <-------+                            |
              (CORS enabled)                               |
                                |------> JDBC <----------+
```

## Project Structure

```
fullstack-app/
├── docker-compose.yml          # Multi-container orchestration
├── frontend/                   # Next.js React application
│   ├── Dockerfile              # Multi-stage build
│   ├── package.json
│   ├── next.config.js
│   ├── tailwind.config.ts
│   └── src/
│       └── app/                # Next.js 14 App Router
│           ├── layout.tsx
│           ├── page.tsx        # Main page with CRUD UI
│           └── globals.css
└── backend/                    # Spring Boot application
    ├── Dockerfile              # Multi-stage Maven build
    ├── pom.xml                 # Maven dependencies
    └── src/
        └── main/
            ├── java/com/fullstack/backend/
            │   ├── BackendApplication.java
            │   ├── controller/
            │   │   └── UserController.java
            │   ├── model/
            │   │   └── User.java
            │   └── repository/
            │       └── UserRepository.java
            └── resources/
                └── application.properties
```

## Quick Start

### Prerequisites
- Docker & Docker Compose installed

### Running the Application

1. **Build and start all services:**
   ```bash
   cd fullstack-app
   docker compose up --build
   ```

2. **Access the application:**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:8080/api/users
   - Database: localhost:5432 (postgres / postgres123)

### Stopping the Application

```bash
docker compose down
```

To also remove the database volume:
```bash
docker compose down -v
```

## API Endpoints

### Users API
- `GET /api/users` - List all users
- `GET /api/users/{id}` - Get user by ID
- `POST /api/users` - Create new user
- `PUT /api/users/{id}` - Update user
- `DELETE /api/users/{id}` - Delete user
- `GET /api/users/health` - Health check

### Request/Response Example

```bash
# Create user
curl -X POST http://localhost:8080/api/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "bio": "Software Developer"
  }'

# Response
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "bio": "Software Developer"
}
```

## Frontend Features

- Create users with name, email, and bio
- View all users in a responsive grid
- Delete users
- Error handling and loading states
- Tailwind CSS styling
- TypeScript support

## Backend Features

- Spring Boot 3.2 with Java 21
- JPA/Hibernate ORM with PostgreSQL
- RESTful CRUD API with error handling
- CORS support for frontend communication
- Automatic database schema generation
- Connection pooling and health checks

## Database

- PostgreSQL 16 Alpine
- Auto-created database: `fullstack_db`
- Default credentials: `postgres / postgres123`
- Persistent volume: `postgres_data`

## Environment Variables

### Backend (Docker Compose)
- `SPRING_DATASOURCE_URL`: PostgreSQL connection string
- `SPRING_DATASOURCE_USERNAME`: Database user
- `SPRING_DATASOURCE_PASSWORD`: Database password
- `SPRING_JPA_HIBERNATE_DDL_AUTO`: Schema generation mode

### Frontend (Docker Compose)
- `NEXT_PUBLIC_API_URL`: Backend API URL (http://localhost:8080/api)

## Development

### Local Frontend Development
```bash
cd frontend
npm install
npm run dev  # Runs on http://localhost:3000
```

### Local Backend Development
```bash
cd backend
mvn clean install
mvn spring-boot:run  # Runs on http://localhost:8080
```

Requires PostgreSQL running locally with matching credentials from `application.properties`.

## Production Deployment

For production, consider:
- Use Docker Swarm or Kubernetes for orchestration
- Configure `.env` files for secrets
- Set up a reverse proxy (nginx)
- Enable HTTPS
- Use managed database services (RDS, Azure Database, etc.)
- Implement API authentication/authorization
- Set up monitoring and logging

## Troubleshooting

### Backend fails to connect to PostgreSQL
```bash
docker compose logs backend
```
Ensure `depends_on` health check passes and wait for PostgreSQL startup.

### Frontend can't reach backend
```bash
# Verify CORS and API URL
docker compose logs frontend
# Check backend is running
docker compose logs backend
```

### Database issues
```bash
# Connect to PostgreSQL
docker compose exec postgres psql -U postgres -d fullstack_db
# List tables
\dt
```

---

Built with ❤️ using Docker
