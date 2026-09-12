# Reading the Airflow Docker Compose File

[← Back to Docker Compose](https://github.com/joycequoos/Docker_Docker_Compose/blob/main/README.md)

A step-by-step analysis of a `docker-compose.yml` that defines a set of services to run Apache Airflow with the Celery executor, using PostgreSQL as the database and Redis as the message backend.

## Table of Contents

- [Common Structure](#common-structure)
- [Common Airflow Settings](#common-airflow-settings)
- [Defined Services](#defined-services)
- [Volumes](#volumes)
- [Next Steps](#next-steps)

---

## Common Structure

### Version

Defines the version of the Docker Compose configuration file.

[![Version](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/01_Versao.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/01_Versao.png)

### x-airflow-common

Defines an anchor called `airflow-common`, used to share common configuration across the various Airflow services.

[![x-airflow-common](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/02_X_Common.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/02_X_Common.png)

## Common Airflow Settings

### Image and Environment

Defines the Airflow Docker image and the environment variables needed for its configuration — including the database connection, Celery backend, web server settings, email, and more.

[![Image and environment](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/03_Imagens_Ambiente.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/03_Imagens_Ambiente.png)

### Volumes

Maps local directories to their corresponding directories inside the containers, ensuring that DAGs, logs, plugins, and data persist between restarts.

[![Volumes](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/04_Volumes.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/04_Volumes.png)

### User and Dependencies

Defines the user that will run the containers and the service dependencies — Redis and PostgreSQL need to be healthy before the Airflow services start.

[![User and dependencies](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/05_Usuarios_Dependencias.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/05_Usuarios_Dependencias.png)

## Defined Services

| # | Service | Description |
| --- | --- | --- |
| 1 | **PostgreSQL** | Database service, with the necessary credentials and a volume to persist the data. Includes a healthcheck to verify the database is ready |
| 2 | **Redis** | Exposes port 6379 and includes a healthcheck to ensure the service is working correctly |
| 3 | **Airflow Webserver** | Airflow's web server, using the common configuration (`airflow-common`), exposing port 8080 and with a health check |
| 4 | **Airflow Scheduler** | Scheduler responsible for orchestrating DAG execution |
| 5 | **Airflow Worker** | Celery workers responsible for running the scheduled tasks |
| 6 | **Airflow Triggerer** | Responsible for triggering event-based tasks |
| 7 | **Airflow Init** | Responsible for initializing the Airflow database and creating the initial web user |
| 8 | **Airflow CLI** | Service for running Airflow command-line commands |
| 9 | **Flower** | Celery monitoring service |

[![PostgreSQL](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/06_Postgres_SQL.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/06_Postgres_SQL.png)
[![Redis](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/07_Redis.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/07_Redis.png)
[![Airflow Webserver](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/08_Airflow_Web.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/08_Airflow_Web.png)
[![Airflow Scheduler](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/09_Airflow_Scheduler.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/09_Airflow_Scheduler.png)
[![Airflow Worker](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/10_Airflow_Worker.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/10_Airflow_Worker.png)
[![Airflow Triggerer](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/11_Airflow_Triggerer.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/11_Airflow_Triggerer.png)
[![Airflow Init](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/12_Airflow_Init.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/12_Airflow_Init.png)
[![Airflow CLI](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/13_Airflow_CLI.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/13_Airflow_CLI.png)
[![Flower](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/14_Flower.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/14_Flower.png)

## Volumes

Defines a Docker volume to persist PostgreSQL's data.

[![PostgreSQL volumes](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/15_Volumes.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/15_Volumes.png)

---

This `docker-compose.yml` configures a complete architecture for running Apache Airflow in a distributed environment, with PostgreSQL, Redis, and the various Airflow services working together.

## Next Steps

- Test the architecture locally with `docker-compose up -d` and monitor each service's logs.
- Adjust sensitive environment variables (passwords, keys) using a `.env` file instead of leaving them hardcoded in the compose file.
- Explore Flower (`localhost:5555`) to monitor Celery's task queues in real time.
- Document how to add new DAGs to the mapped volume without needing to restart the containers.
