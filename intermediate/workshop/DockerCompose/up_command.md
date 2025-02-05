# Lab #8: up Command
The `docker-compose up` command help you to bring up a multi-container application, which you have described in your docker-compose file.

## Pre-requisite:

## Tested Infrastructure

<table class="tg">
  <tr>
    <th class="tg-yw4l"><b>Platform</b></th>
    <th class="tg-yw4l"><b>Number of Instance</b></th>
    <th class="tg-yw4l"><b>Reading Time</b></th>
    
  </tr>
  <tr>
    <td class="tg-yw4l"><b> Play with Docker</b></td>
    <td class="tg-yw4l"><b>1</b></td>
    <td class="tg-yw4l"><b>5 min</b></td>
    
  </tr>
  
</table>

## Pre-requisite

- Create an account with [DockerHub](https://hub.docker.com)
- Open [PWD](https://labs.play-with-docker.com/) Platform on your browser 
- Click on **Add New Instance** on the left side of the screen to bring up Alpine OS instance on the right side

# Assignment
- Create a docker-compose.yml with custom image
- Bringup the containers 

### Create a docker-compose.yml with custom image

#### Setup environment
```
$ mkdir -p docker-compose/build
$ cd docker-compose/build
```

#### Now lets create the Dockerfile
```
FROM nginx:alpine
RUN echo "Welcome to Docker Workshop!" >/usr/share/nginx/html/index.html
CMD ["nginx", "-g", "daemon off;"]
```

#### Create a docker-compose.yml file
```
version: "3.7"
services:
  webserver:
    build:
      context: .
      dockerfile: Dockerfile
    image: webapp:v1
    ports:
      - "80:80"
  dbserver:
     image: mysql:5.7
     container_name: Mysqldb
     restart: unless-stopped
     ports:
       - "3306:3306"
     environment:
       MYSQL_ROOT_PASSWORD: Pa$$w0rd
       MYSQL_USER: test
       MYSQL_PASSWORD: Pa$$w0rd123
       MYSQL_DATABASE: test 
     volumes:
       - db_data:/var/lib/mysql
volumes:
  db_data:
```

### Bringup the containers
```
$ docker-compose up -d
```
NOTE: This will build the images and bringup the containers. <b>-d</b> option which will run the container in background.

#### Checking webserver respone
```
$ curl http://localhost
Welcome to Docker Workshop!
```

#### Checking dbserver respone
```
$ docker exec -it Mysqldb mysql -u root -p
Enter password: 
```

#### Rebuild the docker image and bring the stack up
```
$ docker-compose up -d --build
```

## Contributor
[Savio Mathew](https://www.linkedin.com/in/saviovettoor)

Next » [Lab #9: ps Command](http://dockerlabs.collabnix.com/intermediate/workshop/DockerCompose/)
