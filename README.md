# rozgar-deployment

## Steps
- Install docker using zip - The steps for the same are listed [here](https://docs.docker.com/engine/install/binaries/)
- Setup docker swarm
- Setup Citus
- Build Airflow
```shell
docker build -t samagragovernance/airflow:0.0.4 .
docker save --output airflow.tar samagragovernance/airflow:0.0.4

docker pull postgres:9.6
docker pull redis:5.0.5
docker save --output redis.tar redis:5.0.5
docker save --output postgres.tar postgres:9.6
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
```shell
scp <airflow folder> root@10.88.232.223:/mnt/
cd samagra-airflow
adduser airflow
chown -R airflow:airflow dags
```
- Setup portainer
- Setup airflow
```shell
docker load -i airflow.tar
docker load -i postgres.tar
docker load -i redis.tar
```
- Setup hasura
```shell
docker load -i hasura.tar
```
- Get the other IP and add it to swarm

- Start Airflow
```
docker-compose -f docker-compose-CeleryExecutor.yml up -d
```

- Creating Airflow user

```shell
docker-compose -f docker-compose-CeleryExecutor.yml exec --env AIRFLOW__CORE__SQL_ALCHEMY_CONN="postgresql+psycopg2://airflow:airflow@postgres:5432/airflow" webserver python
```

```python
from airflow import models, settings
from airflow.contrib.auth.backends.password_auth import PasswordUser
user = PasswordUser(models.User())
user.username = 'admin'
user.email = 'admin@rozgarportal.com'
user.password = 'admin'
session = settings.Session()
session.add(user)
session.commit()
session.close()
exit()
```

For creating a superuser
```python
from airflow import models, settings
from airflow.contrib.auth.backends.password_auth import PasswordUser
user = PasswordUser(models.User())
user.username = 'admin'
user.email = 'admin@rozgarportal.com'
user.password = 'admin'
user.superuser = True # Change from above.
session = settings.Session()
session.add(user)
session.commit()
session.close()
exit()
```

- Updating the DAGs
After the folders are updated. Use the following command to update the permissions.

```shell
docker-compose -f docker-compose-CeleryExecutor.yml exec --user root webserver chmod -R 777 dags
```
