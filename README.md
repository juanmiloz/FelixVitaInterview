﻿# FelixVitaInterview

### How to Run the Project Using Docker Compose

#### 1. Clone the Repository
Make sure to clone the project repository to your local machine:

```bash
git clone <repository-url>
cd <repository-name>
```

#### 2. Install Docker and Docker Compose
If you don’t have Docker and Docker Compose installed, follow the official installation guides:

- Docker: https://docs.docker.com/get-docker/
- Docker Compose: https://docs.docker.com/compose/install/

#### 3. Understanding the Docker Compose Setup
The provided `docker-compose.yml` file defines three services: PostgreSQL, an API (backend), and an Angular application (frontend). Here’s a breakdown:

- **PostgreSQL**: This service runs the PostgreSQL database, exposing port `5432`. It uses environment variables for the database name, user, and password.
- **API (Backend)**: This service is built from the `BackendFelixVita/` directory and runs on port `3000`. It depends on the PostgreSQL service, which must be healthy before it starts.
- **Frontend (Angular)**: This service is built from the `FrontFelixVita/` directory and runs on port `4200`. It depends on both the API and PostgreSQL services.

#### 4. Running Docker Compose
Navigate to the directory where the `docker-compose.yml` file is located and run the following command:

```bash
docker-compose up --build
```

This command will build and start the services defined in the `docker-compose.yml` file.

#### 5. Accessing the Application
Once the containers are running:

- **Frontend**: The Angular app will be accessible at `http://localhost:4200`.
- **API**: The backend API will be accessible at `http://localhost:3000`.
- **PostgreSQL**: The database will be running on port `5432`, with credentials defined in the `docker-compose.yml` file (user: `admin`, password: `password`).

#### 6. Stopping the Services
To stop the services, press `CTRL + C` in the terminal where `docker-compose` is running, or run:

```bash
docker-compose down
```

This will stop and remove the containers created by Docker Compose.

## Backend Overview

The backend of this project is built with Node.js and Express (or Python and Flask), and includes the following key features:

- **JWT Authentication**: The backend uses JSON Web Tokens (JWT) to manage user authentication. Once logged in, users receive a token that is required for accessing protected routes such as health metrics.
  
- **Rate Limiting**: To prevent abuse, the backend restricts users to a maximum of 5 requests per minute per IP for the `/metrics` endpoints.

- **Authentication for Metrics Endpoints**: Only authenticated users can access, submit, or view their health metrics. This is enforced using middleware that validates the JWT token.

- **User Registration and Login**: The backend provides two key routes for user management:
  - `POST /register`: Allows users to create an account by providing a username and password. Passwords are securely hashed before being stored in the PostgreSQL database.
  - `POST /login`: Authenticates users by validating their credentials and returns a JWT token upon successful login.

- **PostgreSQL Database Connection**: The backend connects to a PostgreSQL database to store user information (including hashed passwords) and health metrics (water intake, hours of sleep, and mood). The database stores each metric entry with a reference to the authenticated user who submitted it.

- **Password Encryption**: User passwords are securely hashed and encrypted before being stored in the database, ensuring security and privacy.

- **CORS Management**: Cross-Origin Resource Sharing (CORS) is properly configured to allow the Angular frontend to communicate with the backend, even when running on different ports or domains.

## Frontend Overview

The Angular frontend is responsible for interacting with the backend and includes the following key features:

- **Form Alerts**: The frontend provides alerts for form submission status, such as successful registration, login failures, or metric submission feedback (e.g., reminding users to sleep more if they input less than 6 hours).

- **Field Validation**: The frontend ensures that input fields (e.g., water intake, hours of sleep) are properly validated before submission. For instance, numbers are required for water intake and sleep hours, while mood is selected from a dropdown list.

- **Field Restriction Validation**: The frontend includes proper form restrictions to ensure valid input, such as enforcing minimum password lengths during registration and ensuring the required fields are filled before submission.
