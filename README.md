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















