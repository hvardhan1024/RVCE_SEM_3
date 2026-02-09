# DevOps Study Notes - 110 Questions & Answers

## Important Diagrams

### Diagram 1: Kubernetes Architecture

![Pasted image 20260209202415.png](attachments/Pasted%20image%2020260209202415.png)
(k8s architecture)

### Diagram 2

![Pasted image 20260209202502.png](attachments/Pasted%20image%2020260209202502.png)

### Diagram 3

![Pasted image 20260209202508.png](attachments/Pasted%20image%2020260209202508.png)

### Diagram 4

![Pasted image 20260209202513.png](attachments/Pasted%20image%2020260209202513.png)

### Diagram 5

![Pasted image 20260209202535.png](attachments/Pasted%20image%2020260209202535.png)

### Diagram 6

![Pasted image 20260209202547.png](attachments/Pasted%20image%2020260209202547.png)

---

## **📌 Diagrams Ended - Questions Start**

---

## Question 1: Summarize the concept of Docker and discuss two reasons for its preference over traditional virtualization

### **Definition** (2 Marks)

Docker is an open-source containerization platform that enables developers to package applications along with their dependencies into lightweight, portable containers that can run consistently across different computing environments.

### **Reasons for Preference** (3 Marks)

1. **Lightweight Architecture**  
   Docker containers share the host OS kernel, unlike virtual machines that require a full guest OS, resulting in reduced resource consumption.

2. **Faster Deployment**  
   Containers start in seconds, enabling rapid application deployment and scaling compared to traditional virtualization.

### **Solution Scheme**

- Definition: 2M
- Reasons: 3M

---

## Question 2: List the key benefits of adopting a DevOps culture

### **Answer** (Any five benefits – 1M each)

1. **Faster Delivery**  
   Continuous integration and automation accelerate software release cycles.

2. **Improved Collaboration**  
   Enhances coordination between development and operations teams.

3. **Higher Reliability**  
   Automated testing and monitoring reduce production failures.

4. **Better Scalability**  
   Infrastructure as code supports easy scaling of applications.

5. **Continuous Improvement**  
   Feedback loops help in quick identification and resolution of issues.

### **Solution Scheme**

Any five benefits: 1M each

---

## Question 3: Record the dissimilarities of life before and after Docker

### **Answer** (Diagram – 3M, Explanation - Before - 1M, After - 1M)

![Pasted image 20260209194710.png](attachments/Pasted%20image%2020260209194710.png)

### **Explanation**

**Before Docker:**  
Applications faced dependency conflicts, inconsistent environments across development and production, and complex deployment processes.

**After Docker:**  
Containerization provides consistent environments, simplified deployment, better resource utilization, and improved portability across different systems.

### **Solution Scheme**

- Diagram: 3M
- Explanation - Before: 1M
- Explanation - After: 1M

---

## Question 4: List Docker and mention two major advantages of using it

### **Definition** (2 Marks)

Docker is a container-based platform that allows applications to be developed, shipped, and run in isolated environments called containers.

### **Advantages** (3 Marks)

1. **Portability**  
   Docker containers run consistently across development, testing, and production systems.

2. **Efficient Resource Utilization**  
   Multiple containers run on the same host without the overhead of multiple operating systems.

### **Solution Scheme**

- Definition: 2M
- Advantages: 3M

---

## Question 5: Visualize the DevOps movement on the three axes

### **3 Axes** (3 Marks)

1. **The culture of collaboration**
2. **Process**
3. **Tools**

### **Diagram** (2 Marks)

![Pasted image 20260209194750.png](attachments/Pasted%20image%2020260209194750.png)

### **Solution Scheme**

- 3 Axes: 3M
- Diagram: 2M

---

## Question 6: Infer short notes on Docker Hub

### **Purpose** (2 Marks)

Docker Hub is a cloud-based registry service used to store, share, and manage Docker images.

### **Usage / Example** (3 Marks)

- Developers can pull official and community images
- Supports versioning and automated builds
- **Example:** Pulling an Nginx image using `docker pull nginx`

### **Solution Scheme**

- Purpose: 2M
- Usage/Example: 3M

---

## Question 7: Determine the procedure to expose the Docker Daemon to external access on an Ubuntu system

### **The Docker Daemon Technique**

#### **For Ubuntu Users**

**Step 1:** Check the status of Docker

```bash
sudo systemctl status docker
```

**Step 2:** Open the systemd file of Docker

```bash
sudo nano /lib/systemd/system/docker.service
```

**Step 3:** Comment the containerd.sock code under service

