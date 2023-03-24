## Bem-vindo

Este repositório contem docker-compose com o Postgres Padrão.

Deixo abaixo outros padrões para Projetos como Apache Airflow, Apache SUperset, e vou adicionando conforme e necessidade.

Então você pode realizar uma cópia do docker-compose.yml para cada projeto, ou se preferiri, iniciar um container com o postgres padrão e gerência você mesmo a criação dos usuários e bases de dados.

Um detalhe para a porta do container, caso opte por criar um docker-compose para cada projeto, você pode omitir a opção `ports`, agora caso deseje acessar externamente os dados desse banc o é necessário uma porta diferente para cada container

Um detalhe importante é criar um volume e uma rede para cada postgres de cada aplicação caso deseje.

<br>

Abaixo alguns exemplos de configurações

**Volumes**

Voê pode criar seus volumes com o comando do docker ou informar o nome do volume no docker-compose para que o docker crie um volume anônimo conforme abaixo.

````yml
#Volumes anônimos criados pelo Docker no momento em que os serviõs são iniciados
version: '3.5'

service:
  .
  .
  .
  .
  .
  
volumes:
  postgres-superset:
````


````yml
#Volumes criados Externamnte com o comando "docker volume create nome_do_volume"
version: '3.5'

service:
  .
  .
  .
  .
  .
  
volumes:
  postgres-superset:
    external: true
````

**Redes**

Algo importante são as redes do Docker.

Cada container sobe em uma rede privada, então se você deseja ter comunicação entre diversos containers é necessário criar redes externas e adiciona-las no docker-compose de cada serviço.

O Exemplo abaixo mostra uma configuração de redes entre um postgres separado para Apache Superset e o Apache Superset.

````yml
#Serviço de um postgres separado para o Apache Superset
version: '3.5'

services:
  postgres-superset:
    .
    .
    .
    .
  networks: 
   - postgres-superset
  
  
networks:
  postgres-superset:
    external: true
    name: postgres-superset
````

````yml
#Serviço do Apache Superset
version: '3.5'

services:
  superset:
    .
    .
    .
    .
  networks: 
   - postgres-superset
  
  
networks:
  postgres-superset:
    external: true
    name: postgres-superset
````


É importanta adicionar a rede postgres-superset no docker-compose do Apache Superset para que tenha comunicação entre os containers.

As redes são criadas externamento com o comando `docker network create nome_da_rede`.

Com esta configuração o Docker é capaz de resolver nomes, transformando o nome do serviço `postgres-superset` em um endereço válido dentro do Docker


## Exemploso de docker-compose para postgres

**Postgres para Apache Superset**

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
      - postgres-superset
    ports:
      - 5444:5432

volumes:
  postgres-superset:

networks:
  postgres-superset:
    external: true
    name: postgres-superset
```

**Postgres para Apache Airflow**

```yml
version: '3.5'

services:
  postgres-airflow:
    image: postgres:14
    container_name: postgres-airflow
    environment:
      POSTGRES_USER: airflow
      POSTGRES_PASSWORD: airflow
      POSTGRES_DB: airflow
    volumes:
      - postgres-airflow:/var/lib/postgresql/data
    restart: always
    networks:
      - postgres
    ports:
      - 5444:5432

volumes:
  postgres-airflow:

networks:
  postgres-airflow:
    external: true
    name: postgres-airflow
``` 
