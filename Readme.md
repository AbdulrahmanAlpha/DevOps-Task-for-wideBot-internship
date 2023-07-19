## Overview
This Terraform code provisions an infrastructure on AWS that includes a Virtual Private Cloud (VPC) with public and private subnets, an internet gateway, security groups for load balancing, web servers, Redis, MongoDB and SQL Server, an Elastic Kubernetes Service (EKS) cluster, and Kubernetes deployments and services for web app, databases and SQL Server. It also creates a load balancer and target group for the web app.

## Components

### VPC
- Creates a VPC with a CIDR block of 10.0.0.0/16.
- Creates a public and private subnet with CIDR blocks of 10.0.1.0/24 and 10.0.2.0/24, respectively.
- Associates the public subnet with an internet gateway to allow outbound traffic.
- Associates the private subnet with a NAT gateway to allow inbound traffic.
### Security Groups
- Creates security groups for the web application, Redis database, MongoDB database, and SQL Server database.
- Configures the security groups to allow traffic from specific IP addresses and ports.
- Configures the security groups to deny all traffic that is not explicitly allowed.
### Load Balancer
- Creates an AWS Application Load Balancer to distribute incoming traffic to the web application running in the public subnet.
- Configures the listener rules that define how traffic is distributed to the target groups.
### Autoscaling Group
- Creates an AWS Auto Scaling Group to automatically scale the number of instances running the web application based on CPU utilization.
- Configures the launch settings for the instances, including the AMI ID, user data script, and security group.
### Databases
- Creates a Redis, MongoDB, and SQL Server database running in the private subnet.
- Configures the databases to use persistent storage and authentication.
- Configures the databases to only allow traffic from specific security groups.
### Kubernetes
- Uses Kubernetes manifests to deploy the web application and databases to the Kubernetes cluster.
- Configures Kubernetes Services to expose the internal IP addresses of the resources running in the private subnet.
- Configures the web application to connect to the databases using their internal IP addresses.
## Security Components
### Network ACLs
- Creates network ACLs for the public and private subnets to control inbound and outbound traffic.
- Configures the network ACLs to allow only necessary traffic and deny all other traffic.
### IAM Roles
- Creates an IAM role for the EC2 instances running in the Auto Scaling Group to control access to AWS resources.
- Configures the IAM role to grant only necessary permissions and deny all other permissions.
- Attaches the IAM role to the instances in the Auto Scaling Group.
Key Pair
- Creates an EC2 key pair to securely access the instances in the Auto Scaling Group.
- Configures the key pair to only allow access from specific IP addresses.
Associates the key pair with the instances in the Auto Scaling Group.
### SSL Certificate
- Creates an SSL certificate using AWS Certificate Manager to encrypt traffic between the web application and the load balancer.
- Configures the load balancer to use the SSL certificate for HTTPS traffic.

## Infrastructure Architecture

![Diagram](./Blank%20diagram.jpeg)

## Prerequisites
Before you can use this Terraform code, you need to have the following:
- An AWS account
- AWS Command Line Interface (CLI) installed on your machine
- Terraform installed on your machine

## Setup
1. Clone the repository to your local machine.
2. Navigate to the directory where the `main.tf` file is located.
3. Open the file in a text editor.
4. Update the `region` value in the `provider` block to the region where you want to provision the infrastructure.
5. Replace the values of the `Owner` and `Project` tags in the `default_tags` block with your own values.
6. Review the values of the `cidr_block` and `Name` tags for the VPC and subnets and update them if necessary.
7. Review the values of the security groups and update them if necessary. 
8. Review the values of the Kubernetes deployments and services and update them if necessary.
9. Save the changes to the file.

## Configuration
1. Open a terminal or command prompt.
2. Navigate to the directory where the `main.tf` file is located.
3. Run the following command to initialize the Terraform working directory:
   `````
   terraform init
   ```
4. Run the following command to validate the Terraform code:
   ````
   terraform validate
   ````
5. Run the following command to create an execution plan:
   ````
   terraform plan
   ````
   Review the output to ensure that the resources that will be created match your expectations.