```
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

**Step 4:** Add the code to access Docker Daemon

```
ExecStart=/usr/bin/dockerd -H=fd:// -H=tcp://0.0.0.0:2375
```

**Step 5:** Reload the daemon & Restart Docker

```bash
sudo systemctl daemon-reload ; systemctl restart docker
```

**Step 6:** Check the IP address of the system

```bash
sudo ifconfig
```

**Step 7:** Open the Web Browser and add port number - 2375 with /images/json

```
http://<IP_ADDRESS>:2375/images/json
```

**Step 8:** Connect another device using the same hotspot and check the IP address of WiFi with port number - 2375/images/json

### **Solution Scheme**

Steps: 5 Marks

---

## Question 8: Classify the difference between Docker images and Docker containers

### **Answer** (Any three valid differences)

| **Docker Images**         | **Docker Containers**       |
| ------------------------- | --------------------------- |
| Read-only templates       | Running instances of images |
| Used to create containers | Created from images         |
| Stored in registries      | Exist in runtime memory     |
| Static                    | Dynamic                     |
| Cannot execute directly   | Executes application        |

### **Solution Scheme**

Any 3 valid differences: 5M total

---

## Question 9: Articulate the role of Docker Client on using socat to Monitor Docker API Traffic

### **Docker Client**

To monitor traffic using SOCAT:

**Step 1:** Run socat using

```bash
sudo socat -v UNIX-LISTEN:/tmp/dockerapi.sock,fork UNIX-CONNECT:/var/run/docker.sock &
```

**Step 2:** To see the request and response

```bash
sudo docker -H unix:///tmp/dockerapi.sock ps -a
```

### **Solution Scheme**

- Explanation of Docker Client: 2 Marks
- Explanation of SOCAT: 1 Mark
- Steps to Monitor Traffic: 3 Marks

---

## Question 10: Demonstrate the procedure to deploy and activate a local Docker Registry

### **Answer**

A local Docker Registry enables storage and distribution of Docker images within a private environment, reducing dependency on public repositories and improving deployment speed.

### **Deployment Steps**

**Step 1:** Create and make the registry available

```bash
docker run -d -p 5000:5000 -v $HOME/registry:/var/lib/registry registry:2
```

**Application details:**

- `-d` runs the registry in detached mode
- `-p 5000:5000` exposes the registry service on port 5000
- `-v $HOME/registry:/var/lib/registry` ensures persistent storage of images
- `registry:2` specifies the official Docker Registry image

Once executed, the local registry becomes operational and ready to store, push, and pull Docker images using the configured endpoint.

### **Solution Scheme**

- Purpose and role of a local Docker Registry: 1 Mark
- Correct use of Docker command to launch registry: 2 Marks
- Configuration aspects (port mapping, volume binding): 1 Mark
- Successful availability of registry service: 1 Mark

---

## Question 11: List the three axes on which the DevOps movement is based and list the benefits

### **DevOps Movement - Three Axes**

1. **The culture of collaboration**
2. **Processes**
   - A. Planning and prioritizing functionalities
   - B. Development
   - C. Continuous integration and delivery
   - D. Continuous deployment
   - E. Continuous monitoring
3. **Tools**

### **Benefits**

1. **Better collaboration and communication** in teams, which has a human and social impact within the company

2. **Shorter lead times to production**, resulting in better performance and end user satisfaction

3. **Reduced infrastructure costs** with IaC (Infrastructure as Code)

4. **Significant time saved** with iterative cycles that reduce application errors and automation tools that reduce manual tasks, so teams focus more on developing new functionalities with added business value

### **Diagram**

![Pasted image 20260209194924.png](attachments/Pasted%20image%2020260209194924.png)

### **Solution Scheme**

- Three Axes: 4M
- Benefits: 4M
- Diagram: 2M

---

## Question 12: Analyze Docker's architecture by examining the interaction among its core components

### **Docker Architecture Overview**

**Core Components:**

- Docker Engine
- Images
- Containers
- Registries

### **Diagram**

![Pasted image 20260209194947.png](attachments/Pasted%20image%2020260209194947.png)

### **Solution Scheme**

- Identification of core architectural components (Docker Client, Docker Daemon, Docker Host, Images, Containers, Registries): 3 Marks
- Interaction flow analysis (Client–Daemon communication, image flow, container execution): 3 Marks
- Architectural support for container lifecycle (Build, run, stop, manage containers): 2 Marks
- Diagram of Docker architecture (Neat, labeled, logical flow): 2 Marks

---

## Question 13: Assume the Docker is running on your host machine is split into two parts

### **Visual Representation**

A daemon with a RESTful API and a client that talks to the daemon.

![Pasted image 20260209195018.png](attachments/Pasted%20image%2020260209195018.png)

### **Solution Scheme**

- Diagram: 4M
- Explanation of each component: 6M

---

## Question 14: Summarize the step-wise process involved in building and running a simple Docker application

### **Steps**

1. **Create Dockerfile**
2. **Build image**
   ```bash
   docker build -t app .
   ```
3. **Run container**
   ```bash
   docker run -d app
   ```
4. **Verify**
   ```bash
   docker ps
   ```

### **Solution Scheme**

- Commands: 6 Marks
- Explanation: 4 Marks

---

## Question 15: Classify the method to use Docker in the web-browser using a technique with an illustration

### **Steps**

**Step 1:** Check the status of Docker

```bash
sudo systemctl status docker
```

**Step 2:** Open the systemd file of Docker

```bash
sudo nano /lib/systemd/system/docker.service
```

**Step 3:** Comment the containerd.sock code under service

```
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

**Step 4:** Add the code to access Docker Daemon

```
ExecStart=/usr/bin/dockerd -H=fd:// -H=tcp://0.0.0.0:2375
```

**Step 5:** Reload the daemon & Restart Docker

```bash
sudo systemctl daemon-reload ; systemctl restart docker
```

**Step 6:** Check the IP address of the system

```bash
sudo ifconfig
```

**Step 7:** Open the Web Browser and add port number - 2375 with /images/json

**Step 8:** Connect another device using the same hotspot and check the IP address of WiFi with port number - 2375/images/json

### **Illustration**

![Pasted image 20260209195046.png](attachments/Pasted%20image%2020260209195046.png)

### **Solution Scheme**

- Steps: 8 Marks
- Illustration: 2 Marks

---

## Question 16: Record the roles of Docker Daemon, Client, and Registries in Docker's functioning

### **Docker Daemon** (3 Marks)

Manages containers, images, networks, and volumes. It is the core component that handles all Docker operations.

### **Docker Client** (3 Marks)

Sends API requests to Docker Daemon using CLI commands. It is the primary interface for users to interact with Docker.

### **Docker Registry** (4 Marks)

Stores Docker images and enables image sharing (e.g., Docker Hub). Provides centralized image management and distribution.

### **Solution Scheme**

- Daemon: 3M
- Client: 3M
- Registry: 4M

---

## Question 17: Discuss how Docker supports CI/CD pipelines

### **CI Integration** (5 Marks)

- Ensures consistent build environments
- Integrates with Jenkins, GitHub Actions, Azure DevOps
- Automated image creation during CI

### **CD Automation** (5 Marks)

- Containers deployed automatically across environments
- Supports blue-green and rolling deployments
- Reduces deployment failures

### **Solution Scheme**

- CI Integration: 5M
- CD Automation: 5M

---

## Question 18: List the key process elements required to strengthen collaboration between Development and Operations teams

### **Key Elements**

1. **More frequent application deployments** with integration and continuous delivery (called CI/CD)

2. **The implementation and automation of unitary and integration tests**, with a process focused on behavior-driven design (BDD) or test-driven design (TDD)

3. **The implementation of a means of collecting feedback from users**

4. **Monitoring applications and infrastructure**

### **Solution Scheme**

- CI/CD: 3 Marks
- Automated testing (TDD/BDD): 3 Marks
- User feedback mechanisms: 3 Marks
- Monitoring of applications and infrastructure: 3 Marks

---

## Question 19: Recall the Docker architecture with a labeled diagram with its components

### **Labeled Diagram** (6 Marks)

User → Docker Client → Docker Daemon → Containers  
↓  
Registry

![Pasted image 20260209195134.png](attachments/Pasted%20image%2020260209195134.png)

### **Components Explanation** (8 Marks)

- **Client:** Interface for user commands
- **Daemon:** Background service managing Docker objects
- **Images:** Read-only templates
- **Containers:** Running instances
- **Registry:** Image storage
- **Networks:** Container connectivity
- **Volumes:** Persistent data storage

### **Interaction Flow** (6 Marks)

1. User issues command via client
2. Client sends REST API request
3. Daemon processes request
4. Images pulled from registry if needed
5. Container created and executed

### **RESTful API Diagram**

![Pasted image 20260209195153.png](attachments/Pasted%20image%2020260209195153.png)

### **Solution Scheme**

