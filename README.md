# EKS-storage-AWS-EBS

Install AWS, kubectl & eksctl CLI's

Step-00: Introduction

    Install AWS CLI
    Install kubectl CLI
    Install eksctl CLI
    
Step-01: Install AWS CLI
   Ref:-https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
   
   
1)curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"


2)unzip awscliv2.zip

3)sudo ./aws/install --update


4)aws --version

Step-01-03: Configure AWS Command Line using Security Credentials

    Go to AWS Management Console --> Services --> IAM
    Select the IAM User: kalyan
    Important Note: Use only IAM user to generate Security Credentials. Never ever use Root User. (Highly not recommended)
    Click on Security credentials tab
    Click on Create access key
    Copy Access ID and Secret access key
    Go to command line and provide the required details

1)aws configure

2)AWS Access Key ID [None]: ABCDEFGHIAZBERTUCNGG  (Replace your creds when prompted)

3)AWS Secret Access Key [None]: uMe7fumK1IdDB094q2sGFhM5Bqt3HQRw3IHZzBDTm  (Replace your creds when prompted)

4)Default region name [None]: us-east-1

5)Default output format [None]: json

[Test if AWS CLI is working after configuring the above]

1)aws ec2 describe-vpcs


Step-02: Install kubectl CLI

Ref:- https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
-----
mkdir kubectlbinary

cd kubectlbinary

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.24.7/2022-10-31/bin/linux/amd64/kubectl

chmod +x ./kubectl

mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin

echo 'export PATH=$PATH:$HOME/bin' >> ~/.bash_profile

kubectl version --short --client


Step-03: Install eksctl CLI

Ref:- https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html#installing-eksctl
------
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin

eksctl version


Create EKS Cluster & Node Groups
--------------------------------

Step-00: Introduction

    Create EKS Cluster
    Associate EKS Cluster to IAM OIDC Provider
    Create EKS Node Groups
    Verify Cluster, Node Groups, EC2 Instances, IAM Policies and Node Groups
    

Step-01: Create EKS Cluster using eksctl

 [It will take 15 to 20 minutes to create the Cluster Control Plane]

# Create Cluster
eksctl create cluster --name=eksdemo1 \
                      --region=us-east-1 \
                      --zones=us-east-1a,us-east-1b \
                      --without-nodegroup 

# Get List of clusters
eksctl get cluster                  


Step-02: Create & Associate IAM OIDC Provider for our EKS Cluster

    To enable and use AWS IAM roles for Kubernetes service accounts on our EKS cluster, we must create & associate OIDC identity provider.
    To do so using eksctl we can use the below command.
    Use latest eksctl version (as on today the latest version is 0.21.0)

# Template
eksctl utils associate-iam-oidc-provider \
    --region region-code \
    --cluster <cluter-name> \
    --approve

# Replace with region & cluster name
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster eksdemo1 \
    --approve


Step-03: Create EC2 Keypair

   1) Create a new EC2 Keypair with name as kube-demo
   2) This keypair we will use it when creating the EKS NodeGroup.
   3) This will help us to login to the EKS Worker Nodes using Terminal.

Step-04: Create Node Group with additional Add-Ons in Public Subnets

1)These add-ons will create the respective IAM policies for us automatically within our Node Group role.

# Create Public Node Group  
    
eksctl create nodegroup --cluster=eksdemo1 \
                       --region=us-east-1 \
                       --name=eksdemo1-ng-public1 \
                       --node-type=t3.medium \
                       --nodes=2 \
                       --nodes-min=2 \
                       --nodes-max=4 \
                       --node-volume-size=20 \
                       --ssh-access \
                       --ssh-public-key=kube-demo \
                       --managed \
                       --asg-access \
                       --external-dns-access \
                       --full-ecr-access \
                       --appmesh-access \
                       --alb-ingress-access 



Step-05: Verify Cluster & Nodes
    
[Verify Cluster, NodeGroup in EKS Management Console]

 1)Go to Services -> Elastic Kubernetes Service -> eksdemo1

List Worker Nodes
-----------------    
    
# List EKS clusters
eksctl get cluster

# List NodeGroups in a cluster
eksctl get nodegroup --cluster=<clusterName>

# List Nodes in current kubernetes cluster
kubectl get nodes -o wide   
    
    
Login to Worker Node using Keypai kube-demo
-------------------------------------------

# For MAC or Linux or Windows10
ssh -i kube-demo.pem ec2-user@<Public-IP-of-Worker-Node>    
    
    
    
    
    






    
    
    
    
    
    
    
    
    
    
    
