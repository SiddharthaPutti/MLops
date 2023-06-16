# MLops
MLOPS basics and pipelines 

**Docker**:  

A simple clarification, docker is used only to package different version dependencies, like python 3.9, sklearn v2.8 for example. it doesn't package the entire codebase. so when you run a docker image, the container is initialized to create a different environment for the project you need to run. so that, the developer who built this project can share the dependencies in a package with the rest of the team where they can reproduce the results. 

**Image**: a way to package applications and their dependencies. this makes sure every developer has the same set of packages and their versions. This Image can run on any machine(Linux, windows, etc) by installing the packaged versions of dependencies(ex: python version, TensorFlow version).  

**Container**: The running instance of an image is called a container. This can also be defined as the process or an isolated environment that is running on a host operating system. When you start a container from an image, Docker creates a writable layer on top of the image called the container layer. This layer allows the container to have its own filesystem and make changes to it, such as writing data, installing additional packages, or modifying configurations.  

  * basic commands in VScode
    * docker info
    * docker version
    * docker login  
   
    * docker pull [image name]- pull an image from a registry
    * docker run [image name] - run a container
    * docker run -d [image name] - run in detached mode(runs in background, this gives you terminal access)
    * docker start [container name] - start the stopped container
    * docker ps - list running containers currently running
    * docker ps -a - list running and stopped containers
    * docker stop [container name] - stop the container but still be in memory 
    * docker kill [container name] - kill the container that is still in memory (usually we don't use it)
    * docker image inspect [image name] - get image info, useful for debugging purposes
      
  * set memory allocations:
    * docker run --memory="256m" nginx , that is max memory (here nginx is image name)
    * docker run --cpus=.5" nginx , Max CPU

  * how to run an image?
    ```
    # pull and run docker image  
                                        
    docker run --publish 80:80 --name webserver nginx
    ```
    
    * (nginx is container image in docker registry)  
    * (webserver is container local name)  
    * (with the publish command you map the host port to the container listening port)

    ```
    docker container exec -it webserver bash
    ```

    * When you run this command, Docker will locate the running container named "webserver" and start an interactive            * session within it, providing you with a shell prompt. You can then execute commands and interact with the container's 
    * file system, processes, and other resources as if you were directly logged into the container's operating system.  
    

  * clean up
    ```
    docker rm [container name] # container needs to be in stop state, but still in memory
    docker rm $(docker ps -a -q) # list all the stopped containers and remove them all  
    docker images # list all the images  
    docker rmi [image name] # deletes the image  
    docker system prune -a # removes all the images that are not in use(careful while using this command)  
    
    ```

  
    
    
                       
