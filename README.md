## Bem-vindo

Este repositório contem docker-compose com o Postgres

Este é um repositório para iniciar um Postres Padrão


## **Postgres para Apache Superset**

```yml
version: '3.5'

services:
  postgres-superset:
    image: postgres:14
    container_name: postgres-superset
    environment:
      POSTGRES_USER: superset
      POSTGRES_PASSWORD: superset
      POSTGRES_DB: superset
    volumes:
      - postgres-superset:/var/lib/postgresql/data
    restart: always
    networks:
      - postgres
    ports:
      - 5444:5432

volumes:
  postgres-superset:

networks:
  postgres-superset:
    external: true
    name: postgres-superset
```

