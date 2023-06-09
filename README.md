# Example Voting App

A simple distributed application running across multiple Docker containers.      

i made the manifests to deploy the app inside a kubernetes cluster.      

This solution uses Python, Node.js, .NET, with Redis for messaging and Postgres for storage.     

## Architecture

![Architecture diagram](architecture.excalidraw.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time

## Notes

The voting application only accepts one vote per client browser. It does not register additional votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple
example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to
deal with them in Docker at a basic level.

## kubernetes Deployment     

make sure you have a ready kubernetes cluster and configured `kubectl` to interact with your cluster.      
your cluster must have a dynamic persistent volume provisioner or you can make pv object yourself.     

just apply the manifests and you are good to go.     

```
git clone https://github.com/AlirezaPourchali/kuber_assignment.git    

cd kuber_assignment     

kubectl apply -f manifests      
```
the `vote` app will be accessible at `http://<your_node_ip>:30001` and the results can be reached from `http://<your_node_ip>:30002`

to get the address of your node run 

```
kubectl get nodes -o wide      
```    

## Docker Deployment

Download [Docker Desktop](https://www.docker.com/products/docker-desktop) for Mac or Windows. [Docker Compose](https://docs.docker.com/compose) will be automatically installed. On Linux, make sure you have the latest version of [Compose](https://docs.docker.com/compose/install/).

Run in this directory to build and run the app:

```shell
docker compose up
```

ps: you might have a problem with building the worker(dotnet image) , you have to build the image with `buildx` and give the current system architecture and the system you are building it for.     

example:   

```
docker buildx build --platform=linux/amd64,linux/arm64      

```

The `vote` app will be running at [http://localhost:5000](http://localhost:5000), and the `results` will be at [http://localhost:5001](http://localhost:5001).
