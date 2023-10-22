This example provides a base setup for using PostgreSQL and pgAdmin. More details on how to customize the installation and the compose file can be found here (PostgreSQL) and here (pgAdmin).

Project structure:
```
.
├── .env
├── compose.yaml
└── README.md
```
compose.yaml
```
services:
  postgres:
    image: postgres:latest
    ...
  pgadmin:
    image: dpage/pgadmin4:latest
```
Configuration
```
.env
```
Before deploying this setup, you need to configure the following values in the .env file.
```
    POSTGRES_USER
    POSTGRES_PW
    POSTGRES_DB (can be default value)
    PGADMIN_MAIL
    PGADMIN_PW
```
Deploy with docker compose

When deploying this setup, the pgAdmin web interface will be available at port 5050 (e.g. http://localhost:5050).
```
$ docker compose up
Starting postgres ... done
Starting pgadmin ... done
```
Add postgres database to pgAdmin

After logging in with your credentials of the .env file, you can add your database to pgAdmin.

    Right-click "Servers" in the top-left corner and select "Create" -> "Server..."
    Name your connection
    Change to the "Connection" tab and add the connection details:

    Hostname: "postgres" (this would normally be your IP address of the postgres database - however, docker can resolve this container ip by its name)
    Port: "5432"
    Maintenance Database: $POSTGRES_DB (see .env)
    Username: $POSTGRES_USER (see .env)
    Password: $POSTGRES_PW (see .env)

Expected result

Check containers are running:
```
$ docker ps
CONTAINER ID   IMAGE                           COMMAND                  CREATED             STATUS                 PORTS                                                                                  NAMES
849c5f48f784   postgres:latest                 "docker-entrypoint.s…"   9 minutes ago       Up 9 minutes           0.0.0.0:5432->5432/tcp, :::5432->5432/tcp                                              postgres
d3cde3b455ee   dpage/pgadmin4:latest           "/entrypoint.sh"         9 minutes ago       Up 9 minutes           443/tcp, 0.0.0.0:5050->80/tcp, :::5050->80/tcp                                         pgadmin
```
... or via psql
```
docker run -it --rm --network postgres-docker_default postgres psql -h postgres -U puser -d postgres
psql (14.3)
Type "help" for help.

postgres=# SELECT 1;
 ?column? 
----------
        1
(1 row)
```
Stop the containers with
```
$ docker compose down
# To delete all data run:
$ docker compose down -v
```
