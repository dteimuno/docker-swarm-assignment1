# Assignment: Create A Multi-Service Multi-Node Web App

This project involves creating a multi-service, multi-node web application called "Cats vs. Dogs Voting App" using Docker Compose. The app is designed to demonstrate the deployment of a distributed system with interconnected services, networks, and persistent storage.

---

## Project Goal

The objective is to set up a web-based voting application where users can vote for either cats or dogs. The system consists of multiple services working together, each with specific roles, connected via Docker networks, and using persistent storage for the database.

---

## Application Components

The app includes the following services:

### 1. **Vote Service**
- **Purpose**: A web application that allows users to cast votes for cats or dogs.
- **Image**: `bretfisher/examplevotingapp_vote`
- **Network**: `frontend`
- **Ports**: Exposed on port `80`
- **Replicas**: 2 for high availability.

### 2. **Redis Service**
- **Purpose**: Acts as a message queue for storing votes temporarily.
- **Image**: `redis:3.2`
- **Network**: `frontend`
- **Dependencies**: Depends on the `vote` service.
- **Replicas**: 1.

### 3. **Worker Service**
- **Purpose**: Processes votes from Redis and updates the database.
- **Image**: `bretfisher/examplevotingapp_worker`
- **Networks**: `frontend` and `backend`
- **Dependencies**: Depends on the `redis` service.
- **Replicas**: 1.

### 4. **Database Service**
- **Purpose**: Stores votes persistently in a PostgreSQL database.
- **Image**: `postgres:9.4`
- **Volume**: `db-data` for persistent storage.
- **Network**: `backend`
- **Dependencies**: Depends on the `worker` service.
- **Replicas**: 1.
- **Environment Variables**: 
  - `POSTGRES_HOST_AUTH_METHOD=trust` (For simplicity during development).

### 5. **Result Service**
- **Purpose**: A web application that displays the voting results in real-time.
- **Image**: `bretfisher/examplevotingapp_result`
- **Network**: `backend`
- **Ports**: Exposed on port `5001`
- **Dependencies**: Depends on the `db` service.
- **Replicas**: 1.

---

## Application Architecture

The app is structured with two networks and a volume:

### Networks
1. **Frontend**: Connects the `vote` and `redis` services.
2. **Backend**: Connects the `worker`, `db`, and `result` services.

### Volumes
- **db-data**: A named volume used for persisting the PostgreSQL database data.

---

## Deployment Instructions

### Prerequisites
Ensure the following tools are installed on your system:
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

### Steps to Deploy

1. **Clone the Repository**
   Clone the project to your local machine:
   ```bash
   git clone <repository-url>
   cd <repository-directory>
2. **Start the Application Use the following command to build and start the services:**
   ```bash
   docker-compose up -d
   ```
This will:

- Create the necessary Docker networks.
- Initialize the db-data volume.
- Start the services in detached mode.

3. **Access the Application**
- Vote App: Visit http://localhost:80 to cast votes.
- Result App: Visit http://localhost:5001 to view the results.

4. **Monitor Services Use the following command to view the logs of running services:**
   ```bash
   docker-compose logs -f
   ```
5. **Scale Services To scale a service (e.g., vote) to additional replicas:**
   ```bash
   docker-compose up -d --scale vote=3
   ```
6. **Shut Down the Application To stop and remove all services, networks, and volumes:**
   ```bash
   docker-compose down
   ```

**How Services Work Together**
- Vote Service: Accepts user votes and sends them to the Redis queue.
- Redis Service: Temporarily stores the votes as a message queue.
- Worker Service: Processes the votes from Redis and updates the database.
- Database Service: Stores the processed votes persistently in PostgreSQL.
- Result Service: Retrieves data from the database and displays the vote results to users.

**Environment Variables**
- POSTGRES_HOST_AUTH_METHOD=trust: Enables password-less authentication for PostgreSQL during development. In production, replace this with secure authentication methods.

**Debugging and Troubleshooting**
- Use docker ps to view running containers.
- Use docker-compose logs to view detailed logs for each service.
- Ensure that no other applications are using ports 80 or 5001.


**Additional Resources**
- Docker Documentation
- Example Voting App by Bret Fisher

**Author**
This project is inspired by the "Example Voting App" by Bret Fisher and aims to demonstrate the deployment of multi-service applications with Docker Compose.
```css

This file provides a complete explanation of all components, dependencies, and steps for deploying and managing the application. Let me know if you'd like to add more details!
```


