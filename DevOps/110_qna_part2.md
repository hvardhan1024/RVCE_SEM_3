## Question 64: List any five reasons for adopting Kubernetes in a DevOps environment

### **Any five reasons for adopting Kubernetes:**

1. **Container Orchestration**  
   Kubernetes automates the deployment, management, and operation of containerized applications.

2. **Scalability**  
   It supports automatic scaling of applications based on workload demand.

3. **High Availability**  
   Kubernetes ensures application reliability through self-healing features such as pod restarts and rescheduling.

4. **Efficient Resource Utilization**  
   It optimizes the use of infrastructure resources by efficiently scheduling containers across nodes.

5. **Consistent Deployment Environments**  
   Kubernetes provides a uniform platform across development, testing, and production environments, reducing environment-related issues.

(Any other relevant points may also be considered.)

### **Solution Scheme**

5 Reasons: 5 Marks

---

## Question 65: Record the basics of Docker

### **Answer**

- **Docker Image** – Read-only template for containers
- **Docker Container** – Running instance of an image
- **Docker Registry** – Stores and distributes images
- **Dockerfile** – Instructions to build images

**Relationship:**  
Dockerfile → Image → Registry → Container

### **Solution Scheme**

- Basics: 1 Mark
- Image, Container, Registry, Dockerfile: 4 Marks

---

## Question 66: Elaborate the typical procedure for creating and running a container image

### **Solution**

1. Pull image
2. Run container
3. Use runtime options (-d, -p, --name, -e, -v)
4. Verify using `docker ps` and `docker logs`
5. Stop and remove container

### **Solution Scheme**

5 points × 1 Mark = 5 Marks

---

## Question 67: Give the process of sharing a container image using tagging and pushing to a registry

### **Steps**

1. Build image
2. Tag image
3. Login to registry
4. Push image
5. Pull from another system

### **Solution Scheme**

5 steps × 1 Mark = 5 Marks

---

## Question 68: Analyze the process of setting up and running a local single-node Kubernetes cluster using Minikube

### **Running a Local Single-Node Kubernetes Cluster with Minikube**

**Step 1:** Installing Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```

**Step 2:** Start a cluster using the docker driver

```bash
minikube start --driver=docker
```

**Step 3:** Make docker the default driver for Minikube

```bash
minikube config set driver docker
```

**Step 4:** Starting a Kubernetes Cluster with Minikube

```bash
minikube start
```

**Step 5:** Installing the Kubernetes Client (kubectl)

Update the apt package index and install packages needed to use the Kubernetes apt repository

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
```

Download the public signing key for the Kubernetes package repositories

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor \
-o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

Add the appropriate Kubernetes apt repository

```bash
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

Update apt package index and install kubectl

```bash
sudo apt-get update
sudo apt-get install -y kubectl
```

**Step 6:** Displaying cluster information

```bash
kubectl cluster-info
```

### **Solution Scheme**

Steps: 5 Marks

---

## Question 69: Infer the purpose and method of setting up an alias for kubectl with examples

### **Setting up an alias and command-line completion for kubectl**

**Step 1:** Creating an alias  
Add the following line to the ~/.bashrc or equivalent file:

```bash
alias k=kubectl
```

**Step 2:** To add autocomplete permanently to your bash shell

```bash
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

**Step 3:** Shorthand alias for kubectl that also works with completion

```bash
alias k=kubectl
complete -o default -F __start_kubectl k
```

### **Solution Scheme**

Steps: 5 Marks

---

## Question 70: List the process to setup a Kubernetes cluster configure command-line completion for kubectl

### **Setting a Kubernetes Cluster**

### **Setting up an alias and command-line completion for kubectl**

**Step 1:** Creating an alias  
Add the following line to the ~/.bashrc or equivalent file:

```bash
alias k=kubectl
```

**Step 2:** To add autocomplete permanently to your bash shell