- Diagram: 3 Marks
- Components: 8 Marks
- Interaction flow: 6 Marks
- Diagram RESTful API: 3 Marks

---

## Question 20: Give the process of creating, managing, and deploying Docker applications

### **Build Phase** (6 Marks)

```bash
docker build -t webapp .
```

- Dockerfile defines build steps
- Image is generated

### **Manage Phase** (6 Marks)

```bash
docker ps
docker stop webapp
docker restart webapp
docker rm webapp
```

- Monitor, stop, restart, and remove containers

### **Deploy Phase** (8 Marks)

```bash
docker push webapp
docker pull webapp
docker run -d -p 80:80 webapp
```

- Push to registry
- Pull on production server
- Run container for deployment

### **Solution Scheme**

- Build: 6M
- Manage: 6M
- Deploy: 8M

---

## Question 21: List the procedures used by the Docker Daemon to enable browser-based access

### **1. Open your Docker daemon to the world** (10 Marks)

**Step 1:** Check the status of Docker

```bash
sudo systemctl status docker
```

**Step 2:** Open the systemd file of Docker

```bash
sudo nano /lib/systemd/system/docker.service
```

**Step 3:** Comment the containerd.sock code under service

```
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

**Step 4:** Add the code to access Docker Daemon

```
ExecStart=/usr/bin/dockerd -H=fd:// -H=tcp://0.0.0.0:2375
```

**Step 5:** Reload the daemon & Restart Docker

```bash
sudo systemctl daemon-reload ; systemctl restart docker
```

**Step 6:** Check the IP address of the system

```bash
sudo ifconfig
```

**Step 7:** Open the Web Browser and add port number - 2375 with /images/json

**Step 8:** Connect another device using the same hotspot and check the IP address of WiFi with port number - 2375/images/json

### **2. Running containers as daemons** (6 Marks)

**Step 1:** Create a new folder - secondfile

```bash
mkdir secondfile
```

**Step 2:** Move into the folder

```bash
cd secondfile
```

**Step 3:** Create a Dockerfile and add the code

```bash
sudo nano Dockerfile
```

```dockerfile
# To run a simple container
# Use an existing docker image as a base
FROM alpine

# Download and install a dependency
RUN apk add --update redis

# Communicate the process
CMD ["redis-server"]
```

**Step 4:** Build the Dockerfile

```bash
sudo docker build -t secondfile .
```

**Step 5:** Check the Images

```bash
sudo docker images
```

**Step 6:** Run the Image - secondfile

```bash
sudo docker run secondfile
```

**Step 7:** Observe the prompt being withheld by the process & close the connection - ctrl + c

**Step 8:** Re-run the Image in the detached mode

```bash
sudo docker run -d secondfile
```

**Step 9:** Observe the prompt exited

**Step 10:** Check the process state of docker

```bash
sudo docker ps
```

### **3. Moving Docker to a different partition** (4 Marks)

**Step 1:** Stop the Docker service

```bash
sudo systemctl stop docker.service
```

**Step 2:** Move the contents of Docker to a different location

```bash
sudo dockerd --data-root /home/deepika/mydocker
```

**Step 3:** Start the Docker service

```bash
sudo systemctl start docker.service
```

**Step 4:** Check the folder creation and contents by navigating into the directory

```bash
cd /home/deepika/mydocker
```

### **Solution Scheme**

1. Open your Docker daemon to the world: 10 Marks
2. Running containers as daemons: 6 Marks
3. Moving Docker to a different partition: 4 Marks

---

## Question 22: Articulate the necessity of life before and after Docker with the goodness of Docker

### **Diagrams**

![Pasted image 20260209195257.png](attachments/Pasted%20image%2020260209195257.png)

![Pasted image 20260209195312.png](attachments/Pasted%20image%2020260209195312.png)

### **Solution Scheme**

1. The What and Why of Docker: 10 Marks
2. Goodness of Docker: 10 Marks

---

## Question 23: Analyze the internal working of Docker

### **Understanding Docker**

1. **Docker's Architecture**
2. **The Docker Daemon**
   - a. Open your Docker daemon to the world
   - b. Running containers as daemons
   - c. Moving Docker to a different partition
3. **The Docker Client**
   - a. Using socat to Monitor Docker API Traffic
   - b. Using Docker in your Browser
   - c. Using Ports to Connect to Containers
   - d. Allowing Container Communication
   - e. Linking Containers for Port Isolation
4. **Docker Registries**
   - Setting Up a Local Docker Registry
5. **Docker Hub**
   - Finding and Running a Docker Image

### **Solution Scheme**

1. Docker's Architecture
2. The Docker Daemon
3. The Docker Client
4. Docker Registries
5. Docker Hub

---

## Question 24: Tabulate the differences between Virtual machines from Docker containers

### **Comparison Diagram**

![Pasted image 20260209195359.png](attachments/Pasted%20image%2020260209195359.png)

### **Solution Scheme**

Differences: 5 Marks

---

## Question 25: Identify how Docker functions as a lightweight virtual machine

### **Answer**

Docker uses OS-level virtualization through:

- **Namespaces** for process isolation
- **Control groups (cgroups)** for resource management
- **Containerized applications** behave like virtual machines but with minimal overhead and faster startup times

### **Solution Scheme**

Functionality: 5M

---

## Question 26: Outline the transition process from Virtual Machines to Containers

### **Steps**

- Identify VM-based applications
- Remove dependency on full guest OS
- Adopt shared kernel model
- Create Docker images for applications
- Manage containers using Docker Engine

### **Solution Scheme**

Steps: 5M

---

## Question 27: Expose the mechanisms used by Docker to use the "Save Game" approach with heap source control

### **Saving Work** (5 Marks)

**Step 1:** List all the images

```bash
sudo docker images
```

**Step 2:** View the containers

```bash
sudo docker container ls
```

**Step 3:** Run any image in the interactive mode

```bash
sudo docker run -it ubuntu:23.10 bin/bash
```

**Step 4:** Update the Container inside the interactive mode

```bash
apt-get update
```

**Step 5:** Install any tool in the container

```bash
apt-get install nmap
```

**Step 6:** Verify the installation process

```bash
nmap --version
```

**Step 7:** Exit the container

```bash
exit
```

**Step 8:** Display the list of launched containers

```bash
sudo docker ps
```

**Step 9:** Commit the Container with ID and provide a name

```bash
sudo docker commit [CONTAINER_ID] [new_image_name]
```

**Step 10:** Verify the image with

```bash
sudo docker images
```

### **Solution Scheme**

Steps: 5 Marks

---

## Question 28: Sequence the key steps in building Docker images

### **Answer** (Any five – 1M each)

1. Write a Dockerfile
2. Choose a base image
3. Add application dependencies
4. Configure environment variables
5. Build image using `docker build`

### **Solution Scheme**

Any 5 steps: 1M each

---

## Question 29: Formulate a command-level strategy for executing, monitoring, and administering Docker containers

### **Answer** (Any five – 1M each)

- `docker run`
- `docker ps`
- `docker stop`
- `docker restart`
- `docker rm`

### **Solution Scheme**

Any 5 commands: 1M each

---

## Question 30: Analyze the concept of "environments as processes"

### **Concept** (3 Marks)

In Docker, each container represents an isolated Linux process running with its own filesystem, network, and resources.

### **Example** (2 Marks)

A single Docker image running multiple containers ensures identical environments across different systems.

### **Solution Scheme**

- Concept: 3M
- Example: 2M

---

## Question 31: Give the benefits of using containers instead of VMs for development

### **Benefits Diagram**

![Pasted image 20260209195512.png](attachments/Pasted%20image%2020260209195512.png)

### **Solution Scheme**

Any 5 benefits: 1M each

---

## Question 32: Design and justify a container-based approach for executing a GUI application within Docker

### **Steps**

**Step 1:** Create a Dockerfile with an Environment

```dockerfile
# Installing the sources for the locales
FROM ubuntu:23.10

