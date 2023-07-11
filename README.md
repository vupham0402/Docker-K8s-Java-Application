## Prerequisites
### AWS EC2
- Docker-Engine (ubuntu) using docker-setup in userdata
- Kops (ubuntu)
### Route 53
- Create a hosted zone (public hosted zone and match with domain registrar)
### AWS S3
- Create a bucket for storing the state of Kops
### IAM
- Create kopsadmin user with AdministratorAccess policy 
### Docker 
- usermod -aG docker username
- Create vprofileapp, vprofiledb, and vprofileweb as repository on Docker Hub
### Kubernetes - https://kubernetes.io/docs/setup/production-environment/tools/kops/
- Domain for K8s DNS records (GoDaddy, Google, etc.)
- Install awscli and add kopsadmin info to aws configure
- Set up kubectl (chmod +x ./kubectl && sudo mv kubectl /usr/local/bin/)
- Create a cluster (sudo chmod +x kops-linus-amd64 && sudo mv kops-linus-amd64 /usr/local/bin/kops)
### Domain Registrar
- Create NS records for subdomain pointing to Route 53 hosted zone NS servers
## How to run
### Docker
- docker compose build
- docker images to check whether we have 5 repositories
- docker compose up -d
- docker push the name of the repository to upload these images to Docker Hub
### Kubernetes
- kops create cluster --name="Route 53 hosted zone" --state="AWS S3 bucket" --zones=us-east-1a,us-east-1b --node-count=2 --node-size=t3.micro --master-size=t3.small --dns-zone="your DNS" --node-volume-size=10 --master-volume-size=10
- kops update cluster --name "Route 53 hosted zone" --state="AWS S3 bucket" --yes --admin
- kops validate cluster --name "Route 53 hosted zone" --state="AWS S3 bucket"
- Create a volume for DB (aws ec2 create-volume --availability-zone=us-east-1a --size=3 --volume-type=gp2 and copy volume ID)
- Label nodes that matches with its zone (zone=us-east-1a or zone=us-east-1b)
- kubectl create -f . to read all definition files
- kubectl get pod to check if all pods are running
- kubectl get svc -> external-ip -> access the web app
### Route 53
- Define a simple record that selects above load balancer
  
