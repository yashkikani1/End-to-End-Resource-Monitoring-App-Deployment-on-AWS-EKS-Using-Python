# **Cloud Native Resource Monitoring Python App on K8s!**
📌 Work Completed

1. Developed a real-time system monitoring application using Python Flask and `psutil`.
2. Enabled Plotly-based visualization for CPU and memory usage.
3. Containerized the Python application using Docker.
4. Created a Dockerfile, built the image, and ran the application in a Docker container.
5. Set up an AWS ECR repository using Python `boto3` and pushed the Docker image to it.
6. Created an EKS cluster and node group in AWS.
7. Deployed the containerized app on Kubernetes using Python (`kubernetes` library).
8. Created Kubernetes deployments and services programmatically.

---

✅pre rerequisites

Make sure you have the following before starting:

✅ AWS Account
✅ AWS CLI configured with programmatic access
✅ Python 3 installed
✅ Docker and `kubectl` installed
✅ Code editor (e.g., VS Code)

✨ Let’s Start the Project

## **Part 1: Deploying the Flask Application Locally**

### Step 1: Clone the Repository

```bash
git clone <repository_url>
```

### Step 2: Install Dependencies

```bash
pip3 install 
```

### Step 3: Run the Application

```bash
python3 app.py
```

Go to [http://localhost:5000/](http://localhost:5000/) to view the app.



## **Part 2: Dockerizing the Flask Application**

### Step 1: Create a Dockerfile

```Dockerfile
FROM python:3.9-slim-buster
WORKDIR /app
COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt
COPY . .
ENV FLASK_RUN_HOST=0.0.0.0
EXPOSE 5000
CMD ["flask", "run"]
```

### Step 2: Build the Docker Image

```bash
docker build -t <image_name> .
```

### Step 3: Run the Docker Container

```bash
docker run -p 5000:5000 <image_name>
```

Check the app at [http://localhost:5000/](http://localhost:5000/)



## **Part 3: Pushing the Docker Image to ECR**

### Step 1: Create an ECR Repository Using Python

```python
import boto3

ecr_client = boto3.client('ecr')
repository_name = 'my-ecr-repo'
response = ecr_client.create_repository(repositoryName=repository_name)
print(response['repository']['repositoryUri'])
```

### Step 2: Push Docker Image to ECR

Use your AWS Console or CLI for authentication, then:

```bash
docker push <ecr_repo_uri>:<tag>


Part 4: Deploying on Kubernetes (EKS) via Python

Step 1: Create EKS Cluster

Create an EKS cluster via the AWS Console or CLI.

Step 2: Add Node Group

Attach a managed node group to the cluster.

 Step 3: Deploy Using Python

```python
from kubernetes import client, config

config.load_kube_config()
api_client = client.ApiClient()

deployment = client.V1Deployment(
    metadata=client.V1ObjectMeta(name="my-flask-app"),
    spec=client.V1DeploymentSpec(
        replicas=1,
        selector=client.V1LabelSelector(match_labels={"app": "my-flask-app"}),
        template=client.V1PodTemplateSpec(
            metadata=client.V1ObjectMeta(labels={"app": "my-flask-app"}),
            spec=client.V1PodSpec(
                containers=[
                    client.V1Container(
                        name="my-flask-container",
                        image="568373317874.dkr.ecr.us-east-1.amazonaws.com/my-cloud-native-repo:latest",
                        ports=[client.V1ContainerPort(container_port=5000)]
                    )
                ]
            )
        )
    )
)

apps_api = client.AppsV1Api(api_client)
apps_api.create_namespaced_deployment(namespace="default", body=deployment)

service = client.V1Service(
    metadata=client.V1ObjectMeta(name="my-flask-service"),
    spec=client.V1ServiceSpec(
        selector={"app": "my-flask-app"},
        ports=[client.V1ServicePort(port=5000)]
    )
)

core_api = client.CoreV1Api(api_client)
core_api.create_namespaced_service(namespace="default", body=service)
```

📝 **Reminder:** Replace the image URI on line 25 with your own ECR image URI.



✅ Check Kubernetes Resources:

```bash
kubectl get deployment -n default
kubectl get service -n default
kubectl get pods -n default
```

🔗 Expose the App via Port-Forwarding:

```bash
kubectl port-forward service/<service_name> 5000:5000
```

Now open [http://localhost:5000/](http://localhost:5000/) in your browser.

---

Would you like this as a downloadable `.md` file or an HTML web version?