```bash
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

**Step 3:** Shorthand alias for kubectl that also works with completion

```bash
alias k=kubectl
complete -o default -F __start_kubectl k
```

### **Solution Scheme**

- Setting a Kubernetes Cluster: 2.5 Marks
- Setting up an alias and command-line completion for kubectl: 2.5 Marks

---

## Question 71: Classify the purpose and procedure of accessing the Kubernetes Dashboard

### **Introduction** (1 Mark)

The Kubernetes Dashboard is a web-based graphical user interface (GUI) that allows users to interact with and manage a Kubernetes cluster without using only command-line tools.

### **1) Purpose of Kubernetes Dashboard** (2 Marks)

The dashboard is used to:

- Visualize cluster health and resource usage
- Inspect workloads such as pods, deployments, and replica sets
- View services, networking resources, logs, and events

It provides a visual abstraction over kubectl operations for easier cluster monitoring.

### **2) Procedure (Minikube scenario)** (1 Mark)

In Minikube, the dashboard is accessed using:

```bash
minikube dashboard
```

This command:

- Deploys dashboard components if required
- Creates a local proxy
- Opens the dashboard URL in the browser

### **Solution Scheme**

- Purpose: 2.5 Marks
- Procedure: 2.5 Marks

---

## Question 72: Design and justify a complete end-to-end workflow for deploying a first application on Kubernetes

### **Deploying the Node.js app** (5 Marks)

**Step 1:** Initiate the Minikube

```bash
minikube start
```

**Step 2:** Deploying a Node.js app

```bash
kubectl run kubia --image=luksa/kubia --port=8080
```

**Step 3:** Listing pods

```bash
kubectl get pods
```

**Step 4:** Listing pods to view the changed status

```bash
kubectl get pods
```

### **Accessing the web application** (5 Marks)

**Step 1:** Creating a service object and expose the pod

```bash
kubectl run kubia --image=luksa/kubia --port=8080
```

**Step 2:** Expose the Pod via a Service

```bash
kubectl expose pod kubia --type=LoadBalancer --name kubia-http
```

**Step 3:** Listing services

```bash
kubectl get services
```

**Step 4:** Listing services again to view whether an external IP has been assigned

```bash
kubectl get svc
```

**Step 5:** Accessing the Web Application

```bash
curl http://<EXTERNAL_IP>:8080
```

### **Solution Scheme**

- Deploying the Node.js app: 5 Marks
- Accessing the web application: 5 Marks

---

## Question 73: Summarize the technique for accessing a web application deployed on Kubernetes using Services

### **1) Explanation** (3 Marks)

Running the first application demonstrates how Kubernetes schedules and manages containerized workloads using declarative objects and kubectl commands.

### **2) Workflow steps** (5 Marks)

1. **Cluster validation**

   ```bash
   kubectl cluster-info
   ```

2. **Start Minikube**

   ```bash
   minikube start
   ```

3. **Create a pod**

   ```bash
   kubectl run kubia --image=luksa/kubia --port=8080
   ```

4. **Observe pod lifecycle**

   ```bash
   kubectl get pods
   kubectl get pods -o wide
   ```

5. **Inspect and troubleshoot**
   ```bash
   kubectl describe pod kubia
   kubectl logs kubia
   kubectl exec -it kubia -- sh
   ```

### **Solution Scheme**

- Explanation: 3 Marks
- Workflow: 7 Marks

---

## Question 74: Apply the concept of the logical parts of the Kubernetes system

### **Illustrative Representation**

![[Pasted image 20260209201452.png]]

### **Solution Scheme**

Diagram showing interaction between replication controller, pod, and service

---

## Question 75: Demonstrate logical parts of a Kubernetes-based system

### **System Illustration**

![[Pasted image 20260209201505.png]]

### **Solution Scheme**

- Explanation: 3 Marks
- Diagram: 6 Marks

---

## Question 76: Outline the visual representation of the Kubernetes architecture

### **Kubernetes Architecture**

![[Pasted image 20260209201520.png]]

### **Solution Scheme**

- Explanation: 4 Marks
- Diagram: 6 Marks

---

## Question 77: Expose the end-to-end procedure for setting up a Kubernetes cluster using Minikube

### **Running a Local Single-Node Kubernetes Cluster with Minikube**

**Step 1:** Minimum Requirements

- 2 CPUs or more
- 2GB of free memory
- 20GB of free disk space
- Internet connection
- Container (Docker)

**Step 2:** Installing Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```

**Step 3:** Start a cluster using the docker driver

```bash
minikube start --driver=docker
```

**Step 4:** Make docker the default driver for Minikube

```bash
minikube config set driver docker
```

**Step 5:** Starting a Kubernetes Cluster with Minikube

```bash
minikube start
```

