# mongo_mini_cluster
A trivial k3s cluster for mongo express web interface
## creating mongodb deployment
with only one replica and one label app=mongodb which will help the service to match the pods
### clusterip internal service
No need for load-balancing capability as it's only one replica but helpful for pod intercommunication, listening on port 27017
## mongo-express deployment
also containing one replica and one label app=mongo-express
### external service type LoadBalancer
used to listen on port 30001 for incoming requests and redirect them to the service internal port 8081 and then to container port 8081
## one secret object
contains container-specific environment variable: MONGO_INITDB_ROOT_USERNAME , MONGO_INITDB_ROOT_PASSWORD for mongodb deployment.
### env for mongo-express in secret object:
- ME_CONFIG_MONGODB_ADMINUSERNAME    for web interface authentication
- ME_CONFIG_MONGODB_ADMINPASSWORD 
- ME_CONFIG_MONGODB_ENABLE_ADMIN     to enable the web login 
- ME_CONFIG_BASICAUTH_USERNAME       to connect to the mongodb server
- ME_CONFIG_BASICAUTH_PASSWORD
## one configmap object
stores only one env specif for mongo-express ME_CONFIG_MONGODB_SERVER to reach its mongodb server address via the internal service.