#To install without any interactive dialogue
ENV DEBIAN_FRONTEND=noninteractive

# Update the Container to install any software
RUN apt-get update

# Installing Nautilus File Manager
RUN apt-get install nautilus -y

CMD ["nautilus"]
```

**Step 2:** Build the image

```bash
sudo docker build -t nautilus-ubuntu .
```

**Step 3:** Run the dockerfile with the command

```bash
sudo docker run -it --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix nautilus-ubuntu:latest
```

### **Solution Scheme**

- Step 1: 3 Marks
- Step 2: 1 Mark
- Step 3: 1 Mark

---

## Question 33: Design and assess a workflow for provisioning and managing Docker hosts using Docker Machine

### **Illustration**

![Pasted image 20260209195548.png](attachments/Pasted%20image%2020260209195548.png)

### **Solution Scheme**

- Explanation: 1 Mark
- Illustration: 4 Marks

---

## Question 34: List 5 fundamental points of distinction between Virtual Machines and Docker Containers

### **Architecture Diagrams**

![Pasted image 20260209195605.png](attachments/Pasted%20image%2020260209195605.png)

![Pasted image 20260209195611.png](attachments/Pasted%20image%2020260209195611.png)

### **Solution Scheme**

- Architecture: 5 Marks
- Differences: 5 Marks

---

## Question 35: Analyze how Docker enables developers to create environment replicas using the "save game" approach

### **Steps** (6 Marks)

**Step 1:** Export the Docker Container as a TAR file

```bash
docker export Container_ID > filename.tar
```

**Step 2:** Import export-tar into a new image

```bash
docker import ~/filename.tar
```

**Step 3:** Verify the Docker Image

```bash
docker images
```

**Step 4:** Run a container from the imported image

```bash
docker run -it --name restored-container /bin/sh
```

**Alternative Method:**

**Step 1:** Create an image from the container state

```bash
docker commit
```

**Step 2:** Save the Docker Container

```bash
sudo docker save -o filename.tar
```

### **Example** (4 Marks)

A developer builds an image with Java and application libraries. The same image is used in testing and production, ensuring identical environments across systems.

### **Solution Scheme**

- Steps: 6 Marks
- Example: 4 Marks

---

## Question 36: Apply the "Save Game" approach as a cheap source control mechanism

### **Steps**

**Step 1:** List all the images

```bash
sudo docker images
```

**Step 2:** View the containers

```bash
sudo docker container ls
```

**Step 3:** Run any image in the interactive mode

```bash
sudo docker run -it ubuntu:23.10 bin/bash
```

**Step 4:** Update the Container inside the interactive mode

```bash
apt-get update
```

**Step 5:** Install any tool in the container

```bash
apt-get install nmap
```

**Step 6:** Verify the installation process

```bash
nmap --version
```

**Step 7:** Exit the container

```bash
exit
```

**Step 8:** Display the list of launched containers

```bash
sudo docker ps
```

**Step 9:** Commit the Container with ID and provide a name

```bash
sudo docker commit [CONTAINER_ID] [new_image_name]
```

**Step 10:** Verify the image with

```bash
sudo docker images
```

### **Solution Scheme**

Each Step: 1 Mark  
10 Steps: 10 Marks

---

## Question 37: Defend the workflow of building a Docker image running and deploying a Docker container

### **Build Phase** (6 Marks)

1. Write Dockerfile
2. Specify base image
3. Add application code
4. Build image

### **Running & Deployment Phase** (4 Marks)

```bash
docker run -p 8080:80 myapp
docker ps
```

Container runs and application is verified.

### **Solution Scheme**

- Build: 6 Marks
- Running: 2 Marks
- Deployment: 2 Marks

---

## Question 38: Assess the steps of "environments as processes" for running a docker container to a TAR file

### **Steps** (5 Marks)

**Step 1:** Export the Docker Container as a TAR file

```bash
docker export Container_ID > filename.tar
```

**Step 2:** Import export-tar into a new image

```bash
docker import ~/filename.tar
```

**Step 3:** Verify the Docker Image

```bash
docker images
```

**Step 4:** Run a container from the imported image

```bash
docker run -it --name restored-container /bin/sh
```

**Alternative Method:**

**Step 1:** Create an image from the container state

```bash
docker commit
```

**Step 2:** Save the Docker Container

```bash
sudo docker save -o filename.tar
```

### **Solution Scheme**

Steps: 5 Marks

---

## Question 39: Assess the effectiveness of injecting files into a Docker image using the ADD instruction

### **Steps**

**Step 1:** Create a Tar File

```bash
tar -czvf firstfile.tar.gz firstfile
```

**Flags:**

- `-z`: Compress the desired file/directory using gzip
- `-c`: Stand for create file (output tar.gz file)
- `-v`: To display the progress while creating the file
- `-f`: Finally the path of the desire file/directory to compress

**Step 2:** Creating the Dockerfile

```dockerfile
# Use the latest Ubuntu image as the base
FROM ubuntu:latest

# Update the package list
RUN apt-get -y update

