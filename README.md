# EKS-storage-AWS-EBS

Install AWS, kubectl & eksctl CLI's

Step-00: Introduction

    Install AWS CLI
    Install kubectl CLI
    Install eksctl CLI
    
Step-01: Install AWS CLI
   Ref:-https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
   
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install --update
aws --version

Step-01-03: Configure AWS Command Line using Security Credentials

    Go to AWS Management Console --> Services --> IAM
    Select the IAM User: kalyan
    Important Note: Use only IAM user to generate Security Credentials. Never ever use Root User. (Highly not recommended)
    Click on Security credentials tab
    Click on Create access key
    Copy Access ID and Secret access key
    Go to command line and provide the required details

aws configure
AWS Access Key ID [None]: ABCDEFGHIAZBERTUCNGG  (Replace your creds when prompted)
AWS Secret Access Key [None]: uMe7fumK1IdDB094q2sGFhM5Bqt3HQRw3IHZzBDTm  (Replace your creds when prompted)
Default region name [None]: us-east-1
Default output format [None]: json

Test if AWS CLI is working after configuring the above

aws ec2 describe-vpcs

