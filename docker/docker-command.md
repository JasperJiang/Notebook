# Docker command

```bash
docker ps -a | awk '{print $1}' | xargs docker rm
```

```bash
docker rmi $(docker images | grep "^<none>" | awk '{print $3}')
```

```bash
docker ps -a -q
```

```bash
docker stats
```

```bash
docker logs -f <id>
```

```bash
docker run -it --link xxx:xxx -p xxxx:xxxx —name xxxx <id> 
```

```bash
docker run -d -p 5000:5000 --restart=always --name registry \
  -v /DATA/docker_registry:/var/lib/registry \
  registry:2
```

### docker swarm

```bash
docker swarm init
```

```bash
docker stack deploy -c docker-compose.yml common_arch
```

```bash
docker stack down common_arch
```

portainer

```bash
docker service create \
    --name portainer \
    --publish 9000:9000 \
    --constraint 'node.role == manager' \
    --mount type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
    portainer/portainer \
    -H unix:///var/run/docker.sock
```

```bash
docker stack deploy -c docker-compose.yml --with-registry-auth
```

```bash
docker rmi -f $(docker images -f "dangling=true" -q)
```