# Add the tar.gz file from the specified path on the host to the current directory in the container
ADD dummy.tar.gz .
```

**Step 3:** Building the Docker Image

```bash
sudo docker build -t sample-image .
```

**Step 4:** List the images

```bash
sudo docker images
```

**Step 5:** Running the Docker Container

```bash
sudo docker run -it sample-image bash
```

**Step 6:** Verifying the Extraction

```bash
ls
```

### **Solution Scheme**

- Steps: 6 Marks
- Flags: 4 Marks

---

## Question 40: Summarize the sequential steps involved in converting a VM into a Docker container

### **Steps** (8 Marks)

**Step 1:** Create Nginx Container

```bash
sudo docker run -d -p 80:80 --name nginx nginx
```

**Step 2:** Displaying Running Container

```bash
sudo docker ps
```

**Step 3:** Export the Container with the Container name or ID

```bash
sudo docker export nginx > mynginx.tar
```

**Step 4:** Import the created tar file as a new Image

```bash
sudo docker import - mynginx < mynginx.tar
```

**Step 5:** List all the images

```bash
sudo docker images
```

**Step 6:** List all the files in the current location

```bash
ls -l
```

**Step 7:** Load Docker image into your system

```bash
sudo docker run --name container_nginx -d mynginx:latest nginx -g 'daemon off;'
```

**Step 8:** Displaying Running Container

```bash
sudo docker ps
```

### **Flowchart** (6 Marks)

![Pasted image 20260209200035.png](attachments/Pasted%20image%2020260209200035.png)

### **Example** (6 Marks)

The process demonstrates converting a running Nginx container into a portable Docker image that can be deployed on any Docker-enabled system.

### **Solution Scheme**

- Steps: 8 Marks
- Flowchart: 6 Marks
- Example: 6 Marks

---

## Question 41: Design a comprehensive container deployment and management strategy

### **Topics to Cover**

1. **Running GUIs within Docker** (5 Marks)
2. **Inspecting Containers** (3 Marks)
3. **Cleanly killing containers** (3 Marks)
4. **Using Docker Machine to provision Docker Hosts** (6 Marks)
5. **Wildcard DNS** (3 Marks)

### **Solution Scheme**

- Running GUIs within Docker: 5 Marks
- Inspecting Containers: 3 Marks
- Cleanly killing containers: 3 Marks
- Using Docker Machine to provision Docker Hosts: 6 Marks
- Wildcard DNS: 3 Marks

---

## Question 42: Critically assess the strategies involved in building efficient Docker images

### **Topics**

1. **Building Images**
2. **Injecting Files into the Image using ADD** (5 Marks)
3. **Intelligent Cache-Busting using Build-Args** (5 Marks)
4. **Intelligent Cache-Busting using ADD directive** (5 Marks)
5. **Setting the Right Time Zone in the Containers** (5 Marks)

### **Solution Scheme**

- Building Images
- Injecting Files into the Image using ADD: 5 Marks
- Intelligent Cache-Busting using Build-Args: 5 Marks
- Intelligent Cache-Busting using ADD directive: 5 Marks
- Setting the Right Time Zone in the Containers: 5 Marks

---

## Question 43: Apply the "Save Game" approach as a cheap source control mechanism

### **The "Save Game" Approach: Cheap Source Control**

**Step 1:** List all the images

```bash
sudo docker images
```

**Step 2:** View the containers

```bash
sudo docker container ls
```

**Step 3:** Run any image in the interactive mode

```bash
sudo docker run -it ubuntu:23.10 bin/bash
```

**Step 4:** Update the Container inside the interactive mode

```bash
apt-get update
```

**Step 5:** Install any tool in the container

```bash
apt-get install nmap
```

**Step 6:** Verify the installation process

```bash
nmap --version
```

**Step 7:** Exit the container

```bash
exit
```

**Step 8:** Display the list of launched containers

```bash
sudo docker ps
```

**Step 9:** Commit the Container with ID and provide a name

```bash
sudo docker commit [CONTAINER_ID] [new_image_name]
```

**Step 10:** Verify the image with

```bash
sudo docker images
```

### **To Tag the Image**

**Step 1:** List the images

```bash
sudo docker images
```

**Step 2:** Specify the name to the existing image

```bash
sudo docker tag [IMAGE_ID] [Image_Name]
```

### **To share images on the Docker Hub**

**Step 1:** Sign-up in Docker Hub repository

**Step 2:** List all the images in the local

```bash
sudo docker images
```

**Step 3:** Pull an image if none is available

```bash
sudo docker pull ubuntu:23.10
```

**Step 4:** Make necessary changes or modifications

```bash
apt-get update
apt-get install nmap
```

**Step 5:** Tag the Docker image

```bash
sudo docker tag local-image:tagname username/repository:tagname
```

**Step 6:** Push the repository to Docker Hub

```bash
sudo docker push username/repository:tagname
```

### **Solution Scheme**

- The "Save Game" Approach: Cheap Source Control: 10 Marks
- Docker Tagging: 3 Marks
- Sharing Images on the Docker Hub: 7 Marks

---

## Question 44: Evaluate the design considerations involved in building robust Docker containers

### **Injecting Files into the Image using ADD**

**Step 1:** Create a Tar File

```bash
tar -czvf firstfile.tar.gz firstfile
```

**Flags:**

- `-z`: Compress the desired file/directory using gzip
- `-c`: Stand for create file (output tar.gz file)
- `-v`: To display the progress while creating the file
- `-f`: Finally the path of the desire file/directory to compress

**Step 2:** Creating the Dockerfile

```dockerfile
# Use the latest Ubuntu image as the base
FROM ubuntu:latest

# Update the package list
RUN apt-get -y update

# Add the tar.gz file from the specified path on the host to the current directory in the container
ADD dummy.tar.gz .
```

**Step 3:** Building the Docker Image

```bash
sudo docker build -t sample-image .
```

**Step 4:** List the images

```bash
sudo docker images
```

**Step 5:** Running the Docker Container

```bash
sudo docker run -it sample-image bash
```

**Step 6:** Verifying the Extraction

```bash
ls
```

### **Intelligent Cache-Busting using Build-Args**

**Step 1:** Create a Dockerfile

**Step 2:** Add a custom cache invalidation

```dockerfile
FROM debian
LABEL author "Deepika K deepikak@rvce.edu.in"
LABEL description "This is a demo file to show build-args"

RUN apt-get update && apt-get -y upgrade

# Custom cache invalidation
ARG CACHEBUST=1

# Welcome Text
RUN echo ["Welcome"]
```

**Step 3:** Build the image with cache bust as a parameter to set a different value making all the layers to rebuild

```bash
sudo docker build -t test:2.0 --build-arg CACHEBUST=$(date +%s) .
```

### **Intelligent Cache-Busting using ADD directive**

**Step 1:** Create a Dockerfile with necessary Instructions and commands

```dockerfile
FROM debian
LABEL author "Deepika K deepikak@rvce.edu.in"
LABEL description "This is a demo file to show ADD-directive"

RUN apt-get update

