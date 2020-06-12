# xmrig-miner

Mine monero with `xmrig` over a SOCKS5 tor proxy at http://moneroocean.stream

## Using

1. edit `docker-compose.py` to add your wallet address
2. `sudo docker-compose up -d`

## To use with swarm mode

### Setup the swarm

on the manager mode: `sudo docker swarm init`

on the worker nodes, copy-paste the line spit out from the manager node

### Setup the image to your private registry

This is a bit of a process but here is the gist of it:

#### On what will be your registry, probably some file server you have somewhere:

`sudo docker container run -d -p 5000:5000 --name registry -v /path/to/registry:/var/lib/registry registry`

This will run the registry container that will keep track of the tags pushed to it.

#### Allow insecure traffic to the registry from the nodes

You need this as since this is local lan you probably don't have ssl certs passed around.

For every node in the cluster, add this file:

```
# $ /etc/docker/daemon.json
{ "insecure-registries": ["REGISTRY_IP:5000"] }
```

Now restart the docker daemon on all of the nodes.

#### Create the image, tag it, and push to the registry

Now from a manager node:

```
sudo docker-compose build
sudo docker tag IMAGE_NAME REGISTRY_IP:5000/TAG_NAME
sudo docker push REGISTRY_IP:5000/TAG_NAME
```

### OKAY, now that we have a local lan docker registry...

Let's push it to the swarm.

On the the manager node,

```
sudo docker stack deploy STACK_NAME -c docker-compose.yml
sudo docker service scale STACK_NAME=NUMBER_OF_NODES
```

And that's it!

#### Optional, visualize your running mining farm

On the manager node:

```
docker service create \
-p 8080:8080 \
--constraint=node.role==manager \
--mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
dockersamples/visualizer
```

Now, view in browser `http://$IP:8080`

*Visualizing with netapp / grafana coming sometime eventually maybe never*
