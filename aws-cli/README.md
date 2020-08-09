# AWS CLI
<img align="left" src="Images/Aws.jfif" alt="Made with Kubernetes" title="AWS" width="150px"/>
<img align="left" src="Images/aws-cli.png" alt="Made with WordPress" title="AWS-CLI" width="240px"/>
<br/><br/><br/><br/><br/> 

---

The AWS Command Line Interface (AWS CLI) is an open source tool that enables you to interact with AWS services using commands in your command-line shell. With minimal configuration, the AWS CLI enables you to start running commands that implement functionality equivalent to that provided by the browser-based AWS Management Console from the command prompt in your favorite terminal program:

## Install AWS CLI

Download the latest AWS Cli
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

Unzip downloaded package
```bash
sudo yum install -y unzip
sudo unzip awscliv2.zip
```

Run installer
```bash
sudo ./aws/install
You can now run: /usr/local/bin/aws --version
```

We need to check the AWS CLI version using the following command.
```bash
aws --version
```

---
## Configure AWS CLI

Now we need to configure the AWS CLI with the following command. We need AWS Access Key ID, AWS Secret Access Key to configure the same.

To create a new configuration
```bash
aws configure
```

```bash
AWS Access Key ID: AKIAXIHKUEPBUCNLFVEQ
AWS Secret Access Key: egTasdwradaOkPUJaWfepXYkXFsJz15cY56tGhnzZhK
Default region name: us-east-1
Default output format [None]:
```

---

## Create S3 Bucket via AWS CLI

Use the help argument
```bash
aws s3 help
```

Create S3 Bucket
```bash
aws s3 mb s3://kapil45.net
```

Verify S3 status
```bash
aws s3 ls
```

Upload file in S3 Bucket
```bash
aws s3 cp "path_of_file" 
```

Delete S3 Bucket
```bash
aws s3 rb s3://kapil45.net
```

To delete objects in a bucket or your local directory
```bash
aws s3 rm 
```

Move objects from a bucket or a local directory
```bash
aws s3 mv <source> <target> 
```

Synchronizes the contents of a bucke
```bash
aws s3 sync <source> <target>
```

We can use s3api to create a bucket using AWS CLI
```bash
aws s3api help
```

Run the following command to create a sample bucket on us-east-1 region.
```bash
aws s3api create-bucket --bucket test-bucket-989282 --region us-east-1
```

The output will be like:
```bash
“Location”: /test-bucket-989282 (The bucket name provided)
```

---

## Launch EC2 Instances via AWS CLI

Creat Key Pair
```bash
aws ec2 create-key-pair --key-name Mynewkey --query 'KeyMaterial' --output text > Mynewkey.pem
```

Create Security Group
```bash
aws ec2 create-security-group --group-name MySgGroup --description "Security Group for my Instances"
```

```bash
aws ec2 authorize-security-group-ingress --group-name MySgGroup --protocol tcp --port 22 --cidr 103.53.53.6/32
```

Run EC2 Instance
```bash
aws ec2 run-instances --image-id ami-052c08d70def0ac62 --count 1 --instance-type t2.micro --key-name Mynewkey --security-groups MySgGroup --subnet-id subnet-6e7f829e
```

---

## IAM

```bash
aws iam help
```

Create Group 
```bash
aws iam create-group --group-name Mygroup
```

Create User
```bash
aws iam create-user --user-name Myuser
```

Add User in Group
```bash
aws iam add-user-to-group --user-name Myuser --group-name Mygroup
```

Verify Users in Group
```bash
aws iam get-group --group-name Mygroup
```

Creat User policy
```bash
```

Apply policy to the user
```bash
aws iam put-user-policy --user-name Myuser --policy-name MyPowerUserRole --policy-document file://Mypolicy.json
```

Verify user policies
```bash
aws iam list-user-policies --user-name Myuser
```

Set password
```bash
aws iam create-login-profile --user-name Myuser --password mypassword
```

Create Access key
```bash
aws iam create-access-key --user-name Myuser
```

---

## Key Pair

Create Key-Pair
```bash
aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
```

Display Key-Pair
```bash
aws ec2 describe-key-pairs --key-name MyKeyPair
```

Delete Key-Pair
```bash
aws ec2 delete-key-pair --key-name MyKeyPair
```

---

## Security Group

Create Security Group
```bash
aws ec2 create-security-group --group-name my-sg --description "My security group"
aws ec2 create-security-group --group-name MySgGroup --description "Security Group for my Instances" --vpc-id vpc-1a2b3c4d
```

Check Security Group
```bash
aws ec2 describe-security-groups --group-ids sg-903004f8		//EC2-VPC
aws ec2 describe-security-groups --group-names my-sg			////EC2-Classic
```

Add the range to your security group
```bash
aws ec2 authorize-security-group-ingress --group-name MySgGroup --protocol tcp --port 22 --cidr 103.53.53.6/32
```

Delete Security Group
```bash
aws ec2 delete-security-group --group-id sg-903004f8    //EC2-VPC
aws ec2 delete-security-group --group-name my-sg		//EC2-Classic
```











[LinuxDevOps.in](http://linuxdevops.in)