**Step 6:** Installing the Kubernetes Client (kubectl)

Update the apt package index and install packages needed to use the Kubernetes apt repository

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
```

Download the public signing key for the Kubernetes package repositories

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor \
-o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

Add the appropriate Kubernetes apt repository

```bash
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

Update apt package index and install kubectl

```bash
sudo apt-get update
sudo apt-get install -y kubectl
```

**Step 7:** Displaying cluster information

```bash
kubectl cluster-info
```

### **Setting up an alias and command-line completion for kubectl**

**Step 1:** Creating an alias  
Add the following line to the ~/.bashrc or equivalent file:

```bash
alias k=kubectl
```

**Step 2:** To add autocomplete permanently to your bash shell

```bash
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

**Step 3:** Shorthand alias for kubectl that also works with completion

```bash
alias k=kubectl
complete -o default -F __start_kubectl k
```

### **Solution Scheme**

- Running a Local Single-Node Kubernetes Cluster with Minikube: 14 Marks
- Setting up an alias and command-line completion for kubectl: 6 Marks

---

## Question 78: Determine the steps of deploying a simple web application on Kubernetes

### **Steps**

**Step 1:** Start Minikube

```bash
minikube start
```

**Step 2:** Check the status of Minikube with the following command

```bash
minikube status
```

**Step 3:** Status should be displayed something similar to below

```
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

**Step 4:** Run the following command in case, of not running some service

```bash
minikube delete
minikube start --driver=docker
```

**Step 5:** Create a Deployment file → nginx-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest # already exists on Docker Hub
          ports:
            - containerPort: 80
```

**Step 6:** Apply the nginx-deployment.yaml

```bash
kubectl apply -f nginx-deployment.yaml
```

**Step 7:** Check Status

```bash
kubectl get deployments
kubectl get pods
```

**Step 8:** Expose the Deployment as a Service

```bash
kubectl expose deployment nginx-deployment \
--type=NodePort \
--port=80
```

**Step 9:** Check service

```bash
kubectl get services
```

**Step 10:** Access the Web App

```bash
minikube service nginx-deployment
```

### **Output Diagrams**

![[Pasted image 20260209201640.png]]
fig displaying a web app - ip address of k8s via minikube

![[Pasted image 20260209201703.png]]

### **Solution Scheme**

- 10 steps: 15 Marks
- Output: 5 Marks

---

## Question 79: List and briefly state the key components that constitute the architecture of a Kubernetes cluster

### **Control Plane Components**

Manage the overall state of the cluster:

- **kube-apiserver**  
  The core component server that exposes the Kubernetes HTTP API.

- **etcd**  
  Consistent and highly-available key value store for all API server data.

- **kube-scheduler**  
  Looks for Pods not yet bound to a node, and assigns each Pod to a suitable node.

- **kube-controller-manager**  
  Runs controllers to implement Kubernetes API behavior.

- **cloud-controller-manager (optional)**  
  Integrates with underlying cloud provider(s).

### **Node Components**

Run on every node, maintaining running pods and providing the Kubernetes runtime environment:

- **kubelet**  
  Ensures that Pods are running, including their containers.

- **kube-proxy (optional)**  
  Maintains network rules on nodes to implement Services.

- **Container runtime**  
  Software responsible for running containers.

### **Architecture Diagram**

![[Pasted image 20260209201753.png]]

### **Solution Scheme**

- Explanation of Control Plane Components: 10 Marks
- Explanation of Components: 6 Marks
- Diagram: 4 Marks

---

## Question 80: Organize and interpret the complete workflow involved in creating, running, and sharing a container image

### **Creating, Running, and Sharing a Container Image**

1. **Installing Docker and Running a Hello World Container**
2. **Creating a Dockerfile for the Image**
3. **Building the Container Image**
4. **Running the Container Image**
5. **Exploring the Inside of a Running Container**
6. **Stopping and Removing a Container**
7. **Pushing the image to an Image Registry**

### **Solution Scheme**

- Installing Docker and Running a Hello World Container: 3 Marks
- Creating a Dockerfile for the Image: 3 Marks
- Building the Container Image: 3 Marks
- Running the Container Image: 3 Marks
- Exploring the Inside of a Running Container: 3 Marks
- Stopping and Removing a Container: 3 Marks
- Pushing the image to an Image Registry: 2 Marks

---

## Question 91: List and state the fundamental elements involved in creating and managing Kubernetes Pods using YAML

### **Project Structure**

```
Program/
  --> nodejs-configmap.yaml
  --> nodejs-pod.yaml
  --> server.js