# Welcome Text
RUN echo ["Welcome"]
```

**Step 2:** Add an URL of a Repository to be added into the Dockerfile with the location to be installed

```dockerfile
ADD <repository_url> /location
```

**Step 3:** Build the Dockerfile

```bash
sudo docker build -t repo-image:2.3 .
```

**Step 4:** View the docker images

```bash
sudo docker images
```

### **Solution Scheme**

- Injecting Files into the Image using ADD: 10 Marks
- Intelligent Cache-Busting using Build-Args: 5 Marks
- Intelligent Cache-Busting using ADD directive: 5 Marks

---

## Question 45: Analyze the concept of "Environments as Processes"

### **Code a Dockerized Python Flask or Node.js Application**

**Step 1:** Create the Dockerfile and add the similar contents

```dockerfile
# Use official Python image
FROM python:latest

# Set working directory
WORKDIR /app

# Copy requirement file and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Expose port 5000
EXPOSE 5000

# Run the app
CMD ["python", "app.py"]
```

**Step 2:** Create a file → app.py and add similar contents

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Simple Flask Docker!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

**Step 3:** Make a file → requirements.txt

```
Flask==2.3.3
```

**Step 4:** Execute the Docker Build Command

```bash
docker build -t program-3 .
```

**Step 5:** Run the Docker Run command specifying the Port Number

```bash
docker run -p 5000:5000 program-3
```

**Step 6:** Test the Container by verifying the localhost details on Web Browser, the text → Hello from Simple Flask Docker!

```
http://localhost:5000/
```

### **Integrate Git with Docker for Source-Controlled Application Builds**

**Step 1:** Make a folder → program-4 and move into the folder

```bash
mkdir program-4
cd program-4
```

**Step 2:** Create the file → package.json by executing the command

```bash
npm init -y
```

**Step 3:** Create the file → index.js with similar contents

```javascript
const express = require("express")
const app = express()
const PORT = 3000
```

**Step 4:** Add the dependency

```bash
npm install express
```

**Step 5:** Create the Dockerfile and add the similar contents

```dockerfile
# Stage 1 - Build Stage
FROM node:18 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

# Stage 2 - Runtime Stage
FROM node:18-slim
WORKDIR /app
COPY --from=build /app /app
EXPOSE 3000
CMD ["node", "index.js"]
```

**Step 6:** Create a New Repo on GitHub. Click Create New → New Repository. Add a repository name.

**Step 7:** Getting a Personal Access Token (PAT)  
Go to Profile → Developer Settings → Personal Access Tokens → Tokens (classic) and generate a token.

**Step 8:** Initialize Git Repository locally. Note: Enter the Github Username and PAT for the Password

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
```

**Step 9:** Link to Remote Repository

```bash
git remote add origin https://github.com/<username>/<repo>.git
```

**Step 10:** Push Code to GitHub

```bash
git push -u origin main
```

### **Solution Scheme**

- Code a Dockerized Python Flask or Node.js Application: 10 Marks
- Integrate Git with Docker for Source-Controlled Application Builds: 10 Marks

---

## Question 46: List the sequential steps involved in the Docker Hub workflow

### **Docker Hub Workflow**

**Step 1:** Create your repository on GitHub or BitBucket

**Step 2:** Clone the new Git repository

**Step 3:** Add code to your Git repository

**Step 4:** Commit the source

**Step 5:** Push the Git repository

**Step 6:** Create a new repository on the Docker Hub

**Step 7:** Link the Docker Hub repository to the Git repository

**Step 8:** Wait for the Docker Hub build to complete

**Step 9:** Commit and push a change to the source

**Step 10:** Wait for the second Docker Hub build to complete

### **Solution Scheme**

5 Marks for 10 Steps

---

## Question 47: Summarize the process of containerizing a CI setup by running a Jenkins Master within a Docker container

### **Answer** (Any five points – 1M each)

**Step 1:** Prerequisite: Docker must be installed  
Minimum hardware requirements:

- 256 MB of RAM
- 1 GB of drive space (although 10 GB is a recommended minimum if running Jenkins as a Docker container)

**Step 2:** Use the Jenkins Docker Image to install and use Jenkins Server

```bash
sudo docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11
```

**Step 3:** View running Containers

```bash
sudo docker containers ls -a
```

**Step 4:** Navigate to the link http://localhost:8080 to verify the installation of Jenkins

```
http://localhost:8080
```

### **Solution Scheme**

- Steps 4: 4 Marks (1 Mark Each per Step)
- Output: 1 Mark

---

## Question 48: Recall the Automated Build Pipeline using Docker Hub with a flowchart

### **Flowchart**

```
┌──────────────────────────────────────────────────────────────┐
│ Create your repository on GitHub or BitBucket               │
└──────────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────────┐
│ Clone the new Git repository                                │
└──────────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────────┐
│ Add code to your Git repository                             │
└──────────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────────┐
│ Commit the source                                            │
└──────────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────────┐
│ Push the Git repository                                      │
└──────────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────────┐
│ Create a new repository on the Docker Hub                   │
└──────────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────────┐
│ Link the Docker Hub repository to the Git repository        │
└──────────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────────┐
│ Wait for the Docker Hub build to complete                   │
└──────────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────────┐
│ Commit and push a change to the source                      │
└──────────────────────────────────────────────────────────────┘
                          ↓
┌──────────────────────────────────────────────────────────────┐
│ Wait for the second Docker Hub build to complete            │
└──────────────────────────────────────────────────────────────┘
```

### **Solution Scheme**

5 Marks for 10 Steps

---

## Question 49: Break down the steps to integrate CI by Running Jenkins in a Docker Container

### **CI Integration by Running Jenkins in a Docker Container**

**Step 1:** Create Project Structure → program-5

```bash
mkdir program-5
cd program-5
```

**Step 2:** Create a Dockerfile and add similar contents

```dockerfile
FROM jenkins/jenkins:lts

# Switch to root user to install tools
USER root

# Install Docker inside Jenkins container
RUN apt-get update && apt-get install -y docker.io

# Switch back to Jenkins user
USER jenkins
```

**Step 3:** Make a file → docker-compose.yml

```yaml
version: "3.8"
services:
  jenkins:
    build: .
    container_name: jenkins-ci
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock

volumes:
  jenkins_home:
```

**Step 4:** Build and Run Jenkins

**Step 5:** Test the running of Jenkins by logging into http://localhost:8080

### **Solution Scheme**

Step 5: 5 Marks (1 Mark Each)

---

## Question 50: Construct a typical CD pipeline with an illustration

### **CD Pipeline Illustration**

![Pasted image 20260209200234.png](attachments/Pasted%20image%2020260209200234.png)

### **Solution Scheme**

Illustration: 5 Marks

---

## Question 51: Assess the suitability of running the Jenkins Master within a Docker container

### **Steps**

**Step 1:** Prerequisite: Docker must be installed  
Minimum hardware requirements:

- 256 MB of RAM
- 1 GB of drive space (although 10 GB is a recommended minimum if running Jenkins as a Docker container)

