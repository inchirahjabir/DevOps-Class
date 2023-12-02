# Docker Containers

This lab focuses on installing Docker, creating a Dockerfile and building an image, running a Docker container with various options, sharing the container, and finally, building and running a multi-container application using Docker Compose.

## Functionality

1. Docker installation
2. Write a Dockerfile and build a Docker image
3. Run a Docker container with multiple options
4. Share your Docker container with a classmate
5. Build and run a multiple container app with Docker Compose

## Installation

1. [Install Docker](https://www.docker.com/get-started)

2. Make sure the docker installation is working properly by running:
   ```
   docker run hello-world
   ```

## Write a Dockerfile and build a Docker image

1. We open [`lab/hello-world-docker`](lab/hello-world-docker) directory and check out the `server.js`, `package.json` and `Dockerfile` files using: 
    ```
    cat server.js
    cat package.json
    cat Dockerfile
    ```
2. We build the docker container using: 
    ```
    docker build -t hello-world-docker .
    ```

3. We check if your Docker container appears in the local Docker images:
    ```
    docker images
    ```

    Results:
    ```
    REPOSITORY                              TAG                IMAGE ID       CREATED         SIZE
    hello-world-docker                      latest             2092982d1192   2 minutes ago   922MB
    ``` 

## Run a Docker container with multiple options

1. We run the container with the following command:   
   ```
   docker run -p 12345:8080 -d hello-world-docker
   ```

2. We check if the container is running with the following command:
   ```
   docker ps
   ```

   Results
   ```
   b2129951c5ef   inchirah-jabir-docker                             "docker-entrypoint.s…"   5 weeks ago   Up 5 weeks             0.0.0.0:12345->8080/tcp                            vibrant_brattain
   ```

3. We open your web browser and go to `http://localhost:12345`

    Results: 
    The page prints Inchirah Jabir!

4. We print the logs of the container with:
   ```
   docker logs b2129951c5ef
   ```

   Results: 
   ```
   Inchirahs-MBP:hello-world-docker Inchirah$ docker logs b2129951c5ef
   > hello_world_docker@1.0.0 start /usr/src/app
   > node server.js
   Running on http://localhost:8080
   ```

3. We stop the container with:
   ```
   docker stop b2129951c5ef
   ```

## 4. Share your Docker container with a classmate

1. We modify the message printed in the `server.js` to `Inchirah Jabir!`

2. We rebuild the Docker container with the name inchirah-jabir-docker using this modified code. 

3. We run it and navigate to the web app in our browser which will print `Inchirah Jabir!`

3. We register on [Docker Hub](https://hub.docker.com/)

4. We tag our container with the following command:
   ```
   docker tag hello-world-docker inchirahj/inchirah-jabir-docker
   ```
   
5. We log in to Docker Hub from our terminal:
   ```
   docker login
   ```
6. We push the docker image to Docker Hub:
   ```
   docker push inchirahj/inchirah-jabir-docker
   ```
7. We can now find the image in our [repositories](https://hub.docker.com/repositories) in the Docker Hub

8. We ask a classmate to retrieve our Docker container and run it:
   ```
   docker pull inchirahj/inchirah-jabir-docker
   docker run -p 12345:8080 -d inchirahj/inchirah-jabir-docker

## 5. Build and run a multiple container app with Docker Compose

1. We navigate to the [`lab/hello-world-docker-compose`](lab/hello-world-docker-compose) directory and check out the `dbClient.js`, `server.js`, `package.json` and `Dockerfile` files using: 
    ```
    cat dbClient.js
    cat server.js
    cat package.json
    cat Dockerfile
    ```

3. We build the Docker image inside this directory with the name `hello-world-docker-compose` using:
    ```
    docker build -t hello-world-docker-compose .
    ```

4. We fill the missing part of the `docker-compose.yaml` file to make it use the container we just built: 

5. We start the containers with `docker-compose up`

6. We visit `localhost:5000` in your web browser and hit refresh a couple of times

Results: 
The message `Hello World from Docker! I have been seen 1 times` is displayed. When we hit refresh, we see the message `Hello World from Docker! I have been seen 2 times`. The number of times keeps increasing the more we hit refresh. 

7. We stop the containers by running `CTRL+C` in the previous terminal

8. We delete the containers with:
   ```
   docker-compose rm
   ```

9. Start the containers again   
   1. What happened to the counter? Why?
   
   The counter reset as it is the default behavior of Redis. 

   2. We delete the containers again using:
   ```
   docker-compose rm
   ```

10. We make the necessary changes in the Docker compose file so that when we delete and create the containers again the counter keeps its value by adding:

```
   volumes:
    - db-data:/data
```