```

**Step 1:** Write a Node.js app → server.js

```javascript
const http = require("http")
const port = 3000

const server = http.createServer((req, res) => {
  res.end("Hello from Node.js running inside Kubernetes!")
})

server.listen(port, () => {
  console.log("Server running at port ${port}")
})
```

**Step 2:** Create a ConfigMap for the code

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: nodejs-app-config
data:
  server.js: |
    const http = require("http");
    const port = 3000;

    const server = http.createServer((req, res) => {
      res.end("Hello from Node.js running inside Kubernetes!");
    });

    server.listen(port, () => {
      console.log('Server running at port ${port}');
    });
```

**Step 3:** Create the Pod YAML using Node.js as a base image

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: program-8
  labels:
    app: nodejs
spec:
  containers:
    - name: nodejs
      image: node:18
      command: ["node", "/usr/src/app/server.js"]
      volumeMounts:
        - name: app-code
          mountPath: /usr/src/app
      ports:
        - containerPort: 3000
  volumes:
    - name: app-code
      configMap:
        name: nodejs-app-config
```

**Step 4:** Apply the YAML files

```bash
kubectl apply -f nodejs-configmap.yaml
kubectl apply -f nodejs-pod.yaml
```

**Step 5:** Check pod status

```bash
kubectl get pods
```

**Step 6:** Access the Node.js app

```bash
kubectl port-forward pod/nodejs-pod 3000:3000
```

**Step 7:** Check the Output in http://localhost:3000

**Step 8:** Delete All Pods

```bash
kubectl delete pods --all
```

**Step 9:** Delete All Services

```bash
kubectl delete svc --all
```

**Step 10:** Delete All Deployments

```bash
kubectl delete deployments --all
```

### **Output Screenshots**

![[Pasted image 20260209201850.png]]

![[Pasted image 20260209201857.png]]

### **Solution Scheme**

- Steps: 16 Marks
- Output: 4 Marks

---

## Question 92: Indicate the reasons for containers of a pod run on the same node

### **Reasons** (2 Marks)

Containers in a pod must run on the same node to:

- Share the same network namespace (same IP address)
- Share storage volumes
- Enable efficient inter-container communication

### **Diagram** (3 Marks)

![[Pasted image 20260209201917.png]]

### **Solution Scheme**

- Reasons: 2 Marks
- Diagram: 3 Marks

---

## Question 93: Give a YAML descriptor for creating a new pod in a Kubernetes cluster

### **Steps**

**Step 1:** Create a File for Namespaces as namespaces.yaml

**Step 2:** Create a File for Pods as pods.yaml

**Step 3:** Namespaces YAML content

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
---
apiVersion: v1
kind: Namespace
metadata:
  name: prod
```

**Step 4:** Pods YAML content

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend-pod
  namespace: dev
  labels:
    app: frontend
    env: dev
spec:
  containers:
    - name: frontend
      image: nginx
      ports:
        - containerPort: 80
---
apiVersion: v1
kind: Pod
metadata:
  name: backend-pod
  namespace: dev
  labels:
    app: backend
    env: dev
spec:
  containers:
    - name: backend
      image: nginx
      ports:
        - containerPort: 80
