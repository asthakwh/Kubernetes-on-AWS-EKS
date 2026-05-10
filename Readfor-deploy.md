Deployment of a Two-Tier Flask Application on AWS EKS using Kubernetes
        
        sudo apt update &&  sudo apt upgrade -y
        curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
        sudo apt install unzip
        unzip awscliv2.zip
        sudo ./aws/install
        aws --version 

install aws cli unzip it and run
configure aws first to access
 
        aws configure
        AWS Access Key ID [None]: 
        AWS Secret Access Key [None]: 
        Default region name [None]: ap-south-1
        Default output format [None]:
to create nodes

step 1:
        install kubectl from official website

        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

        curl -LO https://dl.k8s.io/release/v1.36.0/bin/linux/amd64/kubectl
        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
        echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
        sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
        kubectl version --client

step2:
To check is is linux x86 platform or not
[          PLATFORM=$(uname -s)_$(uname -m)
echo $PLATFORM        ]# run as a command
curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_${PLATFORM}.tar.gz"
tar -xzf eksctl_${PLATFORM}.tar.gz -C /tmp
sudo install -m 0755 /tmp/eksctl /usr/local/bin
        eksctl version
to check installation
        eksctl create cluster --name spark-cl --region ap-south-1 --node-type t3.small --nodes-min 2 --nodes-max 3
 
        kubectl get nodes
        git clone https://github.com/LondheShubham153/two-tier-flask-app.git
  Also create  cluster in was

        cd two-tier-flask-app/
        cd eks-manifests/
        ls
        kubectl apply -f mysql-secrets.yml -f mysql-configmap.yml -f mysql-deployment.yml -f mysql-svc.yml
        kubectl apply -f two-tier-app-deployment.yml -f two-tier-app-svc.yml
        ls
        kubectl get all
        kubectl get pods
        kubectl describe pods
        kubectl get svc
To Test app:
You’ll get external ip with 
        kubectl get svc

Copy that link ends with (ap-south-1.elb.amazonaws.com )
Paste on browser
        http://af28b908e80e549a9b8ebd2d8fdaac55-1370981855.ap-south-1.elb.amazonaws.com

        kubectl get pods
Check Running pods

ubuntu@ip-172-31-8-38:~/two-tier-flask-app/eks-manifests$ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
mysql-5d56b4b848-hvm9l          1/1     Running   0          9m43s
two-tier-app-6d6ddddffd-5pk59   1/1     Running   0          9m30s

To delete first delete node groups (instance) then cluster 
        eksctl delete cluster --name spark-cl --region ap-south-1
