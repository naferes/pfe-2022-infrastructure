# Pfe 2022 Infrastructure



## Getting started

To make it easy for you to get started with this project, here's a list of recommended next steps to preapre the environment.

### installing docker 
1- Install docker 
2- add the user to the docker groups 
### installing minikube
1- install minikube using the docker driver
2- enabling Ingress Addons
## PostgreSQL Database
```
cd existing_repo
kubectl create -f postgres_database/
```
### Initializing the database 
```
kubectl exec -i -t medical-db -- psql -U postgres < postgres_database/postgresSchema.back
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
1- Adding Dockerfile.dev to the back-office root folder
2- adding dev.proxy.conf.json to th root back-office folder
3- 
```
docker build -t back-office:dev -f Dockerfile.dev .
docker tag back-office:dev <your_dockerhub_username>/back-office:dev
docker push <your_docker_hub_username>/back-office:dev

## Config-server
```
kubectl create -f cinfig-server/
```
## Oauth-ms
```
kubectl create -f oauth-ms/
```
## assessment-ms
```
kubectl create -f assessment-ms/
```
## Healthcare-facility-ms
```
kubectl create -f healthcare-facility-ms/
```
## Healthcare-ms
```
kubectl create -f healthcare-ms/
```
## Notification-ms
```
kubectl create -f notification-ms/
```
## Ingress
1- Find out your minikube ip address
```
minikube ip
```
2- Adding this line to /etc/hosts file
```
<your_minikube_ip_address> dev.wellinkare.info
```
3-
```
kubectl create -f ingress.yml
```