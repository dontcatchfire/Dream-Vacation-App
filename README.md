# Stage 4 Task

This task involves dockerizing an application with a React frontend and a Nodejs backend.

The frontend server of choice is Nginx.

I used a Postgres:16-alpine docker image for the database container.


I wrote a multi-stage build Dockerfile for dockerizing the frontend service. A multi-stage build approach is beneficial for reducing the size of frontend docker image because it is client-facing. In other words, it is the part of the application users interact with the most. A heavy frontend leads to latency and poor user experience.

### docker-compose up

![](</screenshots/docker compose up.png>)


### docker containers running
![](</screenshots/containers running.png>)


### frontend service is being served at port 8080

![](</screenshots/frontend.png>)


### backend service is being served at port 3001

![](</screenshots/backend.png>)