**Step 2:** Use the Jenkins Docker Image to install and use Jenkins Server

```bash
sudo docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11
```

**Step 3:** View running Containers

```bash
sudo docker containers ls -a
```

**Step 4:** Navigate to the link http://localhost:8080 to verify the installation of Jenkins

```
http://localhost:8080
```

### **Example**

A software development team can use Jenkins in Docker for:

- Consistent CI/CD environment
- Easy setup and teardown
- Isolated testing environments
- Quick recovery from failures

### **Solution Scheme**

- Steps: 4 Marks
- Use case: 1 Mark

---

## Question 52: Document the process of facilitating deployment of Docker images

### **Need for facilitating Docker image deployment** (1 Mark)

Facilitating Docker image deployment ensures that the same, tested application image is consistently distributed across environments such as development, staging, and production.

### **Basic steps involved** (2 Marks)

1. **Pull**
   - Command: `docker pull <image>:<tag>`
   - Retrieves an image from a registry to the local system or CI server

2. **Tag**
   - Command: `docker tag <source>:<tag> <target>:<tag>`
   - Assigns a new name or version tag to an existing image for promotion or versioning

3. **Push**
   - Command: `docker push <image>:<tag>`
   - Uploads the tagged image to a registry for use by other environments

### **Command usage** (1 Mark)

Docker CLI commands (docker pull, docker tag, docker push) are used to manage image flow across environments.

### **Purpose** (1 Mark)

This workflow supports versioning, traceability, rollback, and consistent deployments across teams and environments.

### **Solution Scheme**

- Need of Facilitating deployment of Docker images: 1 Mark
- Basic steps (pull → tag → push): 2 Marks
- Command: 1 Mark
- Purpose: 1 Mark

---

## Question 53: Predict a zero-downtime switchover strategy for a containerized application using confd

### **Steps** (5 Marks)

**Step 1:** Run the etcd container with authentication disabled

```bash
docker run -d -p 2379:2379 -p 2380:2380 --name etcd \
-e ALLOW_NONE_AUTHENTICATION=yes bitnami/etcd:latest
```

**Step 2:** Verify the running Docker containers

```bash
docker ps
```

**Step 3:** Set initial backend values in etcd using put

```bash
docker exec etcd etcdctl put /backend/server1 192.168.1.10
sudo docker exec etcd etcdctl put /backend/server2 192.168.1.11
```

**Step 4:** Retrieve the stored backend values using get

```bash
docker exec etcd etcdctl get /backend/server1
sudo docker exec etcd etcdctl get /backend/server2
```

**Step 5:** Create a Docker image with confd and a web server (e.g., Nginx)

```bash
docker build -t nginx-confd .
```

**Step 6:** Run the confd-enabled container with access to etcd

```bash
docker run -d --name nginx_confd \
-p 80:80 \
-e ETCD_ENDPOINT=http://host.docker.internal:2379 \
nginx-confd
```

**Step 7:** Start confd in watch mode inside the container

```bash
docker exec nginx_confd confd \
-backend etcd \
-node http://host.docker.internal:2379 \
-watch
```

**Step 8:** Change backend values in etcd to trigger a switchover

```bash
docker exec etcd etcdctl put /backend/server1 192.168.1.20
```

**Step 9:** Observe automatic configuration reload without restarting the container

```bash
docker logs nginx_confd
```

**Step 10:** Verify uninterrupted service during configuration update

```bash
curl http://localhost
```

### **Solution Scheme**

10 Steps: 5 Marks

---

## Question 54: List reasons for adopting Docker in a DevOps environment

### **Reasons** (4 Marks)

1. **Consistent Environments** - Docker ensures consistency across development, testing, and production
2. **Fast Deployment** - Containers start in seconds
3. **Resource Efficiency** - Multiple containers share the same OS kernel
4. **Easy Scaling** - Quick horizontal scaling of applications

### **Features of CI & CD** (2 Marks)

- Automated builds and testing
- Consistent deployment pipelines

### **Use Case** (4 Marks)

A development team uses Docker to:

1. Build application in development environment
2. Test in isolated containers
3. Deploy same container to staging
4. Promote to production with confidence

### **Solution Scheme**

- Reasons: 4 Marks
- Features - CI & CD: 2 Marks
- Use case: 4 Marks

---

## Question 55: Classify the procedure to configure Docker Hub automated builds

### **Purpose** (1 Mark)

Automated builds ensure images are built and published automatically when code changes occur.

### **Procedure** (4 Marks)

- Prepare repository with Dockerfile
- Create Docker Hub repository
- Link GitHub/Bitbucket
- Configure build rules and context

### **Tagging strategy** (3 Marks)

- `latest` for stable builds
- Semantic version tags (e.g., 1.2.0)
- Commit hash tags for traceability

### **Limitations** (2 Marks)

- Limited git history
- Secure handling of secrets and dependency versioning

### **Solution Scheme**

- Purpose: 1 Mark
- Steps: 4 Marks
- Tagging strategy: 3 Marks
- Limitations: 2 Marks

---

## Question 56: Apply the concept of the Docker contract to demonstrate how it reduces friction

### **Typical Software Workflow** (6 Marks)

![Pasted image 20260209201023.png](attachments/Pasted%20image%2020260209201023.png)

### **After Docker Contract** (4 Marks)

![Pasted image 20260209201031.png](attachments/Pasted%20image%2020260209201031.png)

### **Solution Scheme**

- Typical Software Workflow: 6 Marks
- After Docker Contract: 4 Marks

---

## Question 57: Illustrate how cooperating teams' deliverables can be made clean and unambiguous

### **Use Case Diagram** (10 Marks)

![Pasted image 20260209201051.png](attachments/Pasted%20image%2020260209201051.png)

### **Solution Scheme**

Diagram w.r.t to the use case mentioned: 10 Marks

---

## Question 58: Design a Docker-based deployment strategy that uses etcd to dynamically configure containers

### **Informing your containers with etcd**

**Step 1:** Run the etcd container with 'ALLOW_NONE_AUTHENTICATION=yes'

```bash
docker run -d -p 2379:2379 -p 2380:2380 --name etcd \
-e ALLOW_NONE_AUTHENTICATION=yes quay.io/coreos/etcd:v3.5.13
```

**Step 2:** Verify the running processes of Docker

```bash
sudo docker ps
```

**Step 3:** Set a key-value pair using 'put'

```bash
sudo docker exec etcd etcdctl put /my_name deepika
```

**Step 4:** Retrieve the key-value pair using 'get'

```bash
sudo docker exec etcd etcdctl get /my_name
```

### **Solution Scheme**

- Steps 4: 1 Mark Each (4 × 1 Mark = 4 Marks)
- Output: 1 Mark

---

## Question 59: Visualize a CI pipeline for a Dockerized application

