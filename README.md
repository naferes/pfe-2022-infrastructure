# Pfe 2022 Infrastructure



## Getting started

To make it easy for you to get started with this project, here's a list of recommended next steps to preapre the environment.

### installing docker 
1- Install docker 

2- add the user to the docker groups 
### installing minikube
1- install minikube using the docker driver

2- enabling Ingress Addons

### installing Helm 
following the official Helm website here: https://helm.sh/docs/intro/install/
## PostgreSQL Database
Following this link: https://arctype.com/blog/deploy-postgres-kubernetes/

```
cd existing_repo
helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo postgresql
helm install postgresql-test01 bitnami/postgresql global.postgresql.auth.postgresPassword=secret 
kubectl get all ##for verification
helm ls ##for verification
export POSTGRES_PASSWORD=$(kubectl get secret --namespace default postgresql -o jsonpath="{.data.postgresql-password}" | base64 --decode)

#To connect to your database run the following command:
kubectl run postgresql --rm --tty -i --restart='Never'  --image docker.io/bitnami/postgresql:11.12.0-debian-10-r44 --env="PGPASSWORD=$POSTGRES_PASSWORD" --command -- psql --host postgresql -U postgres -d postgres -p 5432

create database medical_db_int;
\c medical_db_int

create user assessment_ms_pre_prod with password 'secret';
create schema assessment authorization assessment_ms_pre_prod;

create user healthcare_facility_ms_pre_prod with password 'secret';
create schema healthcare_facility authorization healthcare_facility_ms_pre_prod;

create user healthcare_ms_pre_prod with password 'secret';
create schema healthcare authorization healthcare_ms_pre_prod;

create user notification_ms_pre_prod with password 'secret';
create schema notification authorization notification_ms_pre_prod;

create user oauth2_ms_pre_prod with password 'secret';
create schema oauth2 authorization oauth2_ms_pre_prod;

```
## Building Images 
# You can use direclty the Images built my ME: Who I am i ??? ALPHAAAAAAAAAAA
### Config Server Image
1- Replacing the config Repo link by your config repo link (or mine: https://gitlab.com/nferes/wellinkare-mine-config-server.git )

2- Configuring maven to build images: https://www.baeldung.com/jib-dockerizing

3- 
```
mvn compile com.google.cloud.tools:jib-maven-plugin:2.6.0:build -Dimage=$IMAGE_PATH
```

### Oauth-ms, assessment-ms, healthcare-ms, healthcare-facility-ms and notifications ms are the same
1- Creating dokcer hub repositories

2- 
```
mvn compile com.google.cloud.tools:jib-maven-plugin:2.6.0:build -Dimage=$IMAGE_PATH
```
### back Office Image
1- Adding Dockerfile.int to the back-office root folder

2- adding int.proxy.conf.json to th root back-office folder

3- 
```
docker build -t back-office:int -f Dockerfile.int .
docker tag back-office:int <your_dockerhub_username>/back-office:int
docker push <your_docker_hub_username>/back-office:int
```
## Config-server

Replace the variables values in values.yaml with yours file if it's necessary
```
helm lint config-server/
## to check
helm template config-server/
helm install config-server/
```
## Oauth-ms
Replace the variables values in values.yaml with yours file if it's necessary
```
helm lint oauth-ms/
## to check
helm template oauth-ms/
helm install oauth-ms/
```
## assessment-ms
Replce the variables values in values.yaml with yours file if it's necessary
```
helm lint assessment-ms/
## to check
helm template assessment-ms/
helm install assessment-ms/
```
## Healthcare-facility-ms
Replace the variables values in values.yaml with yours file if it's necessary
```
helm lint healthcare-facility-ms/
## to check
helm template healthcare-facility-ms/
helm install healthcare-facility-ms/
```
## Healthcare-ms
Replace the variables values in values.yaml with yours file if it's necessary
```
helm lint healthcare-ms/
## to check
helm template healthcare-ms/
helm install healthcare-ms/
```
## Notification-ms
Replace the variables values in values.yaml with yours file if it's necessary
```
helm lint notification-ms/
## to check
helm template notification-ms/
helm install notification-ms/
```
## Ingress
1- Find out your minikube ip address
```
minikube ip
```
2- Adding this line to /etc/hosts file
```
<your_minikube_ip_address> int.wellinkare.info
```
3-
```
helm lint ingress/
## to check
helm template ingress/
helm install ingress/

```