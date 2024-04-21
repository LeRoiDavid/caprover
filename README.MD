# Install caprover localy

## Install docker in docker

1. Run docker caprover-docker container

````md
```sh
docker run --privileged -d -p 80:80 -p 443:443 -p 3200:3000 --name caprover-docker docker:dind
```
````

2. Enter into the container

````md
```sh
docker exec -ti caprover-docker /bin/sh
```
````

3. Create a folder inside the container /captain/data

````md
```sh
mkdir -p /captain/data
```
````

4. Run caprover inside docker in docker container

````md
```sh
docker run -e ACCEPTED_TERMS=true -e MAIN_NODE_IP_ADDRESS=127.0.0.1 -p 80:80 -p 443:443 -p 3000:3000 -v /var/run/docker.sock:/var/run/docker.sock -v /captain:/captain caprover/caprover
```
````

## Deploy an application localy (nextjs app)

5. Edit the /etc/host file and add the dns resolution for caprover

````md
```sh
127.0.0.1       captain.localhost
127.0.0.1       captain.captain.localhost
```
````

6. Login to caprover with command

````md
```sh
caprover login -u "http://captain.captain.localhost" -p "captain42"
```
````

7. Go to caprover on browser and connect, create a new app

8. Go to the new next application and create captain-definition file

````md
```sh
{
  "schemaVersion": 2,
  "dockerfileLines": [
    "FROM node:18-alpine",
    "RUN mkdir -p /usr/src/app",
    "WORKDIR /usr/src/app",
    "COPY ./package.json /usr/src/app/",
    "RUN npm install && npm cache clean --force",
    "COPY ./ /usr/src/app",
    "ENV NODE_ENV develop",
    "ENV PORT 80",
    "EXPOSE 80",
    "CMD [ \"npm\", \"start\" ]"
  ]
}
```
````

9. Go to the terminal and deploy the app

````md
```sh
caprover deploy
```
````
