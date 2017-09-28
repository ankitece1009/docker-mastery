# docker-mastery
Learn Docker and Devops. Docker Mastery: The Complete Toolset From a Docker Captain.

Docker Mastery: The Complete Toolset From a Docker Captain  
https://www.udemy.com/docker-mastery

Course Resource
* [Docker_CheatSheet](https://github.com/smalltide/docker-mastery/blob/master/resource/cheatsheet.pdf)  
* [Slides1-5](https://github.com/smalltide/docker-mastery/blob/master/resource/Slides1-5.pdf)  
* [Slides6-10](https://github.com/smalltide/docker-mastery/blob/master/resource/Slides6-10.pdf)

Skills
1. Docker
2. DevOps
3. Docker Compose
4. Docker Swarm
5. Docker Machine
6. GitHub
7. DockerHub

install docker bash completion on Mac
```
  > https://docs.docker.com/docker-for-mac/#installing-bash-completion
  > brew install bash-completion
  > ln -s /Applications/Docker.app/Contents/Resources/etc/docker.bash-completion /usr/local/etc/bash_completion.d/docker
  ln -s /Applications/Docker.app/Contents/Resources/etc/docker-machine.bash-completion /usr/local/etc/bash_completion.d/docker-machine
  ln -s /Applications/Docker.app/Contents/Resources/etc/docker-compose.bash-completion /usr/local/etc/bash_completion.d/docker-compose
```
install docker, docker-machine, docker-compose in Linux
```
  > https://store.docker.com
  > https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
  > curl -sSL https://get.docker.com/ | sh (install latest docker using script)
  > docker version
  > sudo usermod -aG docker <linux user> (add user in docker group)
  > curl -L https://github.com/docker/machine/releases/download/v0.12.2/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine && chmod +x /tmp/docker-machine && sudo cp /tmp/docker-machine /usr/local/bin/docker-machine (install docker machine)
  > docker-machine version
  > sudo -i
  > sudo curl -L 
  https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose (install docker compose)
  > chmod +x /usr/local/bin/docker-compose
  > exit
  > docker-compose version  
```
start a nginx web server container
```
  > docker container run --publish 80:80 nginx (--publish for port, -p)
  > docker container run --publish 80:80 --detach nginx (--detach for run in background, -d)
  > curl 127.0.0.1
  > docker container ls
  > docker container stop <container id>
  > docker container ls -a
  > docker container run --publish 80:80 --detach --name webhost nginx (--name for define container name)
  > docker container logs webhost (watch log in webhost container)
  > docker container top webhost (watch process in webhost container)
  > docker container --help
  > docker container rm <container id> (docker container rm 4d5 834 9bc)
  > docker container rm -f <container id> (force delete docker container
```
Container VS. VM: It's Just a Process
```
  > docker container run --name mongo -d mongo
  > docker container top mongo
  > ps aux (show all running process)
  > ps aux | grep mongo
  > docker container stop mongo
  > ps aux | grep mongo
  > docker container start mongo
  > ps aux | grep mongo
```
Some docker container supply resource
```
  > https://github.com/mikegcoleman/docker101/blob/master/Docker_eBook_Jan_2017.pdf
  > https://www.youtube.com/watch?v=sK5i-N34im8 (Cgroups, namespaces, and beyond: what are containers made from?)
  > https://www.youtube.com/watch?v=066-9yw8-7c (Windows Containers and Docker 101)
  > https://www.youtube.com/watch?v=4ZY_4OeyJsw (Windows and Linux Parity with Docker)
  > https://www.youtube.com/watch?v=QASAqcuuzgI (Docker + Microsoft - Investing in the Future of your Applications)
```
Manage Multiple Containers
```
  > docker container run --name nginx -p 80:80 -d nginx
  > docker container run --name httpd -p 8080:80 -d httpd
  > docker container run --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes -p 3306:3306 -d mysql
  > docker container logs mysql (find the root pwd in mysql log)
  > docker container stop nginx httpd mysql
  > docker container rm nginx httpd mysql
  > docker container <stop, or rm> <container id, or name >
  > docker container ls
```
Containers CLI Process Monitoring
```
  > docker container run --name nginx -p 80:80 -d nginx
  > docker container run --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes -p 3306:3306 -d mysql
  > docker container top mysql
  > docker container inspect mysql
  > docker container stats (realtime monitor container stat) 
```
Run Shell Inside Containers
```
  > docker container run -it <image> (start a interactive container)
  > docker container exec <container> <command> (run command in container)
  > docker container run -it --name proxy nginx bash
  > ls -al (in container now)
  > docker container ls -a
  > docker container start -ai proxy
  > ls -al (in container now)
  > docker container exec <container> <command> (ex. ip a)
  > docker container exec mysql ip a
  > docker container exec -it <container> <command> (ex. bash)
  > docker container exec -it mysql bash
  > ps aux (in container now)
  > docker pull alpine
  > docker container run -it alpine sh
```
Linux Package Management Basics: apt, yum, dnf, pkg
```
  > https://www.digitalocean.com/community/tutorials/package-management-basics-apt-yum-dnf-pkg
```
Docker Networks and Container and command
```
  > docker container port <container> (watch container using port)
  > docker container run -p 80:80 --name webhost -d nginx
  > docker container port webhost
  > docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhos (foramt to Go template)
  > docker network ls
  > docker network inspect bridge
  > docker network create --driver=bridge my_app_net
  > docker network inspect my_app_net
  > docker network create --help
  > docker container run -d --name new_nginx --network my_app_net nginx
  > docker network inspect my_app_net
  > docker network connect my_app_net webhost (add webhost in my_app_net network)
  > docker container inspect webhost
  > docker network disconnect my_app_net webhost (remove webhost in my_app_net network)
  > docker container inspect webhost
```
Docker Networks: DNS and How Containers Find Each Other
```
  > docker container ls
  > docker container run -d -p 80:80 --name nginx1 --network my_app_net nginx:alpine
  > docker container run -d --name nginx2 --network my_app_net nginx:alpine
  > docker container run -d --name nginx3 --network my_app_net nginx:alpine
  > docker container exec -it nginx1 ping nginx2 (reach)
  > docker container exec -it nginx2 ping nginx3 (reach)
  > docker container run -d --name nginx4 nginx:alpine
  > docker container run -d --name nginx5 nginx:alpine
  > docker container exec -it nginx4 ping nginx5 (no reach, because default bridge network no build-in DNS)
```
Using Containers to test different Linux CLI
```
  > docker container run --rm -it centos:7 bash (--rm, mean delete container when exit)
  > curl version (in container now)
  > docker container run --rm -it ubuntu:14.04 bash (--rm, mean delete container when exit)
  > curl version (in container now)
```
DNS Round Robin Test, use --network-alias
```
  > docker network create dude
  > docker container run -d --network dude --network-alias web httpd
  > docker container run -d --network dude --network-alias web nginx
  > docker container ls (see 2 web server run on 80 port)
  > docker container run --rm --network dude alpine nslookup web (Address 1: 172.19.0.2 web.dude, Address 2: 172.19.0.3 web.dude)
  > docker container run --rm --network dude alpine ping web (run many times, and response 172.19.0.2 or 172.19.0.3 randomly)
```
Using Docker Hub Registry Images
```
  > docker pull nginx (image name)
  > docker pull nginx:1.11.9 (image name and version)
  > docker image ls
```
Images and Their Layers
```
  > docker image history nginx (nginx image build layers)
  > docker image history mysql (mysql image build layers)
  > docker image inspect nginx 
```
![alt text](https://github.com/smalltide/docker-mastery/blob/master/img/image-layer.png "image-layer")

Image Tagging and Pushing to Docker Hub
```
  > docker image tag <source image:tag> <target image:tag>
  > docker image tag alpine smalltides/alpine
  > cat ~/.docker/config.json
  > docker login
  > docker image push smalltides/alpine (push to dockerhub)
  > docker image tag smalltides/alpine smalltides/alpine:testing
  > docker image ls
  > docker image push smalltides/alpine:testing
```
Building Images: The Dockerfile
```
  > docker build -t <new image name> .
  > docker build -t <new image name> -f <Dockerfile Name> 
  > FROM, ENV, RUN, EXPOSE, COPY, CMD (some Dockfile command)
```
Building Images: Extending Official Nginx Images
```
  > docker container run -p 80:80 --rm nginx
  > curl 127.0.0.1
  > cd dockerfile-sample-2
  > docker build -t nginx-with-html .
  > docker container run -p 80:80 --rm nginx-with-html
  > curl 127.0.0.1 (watch the response html)
  > docker image tag nginx-with-html smalltides/nginx-with-html:latest
  > docker image push smalltides/nginx-with-html:latest
```
Build Your Own Dockerfile and Run Containers From It
```
  > cd dockerfile-assignment-1
  > docker build -t testnode .
  > docker container run --rm -p 80:3000 testnode
  > curl 127.0.0.1 (see Node.js Express App on page)
  > docker image tag testnode smalltides/testnode
  > docker image push smalltides/testnode
  > docker image rm smalltides/testnode
  > docker container run --rm -p 80:3000 smalltides/testnode (download from docker hub)
  > curl 127.0.0.1 (see Node.js Express App on page)
  > https://github.com/BretFisher/node-docker-good-defaults (docker node dev template)
```
Container Lifetime & Persistent Data
```
  > https://12factor.net/zh_cn/
  > https://medium.com/@kelseyhightower/12-fractured-apps-1080c73d481c
```
Persistent Data: Data Volumes
```
  > docker pull mysql
  > docker image inspect mysql
  > docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql
  > docker container inspect mysql (see volume)
  > docker volume list
  > docker volume inspect <volume_id>
  > docker container run -d --name mysql2 -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql (named volume)
  > docker volume list (see a volume named mysql-db)
  > docker container inspect mysql2
  > docker volume inspect mysql-db
  > docker container run -d --name mysql3 -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql (container mysql3 use mysql-db volume)
  > docker volume create --help
```
Persistent Data: Bind host Mounting in container
```
  > cd dockerfile-sample-2
  > docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx (mount host file in container)
  > docker container inspect nginx
```
Database Upgrades with Named Volumes
```
  > docker container run -d --name postgres -v psql-data:/var/lib/postgresql/data postgres:9.6.1 (named volume)
  > docker volume list (see psql-data volume)
  > docker volume inspect psql-data
  > docker container inspect postgres
  > docker container logs postgres
  > docker container stop postgres
  > docker container rm postgres
  > docker container run -d --name postgres -v psql-data:/var/lib/postgresql/data postgres:9.6.2
  > docker volume list (see psql-data volume)
  > docker container logs postgres
```
Edit Code Running In Containers With Bind Mounts
```
  > https://jekyllrb.com
  > cd bindmount-sample-1
  > docker container run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve ( $(pwd) for get current folder to binding into container) 
  > curl 127.0.0.1 (if edit the code of bindmount-sample-1, will see the instant change)
```
Docker Compose: The Multi-Container Tool
```
  > https://docs.docker.com/compose/compose-file/compose-versioning
  > https://github.com/docker/compose/releases
  > curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose chmod +x /usr/local/bin/docker-compose (install on Linux)
```
Basic Compose Commands
```
  > cd compose-sample-2
  > docker-compose up
  > curl 127.0.0.1 (nginx 80 port in and then proxy to httpd 80 port)
  > docker-compose logs
  > docker-compose ps
  > docker-compose top
  > docker-compose down
```
Build a Compose File For a Multi-Container Service, drupal and postgres
```
  > cd compose-assignment-1
  > docker-compose up
  > docker volume ls
  > docker-compose down (down and not remove volume)
  > docker-compose down -v (down and remove volume)
```
Adding Image Building to Compose Files
```
  > https://docs.docker.com/compose/compose-file/#build
  > cd compose-sample-3
  > docker-compose up
  > docker volume ls
  > docker-compose ps
  > docker-compose top
  > docker-compose down (down and not remove volume)
  > docker-compose down -v (down and remove volume)
```
Compose For Run-Time Image Building and Multi-Container Dev and Testing
```
  > cd compose-assignment-2
  > docker-compose up
  > docker volume ls
  > docker volume inspect composeassignment2_drupal-data
  > docker-compose ps
  > docker-compose top
  > docker-compose down (down and not remove volume)
  > docker-compose up (because don't delete volume, some the previous data is keep)
  > docker-compose down -v (down and remove volume)
```
Swarm Mode: Built-In Orchestration
```
  > https://www.youtube.com/watch?v=dooPhkXT9yI
  > https://www.youtube.com/watch?v=_F6PSP-qhdA
  > https://speakerdeck.com/aluzzardi/heart-of-the-swarmkit-topology-management
  > https://www.youtube.com/watch?v=EmePhjGnCXY
  > http://thesecretlivesofdata.com/raft/
```
![alt text](https://github.com/smalltide/docker-mastery/blob/master/img/swarm-cluster.png "swarm-cluster")
![alt text](https://github.com/smalltide/docker-mastery/blob/master/img/manager-worker.png "manager-worker")
![alt text](https://github.com/smalltide/docker-mastery/blob/master/img/docker-service.png "docker-service")


```
```



