# Simple Routing App

For the needs of the project, a simple routing application was developed.
The user can load available vehicles, select a starting location, and choose the nodes they want to visit.

The application:

* fetches data from the **OpenMap API**,
* calculates the optimal path using a **greedy algorithm**,
* and displays the route using the **Leaflet API**.

**Tech stack:** JAX-RS, Angular, MySQL 8
**Deployment:** Docker

---

## Features

* **Register / Login** with JWT Authentication
* Create multiple maps, trucks, and location nodes
* Currently, trucks do not affect the model (only distances between nodes are considered)
* Fetching from the OpenMap API needs improvement; therefore, it is recommended not to use more than **4–5 nodes**, since computation time grows (\~O(n²))
* Services are covered with corresponding tests
* **AI usage breakdown:**

    * Backend: 10%
    * Frontend: 50%
    * Docker: 90%

---

## Installation & Deployment

### 1. Configure JWT Token

In `JwtService`, paste the jwt provided.

In koukouDB, koukouUser, koukouPassword paste the credentials provided

### 2. Configure Database Properties

In `persistence.xml`, paste the following:

```xml
<property name="jakarta.persistence.jdbc.url" value="jdbc:mysql://routingDB:3306/koukou"/>
<property name="jakarta.persistence.jdbc.user" value="koukouUser"/>
<property name="jakarta.persistence.jdbc.password" value="koukouPassword"/>
```

### 3. Build Backend

In the `routing-soft-core` folder, run:

```bash
mvn clean package
```

### 4. Create Docker Network

```bash
docker network create routing-net
```

### 5. Start MySQL 8 Container

```bash
docker run -d \
  --name routingDB \
  --network routing-net \
  -e MYSQL_ROOT_PASSWORD=rootPassword123 \
  -e MYSQL_DATABASE=koukouDb \
  -e MYSQL_USER=koukouUser \
  -e MYSQL_PASSWORD=koukouPassword \
  -p 3306:3306 \
  mysql:8
```

### 6. Build & Run Backend

```bash
docker build -t routing-backend .
docker run -d \
  --name routing-backend \
  --network routing-net \
  -p 8080:8080 \
  routing-backend
```

### 7. Build & Run Frontend

In the `routing-soft-ui` folder:

```bash
docker build -t routing-frontend .
docker run -d \
  --name routing-frontend \
  --network routing-net \
  -p 3000:80 \
  routing-frontend
```

---

## Access

The frontend is available at:
👉 [http://localhost:3000/](http://localhost:3000/)

Steps:

1. Register (wait a little for DB initialization)
2. Login
3. If plans fail to load, restart the backend container 1–2 times (Tomcat sometimes delays dependency injection).

---
