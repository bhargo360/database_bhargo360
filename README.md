# PostgreSQL + pgAdmin Docker Setup

This project sets up a PostgreSQL database and a pgAdmin 4 web interface using Docker Compose.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Setup & Deployment

1.  **Configure Environment Variables**

    Create a `.env` file in the root directory (if not already present) with the following variables. Replace the values with your secure credentials.

    ```bash
    POSTGRES_USER=admin
    POSTGRES_PASSWORD=secure_password_change_me
    POSTGRES_DB=mydb
    PGADMIN_DEFAULT_EMAIL=admin@bhargo360.com
    PGADMIN_DEFAULT_PASSWORD=secure_pgadmin_password_change_me
    ```

2.  **Start the Services**

    Run the following command to start the containers in detached mode:

    ```bash
    docker-compose up -d
    ```

3.  **Verify Running Containers**

    Check if the containers are running:

    ```bash
    docker-compose ps
    ```

## Accessing pgAdmin

-   **URL**: [http://database.bhargo360.com:5111](http://database.bhargo360.com:5111) (or `http://localhost:5111` locally)
-   **Login Credentials**: Use the `PGADMIN_DEFAULT_EMAIL` and `PGADMIN_DEFAULT_PASSWORD` defined in your `.env` file.

## Connecting pgAdmin to the Database

Once logged into pgAdmin, follow these steps to connect to the PostgreSQL container:

1.  Click on **"Add New Server"**.
2.  **General Tab**:
    -   **Name**: Give your server a name (e.g., "Production DB").
3.  **Connection Tab**:
    -   **Host name/address**: `db` (This is the internal service name defined in `docker-compose.yml`).
    -   **Port**: `5432`
    -   **Maintenance database**: `mydb` (or the value of `POSTGRES_DB` in your `.env`).
    -   **Username**: `admin` (or the value of `POSTGRES_USER` in your `.env`).
    -   **Password**: Enter the password defined in `POSTGRES_PASSWORD`.
4.  Click **Save**.

## Data Persistence

-   Database data is persisted in a Docker volume named `pgdata`.
-   This ensures that your data remains safe even if the containers are restarted or removed.

## Connecting External Projects

To connect an external application (e.g., a Node.js API, Python script, or GUI tool like DBeaver) to this database, use the following details:

### Connection Parameters

-   **Host**: `database.bhargo360.com` (or your VPS IP address / `localhost` if running locally)
-   **Port**: `5432`
-   **Database**: `mydb` (value of `POSTGRES_DB`)
-   **Username**: `admin` (value of `POSTGRES_USER`)
-   **Password**: (value of `POSTGRES_PASSWORD`)

### Connection String Example

Most applications accept a connection string in this format:

```
postgresql://admin:secure_password_change_me@database.bhargo360.com:5432/mydb
```

*(Replace the credentials with your actual values from `.env`)*

### Connecting from Another Docker Container

If you have another application running in Docker on the same machine, you can connect them by adding them to the same network or using the host's IP.

**Option A: Same Network (Recommended)**
1.  Identify the network name created by this project (usually `database_bhargo360_db-network`).
2.  Add that external network to your application's `docker-compose.yml`.
3.  Connect using the host `db` or `postgres_db`.

**Option B: Host Networking**
1.  Connect using `host.docker.internal` (Mac/Windows) or the host's local IP address (Linux).
2.  Port: `5432`

