# [![My Skills](https://simpleskill.icons.workers.dev/svg?i=docker)](https://www.docker.com/) Dockerize-Django-Application [![My Skills](https://skillicons.dev/icons?i=django)](https://www.djangoproject.com/)

Containerization is just like cooking 🧑‍🍳 - You gather all ingredients and put in the container . Before cooking , there were in distinct shape , size , color but after cooking its a whole meal 🥗. Just like that , You create different services for your application , precisely **Full Stack Application** , and then you put those services in a docker container - it may be databases , backend servers , frontend server , cache servers , queueing systems etc. Each of them has their own host & port to show them to you but within docker , they are containerized , more definitely - **multi-containerized** (because unlike cooking , if you put all stuffs in a single container , the size of it will be so big that your puny SSDs may not handle it and forget those servers to even run quickly 😄)

But how those containers communicate with each other - if we consider cooking , ingredients got assembled in a large pot over , but here we have **Networks** 🚠.
By defeault when you are locally building the project with docker , the containers communicate and run within **default** network of type **bridge**. But that **may not work** , if you pull your docker project from docker hub and directly build and run that shit !

This tutorial cheatsheet is all about dockerizing a working django backend who throws REST APIS like spiderman's web to you and your frontend 🕸 . **It doesn't tell you anything about django but mostly about the hosting process in docker .**

For those who know nothing but nuts in docker :suspect: , follow this tutorial - [link](https://docker-curriculum.com/)

## Dockerfile vs docker-compose.yml (or YAML) 👦 😎

### Dockerfile and Docker Image
Dockerfile is the primary execution-point where you write docker commands. Among which some are the command to run the servers and install the dependencies and others are commands to store the project in a certain location in the docker conatiner hosted in Linux OS (want to know more about basics , go the link above). 

But how docker runs a project ? Just by creating Images encrypted by SHA algorithms . Images ? yeah , just like git does (if you have used it) , docker captures the snapshot or precisely **current version** of the project (where each files and folder has their own status) with a unique alpha-numeric code generated by SHA encryption. You pull that image , build it and run it . Whenever you run an image , a container is assigned to that image and the *unique image-code* tells which project has to be containerized .
That's how an image can be managed by multiple containers and that's why you have to manage containers , to keep your remaining space in SSD , SAFE !!

So for any static site (site that don't need databases or other services to interact with) , if you want to deploy the project into **Docker Hub** (A place like Github , where you push and pull your docker images) ,  you just need the dockerfile and you are good to go .

But what happens when the server has a database connection ? then you need to create more than one services , for that you need - docker.compose.yml

The General Structure of a dockerfile for a django-project is -
```bash
     #  This image is taken from docker-hub
     FROM python:version-nickname

    # To set some ENV variables directly in dockerfile
    # Tells to show standard errors in logfile 
    ENV PYTHONUNBUFFERED=1
    
    # Tells python not to write .pyc files
    ENV PYTHONDONTWRITEBYTECODE=1

    # The place inside the container , where the project will be saved (you can change it)
    WORKDIR /app
    
    # Copy all files and folders from local directory to /app directory of the container
    COPY . /app/
    
    # Copy the requirements.txt from local directory to /app directory of the container
    COPY ./requirements.txt .
    
    # Install all dependencies
    RUN pip install -r requirements.txt

    # A startup scripti with all startup commands is good to have , CMD allows you to run such commands every time"
    CMD ["sh", "./start.sh"]
```

### docker-compose.yml
This file has an .yml extension which means , it follows YAML syntaxes . Here , like the base image in dockerfile , you create some independent services (for this context , the database is the one) and some other services depends on the independent ones (the backend server is the one dependent service here) , and that's how you made up things to run. This is an additional file, made for dynamic apps. 

The General Structure of a dockerfile for a django-project is -

```bash
    
```

## Commands 
```
- Build Dockerfile
- Build docker-compose.yml
- docker-compose -up -- build (docker compose build && docker compose up)
- docker login
- docker build -t <default - latest> username/repo-name
- docker push username/repo-name
- docker ps -a OR docker container ls  # all container
- docker image ls OR docker images -a  # all images
- docker network ls    # all networks
- docker network create my-network  # create a new network
- docker run -d --name farmers-app --network farmers-network \
  -e DB_HOST=farmers-db \
  -e DB_USER=your_db_user \
  -e DB_PASSWORD=your_db_password \
  -e DB_NAME=your_db_name \
  -p your_django_port:your_django_port \
  parthib23/farmers-api:latest
- docker run -d --name farmers-db --network farmers-network \
  -e POSTGRES_USER=your_db_user \
  -e POSTGRES_PASSWORD=your_db_password \
  -e POSTGRES_DB=your_db_name \
  -p 5432:5432 \
  postgres:17.4-bookworm
