# Demo 1

## Build & Run echo1
```
cat docker_demo/demo_01/echo_1
docker build -f docker_demo/demo_01/echo_1 -t echo:01 docker_demo/demo_01
docker run echo:01
```

## Build & Run echo2
```
cat docker_demo/demo_01/echo_2
docker build -f docker_demo/demo_01/echo_2 -t echo:02 docker_demo/demo_01
docker run echo:02
docker run -e ECHO_MESSAGE="welcome everyone" echo:02
```

## Check Images and Containers
```
docker image ls
docker ps
docker ps -a
```

## Docker Resistry - Login and Push
```
docker login
docker tag echo:01 baselbmz/echo:01
docker push baselbmz/echo:01
```
```
docker build -f docker_demo/demo_01/echo_2 -t registry.visual-meta.com/docker_meetup/echo:01 docker_demo/demo_01
docker push registry.visual-meta.com/basel_meetup/echo:02
```
```
docker rm $(docker ps -aq)
docker image ls
docker image rm echo:02 registry.visual-meta.com/docker_meetup/echo:02
```

# Demo 2

## Files
```
cat docker_demo/demo_02/app_1
cat docker_demo/demo_02/code/app.py
cat docker_demo/demo_02/code/requirements.txt
```

## build, run, logs, exec
```
docker build -f docker_demo/demo_02/app_1 -t app:01 docker_demo/demo_02/
docker run app:01
docker run -p 4000:5000 app:01
docker run -d -p 4000:5000 app:01
docker ps
docker logs lucid_fermi
docker logs -f lucid_fermi
docker exec -it lucid_fermi /bin/sh
(try ps inside, outside)
docker ps
docker stop lucid_fermi
docker ps
```

# Demo 3

## Files
```
cat docker_demo/demo_03/code/app.py
cat docker_demo/demo_03/code/requirements.txt
cat docker_demo/demo_03/app_2
```

## Simple Build and Run
```
docker build -f docker_demo/demo_03/app_2 -t app:02 docker_demo/demo_03

docker run -d --name redis redis
docker run -d -p 4000:5000 --name app_2 --link redis app:02

docker stop app_2 redis
docker rm app_2 redis
```

## Run - Volume, Networks
```
docker run -d --name redis -v /demo_data:/data redis
docker run -d -p 4000:5000 --name app_2 --link redis app:02

docker network create app_db
docker network ls
docker run -d --name redis --network app_db -v /demo_data:/data redis
docker run -d --name app_2 --network app_db -p 4000:5000  app:02
docker run --rm --network app_db redis redis-cli -h redis info server
alias redis-cli='docker run --rm --network app_db redis redis-cli -h redis'
redis-cli get hits
redis-cli set hits 
```


## docker-compose - Install
```
curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

## docker-compose - Build & Run
```
cd docker_demo/demo_03/
docker-compose build
docker-compose up
docker-compose up -d
docker ps
docker-compose ps
docker-compose logs
docker-compose stop
docker-compose rm
```

# Demo 4

## Docker Swarm - Setup
```
docker swarm init
docker swarm join-token worker
docker swarm join-token manager
docker node ls
```

## Docker Swarm - Create a service
```
docker service create --name hello hello-world
docker service ls
docker service ps hello
docker service logs hello
docker service rm hello
```


##  Docker Swarm - Create a Stack
```
docker stack deploy
docker-compose build
docker-compose push
docker stack deploy -c docker-compose.yml app
docker stack ls
docker stack services app
docker stack rm app
```
