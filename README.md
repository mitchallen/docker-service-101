
## Usage

### Step 1. Initialize

```
docker swarm init
```

### Step 2. Deploy

```
docker stack deploy -c docker-compose.yml thing-lab
```

### Step 3. List al services

```
docker service ls
```

## Step 4. List this service stack

```
docker stack services thing-lab 
```

## Step 5. List service tasks

List service tasks (name + underscore + yaml service name).

Tasks are individually named containers, replicas (i.e. pods)

```
docker service ps thing-lab_web
```

## Step 6. List all containers 

```
docker container ls
```

## Step 7. Test server

```
curl http://localhost:4000
```

## Step 8. List stack containers

```
docker stack ps thing-lab
```

## Change replicas

Edit docker-compose.yml and change the replicas

```
docker stack deploy -c docker-compose.yml thing-lab

docker stack ps thing-lab
```

## Restart a container

* List the containers
```
docker container ls
```
or filter the list:
```
docker container ls | grep thing-lab
```
* Copy the CONTAINER_ID of one of the lab containers to the clipboard
* Kill the container
```
docker kill (paste in container id)
```
* List the containers again:
```
docker container ls | grep thing-lab
```
* List the stack
```
docker stack ps thing-lab
```
Notice that the old container is there, just shutdown and replaced by the new one.

## Shutting down a replica with SIGINT

See: https://docs.docker.com/config/containers/start-containers-automatically/

The behavior depends on the restart policy, which is currently:

```
restart_policy:
    condition: on-failure
```

Try killing a container again, but with a signal of SIGINT:

```
docker kill --signal=SIGINT (a running container id)
```

Notice that this time the container is Shutdown, but not replicated

```
docker stack ps thing-lab
```

Run to see that one of the replicas is down for good.

```
docker service ls
```

At this point you need to remove the swarm and start over (see below)

Try again with different restart policies ("always", "unless-stop").

## Remove the stack and the swarm

```
docker stack rm thing-lab

# Will have to run the init command first to recreate

docker swarm leave --force
```

## References

* https://docs.docker.com/get-started/part3/
* https://docs.docker.com/engine/swarm/how-swarm-mode-works/services/
* https://docs.docker.com/config/containers/start-containers-automatically/
* https://blog.codeship.com/ensuring-containers-are-always-running-with-dockers-restart-policy/

