# kfctl

_kfctl_ is the control plane for deploying and managing Kubeflow. The primary mode of deployment is to use [kfctl as a CLI](https://github.com/kubeflow/kfctl/tree/master/cmd/kfctl) with KFDef configurations for different Kubernetes flavours to deploy and manage Kubeflow. Please also look at the docs on [Kubeflow website](https://www.kubeflow.org/docs/started/getting-started/) for deployments options for different cloud providers

Additionally, we have also introduced [Kubeflow Operator](./operator.md) in incubation mode, which apart from deploying Kubeflow, will perform additional functionalities around monitoring the deployment for consistency etc. 

### IMP
Use branch "v1.2-branch". Instruction given in readme in v1.2-branch.


### Build and run
Make sure when you download the "kfctl_aws.yaml" file then comment "enablePodIamPolicy" property in the file.
```shell
go run cmd/kfctl/main.go apply -V -f <something>/<eks_cluster_name>/kfctl_aws.yaml 

NOTE: this command may faile first time. Try again...
```