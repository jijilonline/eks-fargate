https://eksctl.io/usage/fargate-support/
https://souravkarmakar96.medium.com/deploy-amazon-eks-cluster-with-fargate-linux-nodes-through-eksctl-49ebd38036f5

### Creating a cluster with Fargate support
```
eksctl create cluster --fargate --name="eksctl-ferocious-wardrobe-1729817127-cluster"
eksctl get fargateprofile --cluster ridiculous-painting-1574859263 -o yaml

eksctl get fargateprofile --cluster
```
### Creating a cluster with Fargate support using a config file# An example of ClusterConfig with a normal nodegroup and a Fargate profile.
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: fargate-cluster
  region: ap-northeast-1

nodeGroups:
  - name: ng-1
    instanceType: m5.large
    desiredCapacity: 1

fargateProfiles:
  - name: fp-default
    selectors:
      # All workloads in the "default" Kubernetes namespace will be
      # scheduled onto Fargate:
      - namespace: default
      # All workloads in the "kube-system" Kubernetes namespace will be
      # scheduled onto Fargate:
      - namespace: kube-system
  - name: fp-dev
    selectors:
      # All workloads in the "dev" Kubernetes namespace matching the following
      # label selectors will be scheduled onto Fargate:
      - namespace: dev
        labels:
          env: dev
          checks: passed

