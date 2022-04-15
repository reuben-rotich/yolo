# Instructions

1. Clone this repository ` git clone https://github.com/Ammly/yolo.git `
2. Change in to yolo directory ` cd yolo `
3. Run docker compose ` docker-compose up --build `
    - You can pass ` -d ` to the above command if you want to run in detached mode

If you need to go inside a container run the following command
    ` docker exec -it <container id> /bin/bash `
- Run ` docker ps ` to get the container ID

To view container logs
    ` docker logs <container id> `


# Explanation

## Base image used

` node:14-alpine ` - Alpine image is fast and very small in size

## Dockerfile directives

` FROM node:14-alpine ` -  Alpine base image to build from

` WORKDIR /usr/src/(client | backend) ` - A working directory to hold application code

` COPY package*.json ./ ` - Copy ` package.json ` and ` package-lock.json ` files containing project dependencies to the working directory inside the docker image

` RUN npm install ` - Install project dependencies

` COPY . . ` -  Copy project source code to the working directory inside the docker image

` RUN npm run build ` - Compile project for production

` EXPOSE 3000 ` - Expose port 3000 for client and 5000 for backend

` CMD ["node", "server.js"] ` or ` CMD [ "serve", "-s", "build", "-l", "3000" ] ` - Start the server

## Docker-compose

This file (docker-compose.yml) creates three containers
- ` yolomy_client ` - contains the yolomy client service which runs on port `3000 `
- ` yolomy_backend ` - contains yolomy backend service which runs on port ` 5000 `
- ` mongo ` - contains mongo db which runs on port ` 27017 ` and has an attached volume called ` dbdata `

Networks - All containers are in ` yolomy ` bridge network

## Docker Hub

[Yolo Client](https://hub.docker.com/r/ammlyf/yolo_client)

[Yolo Backend](https://hub.docker.com/r/ammlyf/yolo_backend)