```

**Step 5:** Apply the Namespaces & Pod YAML file

```bash
kubectl apply -f namespaces.yaml
kubectl apply -f pods.yaml
```

### **Output**

![[Pasted image 20260209201942.png]]

### **Solution Scheme**

- Steps: 4 Marks
- Output: 1 Mark

---

## Question 94: Infer the main parts of a pod definition

### **Main Parts**

**Metadata**  
Includes the name, namespace, labels, and other information about the pod.

**Spec**  
Contains the actual description of the pod's contents, such as the pod's containers, volumes, and other data.

**Status**  
Contains the current information about the running pod, such as what condition the pod is in, the description and status of each container, and the pod's internal IP and other basic info.

### **Pod Definition Diagram**

![[Pasted image 20260209202010.png]]

### **Solution Scheme**

- Metadata: 2 Marks
- Spec: 2 Marks
- Status: 1 Mark

---

## Question 95: Expose the organization of Pods with labels

### **Diagram**

![[Pasted image 20260209202037.png]]

### **Solution Scheme**

- Explanation: 2 Marks
- Diagram: 3 Marks

---

## Question 96: Interpret the process of sending requests to the pod with an illustration

### **Sending Requests to the Pod**

**Step 1:** Forwarding a local network port to a port in the pod

```bash
kubectl port-forward kubia 8888:8080
kubectl port-forward nginx 8080:80
```

**Step 2:** Connecting to the Pod through the Port Forwarder

```bash
curl localhost:8888
```

### **Diagram**

![[Pasted image 20260209202049.png]]

### **Solution Scheme**

- Steps: 4 Marks
- Diagram: 1 Mark

---

## Question 97: Express the methods for specifying labels while creating a new pod

### **Specifying Labels When Creating a Pod**

**Step 1:** Make a pod with labels - nginx.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    environment: production
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
```

**Step 2:** Create the pod with

```bash
kubectl create -f nginx.yaml
```

**Step 3:** View the labels and specific labels

```bash
kubectl get pod --show-labels
kubectl get pod -L creation_method,environment
```

### **Modifying Labels of Existing Pods**

**Step 1:** Add label to existing pod

```bash
kubectl label pod nginx creation_method=manual
```

**Step 2:** Change of Environment from production to debug

```bash
kubectl label pod nginx environment=debug --overwrite
```

**Step 3:** View the labels and specific labels

```bash
kubectl get pod --show-labels
kubectl get pod -L creation_method,environment
```

### **Solution Scheme**

- Specifying Labels When Creating a Pod: 2.5 Marks
- Modifying Labels of Existing Pods: 2.5 Marks

---

## Question 98: Analyze the procedures a label selector can select resources

### **Listing Subsets of Pods through Label Selectors** (1 Mark)

A label selector can select resources based on whether the resource:

- Contains (or does not contain) a label with a certain key
- Contains a label with a certain key and value
- Contains a label with a certain key, but with a value not equal to the one specified

### **1. Listing Pods using a Label Selector** (2 Marks)

**Step 1:** List the pods created manually

```bash
kubectl get pod -l creation_method=manual
```

**Step 2:** List all pods that include the environment label, irrespective of the value

```bash
kubectl get pod -L environment
```

**Step 3:** List all the pods without environment label

```bash
kubectl get pod -l '!environment'
```

### **2. Using Multiple Conditions in a Label Selector** (2 Marks)

**Step 1:** List multiple conditions in label selector

```bash
kubectl get pod -L creation_method,environment
```

### **Solution Scheme**

- Listing Subsets of Pods through Label Selectors: 1 Mark
- Listing Pods using a Label Selector: 2 Marks
- Using Multiple Conditions in a Label Selector: 2 Marks

---

## Question 99: Investigate the process of annotating Pods

### **Looking Up an Object's Annotations** (2 Marks)

**Step 1:** List the pods created manually

```bash
kubectl get pod kubia -o yaml
```

### **Adding and Modifying Annotations** (2 Marks)

**Step 1:** Add annotations to existing pods

```bash
kubectl annotate pod nginx mycompany.com/someannotation="foo bar"
```

**Step 2:** Modify the existing annotations

```bash
kubectl annotate pod nginx mycompany.com/someannotation="fool bar" --overwrite
```

**Step 3:** Check the annotation with the command

```bash
kubectl describe pod nginx
```

### **Output** (1 Mark)

### **Solution Scheme**

- Looking Up an Object's Annotations: 2 Marks
- Adding and Modifying Annotations: 2 Marks
- Output: 1 Mark

---

## Question 100: Summarize the procedure for examining a YAML descriptor of an existing pod

### **Examining a YAML Descriptor of an Existing Pod** (8 Marks)

**Step 1:** Getting the full YAML of a deployed pod

```bash
kubectl get pod kubia -o yaml
```

**Step 2:** Introducing the main parts of a Pod Definition

### **Creating a Simple YAML Descriptor for a Pod** (4 Marks)

**Step 1:** Basic Pod Manifest: nginx.yaml

**Step 2:** Using kubectl explain to discover possible API object fields

```bash
kubectl explain pods
```

### **Using kubectl create to Create the Pod** (8 Marks)

