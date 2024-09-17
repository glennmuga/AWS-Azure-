## *EKS-Cluster*

### *Create an EC2 instance EKS-mgr   Ubuntu t2.micro All traffic 1x10*
### *Then create a role and give full access to Elasticcontainer full access ,amazonekscluster policy and IAM full*
### *unzip file*
 - apt install unzip -y
 - curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
 - unzip awscliv2.zip
### *install aws*
 - sudo ./aws/install
### *configure aws*
 - aws configure

(*Region without a,b and in output format enter table*)
   
### *Install EKS Tool*
 - curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
 - sudo mv /tmp/eksctl /usr/local/bin
 - eksctl version
### *Install Kubectl*
 
 -  curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
 -  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
 -  kubectl version --client
### *Create EKS Cluster*
 -  eksctl create cluster --name my-cluster --region region-code --version 1.29 --vpc-public-subnets subnet-ExampleID1,subnet-ExampleID2 --without-nodegroup

 (*Here in this case just add the region us-west-1 instead of region code, two empty subnets from vpc < subnet instead of ExampleID1 and ID2*)

   ### *Creating node3 group*
   
  - ssh-keygen
    
  eksctl create nodegroup \
  --cluster my-cluster \
  --region us-east-2 \
  --name my-node-group \
  --node-ami-family Ubuntu2004 \
  --node-type t2.small \
  --subnet-ids subnet-086ced1a84c94a342,subnet-01695faa5e0e61d97 \
  --nodes 3 \
  --nodes-min 2 \
  --nodes-max 4 \
  --ssh-access \
  --ssh-public-key /root/.ssh/id_rsa.pub
    
    (*Change the subnet here as previously done*)
    (*Change the region here*)

### *When You want to delete cluster*
  - eksctl delete cluster --name my-cluster

### *Create replicaset*
### *Replicaset is creating multiple pods simultaneously*
  - vim replicaset.yml
   
### *Paste the below content inside* 
    
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  replicas: 4
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: apache-app
        image: nginx

  - kubectl apply -f replicaset.yml
  - kubectl get pods

### *Here after pod just paste one of the pods created simultaneously with the same timings*

  - kubectl describe pod frontend-2j7rv
   

### *When number of pods are reduced the last pod will be deleted*
### *When number of pods are increased the it will add on to the number of existing pods*
### *Resizing the pod size with autoscaling and manual scaling*
### *Once the pods are autoscaled they can't be autoscaled back again thus in that case they need to be manually scaled*
### *Manual scaling is vertical and automatic scaling is horizontal*

(*This command will just give an overview of all the pods in detail*)

- kubectl get pod -o wide
- kubectl autoscale rs frontend --max=10 --min=3

### *Manual scaling*

- kubectl scale --replicas=9 -f replicaset.yml

- kubectl get rs

### *When You want to delete cluster*

- eksctl delete cluster --name my-cluster

### *Deployment*

- vim glenn-deployment.yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: glenn-deployment
  labels:
    app: nginx
spec:
  replicas: 3
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
        image: nginx:1.14.2
        ports:
        - containerPort: 80

  - kubectl apply -f mahek-deployment.yml
  - kubectl get pods
  - kubectl get rs
  - kubectl get deployment
  - kubectl describe deployment glenn-deployment
  - kubectl autoscale deployment glenn-deployment --max=10 --min=3
  - kubectl get pods

### *Rolling Updates*

(*Changing the version of nginx*)

  -  kubectl set image deployment.v1.apps/glenn-deployment nginx=nginx:1.16.1  
  -  kubectl describe deployment glenn-deployment
  -  kubectl edit deployment/glenn-deployment

     (*change max version 50% and change version 1.14.2*)

### *Create new instance with amazon linux and then install awi and docker*
 - yum install unzip -y
 - curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
 - unzip awscliv2.zip
 - sudo ./aws/install
### *Install Docker*
 - yum install docker -y
 - docker info
 - systemctl start  docker
 - systemctl enable docker
 - docker info
### *ECR*
 -  wget https://www.free-css.com/assets/files/free-css-templates/download/page295/antique-cafe.zip

    (*Go to free-css website and select a template >> right click on the download button and copy link*)

 -  vim Dockerfile

   (*This is if you wish to pull the image from other website*)

    FROM centos:latest
    MAINTAINER mglen05@gmail.com
    RUN yum install httpd -y
    RUN yum install zip -y
    RUN yum install unzip -y
    ADD https://www.free-css.com/assets/files/free-css-templates/download/page295/antique-cafe.zip
    WORKDIR /var/www/html
    RUN unzip antique-cafe.zip
    RUN cp -rvf.antique-cafe.zip/* .
    RUN rm -rf antique-cafe.zip
    CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
    EXPOSE 80

(*This is if you wish to create your own image*)

    FROM ubuntu:22.04
    RUN apt-get update && apt-get install -y apache2
    RUN apt-get install -y tree openssh-server openssh-client
    RUN cd /var/www/html
    RUN echo "Welcome to Devops" > /var/www/html/index.html
    RUN service apache2 start

(*Now on dashboard go to AWS Elastic Container Registry > name it as lti-devops > keep it mutable > create > click on lti devops > view push command > copy the commands uptill docker build -t lti-devops*)

   docker build -t lti-devops .
   docker images
   
(*These are the commands from the ECR of dashboard*)
   
   docker tag lti-devops:latest 481665131265.dkr.ecr.us-west-1.amazonaws.com/lti-devops:latest
   docker push 481665131265.dkr.ecr.us-west-1.amazonaws.com/lti-devops:latest

(*Then go to ECR >> click on the lti-devops >> action >> images >> copy URL to deploy in the pod*)

 ### *Go to your previous cluster >> install nodes >> Do the deployment and edit image with the copied URL*

 - vim glenn-deployment.yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mahek-deployment
  labels:
    app: nginx
spec:
  replicas: 3
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
        image: (*paste the URL*)
        ports:
        - containerPort: 80

  - kubectl apply -f glenn-deployment.yml
  - kubectl get pods
  - kubectl get deployment
  - kubectl describe deployment glenn-deployment
