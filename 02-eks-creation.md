## EKS Setup

## EKS Clusters Creation

eksctl create cluster --name hub-cluster --region us-east-1

eksctl create cluster --name spoke-cluster-1 --region us-east-1

eksctl create cluster --name spoke-cluster-2 --region us-east-1

## EKS Clusters Deletion

eksctl delete cluster --name hub-cluster --region us-east-1

eksctl delete cluster --name spoke-cluster-1 --region us-east-1

eksctl delete cluster --name spoke-cluster-2 --region us-east-1

## Configure kubectl for EKS

    - aws eks update-kubeconfig --name your-cluster-name

    - Verify the configuration by running a kubectl command against your EKS cluster:

    ```bash
    kubectl get nodes
    ```