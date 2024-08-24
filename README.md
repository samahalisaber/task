Task Documentation
==========
1- install the required packages
```
yum install git wget zip yum-utils podman -y
```
### Clone the Repository:

2- clone the repository
```
git clone https://github.com/sameh-Tawfiq/Microservices
```
### Dockerize the Application:

3- create Dockerfile to containerize the Python application

4- build the Dockerfile
```
cd Microservices
podman build . -t docker.io/samahalisaber/python-task:v1.2
```
5- push the image to image registry
```
podman login docker.io
podman push docker.io/samahalisaber/python-task:v1.2
```
### Provision a Kubernetes Cluster:

6- install terraform 
```
yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
yum -y install terraform
```
7- in aws create IAM user through aws console to use it in provisioning EKS and assign to it the right policies

8- generate access key for the user and save the access key and the secret key

9- prepare terraform files to provision EKS
  *including ( provider, vpc, igw, subnets, routes, eks cluster, node group, IAM policies)*
```
cd terraform
```
10- initialize the working directory and download the necessary provider plugins
```
terraform init
```
11- start provisioning EKS
```
terraform apply
```
12- wait until *apply complete!* message

13- install awscli binaries
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
./aws/install
```
14- update your kubeconfig file to include the credentials and endpoint information for an EKS
```
aws eks --region <region> update-kubeconfig --name <cluster-name>
```
15- now we can use kubectl command 

### Deploy the Microservice:

16- deploy the containerized Python microservice
```
kubectl create -f yamls/deploy.yaml
kubectl create -f yamls/svc.yaml
```
### Expose the Service to the Internet:

17- install nginx ingress controller through helm package manager
```
kubectl create ns ingress-controller
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx -n ingres-controller
```
18- create ingress resource to access the service externally
```
kubectl create -f yamls/ingress.yaml
```
### Implement a CI/CD pipeline:

19- configure kubernetes cloud plugin in jenkins (add EKS url, token for authentication)

20- configure pipeline script from SCM (add repository url, credentials, script path)

30- configure jenkinsfile with build and deploy stages and variable image tag depends on the build number

