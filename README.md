## Full build
1) Set env variable
```shell
export AWS_CLUSTER_NAME=harish-cluster
export AWS_REGION=ap-south-1
export K8S_VERSION=1.18
export EC2_INSTANCE_TYPE=m5.large
```

2) Start cluster
```shell
   cat << EOF > cluster.yaml
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
name: ${AWS_CLUSTER_NAME}
version: "${K8S_VERSION}"
region: ${AWS_REGION}
managedNodeGroups:
- name: harish-delete-safe-kubeflow-mng
  desiredCapacity: 3
  instanceType: ${EC2_INSTANCE_TYPE}
  EOF
```
```shell
eksctl create cluster -f cluster.yaml
```

4) Get the IAM role
```shell
aws iam list-roles \
| jq -r ".Roles[] \
| select(.RoleName \
| startswith(\"eksctl-$AWS_CLUSTER_NAME\") and contains(\"NodeInstanceRole\")) \
.RoleName"

>>> Output => eksctl-something.... 
```

5) Clone
```shell
   git clone  https://github.com/harishb2k/kfctl.git
   git checout v1.2-branch
```
7) Build docker and run
```shell
>> docker build -t kf .
>> docker build  -f DockerfileFinal -t kff .

Run:
>> docker run -it  -v ~/.kube/:/root/.kube -v ~/.aws:/root/.aws  kff /bin/bash
>> apt-get install -y awscli
>> Type 6 
>> Type 44

>> Go to /kubeflow-setup
   Edit "kfctl_aws.yaml"
   >> region: ap-south-1
   enablePodIamPolicy: true
   roles:
   - <Role from step (4)>

>> export PATH=$PATH:/kubeflow-setup/

>>  kfctl apply -V -f kfctl_aws.yaml
```



### Build Cli -> not needed for docker setup
```shell
 go build -o kfctl cmd/kfctl/main.go
```