6. Run the following command to apply the Terraform code:
   ````
   terraform apply
   ````
7. When prompted to confirm the creation of resources, type `yes` and press Enter.
8. Wait for Terraform to provision the infrastructure. This may take several minutes.
9. Once the infrastructure has been provisioned, you will see output similar to the following:
   ````
   kubernetes_service.sqlserver: Creation complete after 1m30s [id=default/sqlserver]
   aws_lb_target_group.webapp: Creation complete after 1m30s [id=arn:aws:elasticloadbalancing:us-east-1:123456789012:targetgroup/webapp/abcdefghijklmn]
   aws_lb.webapp: Creation complete after 1m30s [id=arn:aws:elasticloadbalancing:us-east-1:123456789012:loadbalancer/app/webapp-lb/abcdefghijklmn]
   kubernetes_service.redis: Creation complete after 1m30s [id=default/redis]
   kubernetes_service.webapp: Creation complete after 1m30s [id=default/webapp]
   aws_security_group.sql: Creation complete after 1m30s [id=sg-abcdefghijklmno]
   aws_security_group.mongo: Creation complete after 1m30s [id=sg-abcdefghijklmnop]
   aws_security_group.redis: Creation complete after 1m30s [id=sg-abcdefghijklmnopq]
   aws_security_group.web: Creation complete after 1m30s [id=sg-abcdefghijklmnopqr]
   aws_security_group.lb: Creation complete after 1m30s [id=sg-abcdefghijklmnopqrs]
   aws_vpc.main: Creation complete after 1m30s [id=vpc-abcdefg]
   aws_subnet.private: Creation complete after 1m30s [id=subnet-abcdefg]
   aws_subnet.public: Creation complete after 1m30s [id=subnet-abcdefgh]
   aws_internet_gateway.gw: Creation complete after 1m30s [id=igw-abcdefghijklmno]
   aws_route_table.public: Creation complete after 1m30s [id=rtb-abcdefghijklmno]
   aws_route_table_association.public: Creation complete after 1m30s [id=rtbassoc-abcdefghijklmno]
   aws_iam_role.eks: Creation complete after 1m30s [id=eks-cluster]
   aws_iam_role_policy_attachment.eks-policy: Creation complete after 1m30s [id=eks-cluster-2022071812345678900000000000000000000000000000000000000000000000]
   kubernetes_deployment.webapp: Creation complete after 1m30s [id=default/webapp]
   kubernetes_deployment.mongodb: Creation complete after1m30s [id=default/mongodb]
   kubernetes_deployment.sqlserver: Creation complete after 1m30s [id=default/sqlserver]
   kubernetes_service.sqlserver_lb: Creation complete after 1m30s [id=default/sqlserver-lb]
   kubernetes_service.mongodb: Creation complete after 1m30s [id=default/mongodb-lb]
   ````
   This indicates that all the resources have been created successfully.
   
## Additional Instructions
- To destroy the infrastructure, run the following command in the same directory where the `main.tf` file is located:
  ````
  terraform destroy
  ````
  When prompted to confirm the destruction of resources, type `yes` and press Enter.

- To update the infrastructure, modify the `main.tf` file and run the following commands in order:
  ````
  terraform plan
  terraform apply
  ````
  Review the output of the `terraform plan` command to ensure that the changes that will be made match your expectations. When prompted to confirm the changes, type `yes` and press Enter.
  
- To view the Kubernetes resources, run the following command:
  ````
  kubectl get all
  ````
  
- To access the web app, you can use the DNS name of the load balancer. You can find the DNS name in the output of the `terraform apply` command under the `aws_lb.webapp` resource. Alternatively, you can run the following command:
  ````
  aws elbv2 describe-load-balancers --load-balancer-arns <load-balancer-arn> --query 'LoadBalancers[].DNSName' --output text
  ````
  Replace `<load-balancer-arn>` with the ARN of the load balancer, which can be found in the output of the `terraform apply` command under the `aws_lb.webapp` resource. Once you have the DNS name, you can access the web app by entering it in a web browser.

