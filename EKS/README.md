# Amazon EKS Learning Guide

## 1. Introduction to Kubernetes and EKS

- Understand the basics of Kubernetes and Amazon EKS.
- **Example:** Explore Kubernetes concepts such as pods, deployments, and services.

## 2. EKS Cluster Creation

- Learn how to create and manage EKS clusters.
- **Example:** Use the AWS Management Console or AWS CLI to create an EKS cluster.
  ```bash
  # Create an EKS cluster using AWS CLI
  aws eks create-cluster --name my-eks-cluster --role-arn arn:aws:iam::123456789012:role/eks-cluster-role
  ```

## 3. Worker Nodes:

Worker nodes in Amazon EKS are the underlying compute resources that run your containerized applications. They are part of an Amazon EC2 Auto Scaling group, providing the ability to automatically adjust the number of nodes in the cluster based on demand.

#### Example: Launch Worker Nodes using Amazon EC2 Instances

To launch worker nodes for your EKS cluster, you can use the AWS CLI. Below is a basic example:

```bash
# Launch EKS worker nodes using AWS CLI
aws eks create-nodegroup \
  --cluster-name my-eks-cluster \
  --nodegroup-name my-nodegroup \
  --node-type t2.micro \
  --node-ami auto
```
In this example:
- `my-eks-cluster` is the name of your EKS cluster.
- `my-nodegroup` is the name you assign to your node group.
- `t2.micro` is the instance type for your worker nodes.
- `auto` specifies that EKS should automatically choose the latest recommended Amazon Machine Image (AMI) for your nodes.

Adjust the parameters based on your specific requirements and the desired configuration for your worker nodes.

**Note:** Ensure that your AWS CLI is configured with the necessary credentials, and you have the required permissions to create resources in your AWS account.

Launching worker nodes is a crucial step in setting up your EKS cluster, providing the underlying infrastructure to run your containerized applications.

## 4. Pods, Deployments, and Services:

In Amazon EKS, you work with Kubernetes concepts such as pods, deployments, and services to deploy and manage containerized applications.

#### Example: Deploy a Simple Application with a Kubernetes Deployment and Service

Below is a basic example of deploying a simple application using Kubernetes manifests for a Deployment and a Service.

**1. Create a Deployment:**

```yaml
# Kubernetes Deployment YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: my-app-image
```
**2. Create a Service:**
```yaml
# Kubernetes Service YAML
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```
In this example:
- The Deployment (`my-app`) specifies the desired state for the application, including the number of replicas and the container image to use.
- The Service (`my-app-service`) exposes the application internally in the cluster, making it accessible to other pods.
Adjust the container image, ports, and other parameters based on your specific application requirements.

**Note:** Ensure that your EKS cluster is properly configured, and kubectl is set up to communicate with the cluster before applying these manifests.

Deploying applications using Kubernetes manifests allows for declarative and version-controlled management of your application workloads.
## 5. Networking in EKS:

Networking in Amazon EKS involves configuring the Virtual Private Cloud (VPC) and understanding how Kubernetes networking works within the EKS cluster.

#### Example: Explore VPC CNI (Container Network Interface) Networking Mode

When creating an EKS cluster, you have the option to choose a networking mode. One common choice is the VPC CNI (Container Network Interface) mode.

**1. Create an EKS Cluster with VPC CNI:**

```bash
# Create an EKS cluster with VPC CNI networking mode using eksctl
eksctl create cluster --name my-eks-cluster --vpc-cidr=10.0.0.0/16 --nodegroup-name my-nodegroup --node-type t2.micro
```
In this example:
- `my-eks-cluster` is the name of your EKS cluster.
- `10.0.0.0/16` is the CIDR block for your VPC.
- `my-nodegroup` is the name of your node group.
- `t2.micro` is the instance type for your worker nodes.

**2. Understanding VPC CNI:**

VPC CNI allows Kubernetes pods to have the same IP address as the underlying EC2 instances, providing a simpler and more efficient networking model.

**3. Network Configurations:**
Adjust VPC configurations, subnets, and security groups based on your organization's networking requirements and best practices.

**Note:** Ensure that your AWS CLI or eksctl is properly configured with the necessary credentials.