**Step 1:** Create a new file for a pod in yaml and add the following contents

```bash
sudo nano pod.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
```

**Step 2:** Apply the Yaml file to build the pod

```bash
kubectl apply -f pod.yaml
```

**Step 3:** Viewing the newly created pods in the list of Pods

```bash
kubectl get pods
```

### **Solution Scheme**

- Examining a YAML Descriptor of an Existing Pod: 8 Marks
- Creating a Simple YAML Descriptor for a Pod: 4 Marks
- Using kubectl create to Create the Pod: 8 Marks

---

## Question 101: Illustrate the role of labels in organizing Pods

### **Uncategorized Pods Diagram**

![[Pasted image 20260209202200.png]]

### **Specifying Labels when Creating a Pod**

**Step 1:** Make a pod with labels - nginx.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    environment: production
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
```

**Step 2:** Create the pod with

```bash
kubectl create -f nginx.yaml
```

**Step 3:** View the labels and specific labels

```bash
kubectl get pod --show-labels
kubectl get pod -L creation_method,environment
```

### **Modifying Labels of Existing Pods**

**Step 1:** Add label to existing pod

```bash
kubectl label pod nginx creation_method=manual
```

**Step 2:** Change of Environment from production to debug

```bash
kubectl label pod nginx environment=debug --overwrite
```

**Step 3:** View the labels and specific labels

```bash
kubectl get pod --show-labels
kubectl get pod -L creation_method,environment
```

### **Solution Scheme**

- Introducing Labels: 4 Marks
- Specifying Labels when Creating a Pod: 4 Marks
- Modifying Labels of Existing Pods: 2 Marks

---

## Question 102: Design a Kubernetes cluster organization strategy for a multi-team environment

### **Using Namespaces to Group Resources**

### **1. Understanding the Need for Namespaces** (2 Marks)

Namespaces provide logical isolation for teams, projects, and environments within the same physical cluster.

### **2. Discovering Other Namespaces and Their Pods** (4 Marks)

**Step 1:** List all the namespaces in the cluster

```bash
kubectl get ns
```

**Step 2:** Retrieve all the namespace in the cluster: kube-system

```bash
kubectl get pod -n kube-system
```

Note: `--namespace` can also be used instead of `-n`

### **3. Creating a Namespace** (4 Marks)

**Step 1:** Creating a Namespace - namespace.yaml

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: custom-namespace
```

**Step 2:** Creating a Namespace with kubectl create namespace

```bash
kubectl create namespace custom-namespace
```

### **Solution Scheme**

- Understanding the Need for Namespaces: 2 Marks
- Discovering Other Namespaces and Their Pods: 4 Marks
- Creating a Namespace: 4 Marks

---

## Question 103: Analyze different Pod and resource termination strategies in Kubernetes

### **Stopping and Removing Pods**

### **1. Deleting a Pod by Name** (2 Marks)

**Step 1:** Delete the pod with name

```bash
kubectl delete pod nginx
```

### **2. Deleting Pods using Label Selectors** (2 Marks)

**Step 1:** Deleting pods with specified label

```bash
kubectl delete pod -l creation_method=manual
```

### **3. Deleting Pods by Deleting the Whole Namespace** (2 Marks)

**Step 1:** Deleting pods with specific namespace

```bash
kubectl delete ns custom-namespace
```

### **4. Deleting all Pods in a Namespace, While Keeping the Namespace** (2 Marks)

**Step 1:** Deleting all the pods in a namespace while keeping the namespace

```bash
kubectl delete pod --all
```

### **5. Deleting (almost) All Resources in a Namespace** (2 Marks)

**Step 1:** Deleting all the resources in a namespace

```bash
kubectl delete all --all
```

### **Solution Scheme**

- Deleting a Pod by Name: 2 Marks
- Deleting Pods using Label Selectors: 2 Marks
- Deleting Pods by Deleting the Whole Namespace: 2 Marks
- Deleting all Pods in a Namespace, While Keeping the Namespace: 2 Marks
- Deleting (almost) All Resources in a Namespace: 2 Marks

---

## Question 104: Handle the issue of stopping and removing pods in Kubernetes

### **Stopping and Removing Pods**

### **1. Deleting a Pod by Name** (2 Marks)

**Step 1:** Delete the pod with name

