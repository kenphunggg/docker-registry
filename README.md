# Make your own [Docker registry](https://hub.docker.com/)
## What is a **Docker registry**
Docker registry is a system for storing and distributing Docker images. Users can pull and manage images for container deployment.
## Prerequisites
### Make sure you have installed [Docker](https://docs.docker.com/engine/install/ubuntu/) on your device
### On your remote machine, update ```/etc/docker/daemon.json``` to allow access to the insecure registry. ```Registry-ip``` is your device's ip address
```json
{
  "insecure-registries": ["<registry-ip>:5000"]
}
``` 
### Restart docker
```bash
systemctl restart docker
```
## Setting up docker registry
### Run the Docker registry container
```bash
$ docker run -d -p 5000:5000 --name registry -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 registry:2
```
### Verify status of the container
```bash
$ docker container ls
```
#### It should return as below
```bash
ONTAINER ID   IMAGE        COMMAND                  CREATED         STATUS         PORTS                                         NAMES
317f3e26de8d   registry:2   "/entrypoint.sh /etc…"   6 minutes ago   Up 6 minutes   0.0.0.0:5000->5000/tcp, [::]:5000->5000/tcp   registry
```
### Tag and push your image
#### You need to have an image first
```bash
$ docker pull docker.io/bonavadeur/shuka:v1.3
```
#### Then tag the image point to your local registry
```bash
$ docker tag docker.io/bonavadeur/shuka:v1.3 localhost:5000/lazyken/shuka:1
```
#### Now push it to your local registry
```bash
$ docker push localhost:5000/lazyken/shuka:1
```
## Using your local registry
### On your remote machine, update ```/etc/docker/daemon.json``` to allow access to the insecure registry. ```Registry-ip``` is your device's ip address
```json
{
  "insecure-registries": ["<registry-ip>:5000"]
}
``` 
### Restart docker
### Testing your local registry
```bash
$ docker pull 100.82.175.64:5000/lazyken/shuka:1
```
#### It should return as below
```bash
1: Pulling from lazyken/shuka
a803e7c4b030: Pull complete 
bf3336e84c8e: Pull complete 
8973eb85275f: Pull complete 
f9afc3cc0135: Pull complete 
39312d8b4ab7: Pull complete 
1ffb55084ac5: Pull complete 
603d96c4d679: Pull complete 
276348a2c42f: Pull complete 
946ffc13bf75: Pull complete 
f4b27006dae3: Pull complete 
Digest: sha256:92b17a46559202b3584a3e9e1373914ed0e66bc55ba6d3a1353312dae25de79b
Status: Downloaded newer image for 100.82.175.64:5000/lazyken/shuka:1
100.82.175.64:5000/lazyken/shuka:1
```