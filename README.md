# Leitura do Docker Compose do Airflow

[← Voltar a Docker Compose](https://github.com/joycequoos/Docker_Docker_Compose/blob/main/README.md)

Análise passo a passo de um `docker-compose.yml` que define um conjunto de serviços para executar o Apache Airflow com executor Celery, usando PostgreSQL como banco de dados e Redis como backend de mensagens.

## Índice

- [Estrutura comum](#estrutura-comum)
- [Configurações comuns do Airflow](#configurações-comuns-do-airflow)
- [Serviços definidos](#serviços-definidos)
- [Volumes](#volumes)
- [Próximos passos](#próximos-passos)

---

## Estrutura comum

### Versão

Define a versão do arquivo de configuração do Docker Compose.

[![Versão](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/01_Versao.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/01_Versao.png)

### x-airflow-common

Define uma âncora chamada `airflow-common`, usada para compartilhar configurações comuns entre os vários serviços do Airflow.

[![x-airflow-common](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/02_X_Common.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/02_X_Common.png)

## Configurações comuns do Airflow

### Imagem e ambiente

Define a imagem Docker do Airflow e as variáveis de ambiente necessárias para sua configuração — incluindo conexão com o banco de dados, backend Celery, configurações de servidor web, e-mail, etc.

[![Imagem e ambiente](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/03_Imagens_Ambiente.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/03_Imagens_Ambiente.png)

### Volumes

Mapeia diretórios locais para os diretórios correspondentes dentro dos contêineres, garantindo que DAGs, logs, plugins e dados persistam entre reinicializações.

[![Volumes](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/04_Volumes.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/04_Volumes.png)

### Usuário e dependências

Define o usuário que executará os contêineres e as dependências de serviços — Redis e PostgreSQL precisam estar saudáveis antes de iniciar os serviços do Airflow.

[![Usuário e dependências](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/05_Usuarios_Dependencias.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/05_Usuarios_Dependencias.png)

## Serviços definidos

| # | Serviço | Descrição |
| --- | --- | --- |
| 1 | **PostgreSQL** | Serviço de banco de dados, com as credenciais necessárias e um volume para persistir os dados. Inclui um healthcheck para verificar se o banco está pronto |
| 2 | **Redis** | Expõe a porta 6379 e inclui um healthcheck para garantir que o serviço está funcionando corretamente |
| 3 | **Airflow Webserver** | Servidor web do Airflow, usando a configuração comum (`airflow-common`), expondo a porta 8080 e com verificação de saúde |
| 4 | **Airflow Scheduler** | Agendador responsável por orquestrar a execução dos DAGs |
| 5 | **Airflow Worker** | Trabalhadores Celery responsáveis por executar as tarefas agendadas |
| 6 | **Airflow Triggerer** | Responsável por disparar tarefas baseadas em eventos |
| 7 | **Airflow Init** | Responsável por inicializar o banco de dados do Airflow e criar o usuário web inicial |
| 8 | **Airflow CLI** | Serviço para executar comandos da linha de comando do Airflow |
| 9 | **Flower** | Serviço de monitoramento do Celery |

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

Define um volume Docker para persistir os dados do PostgreSQL.

[![Volumes do PostgreSQL](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/raw/main/img/15_Volumes.png)](https://github.com/joycequoos/Leitura_Docker_Compose_Airflow/blob/main/img/15_Volumes.png)

---

Esse `docker-compose.yml` configura uma arquitetura completa para executar o Apache Airflow em um ambiente distribuído, com PostgreSQL, Redis e os vários serviços do Airflow trabalhando em conjunto.

## Próximos passos

- Testar a arquitetura localmente com `docker-compose up -d` e acompanhar os logs de cada serviço.
- Ajustar variáveis de ambiente sensíveis (senhas, chaves) usando um arquivo `.env` em vez de deixá-las hardcoded no compose file.
- Explorar o Flower (`localhost:5555`) para monitorar as filas de tarefas do Celery em tempo real.
- Documentar como adicionar novos DAGs ao volume mapeado sem precisar reiniciar os contêineres.