Choosing the right networking mode is essential for the proper functioning and scalability of your EKS cluster. VPC CNI is a commonly used mode that integrates well with Amazon VPC networking.

## 6. Security in EKS:

Security is a critical aspect of managing an Amazon EKS cluster, involving various considerations such as Role-Based Access Control (RBAC) and securing communication within the cluster.

#### Example: Define RBAC Roles and Role Bindings for Kubernetes Resources

**1. Create RBAC Role:**

```yaml
# Kubernetes RBAC Role YAML
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
```
**2. Create RBAC RoleBinding:**
```yaml
# Kubernetes RBAC RoleBinding YAML
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: User
  name: user1
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```
In this example:
- The Role (`pod-reader`) defines permissions for reading pods in the default namespace.
- The RoleBinding (`read-pods`) associates the role with a specific user (user1), granting the defined permissions.

**3. Apply RBAC Configurations:**
Apply these RBAC configurations using `kubectl apply -f <filename.yaml>`.

**4. RBAC Best Practices:**
- Define roles and role bindings based on the principle of least privilege.
- Regularly audit and review RBAC configurations to ensure they align with organizational security policies.

RBAC plays a crucial role in controlling access to resources within the EKS cluster, enhancing security by granting permissions only where necessary.


## 7. Logging and Monitoring:

Setting up logging and monitoring is essential for gaining insights into the performance and health of your Amazon EKS cluster and the applications running within it.

#### Example: Use Amazon CloudWatch and Prometheus/Grafana for Monitoring

**1. Amazon CloudWatch Integration:**

- Configure your EKS cluster to send metrics, such as CPU and memory usage, to Amazon CloudWatch.

**2. Prometheus and Grafana Integration:**

- Deploy Prometheus and Grafana to collect and visualize Kubernetes metrics.

**3. Example Prometheus Deployment:**

```yaml
# Kubernetes Prometheus Deployment YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus
  template:
    metadata:
      labels:
        app: prometheus
    spec:
      containers:
      - name: prometheus
        image: prom/prometheus:v2.30.3
        args:
        - "--config.file=/etc/prometheus/prometheus.yml"
        ports:
        - containerPort: 9090
```
**4. Example Grafana Deployment:**
```yaml
# Kubernetes Grafana Deployment YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana
        image: grafana/grafana:8.2.5
        ports:
        - containerPort: 3000
```
**5. Configure Grafana Dashboards:**
Import pre-built dashboards for EKS and Kubernetes from Grafana's public dashboard repository.

**6. Access Grafana UI:**
Access Grafana's web UI to visualize metrics and create custom dashboards.
Monitoring your EKS cluster using a combination of CloudWatch and Prometheus/Grafana provides comprehensive insights into resource utilization, application performance, and cluster health.

## 8. Scaling and High Availability:

Implementing strategies for scaling and ensuring high availability in your Amazon EKS cluster is crucial for meeting varying workloads and maintaining reliability.

#### Example: Implement Horizontal Pod Autoscaling (HPA) for Applications

**1. Example Horizontal Pod Autoscaler (HPA) Configuration:**

```yaml
# Kubernetes Horizontal Pod Autoscaler YAML
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 5
  targetCPUUtilizationPercentage: 80
```
In this example:
- The HPA targets a specific Deployment (`my-app`) in your EKS cluster.
- It adjusts the number of replicas based on CPU utilization, scaling between 2 and 5 replicas to maintain an average CPU utilization of 80%.

**2. Scaling Best Practices:**
- Monitor and analyze your application's performance metrics to determine appropriate scaling thresholds.
- Consider vertical scaling (adjusting resources per pod) in addition to horizontal scaling (adjusting the number of replicas).
**3. High Availability Strategies:**
- Spread your worker nodes across multiple Availability Zones (AZs) to ensure redundancy and fault tolerance.
- Design applications to be stateless and distributed, allowing for resilience in case of node failures.
**4. Testing Scalability:**
- Regularly test your scaling configurations to ensure that your applications can handle increased workloads.

Implementing HPA and high availability strategies ensures that your EKS cluster can dynamically adapt to changing demands, providing a scalable and reliable environment for your containerized applications.