```bash
kubectl delete pod nginx
```

### **2. Deleting Pods using Label Selectors** (2 Marks)

**Step 1:** Deleting pods with specified label

```bash
kubectl delete pod -l creation_method=manual
```

### **3. Deleting Pods by Deleting the Whole Namespace** (2 Marks)

**Step 1:** Deleting pods with specific namespace

```bash
kubectl delete ns custom-namespace
```

### **4. Deleting all Pods in a Namespace, While Keeping the Namespace** (2 Marks)

**Step 1:** Deleting all the pods in a namespace while keeping the namespace

```bash
kubectl delete pod --all
```

### **5. Deleting (almost) All Resources in a Namespace** (2 Marks)

**Step 1:** Deleting all the resources in a namespace

```bash
kubectl delete all --all
```

### **Solution Scheme**

- Deleting a Pod by Name: 2 Marks
- Deleting Pods using Label Selectors: 2 Marks
- Deleting Pods by Deleting the Whole Namespace: 2 Marks
- Deleting all Pods in a Namespace, While Keeping the Namespace: 2 Marks
- Deleting (almost) All Resources in a Namespace: 2 Marks

---

## Question 105: Apply Labels and Namespaces to organize Kubernetes Pods

### **Project Structure**

```
Program/
  --> namespaces.yaml
  --> pods.yaml
```

**Step 1:** Create a File for Namespaces as namespaces.yaml

**Step 2:** Create a File for Pods as pods.yaml

**Step 3:** Namespaces YAML

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
---
apiVersion: v1
kind: Namespace
metadata:
  name: prod
```

**Step 4:** Pods YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend-pod
  namespace: dev
  labels:
    app: frontend
    env: dev
spec:
  containers:
    - name: frontend
      image: nginx
      ports:
        - containerPort: 80
---
apiVersion: v1
kind: Pod
metadata:
  name: backend-pod
  namespace: dev
  labels:
    app: backend
    env: dev
spec:
  containers:
    - name: backend
      image: nginx
      ports:
        - containerPort: 80
---
apiVersion: v1
kind: Pod
metadata:
  name: frontend-pod
  namespace: prod
  labels:
    app: frontend
    env: prod
spec:
  containers:
    - name: frontend
      image: nginx
      ports:
        - containerPort: 80
---
apiVersion: v1
kind: Pod
metadata:
  name: backend-pod
  namespace: prod
  labels:
    app: backend
    env: prod
spec:
  containers:
    - name: backend
      image: nginx
      ports:
        - containerPort: 80
```

**Step 5:** Apply the Namespaces & Pod YAML file

```bash
kubectl apply -f namespaces.yaml
kubectl apply -f pods.yaml
```

**Step 6:** Verify Pods by Namespace

```bash
kubectl get pods -n dev
kubectl get pods -n prod
```

**Step 7:** Check pod status

```bash
kubectl get pods
```

**Step 8:** Filter Pods by Label

List all frontend pods in dev namespace:

```bash
kubectl get pods -n dev -l app=frontend
```

List all backend pods in prod namespace:

```bash
kubectl get pods -n prod -l app=backend
```

**Step 9:** Clean-up process

```bash
kubectl delete pods --all
```

**Step 10:** Delete All Services

```bash
kubectl delete svc --all
```

### **Solution Scheme**

- Project Structure: 2 Marks
- Steps: 16 Marks
- Output: 2 Marks

---

## Question 106: Demonstrate the creation of pods using YAML descriptors as a complete procedure

### **Creating Pods from YAML or JSON Descriptors**

### **1. Examining a YAML Descriptor of an Existing Pod** (6 Marks)

**Step 1:** Getting the full YAML of a deployed pod

```bash
kubectl get pod kubia -o yaml
```

**Step 2:** Introducing the main parts of a Pod Definition

### **2. Creating a Simple YAML Descriptor for a Pod** (8 Marks)

**Step 1:** Basic Pod Manifest: nginx.yaml

**Step 2:** Using kubectl explain to discover possible API object fields

```bash
kubectl explain pods
```

### **3. Using kubectl create to Create the Pod** (8 Marks)

**Step 1:** Create a new file for a pod in yaml and add the following contents

```bash
sudo nano pod.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
```

**Step 2:** Apply the Yaml file to build the pod

```bash
kubectl apply -f pod.yaml
```

