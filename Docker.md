## Docker
            Docker is a container in use to share the full environment with the code in a single package
            1. Docker in this there are a Docker file, Image, Docker container.

### Image
 - I can onec a creat an image with the help of a Docker file or  I can pull the predefine images from the DockerHub
 - How to pull the existing image
 - The flow is like First to build the docker file after that we get a image and after running the image we get the docker container
             docker ps
`docker ps`
- to see the all running container in the terminal  
           
```
docker pull hello-world
docker images
docker run hello-world
ex. 


docker run -d -e MYSQL_ROOT_PASSWORD= root mysql
```

