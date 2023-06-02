# [port-forwarding-mongodb](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/)
Practice port-forwarding with Mongodb
## Use Port Forwarding to Access Applications in a Cluster
### Install mongosh
1. Import the MongoDB public GPG Key
`wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -`
2. Create a list file for MongoDB 
`echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list`
3. Reload local package database 
`sudo apt-get update`
4. Install the mongosh package (latest stable version) 
`sudo apt-get install -y mongodb-mongosh`
Create MongoDB deployment and service
1. Create a deployment that runs MongoDB 
`kubectl apply -f https://k8s.io/examples/application/mongodb/mongo-deployment.yaml`
2. Check pod status to verify status 'Running'
`kubectl get pods`
3. Check deployment status
`kubectl get deployment`
4. View ReplicaSet status
`kubectl get replicaset`
5. Create a service to expose MongoDB on the network
`kubectl apply -f https://k8s.io/examples/application/mongodb/mongo-service.yaml`
6. Check the service was created
`kubectl get service mongo`
7. Verify that the MongoDB server is running in the Pod, and listening on port 27017
`kubectl get pod mongo-78c7d87875-nsn2j --template='{{(index (index .spec.containers 0).ports 0).containerPort}}{{"\n"}}'`
8. Forward a local port to a port on the service
`kubectl port-forward service/mongo 28015:27017`
9. Start mongosh CLI
`mongosh --port 28015`
