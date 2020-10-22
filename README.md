# rozgar-deployment

## Task List
- Install docker using zip
- Setup docker swarm
- Setup Citus
- Build Airflow
```shell
docker build -t samagragovernance/airflow:0.0.4 .
docker save --output airflow.tar samagragovernance/airflow:0.0.4
```
- Build Portainer
```shell
docker pull portainer/portainer-ce
docker pull portainer/agent
docker save --output portainer-agent.tar portainer/agent
docker save --output portainer.tar portainer/portainer-ce
```

- Build Hasura
```shell
docker pull hasura/graphql-engine:v1.3.2
docker save --output hsaura.tar hasura/graphql-engine:v1.3.2
```
- Move files to server
```shell
scp binaries
```
- Docker load all this
- Create docker-compose files to run
- Transfer the docker containers and Airflow git
- Setup portainer
- Setup airflow
```shell
docker load -i hasura.tar
```
- Setup hasura
```shell
docker load -i hasura.tar
```
- Get the other IP and add it to swarm
- Creating Airflow user
