## Provisioning EKS Cluster using `eksdemo` tool

Provisioning EKS Cluster and managing it is a tedius task, right? But, what if I say you can do both with a single tool?. Yes, you heard it right. In this article, we will see how we can provision and manage an EKS Cluster using a tool named `eksdemo`.

Now you may have doubt like already we have `eksctl` it can also provision the EKS Cluster and manage the complex task like creating VPC, Subnets, Security Groups, etc by using single command. Then why we need another tool additionally. Right? Let me explain you.

### What is eksdemo?

**`eksdemo` is a tool that helps you to provision and manage EKS Cluster with a single command. Actaully It is built on top of eksctl. It is just a wrapper around eksctl**. It have all the features of eksctl and also have some additional features like

- Managing Application Catalog
- Handling Dependencies for Applications
- Query and search AWS resources using `kubectl-like` commands
- Table Based Output and More.

You may not understand the above features now. But I will try to explain you with an example.

## Example Usage of `eksdemo`:

The one of the best feature of `eksdemo` is it can manage the dependencies for the applications very easily.<br>

Let's consider an example, Let's say we have provisioned the EKS Cluster using `eksctl` or any other approaches already. And now we want to install the `karpenter` application on the EKS Cluster. The traditional way is we need to install the `karpenter` application manually on the EKS Cluster by running multiple commands and we have manage the dependency by ourselves like creating IAM roles, policies, Subnets, etc. But with the `eksdemo` tool, we can install the `karpenter` application on our EKS Cluster with a single command. It will manage the dependencies for the `karpenter` application automatically.

Let's say we want to install `karpenter` application on our EKS Cluster, we can do it with a single command like below:

```bash
eksdemo install autoscaling-karpenter -c <cluster-name>
```
The above command will install the `karpenter` on the cluster automatically. And it will also the manage the dependencies for the `karpenter` application.

**Note:** `eksdemo` is still in the beta version. Don't use it in the production environment. But it is very powerful tool for the development and testing environment because it can spin up cluster quickly and effinciently.<br>

Now, Let's see how we can install and use the `eksdemo` tool.

## Pre-requisites

- An AWS account with necessary permissions to create EKS clusters.
- A Machine with AWS CLI, eksctl, and kubectl installed on it.

If you don't have these tools installed on your machine, you can follow the below links to install them:

