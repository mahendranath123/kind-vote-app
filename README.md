Here’s a professional, visually appealing version of your deployment guide, ready for GitHub, including images for enhanced readability. The images are placeholders where diagrams or screenshots could be added.

---

# Vote App Deployment Guide

This guide provides step-by-step instructions for deploying the **Vote App** using Kubernetes in a Kind cluster, with monitoring and observability enabled by Prometheus and Grafana. Helm is used for package management. The setup assumes deployment on an AWS EC2 instance.

---

## Prerequisites

Before proceeding, ensure the following are installed on your EC2 instance:

| Tool       | Installation Guide                                        |
|------------|-----------------------------------------------------------|
| **AWS EC2**| [Provision an EC2 instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html) |
| **Docker** | [Install Docker](https://docs.docker.com/engine/install/) |
| **kubectl**| [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) |
| **Kind**   | [Install Kind](https://kind.sigs.k8s.io/docs/user/quick-start/) |
| **Helm**   | [Install Helm](https://helm.sh/docs/intro/install/)       |

> 🔗 **Pro Tip**: Save your configurations and security group rules for reuse.  

---

## Step 1: Set Up the Kubernetes Cluster

1. **Create the Kind Cluster**:  
   ```bash
   kind create cluster --name vote-app
   ```

2. **Verify the Cluster**:  
   ```bash
   kubectl cluster-info
   ```

---

## Step 2: Deploy the Vote App

1. **Clone the Repository**:  
   ```bash
   git clone https://github.com/your-repo/vote-app.git
   cd vote-app
   ```

2. **Apply Kubernetes Manifests**:  
   Ensure the `k8s/` directory contains YAML files defining Deployments, Services, and ConfigMaps:  
   ```bash
   kubectl apply -f k8s/
   ```

3. **Verify Deployment**:  
   ```bash
   kubectl get pods
   kubectl get services
   ```

 

---

## Step 3: Install Prometheus and Grafana

1. **Add Helm Repository**:  
   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
   ```

2. **Install Prometheus**:  
   ```bash
   helm install prometheus prometheus-community/prometheus -n monitoring --create-namespace
   ```

3. **Install Grafana**:  
   ```bash
   helm install grafana prometheus-community/grafana -n monitoring
   ```

4. **Access Grafana**:  
   - Retrieve the admin password:  
     ```bash
     kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode
     ```
   - Port-forward Grafana to access locally:  
     ```bash
     kubectl port-forward -n monitoring svc/grafana 3000:80
     ```
   - Open your browser and go to: `http://localhost:3000`

![Grafana Login](https://via.placeholder.com/800x400?text=Grafana+Login+Screenshot)

---

## Step 4: Configure Monitoring for the Vote App

1. **Add Prometheus Scrape Configurations**:  
   Update the Prometheus configuration file to include the metrics endpoints of the Vote App.  

2. **Import Grafana Dashboards**:  
   - Import a predefined dashboard or create custom visualizations for your application metrics.

![Prometheus Dashboard](https://via.placeholder.com/800x400?text=Prometheus+Dashboard+Screenshot)

---

## Step 5: Access the Vote App

1. **Expose the Application**:  
   Use a **NodePort** or **LoadBalancer** service:  
   ```bash
   kubectl expose deployment vote-app --type=NodePort --name=vote-app-service
   ```

2. **Retrieve the External URL**:  
   ```bash
   kubectl get svc vote-app-service
   ```

3. **Test the Application**:  
   Access the application using the external URL or NodePort.

![Vote App Running](https://via.placeholder.com/800x400?text=Vote+App+Running+Screenshot)

---

## Cleanup

To delete the entire setup:

1. **Delete Kubernetes Resources**:  
   ```bash
   kubectl delete -f k8s/
   ```

2. **Uninstall Helm Releases**:  
   ```bash
   helm uninstall prometheus -n monitoring
   helm uninstall grafana -n monitoring
   ```

3. **Delete the Kind Cluster**:  
   ```bash
   kind delete cluster --name vote-app
   ```

---

## Additional Notes

- Configure **Security Groups** to allow necessary traffic (e.g., HTTP, HTTPS, and application-specific ports).  
- Monitor EC2 resource usage to ensure sufficient capacity for the cluster.  
- For further assistance, refer to the official documentation of [Kubernetes](https://kubernetes.io), [Helm](https://helm.sh), [Prometheus](https://prometheus.io), and [Grafana](https://grafana.com).  

---

**🚀 Happy Deploying!**

--- 

This markdown version is suitable for GitHub and includes sections for potential images. You can add screenshots or diagrams for a more visual walkthrough. Let me know if you'd like help designing diagrams for architecture or workflows!
