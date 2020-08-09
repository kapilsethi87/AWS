<img src="/kops/Images/Kops.jpg" width="400px" alt="kops logo">

# kops - Kubernetes Operations

## What is kops?

We like to think of it as `kubectl` for clusters.

`kops` helps you create, destroy, upgrade and maintain production-grade, highly
available, Kubernetes clusters from the command line. AWS (Amazon Web Services)
is currently officially supported, with GCE and OpenStack in beta support, and VMware vSphere
in alpha, and other platforms planned.

Installing and launching a Kubernetes cluster hosted on AWS, GCE, DigitalOcean or OpenStack

See [Official Document](https://kops.sigs.k8s.io/getting_started/install/)


## KOPS Configuration in AWS::

### Step 1:
Create Ubuntu EC2 instance

---

### Step 2:
Install Aws Cli
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install -y unzip
sudo unzip awscliv2.zip
sudo ./aws/install
```

Check AWS CLI Version
```bash
aws --version
```

Create IAM user & make a note of access key & secruity key
aws configure -- Give access & security access key details here.
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

### Step 3:
Install Kubectl on AWS

Download the latest release with the command:
```bash
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
```

Make the kubectl binary executable.
```bash
sudo chmod +x ./kubectl
```

Move the binary in to your PATH.
```bash
sudo mv ./kubectl /usr/local/bin/kubectl
```

Verify kubectl
```bash
kubectl --help
```

---


### Step 4:
Create role with below access and attach to the instance you would use to create the Kops cluster, so that you do not have to expose the secret and access key 
```bash
AmazonEC2FullAccess
AmazonS3FullAccess
AmazonRoute53FullAccess
IAMFullAccess
```

---

### Step 5:
Install KOPS

Download the latest release with the command:
```bash
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
```

Make the kops binary executable
```bash
sudo chmod +x kops-linux-amd64
```

Move the kops binary in to your PATH.
```bash
sudo mv kops-linux-amd64 /usr/local/bin/kops
```

Verify Kops
```bash
kops --help
```

Verify Kops Version
```bash
kops version
```

---

### Step 6:
Create a route53 domain for your cluster

---

### Step 7:
Create S3 bucket
```bash
aws s3 mb s3://kartik5.net
```

You can export path and kops would use default location. It is suggested to export this path in profile.
```bash
export KOPS_STATE_STORE=s3://kartik5.net
```

---

### Step 8:
Create sshkeys before creating cluster & this keypair is used for ssh into kubernetes cluster
```bash
ssh-keygen
```

---

### Step 9:

We will need to note which availability zones are available to us. In this example we will be deploying our cluster to the us-west-2 region.
```bash
aws ec2 describe-availability-zones --region us-east-2
```

Create Kubernetes cluster using kops
```bash
kops create cluster --zones=us-east-2a --name=kartik5.net --dns-zone=kapil1.net --dns private
```

Check Cluster
```bash
kops get cluster
```

Below command may take some time to create the required infrastructure resources on AWS. Execute the validate command to check its status and wait until the cluster becomes ready
```bash
kops update cluster --name kapil1.net –yes
```

For the above above command, you might see validation failed error initially when you create cluster and it isexpected behaviour, you have to wait for some more time and check again.
```bash
kops validate cluster
```

To list nodes
```bash
kubectl get nodes 
```


We are done with the installation; remember you will have to wait for couple of minutes to ready the k8s cluster.

---

## Deploying Nginx container on Kubernetes

Deploying Nginx Container
```bash
kubectl run sample-nginx --image=nginx --replicas=2 --port=80
kubectl get pods
kubectl get deployments
```

Expose the deployment as service. This will create an ELB in front of those 2 containers and allow us to publicly access them:
```bash
kubectl expose deployment sample-nginx --port=80 --type=LoadBalancer
kubectl get services -o wide
```

To delete cluster
```bash
kops delete cluster dev.k8s.valaxy.in --yes
```



[LinuxDevOps.in](http://linuxdevops.in)