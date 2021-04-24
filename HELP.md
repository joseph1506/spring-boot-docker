Build
mvn clean install

docker build -t spring-boot-docker:latest .

docker run -p 8081:8081 -t spring-boot-docker:latest

docker container ls

docker container kill <<containerId>>

docker tag spring-boot-docker:latest joseph1506/spring-boot-docker:latest
docker push joseph1506/spring-boot-docker:latest

kubectl delete -f kubernetes.yaml
kubectl apply -f kubernetes.yaml



Registry Name: joeaksregistry.azurecr.io


az acr login -n joeaksregistry
az acr update -n joeaksregistry --admin-enabled true

docker tag spring-boot-docker:latest joeaksregistry.azurecr.io/spring-boot-docker:latest

docker push joeaksregistry.azurecr.io/spring-boot-docker:latest


create aks cluster

az aks get-credentials -g akstestrg -n joeakscluster

az acr credential show -n joeaksregistry --query "passwords[0].value" -o tsv

kubectl create secret docker-registry acrsecret --docker-server=joeaksregistry.azurecr.io --docker-username=joeaksregistry --docker-password=9vMAicu4lWn30hGr2tSMuDfYv5AMWRn= --docker-email=josephin.angel@gmail.com

acrsecret is given in the kubernetes.yaml


kubectl create -f kubernetes.yaml


kubectl get pods

kubectl get service/spring-boot-docker-service


http://52.190.33.182:9081/rest/test
http://52.190.33.182:30001/rest/test

kubectl describe endpoints spring-boot-docker

kubectl describe service spring-boot-docker-service | select-string Endpoints

az aks update -n joeakscluster -g akstestrg --attach-acr joeaksregistry.azurecr.io


no need to configure NSG for the service as it will be done automatically by AKS