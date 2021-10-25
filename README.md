https://www.kubeflow.org/docs/distributions/aws/deploy/install-kubeflow/

## Full build
1) Set env variable
```shell
export AWS_CLUSTER_NAME=harish-<somename>
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
- name: kubeflow-mng-${AWS_CLUSTER_NAME}
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

5) Run
```shell
docker run -it  -v ~/.kube/:/root/.kube -v ~/.aws:/root/.aws  harishb2k/kfctl /bin/bash

>> export AWS_CLUSTER_NAME=harish-<your cluster name>
>> export AWS_REGION=ap-south-1
>> export PATH=$PATH:/kubeflow-setup/
>> mkdir ${AWS_CLUSTER_NAME} && cd ${AWS_CLUSTER_NAME}
>> cp ../kfctl_aws.yaml .
>> kfctl apply -V -f kfctl_aws.yaml
```

6) Get the end-point
```shell
kubectl get ingress -n istio-system
```

### Build Cli -> not needed for docker setup
```shell
 go build -o kfctl cmd/kfctl/main.go
```