- To access the SQL Server, MongoDB or Redis databases from the web app, you can use the service names as host names. For example, to connect to the SQL Server database, you can use `sqlserver_lb` as the host name. To connect to the MongoDB database, you can use `mongodb` as the host name. To connect to the Redis database, you can use `redis` as the host name.

- To access the Kubernetes dashboard, run the following command:
  ``````
  kubectl proxy
  ```
  This will start a proxy server that allows you to access the Kubernetes dashboard from your local machine. Once the proxy server is running, open a web browser and go to `http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/`. You will be prompted to enter a token to authenticate. To obtain the token, run the following command:
  ````
  kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep eks-admin | awk '{print $1}')
  `````
  This will output a token that you can copy and paste into the authentication prompt.

- To update the VPC configuration, you can modify the `main.tf` file and run the following commands in order:
  ````
  terraform plan
  terraform apply
  `````
  Review the output of the `terraform plan` command to ensure that the changes that will be made match your expectations. When prompted to confirm the changes, type `yes` and press Enter.

- To update the Kubernetes resources, you can modify the `main.tf` file and run the following commands in order:
  ````
  terraform plan
  terraform apply
  `````
  Review the output of the `terraform plan` command to ensure that the changes that will be made match your expectations. When prompted to confirm the changes, type `yes` and press Enter.

- To troubleshoot issues with the Terraform code or infrastructure, you can use the following tools and methods:
  - The AWS Management Console: This provides a web-based interface for managing your AWS resources. You can use it to view the status of your resources, modify their settings, and troubleshoot issues.
  - The AWS CLI: This provides a command-line interface for interacting with AWS. You can use it to perform various operations on your resources, such as starting and stopping instances, creating and deleting security groups, and managing IAM policies.
  - The Terraform CLI: This provides a command-line interface for interacting with Terraform. You can use it to validate, plan, apply, and destroy Terraform configurations.
  - The Kubernetes CLI (kubectl): This provides a command-line interface for interacting with Kubernetes. You can use it to view the status of your deployments, services, and pods, modify their settings, and troubleshoot issues.
  - The system logs: You can view the system logs of your resources to troubleshoot issues. For example, you can view the logs of a pod by running the following command:
    ````
    kubectl logs <pod-name>
    ```
    Replace `<pod-name>` with the name of the pod that you want to view the logs for.
  - The event logs: You can view the event logs of your resources to troubleshoot issues. For example, you can view the event logs of a deployment by running the following command:
    ````
    kubectl describe deployment <deployment-name>
    ```
    Replace `<deployment-name>` with the name of the deployment that you want to view the event logs for.

- To secure the infrastructure, you can take the following measures:
  - Use strong passwords: Use strong, unique passwords for all your resources, such as your AWS account, your databases, and your Kubernetes clusters.
  - Enable multi-factor authentication (MFA): Enable MFA for your AWS account and your IAM users to add an extra layer of security.
  - Use security groups: Use security groups to restrict traffic to your resources. For example, you can create a security group that allows incoming traffic only from specified IP addresses.
  - Use network ACLs: Use network ACLs to control traffic at the subnet level. For example, you can create a network ACL that allows incoming traffic only from specified IP addresses.
  - Use encryption: Use encryption to protect sensitive data in transit and at rest. For example, you can enable SSL/TLS encryption for your web app, and use encrypted EBS volumes for your databases.
  - Use role-based access control (RBAC): Use RBAC to control access to your Kubernetes resources. For example, you can create a role that allows a user to view the status of a deployment, but not modify it.
  - Use least privilege: Use the principle of least privilege to grant permissions to your resources. For example, grant only the permissions that are necessary to perform a specific task, and revoke them when the task is complete.
  - Monitor your resources: Monitor your resources for suspicious activity, such as unauthorized access attempts or unusual traffic patterns. You can use AWS CloudTrail, Amazon GuardDuty, or other monitoring tools to do this. 