**Step 3:** Viewing the newly created pods in the list of Pods

```bash
kubectl get pods
```

### **Solution Scheme**

- Examining a YAML Descriptor of an Existing Pod: 6 Marks
- Creating a Simple YAML Descriptor for a Pod: 8 Marks
- Using kubectl create to Create the Pod: 8 Marks

---

## Question 107: List the key concepts related to Pods in Kubernetes

### **Introducing Pods**

1. **Understanding the Need for Pods** (4 Marks)
2. **Understanding Pods** (8 Marks)
3. **Organizing Containers Across Pods Properly** (8 Marks)

### **Solution Scheme**

- Understanding the Need for Pods: 4 Marks
- Understanding Pods: 8 Marks
- Organizing Containers Across Pods Properly: 8 Marks

---

## Question 108: Compile the steps involved in the process using Namespaces to group resources

### **Using Namespaces to Group Resources**

### **1. Understanding the Need for Namespaces** (4 Marks)

Namespaces provide logical isolation for teams, projects, and environments within the same physical cluster.

### **2. Discovering Other Namespaces and Their Pods** (8 Marks)

**Step 1:** List all the namespaces in the cluster

```bash
kubectl get ns
```

**Step 2:** Retrieve all the namespace in the cluster: kube-system

```bash
kubectl get pod -n kube-system
```

Note: `--namespace` can also be used instead of `-n`

### **3. Creating a Namespace** (8 Marks)

**Step 1:** Creating a Namespace - namespace.yaml

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: custom-namespace
```

**Step 2:** Creating a Namespace with kubectl create namespace

```bash
kubectl create namespace custom-namespace
```

### **Solution Scheme**

- Understanding the Need for Namespaces: 4 Marks
- Discovering Other Namespaces and Their Pods: 8 Marks
- Creating a Namespace: 8 Marks

---

## Question 109: Evaluate the role of annotations in managing Kubernetes Pods

### **Annotating Pods**

### **1. Looking Up an Object's Annotations** (10 Marks)

**Step 1:** List the pods created manually

```bash
kubectl get pod kubia -o yaml
```

### **2. Adding and Modifying Annotations** (10 Marks)

**Step 1:** Add annotations to existing pods

```bash
kubectl annotate pod nginx mycompany.com/someannotation="foo bar"
```

**Step 2:** Modify the existing annotations

```bash
kubectl annotate pod nginx mycompany.com/someannotation="fool bar" --overwrite
```

**Step 3:** Check the annotation with the command

```bash
kubectl describe pod nginx
```

### **Solution Scheme**

- Looking Up an Object's Annotations: 10 Marks
- Adding and Modifying Annotations: 10 Marks

---

## Question 110: Illustrate the uncategorized pods in a microservices architecture

### **1. Introducing Labels** (4 Marks + Diagram)

### **2. Specifying Labels When Creating a Pod** (8 Marks)

**Step 1:** Make a pod with labels - nginx.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    environment: production
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
```

**Step 2:** Create the pod with

```bash
kubectl create -f nginx.yaml
```

**Step 3:** View the labels and specific labels

```bash
kubectl get pod --show-labels
kubectl get pod -L creation_method,environment
```

### **3. Modifying Labels of Existing Pods** (8 Marks)

**Step 1:** Add label to existing pod

```bash
kubectl label pod nginx creation_method=manual
```

**Step 2:** Change of Environment from production to debug

```bash
kubectl label pod nginx environment=debug --overwrite
```

**Step 3:** View the labels and specific labels

```bash
kubectl get pod --show-labels
kubectl get pod -L creation_method,environment
```

### **Solution Scheme**

- Introducing Labels + Diagram: 4 Marks
- Specifying Labels When Creating a Pod: 8 Marks
- Modifying Labels of Existing Pods: 8 Marks

---

## 📚 End of Study Notes

**Total Questions: 110**

**Topics Covered:**

- DevOps Culture and Principles
- Docker Fundamentals and Advanced Concepts
- Container Management and Orchestration
- Continuous Integration and Continuous Deployment (CI/CD)
- Kubernetes Architecture and Components
- Pod Management and Configuration
- Docker Hub and Registry Operations
- Infrastructure as Code (IaC)
- Monitoring and Logging

**Good Luck with Your Exams! 🎓**
