# Task Management API

A Task Management system built using [NestJS](https://nestjs.com/), designed to handle RESTful API requests. It supports user authentication and authorization, task creation and management with ownership restrictions, validation, error handling, data persistence using PostgreSQL with [TypeORM](https://typeorm.io/), and logging.

## Features

- **REST API**: Exposes a RESTful interface for managing tasks and users.
- **Validation & Error Handling**: Validates incoming requests and provides structured error responses.
- **Data Persistence**: Uses PostgreSQL with TypeORM for storing user and task data.
- **Authentication & Authorization**: Secured API endpoints using JWT-based authentication and role-based access controls (RBAC).
- **Task Ownership**: Restricts task operations (create, update, delete) to the task owner.
- **Logging**: Provides application-level logging to capture important events and errors.

## Technologies

- **NestJS**: A framework for building efficient and scalable Node.js server-side applications.
- **PostgreSQL**: A powerful, open-source relational database for data persistence.
- **TypeORM**: An ORM for TypeScript and JavaScript that works with PostgreSQL to map objects to database records.
- **JWT (JSON Web Tokens)**: For handling authentication and authorization.
- **Class-validator & Class-transformer**: Used for request data validation.

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/task-management-api.git
   cd task-management-api
   ```

2. Install the dependencies:

   ```bash
   npm install
   ```

3. Set up PostgreSQL:

   - Create a new PostgreSQL database.
   - Copy the `.env.example` to `.env` and update the environment variables, especially the database configuration:

     ```bash
     cp .env.example .env
     ```

     Example `.env` content:

     ```
     DATABASE_HOST=localhost
     DATABASE_PORT=5432
     DATABASE_USERNAME=your_pg_username
     DATABASE_PASSWORD=your_pg_password
     DATABASE_NAME=task_management
     JWT_SECRET=your_secret_key
     ```

4. Run database migrations to set up tables:

   ```bash
   npm run typeorm migration:run
   ```

5. Start the server:

   ```bash
   npm run start:dev
   ```

   The API should now be running at `http://localhost:3000`.

## Usage

### REST API

#### Authentication

- **POST** `/auth/signup`: Register a new user.
- **POST** `/auth/signin`: Authenticate with email and password to get a JWT token.

#### Task Management

- **GET** `/tasks`: Get all tasks for the logged-in user.
- **GET** `/tasks/:id?search=":value"&status=":value"`: Get details of a specific task.
- **POST** `/tasks`: Create a new task.
- **PATCH** `/tasks/:id/status`: Update an existing task.
- **DELETE** `/tasks/:id`: Delete a task (only if the user is the owner).

#### Example request headers for authenticated users:

```json
{
  "Authorization": "Bearer your_jwt_token"
}
```

### Validation & Error Handling

Validation of incoming requests is handled using `class-validator`. If a request fails validation, a 400 Bad Request error will be returned with a detailed error message.

Example validation error response:

```json
{
  "statusCode": 400,
  "message": ["title must be a string", "description must be a string"],
  "error": "Bad Request"
}
```

### Authentication & Authorization

- JWT-based authentication is used for securing the API endpoints.
- Users can only perform actions on their own tasks (task ownership restriction).
- Authorization guards are implemented using `@nestjs/passport` and JWT strategy.

### Logging

Logs are generated to capture important events and errors. By default, logs are output to the console, but this can be configured to use external logging services like Winston or Sentry.
