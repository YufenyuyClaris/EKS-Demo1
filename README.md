# EKS-Demo1
EKS cluster setup
Steps:
1. Create a new EC2 Keypair
 (or)
   Use an existing keypair. But Make sure you have the pem file to connect to EC2 instance that will be created in Step2

2. Create a stack with template eks-cluster-cft.yaml and change the parameters accordingly

Parameters:

```
SSHKeyPair=<output from step1>
EnvironmentName=<<Name of environment> --> By default it is eks-cluster
```

This CFT provisions the following resources

    - Entire VPC Setup
    - EKS Cluster
    - EKS Cluster role --> https://docs.aws.amazon.com/eks/latest/userguide/service_IAM_role.html
    - EKS Node role --> https://docs.aws.amazon.com/eks/latest/userguide/create-node-role.html
    - EKS Cluster Security group
    - EKS Node Security group
    - EKS Nodegroup
    - ALB with dummy loadbalancer
    - A Public EC2 instance with required tools installed

3. Login to the EC2 instance created & Run below command
```
aws configure
```
--> Fill secret key and access key of your user (If you don't have them, create new access key and secret key)

Now, EC2 instance is setup with all tools required for EKS Advanced demo

Configuring the EC2 instance to be able to access nodes

# Kubectl Installation
          curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.23.15/2023-01-11/bin/linux/amd64/kubectl
          chmod +x ./kubectl
          mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
          echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
          kubectl version --short --client
 
 #AWS IAM Authenticator Installation
          curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/aws-iam-authenticator_0.5.9_linux_amd64
          chmod +x ./aws-iam-authenticator
          mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$PATH:$HOME/bin
          echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
          aws-iam-authenticator help   
         
    # Kubeconfig file configuration
          aws eks --region ${AWS::Region}  update-kubeconfig --name ${EnvironmentName}
          export KUBECONFIG=~/.kube/config
          curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
          sudo mv /tmp/eksctl /usr/bin
          eksctl version
         
    # Helm installation
          curl -L https://git.io/get_helm.sh | bash -s -- --version v3.8.2
         
    # OpenSSL Installation
          yum install openssl11 -y
          rm -rf /bin/openssl
          mv /bin/openssl11 /bin/openssl

# Getting the pods
    clone github url link
    Deploy the two containers using 
    kubectl apply -f containername.yaml
    Kubectl get pods
    kubectl exec -it containername -- bin/bash
    
#  Database configuration. Here 2 seperate databases will be deployed. map the database ports to thier respective containers and deploy the databases.
    cd into the path having the database configuration file and add the secrets
    kubectl apply -f mongo-secrets.yaml
    Reference the secret in the database configuration file
    kubectl apply -f mongodb.yaml
    kubectl get all
    kubectl get pod
    
    
# We have to create an internal service
  kubectle apply -f mongodb.yaml (to deploy) the services in thesame file)
  
  
      
    
    
    

    
