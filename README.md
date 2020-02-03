# Metadata Service 

1) POST to create an entry in the database
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"group":"sunitparekh","name":"city","value":"Pune"}' \
  http://localhost:8080/metadata

2) GET an entry posted in step 1
curl http://localhost:8080/metadata/<id-received-in-post-response>



# files to chanage for connecting to actual mongodb instance
1) src/main/java/org/boot/services/metadata/InMemoryMongoDB.java
comment @Configuration on line # 8

2) src/main/resources/application.properties
comment out line 24 
uncomment line 25 (remember the name of the mongodb server or change it as you like)


#### Assignment

1. Dockerize the Application-> Run `docker-compose up`
2. Run the Application locally with a single Replica-> Run `docker stack deploy -c docker-stack.yml metadata-service`
3. Publish the Docker Image to a custom Repository : `docker push manalikhanna/metadata-service:latest`
4. Create 3 Machines -> 1 Manager, 2 Nodes
    ```bash
    docker-machine create --driver virtualbox manager
    docker-machine create --driver virtualbox node1
    docker-machine create --driver virtualbox node2
    ```
5. Login to Manager Machine using `docker-machine ssh manager`
6. Get the Machine's IPs from `docker-machine ls` command
7. Run the following command on manager machine to init the swarm manager->`docker swarm init --advertise-addr <manager_machine_ip>`
8. Run the output of above command on the other 2 nodes -> `docker swarm join --token <token> <manager_node_ip>:<manager_node_port>`
9. Copy `docker-swarm.yml` to manager node and run the command -> ` docker stack deploy -c docker-compose.yml metadata-service-stack
10. Test the Swarm setup by using curl statements on the 3 different IPs. The `GET` request for all 3 should yield the same result.
