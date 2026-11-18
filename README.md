# CI/CD DevOps Security Website

Ye project ek static security/Guarder website ko Docker container mein package aur AWS aur GCP ke deployment workflows ke saath host karne ka end-to-end DevOps example hai.

## Architecture

![Project Architecture](images/Architecture-new.png)

Architecture flow:

1. Developer code GitHub repository mein push karta hai.
2. AWS CodePipeline/CodeBuild ya GCP Cloud Build pipeline trigger hoti hai.
3. Docker image build aur AWS ECR ya Google Artifact Registry mein push ki jaati hai.
4. ECS ya GKE par application deploy hoti hai.
5. AWS Route 53 aur Load Balancer ke through users application access karte hain.
6. CloudWatch aur AWS Secrets Manager jaise services monitoring aur secret management provide karte hain.

## Project Features

- Responsive HTML/CSS website
- Nginx-based Docker image
- AWS CodeBuild build specification
- AWS ECR image registry integration
- AWS ECS deployment configuration
- GCP Cloud Build pipeline
- GKE/Kubernetes deployment manifest
- Application health aur infrastructure monitoring ke liye architecture-ready setup

## Project Structure

```text
.
|-- Dockerfile
|-- buildspec.yml
|-- cloudbuild.yaml
|-- k8s.yaml
|-- index.html
|-- about.html
|-- service.html
|-- guard.html
|-- contact.html
|-- css/
|-- js/
|-- images/
|   `-- Architecture-new.png
`-- fonts/
```

## Technologies

- HTML5
- CSS3 and Bootstrap
- JavaScript
- Docker
- Nginx
- AWS ECR
- AWS CodeBuild
- AWS CodePipeline
- AWS ECS
- AWS Route 53
- Google Cloud Build
- Google Artifact Registry
- Google Kubernetes Engine
- Kubernetes

## Run Locally

Docker installed hona required hai.

```bash
docker build -t guarder-app .
docker run --name guarder-app -p 8080:80 guarder-app
```

Browser mein open karein:

```text
http://localhost:8080
```

Container stop aur remove karne ke liye:

```bash
docker stop guarder-app
docker rm guarder-app
```

## AWS Deployment

`buildspec.yml` AWS CodeBuild build aur image push process define karta hai:

1. AWS ECR mein login karein.
2. Docker image build karein.
3. Image ko ECR repository mein push karein.
4. ECS deployment ke liye `imagedefinitions.json` generate karein.

Deployment se pehle AWS region, account ID, ECR repository name, ECS cluster name aur container name ko apne environment ke according update karein.

## GCP Deployment

`cloudbuild.yaml` Google Cloud Build pipeline define karta hai:

1. Docker image build hoti hai.
2. Image Google Artifact Registry mein push hoti hai.
3. `k8s.yaml` apply karke GKE par deployment update hoti hai.

Cloud Build se pehle in values ko apne GCP project ke according update karein:

```yaml
asia-south1-docker.pkg.dev/<PROJECT-ID>/<REPOSITORY>/<IMAGE>
```

`k8s.yaml` mein 2 replicas aur `LoadBalancer` service configured hai.

GKE par manually deploy karne ke liye:

```bash
gcloud container clusters get-credentials <CLUSTER-NAME> \
  --region <REGION>

kubectl apply -f k8s.yaml
kubectl get service gcp-service
```

## Kubernetes Configuration

`k8s.yaml` mein:

- `gcp-app` Deployment hai
- Default replicas: `2`
- Container port: `80`
- `gcp-service` LoadBalancer service port: `80`

## Security Recommendations

- AWS aur GCP credentials repository mein store na karein.
- Build aur runtime mein IAM least-privilege permissions use karein.
- AWS Secrets Manager ya GCP Secret Manager mein sensitive values rakhein.
- Public images aur dependencies ke vulnerabilities scan karein.
- ECS aur GKE ke services ke liye HTTPS/TLS enable karein.
- Container aur cluster security policies regularly review karein.

## Future Improvements

- Automated unit/integration tests add karein.
- CI/CD pipeline mein vulnerability scanning add karein.
- Infrastructure as Code ke liye Terraform add karein.
- AWS aur GCP mein monitoring dashboards aur alerts configure karein.
- Blue-green ya canary deployment strategy add karein.
- Custom domain aur SSL certificate configure karein.
