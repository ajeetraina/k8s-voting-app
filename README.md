<img width="1185" alt="image" src="https://github.com/user-attachments/assets/a5562b60-78fc-4619-b1db-8e1e504b576e" /># A Simple Voting App using Compose Bridge

A simple distributed application running across multiple Docker containers. This solution uses Python, Node.js, .NET, with Redis for messaging and Postgres for storage.

## Prerequisite

- Docker Desktop
- Enable Kubernetes with Kind cluster


<img width="1376" alt="image" src="https://github.com/user-attachments/assets/4e3e86bc-232a-4747-8540-9e2e5af70069" />



## Bringing up the application stack

```shell
docker compose up -d --build
```

The `vote` app will be running at [http://localhost:8080](http://localhost:8080), and the `results` will be at [http://localhost:8081](http://localhost:8081).




## Architecture

![Architecture diagram](architecture.excalidraw.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time



## Option 1: Using K8s YAML file

- Stop the compose services

```
docker compose down
```

- Go to k8s-specification directory and run the following command:

```
kubectl apply -f k8s-specifications
```


```
deployment.apps/db created
service/db created
deployment.apps/redis created
service/redis created
deployment.apps/result created
service/result created
deployment.apps/vote created
service/vote created
deployment.apps/worker created
```

- Port forwarding

```
kubectl port-forward svc/vote 5000:8080
```

<img width="868" alt="image" src="https://github.com/user-attachments/assets/51b65b55-502e-4ac4-bdcf-391b3359c776" />




## Option 2: Using Compose Bridge

## Converting to K8s objects

Select the  Compose Stack under Docker Dashboard and you'll see View Configuration.



<img width="1078" alt="image" src="https://github.com/user-attachments/assets/18a5b20d-889c-48ee-a1aa-75a0dc3e5906" />

Click on View Configurations and click "Convert and deploy to Kubernetes".

<img width="1095" alt="image" src="https://github.com/user-attachments/assets/10fc3e18-6b8c-4f45-9aa3-3f2e11b6bf66" />



<img width="804" alt="image" src="https://github.com/user-attachments/assets/6590ef7c-a5ee-4849-8223-6c6f6879b2a1" />



<img width="1185" alt="image" src="https://github.com/user-attachments/assets/3738fb4d-ca64-495c-9b4f-20509a9aade1" />

## Notes

The voting application only accepts one vote per client browser. It does not register additional votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple
example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to
deal with them in Docker at a basic level.