### **Topics to Cover**

1. **Design CI Pipeline** (5 Marks)
2. **Steps of Docker Hub Workflow** (5 Marks)
3. **Flowchart for Docker Hub Workflow** (5 Marks)
4. **Running a Jenkins Master** (5 Marks)

### **Solution Scheme**

- Design CI Pipeline: 5 Marks
- Steps of Docker Hub Workflow: 5 Marks
- Flowchart for Docker Hub Workflow: 5 Marks
- Running a Jenkins Master: 5 Marks

---

## Question 60: Examine the architecture of a developer's laptop before and after use of Jenkins server

### **Architecture Diagrams**

**An overloaded Jenkins Server:**

![Pasted image 20260209201130.png](attachments/Pasted%20image%2020260209201130.png)

**Before Jenkins Server:**

![Pasted image 20260209201142.png](attachments/Pasted%20image%2020260209201142.png)

**After Jenkins Server:**

![Pasted image 20260209201151.png](attachments/Pasted%20image%2020260209201151.png)

### **Solution Scheme**

- An overloaded Jenkins Server: 4 Marks
- Before Jenkins Server: 8 Marks
- After Jenkins Server: 8 Marks

---

## Question 61: Analyze the challenges involved in deploying Docker images in environments with limited network connectivity

### **1. Manually mirroring registry images** (5 Marks)

**Step 1:** Pull the image from the Docker Hub

```bash
sudo docker pull deepikakripanithi/ubuntu:latest
```

**Step 2:** View the images

```bash
sudo docker images
```

**Step 3:** Retag the Image

```bash
sudo docker tag deepikakripanithi/ubuntu:latest deepikakripanithi/ubuntu2:1.0
```

**Step 4:** Login into the Docker Hub with the credentials

**Step 5:** Push the retagged image

```bash
sudo docker push deepikakripanithi/ubuntu2:1.0
```

### **2. Delivering Images Over Constrained Connections** (5 Marks)

**Step 1:** Create a Dockerfile with multiple images

```dockerfile
FROM alpine as img1
COPY file1.txt .

FROM alpine as img2
COPY file2.txt .
```

**Step 2:** Build the images respectively

![Pasted image 20260209201219.png](attachments/Pasted%20image%2020260209201219.png)

```bash
sudo docker build -t image1 --target img1 .
sudo docker build -t image2 --target img2 .
```

// Mention the images are transferred to a remote system over the network

**Step 3:** Run the images in the detached mode

```bash
sudo docker run -it --name image1 -d image1:latest
sudo docker run -it --name image2 -d image2:latest
```

**Step 4:** Inspect the bridge in the network to get the IP addresses

```bash
sudo docker network inspect bridge
```

**Step 5:** Execute a container and ping the other

```bash
sudo docker container exec -it image1 /bin/sh
```

### **3. Sharing Docker objects as TAR files** (5 Marks)

**Step 1:** Export the Container with the Container name or ID

```bash
sudo docker export [container-name] > nginx.tar
```

**Step 2:** Import the created tar file as a new Image

```bash
sudo docker import - mynginx < nginx.tar
```

**Step 3:** List all the images

```bash
sudo docker images
```

**Step 4:** Save the created image as a new Tar File

```bash
sudo docker save -o mynginx1.tar nginx
```

**Step 5:** Load the Tar File

```bash
sudo docker load --input mynginx1.tar
```

### **Diagram** (5 Marks)

### **Solution Scheme**

- Manually Mirroring Registry Images: 5 Marks
- Delivering Images Over Constrained Connections: 5 Marks
- Sharing Docker objects as TAR files: 5 Marks
- Diagram: 5 Marks

---

## Question 62: Evaluate the effectiveness of using confd to enable zero-downtime switchovers

### **Using confd to enable zero-downtime switchovers** (15 Marks)

**Step 1:** Run the etcd container with authentication disabled

```bash
docker run -d -p 2379:2379 -p 2380:2380 --name etcd \
-e ALLOW_NONE_AUTHENTICATION=yes bitnami/etcd:latest
```

**Step 2:** Verify the running Docker containers

```bash
docker ps
```

**Step 3:** Set initial backend values in etcd using put

```bash
docker exec etcd etcdctl put /backend/server1 192.168.1.10
sudo docker exec etcd etcdctl put /backend/server2 192.168.1.11
```

**Step 4:** Retrieve the stored backend values using get

```bash
docker exec etcd etcdctl get /backend/server1
sudo docker exec etcd etcdctl get /backend/server2
```

**Step 5:** Create a Docker image with confd and a web server (e.g., Nginx)

```bash
docker build -t nginx-confd .
```

**Step 6:** Run the confd-enabled container with access to etcd

```bash
docker run -d --name nginx_confd \
-p 80:80 \
-e ETCD_ENDPOINT=http://host.docker.internal:2379 \
nginx-confd
```

**Step 7:** Start confd in watch mode inside the container

```bash
docker exec nginx_confd confd \
-backend etcd \
-node http://host.docker.internal:2379 \
-watch
```

**Step 8:** Change backend values in etcd to trigger a switchover

```bash
docker exec etcd etcdctl put /backend/server1 192.168.1.20
```

**Step 9:** Observe automatic configuration reload without restarting the container

```bash
docker logs nginx_confd
```

**Step 10:** Verify uninterrupted service during configuration update

```bash
curl http://localhost
```

### **Diagram** (5 Marks)

![Pasted image 20260209201251.png](attachments/Pasted%20image%2020260209201251.png)

### **Solution Scheme**

- Using confd to enable zero-downtime switchovers: 15 Marks
- Diagram: 5 Marks

---

## Question 63: Design a Docker-based solution for configuring container images across multiple environments

### **Topics to Cover**

1. **Dockerfile with etcd** (5 Marks)
2. **Building & Running Containers** (5 Marks)
3. **Informing your containers with etcd** (5 Marks)

**Step 1:** Run the etcd container with 'ALLOW_NONE_AUTHENTICATION=yes'

```bash
sudo docker run -d -p 2379:2379 -p 2380:2380 --name etcd \
-e ALLOW_NONE_AUTHENTICATION=yes bitnami/etcd:latest
```

**Step 2:** Verify the running processes of Docker

```bash
sudo docker ps
```

**Step 3:** Set a key-value pair using 'put'

```bash
sudo docker exec etcd etcdctl put /my_name deepika
```

**Step 4:** Retrieve the key-value pair using 'get'

```bash
sudo docker exec etcd etcdctl get /my_name
```

4. **Use case** (5 Marks)

### **Solution Scheme**

- Dockerfile with etcd: 5 Marks
- Building & Running Containers: 5 Marks
- Informing your containers with etcd: 5 Marks
- Use case: 5 Marks

---
