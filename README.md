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

Some common questions I have until now:
 * Is the docker hub similar to Kubernetes?
    * Docker Hub is a cloud-based registry service provided by Docker that allows users to store and distribute container images.  
    *  Kubernetes is an open-source container orchestration platform. It provides a framework for automating the deployment, scaling, and management of containerized applications.  

* How to write a dockerfile for Python project?
      ```
       # Use an official Python runtime as the base image
       FROM python:3.9
       
       # Set the working directory in the container
       WORKDIR /app
       
       # Copy the requirements.txt file to the container
       COPY requirements.txt.
       
       # Install the project dependencies (no cache ensures, pip avoids using any cached packages while installing)
       RUN pip install --no-cache-dir -r requirements.txt
       
       # Copy the rest of the project files to the container
       COPY . .
       
       # Specify the command to run when the container starts
       CMD [ "python", "your_script.py" ]
      ```
  
      Now, the requirements.txt file should look like this,
      ```
      Flask==2.0.1
      numpy==1.21.0
      pandas==1.3.0
      ```
  

      Build and run the docker image by saying:
     ```
     docker build -t your_image_name .
     docker run your_image_name
     ```

 * Is the version number required to specify in requirements.txt?
    * You can specify without a version number, docker by default installs the latest version if not specified, but it is highly recommended to mention the specific version you used for the project.
  
 * What If you forgot to mention any packages in the requirements.txt?
     * If you forget to include a required package in the requirements.txt file, the container may not be able to run properly after creating the image. As a result, if your application relies on a package that is not listed in the requirements.txt file, running the container may lead to errors or unexpected behavior. If you realize that you have forgotten to include a package in the requirements.txt file, you will need to modify the file, add the missing package, and rebuild the Docker image to include the updated dependencies.

 * Should I copy all the project files? why is it required to copy all the files in the docker? it only requires dependencies to be installed in the container?
      * it is recommended to copy all the porject files to the docker image. you can just specify
        ```
        COPY . .
        ```
        This will make sure to copy all the files in the working directory to copy. you can also copy files that are only rewuired by:
        ```
        COPY main.py
        # or
        COPY data/ data/ 
        ```
        In this case, the main.py file will be copied to the root of the /app directory within the container, and the data directory (along with its contents) will be copied to the /app/data directory within the container.

      * including other files makes it easy for accessing and changing the code for debugging purposes by other developers. YES, others can view the code from the docker image and are able to make changes within the container.
      * Including these additional files in the Docker image ensures that your application has all the required dependencies and resources in a self-contained environment.
      * To access the files from Image:  
        ```
        docker exec -it <container_id> /bin/bash

        # This command opens an interactive shell session within the running container.
        # They can navigate to the project directory (the working directory specified in the Dockerfile, e.g., /app) and view, edit, or execute the project files as needed.
        ```  
        ```
        # you can execute python script inside the running container by:
        
        docker exec <container_id> python /path/to/script.py
        ```

      

      
        
        
   

  
    
    
                       