- [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- [Install eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)
- [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Steps to Install and Use `eksdemo` Tool

### Installing `eksdemo`

Navigate to the [Releases page of eksdemo](https://github.com/awslabs/eksdemo/releases/tag/v0.15.0) and download the binary file based on your OS. For example, If you are using or Linux or Mac, You can download the binary file using `curl` or `wget` command.
![Screenshot 2024-08-07 114539](https://github.com/user-attachments/assets/cb859c70-7245-4410-97c5-9dd242ca3ba9)


### For Linux:

```bash
wget https://github.com/awslabs/eksdemo/releases/download/v0.15.0/eksdemo_Linux_x86_64.tar.gz
```

Extract the tar file using the below command:

```bash
tar -xvf eksdemo_Linux_x86_64.tar.gz
```

Move the binary file to the `/usr/local/bin` directory.

```bash
sudo mv eksdemo /usr/local/bin/
```

![Screenshot 2024-08-07 152305](https://github.com/user-attachments/assets/acf2e5a1-dfbe-4b8b-b6bc-dfdcbf39ffe9)


Now, you can verify the installation by running the below command:

```bash
eksdemo version
```
For Mac, You can use te same method like above. And for Windows, You can download the binary file from the releases page and you can add the binary file to the PATH.

### Provisioning EKS Cluster

Now, we have installed the `eksdemo` tool. Let's see how we can provision the EKS Cluster using the `eksdemo` tool.

- Run the below command to create the EKS Cluster:

```bash
eksdemo create cluster -c <cluster-name>
```
![Screenshot 2024-08-07 152215](https://github.com/user-attachments/assets/392a493f-0d72-43ab-9709-55f80e3fec88)
![Screenshot 2024-08-07 152226](https://github.com/user-attachments/assets/6b4a8bb6-6735-4c94-954a-8851687622f8)
![Screenshot 2024-08-07 152236](https://github.com/user-attachments/assets/8bb307a6-14dd-4787-bc3e-710addfb2fb8)
![Screenshot 2024-08-07 152249](https://github.com/user-attachments/assets/e35ae981-0c0d-4d2b-949d-a65bd6b548f0)

The above command will create the EKS Cluster with the default configurations. Also one thing to mention, as I told earlier, `eksdemo` is built on top of `eksctl`. So behind the scenes, it will use the `eksctl` to create the EKS Cluster.

- You can verify the EKS Cluster creation by running the below command:

```bash
eksdemo get cluster
```
![Screenshot 2024-08-07 152520](https://github.com/user-attachments/assets/21b329c7-4861-44a9-8e03-62996642dec4)

The above command will show the EKS Cluster details in a user-friendly table format.

- You can also explore the `AWS Console` to see the EKS Cluster details and the resources provisioned for the EKS Cluster.
![Screenshot 2024-08-07 152609](https://github.com/user-attachments/assets/1c559f3e-a999-4a23-82d9-4f5e76fa376d)
![Screenshot 2024-08-07 152629](https://github.com/user-attachments/assets/4ee670d7-10a9-42ce-ba78-68d95c40574f)
![Screenshot 2024-08-07 152642](https://github.com/user-attachments/assets/a35bb7de-3b3d-4606-b456-07adf6284de9)


The above command will show the EKS Cluster details in a user-friendly table format.

### Installing `Karpeneter` Application(Managing Application Catalog)

- As I told earlier, You can install the applications on the EKS Cluster with a single command using the `eksdemo` tool. Let's see how we can install the `karpenter` application on the EKS Cluster. I am going to install the `karpenter` application on my EKS Cluster now. You can also run the below command to install the `karpenter` application:

```bash
eksdemo install autoscaling-karpenter -c <cluster-name>
```
![Screenshot 2024-08-07 153201](https://github.com/user-attachments/assets/247b61da-de55-41cf-b74f-765509284f01)

The above command will install the `karpenter` application on the EKS Cluster.

- You can verify the installation by running the below command:

```bash
kubectl get pods -n karpenter
```
![Screenshot 2024-08-07 153403](https://github.com/user-attachments/assets/fe5a2b47-83ae-45a5-a747-0a0775a104c8)

Point to note: Actually `eksdemo` using the `helm` charts behind the scenes to install the applications on the EKS Cluster. You don't need to have `helm` installed on your machine. `eksdemo` will manage it for you.

### `kubectl-like` Commands for Searching and Querying AWS Resources

We can easily search and query the AWS resources using the `kubectl-like` commands. Let's see how we can search and query the AWS resources using the `kubectl-like` commands.

- Let's say we want to know the details of the AWS Resources like IAM Roles, CloudFormation Stacks, EC2 Instances associated with our EKS Cluster. We can achieve this by running the below commands:

```bash
eksdemo get ec2-instance -c <cluster-name>
eksdemo get iam-role -c <cluster-name>
eksdemo get cloudformation-stack -c <cluster-name>
```

![Screenshot 2024-08-07 154219](https://github.com/user-attachments/assets/50db1121-c68c-4169-9a55-db569bf91f69)


You can see the details of the AWS Resources in a user-friendly table format. It's very easy to understand.


## `eksdemo` vs `EKS Blueprint`

`EKS Blueprint` is a tool that helps you to provision the EKS Cluster with the application dependencies. But it is depends on the IaC. But in case of `eksdemo` it is very easy to use and it is very user friendly. EKS Blueprint is a powerful tool for Production Environment. But since `eksdemo` is still in the beta version, It is not recommended to use in the production environment. But it is very powerful tool for the development and testing environment. Also `eksdemo` is best for Iterative testing compared to `EKS Blueprint`. Another advantage of `eksdemo` is it is very easy to use. It is very user friendly. It will show Output in the table format which is very easy to understand.<br>

This demo is just for a basic understanding of the `eksdemo` tool. In my next medium article, I’ll show you “How to optimize your Kubernetes costs using Kubecost(An open-source tool).” Kubecost installation is not so easy and it is a bit complex. We need to install the dependencies like EBS CSI Driver, Prometheus, etc. But with the `eksdemo` tool, we can install the `kubecost` application on the EKS Cluster with a single command. It will manage the dependencies for `kubecost` application automatically. I hope you will enjoy the next article. Follow my [Medium](https://medium.com/@mathesh-me) profile to get the latest updates.

To get more information about the `eksdemo` tool, you can refer to the [official documentation](https://github.com/awslabs/eksdemo)
