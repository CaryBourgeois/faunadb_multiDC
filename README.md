# faunadb_multiDC



This is an example of how you might use the public FaunaDB docker image with docker-compose to create a 3 data center 1 node per DC cluster.

The goal of this is to give you a development and test FaunaDB cluster on a local machine. You will need to have enough capacity on you  local machine to allocate at least 12GB of RAM to Docker as each of the 3 nodes will try to allocate 3GB. Each node will also request 2 cCPU's.

Before you use this you will want to have pulled down the latest version of the FaunaDB docker image using this command: 
`docker pull fauna/faunadb:latest`

To start the cluster, clone this repository and then navigate to the directory. From that directory issue the following command:
`docker-compose up -d`

Once the cluster is ready you will need to execute the following command to start replication between the Data centers:
`docker exec -it $(docker ps -q -f "name=faunadb-node1") /bin/bash -c "cd /faunadb/enterprise && bin/faunadb-admin update-replication dc1 dc2 dc3"`

You can check the status of the cluster using the following commad:
`docker exec -it $(docker ps -q -f "name=faunadb-node1") /bin/bash -c "cd /faunadb/enterprise && bin/faunadb-admin status"`

You can also access the database using this command:
`curl -f http://localhost:8443/ping`

You should recieve this response:
`{ "resource": "Scope write is OK" }`

When you are all done, you can bring the instances down with this command:
`docker-compose down`

The data for each node is saved in the respective `node` directories. If you want to start a fresh DB you can clear all the data by deleting the `data` and `log` directories under each node directory

Have fun!
