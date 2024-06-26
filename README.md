# docker_lab2
lab2
# ITI - Docker Lab Two🐋
## Task 1: Run a container using nginx image, and mount a directory from your host into the Docker container.
example: /home/samy/nginx:/home/nginx (bind mount)

### Steps
#### 1. Create Bind Mount Directory
```bash
mkdir file_dirictory

```

#### 2. Run a container using nginx image
```bash
docker run -d --name file_dirictory -v /root file_dirictory:/user/share/nginx/html nginx
docker exec -it file_dirictory bash
```

#### 3. Echo any content to show when curl ip-address
```bash
cd /user/share/nginx/html
echo "Hello iam buthina" > index.html
exit
docker inspect -f '{{.NetworkSettings.IPAddress}}' file_dirictory   ->172.17.0.7
curl 172.17.0.7
```

---
## Task 2: Create 2 docker network (net-1 & net-2)

### Steps
#### 1. Create 2 docker network (net-1 & net-2)
```bash
docker network create networ_1_new
docker network create network_2_new
```

#### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them.
```bash
docker run -d --name nginx_networ_1_new --net networ_1_new nginx:alpine
docker run -d --name nginx_network2 --net network_2_new nginx:alpine
```

#### 3. Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.
```bash
docker run -d --name nginx_network_3_new --net network_2_new nginx:alpine
```

#### 4. Inspect the 3 containers to know their IPs and write them aside.
```bash
docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_networ_1_new
172.18.0.2

docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_network2
172.18.0.3

docker inspect -f '{{.NetworkSettings.Networks.network_2.IPAddress}}' nginx_network_3_new
172.20.0.2
 docker inspect nginx_networ_1_new | grep IPAddress
```

#### 5. Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)
```bash
docker exec -it nginx_networ_1_new sh 
Use ping or curl
ping 172.20.0.2
ping fail
```

#### 6. Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)
```bash
docker exec -it nginx_networ_1_new sh 
curl 172.18.0.2
ping 172.18.0.3
ping success 
```

## Task 3: Explain the difference between Docker volumes and Bind Mount.
Docker volumes are managed storage entities within Docker's ecosystem, abstracting away underlying host file systems.
They are highly versatile and can be easily manipulated using Docker commands and APIs, making them suitable for various storage scenarios.
Docker volumes are ideal for managing persistent data in Dockerized applications, offering features like data persistence and sharing across multiple containers.
Due to their Docker-managed nature, volumes provide better portability and compatibility across different Docker environments.
Bind mounts establish a direct link between a directory on the host machine and a directory inside a container.
They offer a straightforward way to share files and directories between the host and container, making them convenient for development and debugging tasks.
Unlike Docker volumes, bind mounts rely entirely on the host's file system structure, which can limit portability between different host environments.
Bind mounts are commonly used for scenarios where real-time synchronization between host and container directories is required, such as development environments or accessing configuration files.
#   d o c k e r _ d a y 2  
 