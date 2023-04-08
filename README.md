# Using Secrets, Volumes and ConfigMaps

In this section, deploy a mongo database and a mongo-express viewer for the database.

In a normal deployment, where a database is to deployed, there are certain things configured to make sure the database is accessible and available for use in the environent it is deployed in. 
These are:
1. environment variables 
2. passwords and usernames

In the normal Docker environment, these variables will be passed directly into the containers either inline or through a docker-compose file.

In Kubernetes, same is done but in different formats. 
- The passwords and username have to be kept 'secret' and hence the Secret config file is used for this purpose.
- They are then injected into the container/Pod at runtime, the configMap file is used for this purpose too.
You can guess the Secret file is used to keep secrets and configMap is used to keep configuration that has to be injected at runtime.

The order they are referenced by Kubernetes is as such; 
- the Secrets are injected first, 
- the configMaps second, as they can reference secrets kept in the Secret file
- the Deployments, to start the pods
- the Services, to provide network configuration for the pods.

In this order, a bash script can be used to deploy these:
```Bash
kubectl create -f mongo/secret.yaml
kubectl create -f mongo/configMap.yaml
kubectl create -f mongo/mongodb.yaml
kubectl create -f mongo/service-db.yaml
kubectl create -f mongo/mongoexpress.yaml
kubectl create -f mongo/service-express.yaml
```
to delete the deployments and services:
```Bash
kubectl delete -f mongo/secret.yaml
kubectl delete -f mongo/configMap.yaml
kubectl delete -f mongo/mongodb.yaml
kubectl delete -f mongo/service-db.yaml
kubectl delete -f mongo/mongoexpress.yaml
kubectl delete -f mongo/service-express.yaml
```

## Using Helm (Kubernetes Package Manager)
Helm is a package manager for Kubernetes. Basically, it is a high-level way to deploy containers and orchestrate them using Kubernetes but without getting your hands dirty with Kubernetes.

For this deployment, Helm is used to create charts that contain placeholders that can be populated with Kubernetes YAML configurations.
This deployment uses Secrets and ConfigMaps for the MongoDB deployment and for the Mongo-Express viewer uses the default Deployment and Service files.

- Create the helm charts for both deployments.
```Bash
helm create mongodb-chart
helm create mongo-express-chart
```
For this, I created my own Helm configurations that mirror the Kubernetes YAML files as closely as possible.
The values that populate the placeholders in the Helm charts are found outside in the *values.yaml files.

Load the values files into the Helm charts and check whether the configurations match using the `helm template` command.
```Bash
helm template -f mongodb-values.yaml mongo-chart/
helm template -f mongo-express-values.yaml mongo-express-chart/
```