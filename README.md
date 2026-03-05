# Terraform_Azure_Resources
using terraform to create and maintain azure resources maintenance. 
# Notes and diagrams of Day01 Video

## What is Infra as a Code
- Provisioning your infra through code

![image](https://github.com/user-attachments/assets/79596c6d-2723-405e-bb12-dad162354987)

## Why do we use Infra as a code?

- When you can do that from the Azure console, login to the portal and create, drag, and drop.
- Why take the pain of writing code for drag and drop?

<img width="727" alt="image" src="https://github.com/user-attachments/assets/fcc61fb6-5327-478a-a08b-b7b633f9d3d2" />


- Let's assume you have to provision the infra to deploy a three-tier application
Time taken: 2 hours

<img width="697" alt="image" src="https://github.com/user-attachments/assets/dcc34d89-8441-4337-8c72-dc73b3c9a3da" />

**it's good for a personal project or when you are learning something**

- What if you are working somewhere in a company and let's say you have 4 environments? Time taken: 8 hours
no big deal? right

<img width="743" alt="image" src="https://github.com/user-attachments/assets/82ce50a2-76e1-4853-92f2-81134cd0ec50" />

- How about 100s of servers to be deployed? Tricky?
That's not it
- To save the cost , decommission every day(not possible) keep running and keep accumulating the cost
- Identical environment, works on my machine
- Automate the provisioning, manage and destroy the infra
- Reliability, efficiency, and security?

<img width="682" alt="image" src="https://github.com/user-attachments/assets/4092b8ea-6a88-4e8e-9fda-cd35425b626e" />


## Benefits of IaaC:
- Consistent environment
- Easy to track cost
- Write once, deploy many (single code base)
- Time saving
- Human error
- Cost saving
- Version control, changes are tracked in git
- Automated cleanup/scheduled destruction
- Easy to set and destroy
- The developer can focus on app development
- Easy to create an identical production environment for troubleshooting


## What is Terraform
IaaC tool that helps do all these tasks

<img width="658" alt="image" src="https://github.com/user-attachments/assets/5f090225-fb4b-4022-bf7e-248343d7d5cb" />


## How it works
Write your terraform files --> Run terraform commands --> Call the target cloud provider API to provision the infra using Terraform Provider

<img width="775" alt="image" src="https://github.com/user-attachments/assets/d75208b8-5a1f-4f18-8743-7fc8930c6106" />

Phases: init --> validate --> plan --> apply --> destroy

## Task for Day02 

### Install Terraform

```bash
https://developer.hashicorp.com/terraform/install
```
### Common Error

```
brew install hashicorp/tap/terraform
Error: No developer tools installed.
Install the Command Line Tools:
  xcode-select --install
```
- Install the code tool for mac --> popup will appear, install using that

### Use below commands

```bash
terraform -install-autocomplete
alias tf=terraform
terraform -version
```

02-step

# Day02 - Terraform Providers

## What is a terraform provider?

- Providers are plugins using which we call the cloud APIs or any third-party APIs for infra provisioning and management.

<img width="634" alt="image" src="https://github.com/user-attachments/assets/e451a76f-75ea-4000-b63f-3aec5b313810" />

## Provider version v/s Terraform core version

<img width="412" alt="image" src="https://github.com/user-attachments/assets/18b67936-5744-43dc-a748-552544969591" />

## Why version matters

<img width="554" alt="image" src="https://github.com/user-attachments/assets/2980dbf7-0556-4618-acab-f85ad10db2ec" />

## Version constraints and operators

![image](https://github.com/user-attachments/assets/9bccafe8-78a9-4def-9b7b-e745b207792d)

03-step

# Task for Day03

- Get yourself familiarized with Terraform documentation
  
`https://registry.terraform.io/providers/hashicorp/azurerm/latest`
- Create the below Azure resources using azurerm Terraform provider
    - Resource Group
    - Storage account

## Commands used in the demo

- Log in to Azure
```
- az login
```

- Create Service Principal 
```
az ad sp create-for-rbac -n az-demo --role="Contributor" --scopes="/subscriptions/$SUBSCRIPTION_ID"
```
Note: Use the values generated here to export the variables in the next step

- Set env vars so that the service principal is used for authentication

```
export ARM_CLIENT_ID=""
export ARM_CLIENT_SECRET=""
export ARM_SUBSCRIPTION_ID=""
export ARM_TENANT_ID=""
```

04-step

# Day04 - Terraform State File

## How Terraform update Infrastructure

- Goal is to keep the actual state same as the desired state
- The actual state resides inside a file called statefile
  
<img width="618" alt="image" src="https://github.com/user-attachments/assets/66582b79-fd7f-41b7-b287-974319bef8d8" />

## State file best practices

<img width="587" alt="image" src="https://github.com/user-attachments/assets/7e0b774f-bf83-4576-b8e8-618ee248f8f7" />

## Assignment for day04

- Create Azure resources such as resource group and storage account using a remote backend

05-step

## Task for Day05

- Using the files created in the previous task (day04), update them to use variables below
- Add an input variable named "environment" and set the default value to "staging"
- Create the terraform.tfvars file and set the environment value to demo
- Test the variable precedence by passing the variables in different ways: tfvars file, environment variables, default, etc.
- Create a local variable with a tag called common_tags with values as env=dev, lob=banking, stage=alpha, and use the local variable in the tags section of main.tf
- Create an output variable to print the storage account name

06-step

## Task for Day06

### Using the files from previous task(day05), divide the entire folder structure into seperate files such as
- backend.tf
- provider.tf
- resource-group.tf
- storage-account.tf
- local.tf
- variables.tf
- terraform.tfvars
- output.tf

07-step

## Task for Day07

### Using the files from previous task(day06) , understand the use the below type constraints

- Name: environment, type=string
- Name: storage-disk, type=number
- Name: is_delete, type=boolean
- Name: Allowed_locations, type=list(string)
- Name: resource_tags , type=map(string)
- Name: network_config , type=tuple([string, string, number])
- Name: allowed_vm_sizes, type=list(string)
- Name: vm_config,
```
  type = object({
    size         = string
    publisher    = string
    offer        = string
    sku          = string
    version      = string
  })
```

08-step

Terraform Functions Learning Guide - Assignments
Console Commands
Practice these fundamental commands in terraform console before starting the assignments:

# Basic String Manipulation
lower("HELLO WORLD")
max(5, 12, 9)
trim("  hello  ")
chomp("hello\n")
reverse(["a", "b", "c"])
Assignments
Assignment 1: Project Naming Convention
Functions Focus: lower, replace

Scenario:
Your company requires all resource names to be lowercase and replace spaces with hyphens.

Input:

"Project ALPHA Resource"
Required Output:

"project-alpha-resource"
Tasks:

Create a variable project_name with the given input
Create a local that uses the required functions
Use the transformed name to create an Azure resource group
Add an output to display the transformed name
Assignment 2: Resource Tagging
Function Focus: merge

Scenario:
You need to combine default company tags with environment-specific tags.

Input:

# Default tags
{
    company    = "TechCorp"
    managed_by = "terraform"
}

# Environment tags
{
    environment  = "production"
    cost_center = "cc-123"
}
Tasks:

Create locals for both tag sets
Merge them using the appropriate function
Apply them to a resource group
Create an output to display the combined tags
Assignment 3: Storage Account Naming
Function Focus: substr

Scenario:
Azure storage account names must be less than 24 characters and use only lowercase letters and numbers.

Input:

"projectalphastorageaccount"
Requirements:

Maximum length: 23 characters
All lowercase
No special characters
Tasks:

Create a function to process the storage account name
Ensure it meets Azure requirements
Apply it to a storage account resource
Add validation to prevent invalid names
Assignment 4: Network Security Group Rules
Functions Focus: split, join

Scenario:
Transform a comma-separated list of ports into a specific format for documentation.

Input:

"80,443,8080,3306"
Required Output:

"port-80-port-443-port-8080-port-3306"
Tasks:

Create a variable for the port list
Transform it using appropriate functions
Create an output with the formatted result
Add validation for port numbers
Assignment 5: Resource Lookup
Function Focus: lookup

Scenario:
Implement environment configuration mapping with fallback values.

Input:

environments = {
    dev = {
        instance_size = "small"
        redundancy    = "low"
    }
    prod = {
        instance_size = "large"
        redundancy    = "high"
    }
}
Tasks:

Create the environments map
Implement lookup with fallback
Create outputs for the configuration
Handle invalid environment names
Assignment 6: VM Size Validation
Functions Focus: length, contains

Scenario:
Implement validation rules for VM sizes.

Requirements:

Length between 2 and 20 characters
Must contain 'standard'
Test Cases:

Valid:    "standard_D2s_v3"
Invalid:  "basic_A0"
Invalid:  "standard_D2s_v3_extra_long_name"
Tasks:

Create a variable for VM size
Implement both validation rules
Test with various inputs
Create helpful error messages
Assignment 7: Backup Configuration
Functions Focus: endswith, sensitive

Scenario:
Create a secure backup configuration handler.

Input:

backup_name = "daily_backup"
credential  = "xyz123" # Should be sensitive
Requirements:

Name must end with '_backup'
Credentials must be marked sensitive
Handle validation failures
Tasks:

Create variables for both inputs
Implement proper validation
Handle sensitive data correctly
Create secure outputs
Assignment 8: File Path Processing
Functions Focus: fileexists, dirname

Scenario:
Validate Terraform configuration file paths.

Paths to Validate:

./configs/main.tf
./configs/variables.tf
Tasks:

Create path validation function
Extract directory names
Handle missing files
Create status outputs
Assignment 9: Resource Set Management
Functions Focus: toset, concat

Scenario:
Manage unique resource locations.

Input:

user_locations    = ["eastus", "westus", "eastus"]
default_locations = ["centralus"]
Tasks:

Combine location lists
Remove duplicates
Create location validation
Output unique locations
Assignment 10: Cost Calculation
Functions Focus: abs, max

Scenario:
Process monthly infrastructure costs.

Input:

monthly_costs = [-50, 100, 75, 200]
Required:

Convert negative values to positive
Find maximum cost
Calculate averages
Tasks:

Create cost processing function
Handle negative values
Calculate statistics
Create cost report output
Assignment 11: Timestamp Management
Functions Focus: timestamp, formatdate

Scenario:
Generate formatted timestamps for different purposes.

Required Formats:

Resource Names: YYYYMMDD
Tags: DD-MM-YYYY
Tasks:

Create timestamp generation
Format for different uses
Implement validation
Create formatted outputs
Assignment 12: File Content Handling
Functions Focus: file, sensitive

Scenario:
Securely handle configuration file content.

Requirements:

Read from config.json
Mark content as sensitive
Handle file errors
Validate JSON structure
Tasks:

Implement secure file reading
Add error handling
Validate file content
Create secure outputs

09-step

Advanced Azure Infrastructure with Terraform - Hands-on Assignment
Assignment Overview
You'll create a scalable web application infrastructure in Azure using Terraform. The infrastructure will include a Virtual Machine Scale Set (VMSS) behind a load balancer with proper security and scaling configurations.

Requirements
Base Infrastructure
Create a resource group in one of these regions:
East US
West Europe
Southeast Asia Also create the validation rule that restrict other regions
Networking
Create a VNet with two subnets:
Application subnet (for VMSS)
Management subnet (for future use)
Configure an NSG that:
Only allows traffic from the load balancer to VMSS
Uses dynamic blocks for rule configuration
Denies all other inbound traffic
Compute
Set up a VMSS with:
Ubuntu 20.04 LTS
VM sizes with conditions based on environment(hint: use lookup function):
Dev: Standard_B1s
Stage: Standard_B2s
Prod: Standard_B2ms
Configure auto-scaling:
Scale in when CPU < 10%
Scale out when CPU > 80%
Minimum instances: 2
Maximum instances: 5
Load Balancer
Create an Azure Load Balancer:
Public IP
Backend pool connected to VMSS
Health probe on port 80
Technical Requirements
Variables
Create a terraform.tfvars file with:
Environment name
Region
Resource name prefix
Instance counts
Network address spaces
Locals
Implement locals block for:
Common tags
Resource naming convention
Network configuration
Dynamic Blocks
Use dynamic blocks for:
NSG rules
Load balancer rules

10-step

Assignment for day15
Follow the steps from the video and include below as well
use variables
use bastion host in terraform
use count for vm and count.index
test the connection without peering
test the connection with peering

11-step

Learn Terraform Azure AD
It contains Terraform conifguration files for you to use to learn how to to manage Azure AD users and groups using Terraform.

References:
https://registry.terraform.io/providers/hashicorp/azuread/latest/docs/guides/service_principal_client_secret

https://registry.terraform.io/providers/hashicorp/azuread/latest/docs/guides/microsoft-graph https://registry.terraform.io/providers/hashicorp/azuread/latest/docs/data-sources/domains

Task for day16

12-step

use variables
use bastion host in terraform
use count for vm and count.index
test the connection without peering
ytest the connection with peering

13-step

Day18 Task
Deploy an Azure function app using Terraform
You can use below Sample app for the task
https://github.com/rishabkumar7/azure-qr-code

14-step

Task details for Day 21 - Azure policy and Governance
Create three policies as below:
Location restriction (limit resource creation to specific regions such as eastus, westus)
VM size control (restrict to cost-effective sizes) Only the below VM types should be allowed
"Standard_B2s"
"Standard_B2ms"
Mandatory tagging (enforce department and project tags)
Policy Assignment
Assign policies to subscription
Use Data source to fetch the subscription details
Apply configurations
Attempting non-compliant resource creation
Verifying policy enforcement
Creating compliant resources
Creating non-compliant resources

15-step

CloudOps Goal Tracker - Three-Tier Architecture
This project demonstrates a modern three-tier architecture:

Presentation Layer (Frontend): Node.js/Express server serving a JavaScript frontend
Business Logic Layer (Backend): Go API service
Data Layer: PostgreSQL database
Architecture Overview
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│     Frontend    │     │     Backend     │     │    Database     │
│    (Node.js)    │────▶│      (Go)       │────▶│   (PostgreSQL)  │
│   Port: 3000    │     │   Port: 8080    │     │    Port: 5432   │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                 
Running the Application
You can run the entire application stack using Docker Compose:

cd docker-local-deployment
docker-compose up -d
Accessing Components
Frontend: http://localhost:3000
Backend API: http://localhost:8080
Developing Components Individually
Frontend Development
cd frontend
npm install
npm start
The frontend is a Node.js/Express application that:

Serves static files from the /public directory
Provides API proxying to the backend
Handles all user interactions
Backend Development
cd backend
go mod download
go run main.go
The backend is a Go API service that:

Provides JSON REST API endpoints
Connects to the PostgreSQL database
Implements business logic
Exposes metrics for monitoring
Data Layer
The PostgreSQL database:

Stores goal tracking data
Initializes with the schema defined in docker-local-deployment/database/init.sql
API Endpoints
Backend API (Go Service)
GET /goals - Get all goals
POST /goals - Add a new goal
DELETE /goals/:id - Delete a goal by ID
GET /health - Health check endpoint
GET /metrics - Prometheus metrics endpoint
Frontend API Proxy (Node.js)
GET /api/goals - Proxy to backend's GET /goals
POST /api/goals - Proxy to backend's POST /goals
DELETE /api/goals/:id - Proxy to backend's DELETE /goals/:id
Local Deployment using Docker Compose
Prerequisites
Docker (version 20.10+)
Docker Compose (version 2.0+)
Step 1: Go to the docker-local-deployment directory
cd docker-local-deployment
Step 2: Copy paste the below command to run the application
docker-compose up -d
Step 3: Access the application
Frontend: http://localhost:3000
Backend API: http://localhost:8080
Database: http://localhost:5432 (use pgAdmin or any other client to connect )
3-Tier Application Infrastructure on Azure
This Terraform project deploys a secure and scalable 3-tier application infrastructure in Azure, consisting of frontend, backend, and database tiers.

Architecture Overview
Components
Frontend Tier:

Node.js application running in Docker containers
VM Scale Set with auto-scaling
Application Gateway with WAF for security
Deployed in public subnets across 2 availability zones
Backend Tier:

Go application running in Docker containers
VM Scale Set with auto-scaling
Internal Load Balancer
Deployed in private subnets across 2 availability zones
Database Tier:

Azure Database for PostgreSQL Flexible Server
Primary server with read-write capability
Read replica for read-only operations
Deployed in database subnets across 2 availability zones
Supporting Infrastructure:

Docker Hub for container images
Azure Key Vault for secrets management (including Docker Hub credentials)
Azure Bastion for secure SSH access
Private DNS Zones for name resolution
Network Security Groups for each subnet
Prerequisites
Azure CLI installed and configured
Terraform v1.5.0 or later
Azure subscription and permissions to create resources
Docker installed locally for building and pushing container images
Service Principal Setup for Terraform
Before deploying infrastructure with Terraform, you need to create a service principal with contributor permissions. This allows Terraform to authenticate with Azure and create resources on your behalf.

Create Service Principal
Login to Azure CLI:

az login
Get your subscription ID:

az account show --query id --output tsv
Create a service principal with Contributor role:

az ad sp create-for-rbac --name "terraform-sp" \
  --role="Contributor" \
  --scopes="/subscriptions/<YOUR_SUBSCRIPTION_ID>"
This command will output JSON similar to:

{
  "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "displayName": "terraform-sp",
  "password": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
Set environment variables for Terraform authentication:

export ARM_CLIENT_ID="<appId>"
export ARM_CLIENT_SECRET="<password>"
export ARM_SUBSCRIPTION_ID="<YOUR_SUBSCRIPTION_ID>"
export ARM_TENANT_ID="<tenant>"
Alternative: Create a .env file (recommended for persistent use):

cat << EOF > .env
export ARM_CLIENT_ID="<appId>"
export ARM_CLIENT_SECRET="<password>"
export ARM_SUBSCRIPTION_ID="<YOUR_SUBSCRIPTION_ID>"
export ARM_TENANT_ID="<tenant>"
EOF

# Source the environment variables
source .env
Verify the service principal can authenticate:

az login --service-principal \
  --username $ARM_CLIENT_ID \
  --password $ARM_CLIENT_SECRET \
  --tenant $ARM_TENANT_ID
Security Best Practices
Store credentials securely: Never commit the .env file or credentials to version control. Add .env to your .gitignore file:
echo ".env" >> .gitignore
Use least privilege: The Contributor role provides broad permissions. For production, consider creating custom roles with minimal required permissions
Rotate credentials regularly: Service principal secrets should be rotated periodically
Use Azure Key Vault: For production deployments, consider storing service principal credentials in Azure Key Vault
Project Structure
infra/
├── main.tf                 # Root configuration file
├── variables.tf            # Input variables for the root module
├── outputs.tf              # Output values after deployment
├── providers.tf            # Provider configurations
├── backend.tf              # Remote state configuration
├── modules/                # All modular components
│   ├── networking/         # VNet, subnets, NSGs, Bastion
│   ├── compute/            # VM Scale Sets and load balancers
│   ├── database/           # PostgreSQL Flexible Server
│   ├── dns/                # Private DNS Zones
│   └── keyvault/           # Azure Key Vault
└── environments/           # Environment-specific configurations
    └── prod/               # Production environment
Deployment Instructions
1. Set up Terraform Backend
Create an Azure Storage Account for storing Terraform state:

# Login to Azure
az login

# Create Resource Group for Terraform state
az group create --name tfstate-rg --location eastus2

# Create Storage Account
az storage account create --name tfstate<unique_suffix> --resource-group tfstate-rg --sku Standard_LRS --encryption-services blob

# Create Storage Container
az storage container create --name tfstate --account-name tfstate<unique_suffix>
2. Initialize Terraform
cd infra
terraform init \
  -backend-config="resource_group_name=tfstate-rg" \
  -backend-config="storage_account_name=tfstate<unique_suffix>" \
  -backend-config="container_name=tfstate" \
  -backend-config="key=prod.terraform.tfstate"
3. One-Step Deployment Approach
Since we're now using Docker Hub instead of Azure Container Registry, we can deploy the entire infrastructure in one step. First, make sure your Docker images are pushed to Docker Hub:

# Log in to Docker Hub
docker login -u YOUR_DOCKERHUB_USERNAME

# Build and tag your images (Run from the root of the project)
docker build -t YOUR_DOCKERHUB_USERNAME/frontend:latest ./frontend
docker build -t YOUR_DOCKERHUB_USERNAME/backend:latest ./backend

# Push to Docker Hub
docker push YOUR_DOCKERHUB_USERNAME/frontend:latest
docker push YOUR_DOCKERHUB_USERNAME/backend:latest
After pushing your images to Docker Hub, deploy the infrastructure with your Docker Hub credentials:

cd infra
terraform apply \
  -var-file="environments/prod/terraform.tfvars" \
  -var="dockerhub_username=YOUR_DOCKERHUB_USERNAME" \
  -var="dockerhub_password=YOUR_DOCKERHUB_PAT" \
  -var="frontend_image=YOUR_DOCKERHUB_USERNAME/frontend:latest" \
  -var="backend_image=YOUR_DOCKERHUB_USERNAME/backend:latest"
This command will:

Deploy all infrastructure components including compute resources
Store your Docker Hub Personal Access Token securely in Azure Key Vault
Configure the VM Scale Sets to pull images from Docker Hub
Use the specified Docker images for frontend and backend deployments
4. Access the Application
After deployment completes, access your application:

Frontend: Use the Application Gateway public IP address:

echo "Frontend URL: http://$(terraform output -raw frontend_public_ip)"
Backend: Access via internal load balancer (from within the VNet):

echo "Backend internal endpoint: http://$(terraform output -raw backend_internal_lb_ip):8080"
Database: Access via private endpoints from the backend tier:

echo "PostgreSQL Server: $(terraform output -raw postgres_server_fqdn)"
echo "PostgreSQL Replica: $(terraform output -raw postgres_replica_name)"
SSH into the Bastion host to access the backend and frontend:

terraform output -raw frontend_ssh_private_key > frontend_key.pem
terraform output -raw backend_ssh_private_key > backend_key.pem 
Infrastructure Management
Scaling
The VM Scale Sets will automatically scale based on CPU usage. You can modify the scaling rules in the compute module.

Monitoring
The deployment includes Azure Monitor integration. Configure alerts and dashboards in the Azure Portal.

Security
All subnets are protected with Network Security Groups
Application Gateway has WAF enabled
PostgreSQL is only accessible via private endpoints
Key Vault stores sensitive information (including Docker Hub credentials)
SSH access is only available via Bastion Host
Cleanup
To destroy the infrastructure when no longer needed:

terraform destroy -auto-approve
If You get a error in the destrucion process rerun the above command again

Contributing
Please follow the standard Git workflow:

Fork the repository
Create a feature branch
Make your changes
Submit a pull request
License
This project is licensed under the MIT License - see the LICENSE file for details.

16-step

md
🚀 AKS GitOps Deployment Guide
Complete step-by-step guide to deploy AKS with GitOps using Terraform and ArgoCD

This repository provides a production-ready setup for deploying applications to Azure Kubernetes Service (AKS) using GitOps principles with ArgoCD and Terraform.

📋 Table of Contents
Architecture Overview
Prerequisites
🔄 Complete Recreation Guide
Quick Start
Multi-Environment Setup
Accessing ArgoCD WebUI
Deploying Applications
Verification
Troubleshooting
Clean Up
Check out the video below for Day28 👇
Day 28/28 - Terraform end-to-end Project with AKS and GitOps

🏗️ Architecture Overview
Components
Infrastructure: Azure Kubernetes Service (AKS) with auto-scaling
GitOps Platform: ArgoCD for continuous deployment
State Management: Terraform with remote state (optional)
Authentication: Azure AD integration with local admin accounts
Networking: Azure CNI with network policies
Screenshot 2025-07-18 at 5 31 50 AM
Environment Structure
.
├── dev                     dev env
│   ├── argocd-app-manifest.yaml
│   ├── backend.tf
│   ├── backend.tf.example
│   ├── deploy-argocd-app.sh
│   ├── kubernetes-resources.tf
│   ├── main.tf
│   ├── outputs.tf
│   ├── provider.tf
│   ├── terraform.tfvars
│   ├── validate-deployment.sh
│   └── variables.tf
├── prod           #prod env
│   ├── argocd-app-manifest.yaml
│   ├── backend.tf
│   ├── backend.tf.example
│   ├── deploy-argocd-app.sh
│   ├── kubernetes-resources.tf
│   ├── main.tf
│   ├── outputs.tf
│   ├── provider.tf
│   ├── terraform.tfvars
│   └── variables.tf
├── README.md
└── test            #testing env
    ├── argocd-app-manifest.yaml
    ├── backend.tf
    ├── backend.tf.example
    ├── deploy-argocd-app.sh
    ├── kubernetes-resources.tf
    ├── main.tf
    ├── outputs.tf
    ├── provider.tf
    ├── terraform.tfvars
    └── variables.tf

✅ Prerequisites
1. Essential Tools (Required)
# Install Azure CLI (Required for authentication and service principal)
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

# Install Terraform (latest) (Required to deploy infrastructure)
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
2. Optional Tools (for manual management)
# Install kubectl (for manual cluster operations - Terraform handles K8s deployment)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install Helm (for manual chart operations - Terraform uses Helm provider)
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
Note: Terraform automatically handles ArgoCD installation, Kubernetes resources, and application deployment via providers. You only need kubectl/helm for manual troubleshooting and verification.

3. Azure Authentication & Service Principal
Step 1: Login to Azure
# Login to Azure
az login

# Set your subscription
az account set --subscription "your-subscription-id"

# Verify authentication
az account show
Step 2: Create Service Principal for Terraform
# Get your subscription ID
SUBSCRIPTION_ID=$(az account show --query id --output tsv)
echo "Subscription ID: $SUBSCRIPTION_ID"

# Create service principal with Contributor role
az ad sp create-for-rbac \
  --name "terraform-aks-gitops" \
  --role "Contributor" \
  --scopes "/subscriptions/$SUBSCRIPTION_ID" \
  --sdk-auth

# Save the output - you'll need these values for Terraform authentication:
# {
#   "clientId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
#   "clientSecret": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
#   "subscriptionId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
#   "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
# }
Step 3: Configure Terraform Authentication
Option A: Environment Variables (Recommended)

# Export service principal credentials
export ARM_CLIENT_ID="your-client-id"
export ARM_CLIENT_SECRET="your-client-secret"
export ARM_SUBSCRIPTION_ID="your-subscription-id"
export ARM_TENANT_ID="your-tenant-id"

# Verify Terraform can authenticate
terraform version
Option B: Azure CLI Authentication (Alternative)

# If you prefer to use Azure CLI authentication instead of service principal
az login
az account set --subscription "your-subscription-id"
4. SSH Key (Optional)
# Generate SSH key for node access (optional)
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_azure -N ""
🔄 Complete Recreation Guide
⚠️ CRITICAL: Follow this exact sequence for successful recreation
Before deploying ANY infrastructure, you MUST first set up your GitOps repository. ArgoCD will fail to deploy applications without access to the manifest files.

🎯 Step 1: Create Your GitOps Repository (FIRST!)
1.1 Create New GitHub Repository
Go to GitHub.com and create a new repository:

Repository name: gitops-configs (or your preferred name)
Description: "Kubernetes manifests for 3-tier application GitOps deployment"
Visibility: Public (recommended) or Private with proper access configured
✅ Initialize with README
Clone your new repository:

# Replace YOUR_USERNAME with your GitHub username
git clone https://github.com/YOUR_USERNAME/gitops-configs.git
cd gitops-configs
1.2 Copy and Push Manifest Files
# From this project's directory, copy the 3-tier application manifests
# Make sure you're in the root of this project first
cd Terraform-Full-Course-Azure/lessons/day28

# Copy all manifest files to your GitOps repository
cp -r manifest-files/* /path/to/your/gitops-configs/

# Or if you're already in the gitops-configs directory:
# cp /path/to/this/project/manifest-files/3tire-configs/* .

# Navigate to your GitOps repository
cd /path/to/your/gitops-configs

# Verify the files are copied correctly
ls -la
.
├── 3tire-configs
│   ├── argocd-application.yaml
│   ├── backend-config.yaml
│   ├── backend.yaml
│   ├── frontend-config.yaml
│   ├── frontend.yaml
│   ├── kustomization.yaml
│   ├── namespace.yaml
│   ├── postgres-config.yaml
│   ├── postgres-pvc.yaml
│   └── postgres.yaml
1.3 Update Repository URL in Manifest Files
CRITICAL: Update the ArgoCD application to point to YOUR repository:

# Edit the ArgoCD application manifest
vim argocd-application.yaml

# Find this line (around line 11):
# repoURL: https://github.com/itsBaivab/gitops-configs.git

# Change it to YOUR repository:
# repoURL: https://github.com/YOUR_USERNAME/gitops-configs.git

# Save and exit (:wq in vim)
1.4 Commit and Push to Your GitOps Repository
# Add all manifest files
git add .

# Commit with a descriptive message
git commit -m "ANY COMMIT OF YOUR CHOICE"

# Push to GitHub
git push origin main
1.5 Verify Your GitOps Repository
# Verify your repository is accessible
curl -s https://api.github.com/repos/YOUR_USERNAME/gitops-configs

---

### 🔧 Step 2: Update Terraform Configuration Files

#### 2.1 Update Repository URLs in ALL Environment Files

**You must update ALL three environment configurations:**

```bash
# Navigate back to the project directory
cd /home/baivab/repos/Terraform-Full-Course-Azure/lessons/day28

# Update Development Environment
vim dev/terraform.tfvars
# Find line ~15: app_repo_url = "https://github.com/itsBaivab/gitops-configs.git"
# Change to:     app_repo_url = "https://github.com/YOUR_USERNAME/gitops-configs.git"

# Update Test Environment  
vim test/terraform.tfvars
# Find line ~15: app_repo_url = "https://github.com/itsBaivab/gitops-configs.git"
# Change to:     app_repo_url = "https://github.com/YOUR_USERNAME/gitops-configs.git"

# Update Production Environment
vim prod/terraform.tfvars  
# Find line ~15: app_repo_url = "https://github.com/itsBaivab/gitops-configs.git"
# Change to:     app_repo_url = "https://github.com/YOUR_USERNAME/gitops-configs.git"
2.2 Verify All Repository URLs Are Updated
2.3 Optional: Customize Resource Names
# If you want to use custom resource group and cluster names:
# Edit each terraform.tfvars file and modify:

# resource_group_name     = "my-custom-aks-rg"
# kubernetes_cluster_name = "my-custom-aks-cluster"

# Note: Keep environment naming consistent across dev/test/prod
✅ Step 3: Validation Before Infrastructure Deployment
3.1 Validate GitOps Repository Access
# Test that your GitOps repository is publicly accessible
curl -s https://raw.githubusercontent.com/YOUR_USERNAME/gitops-configs/main/namespace.yaml

# This should return the namespace.yaml content. If you get a 404, check:
# 1. Repository name is correct
# 2. Repository is public OR you have access tokens configured
# 3. Files were pushed to the main branch
Step 2: Configure Remote State Backend (Optional but Recommended)
# Navigate to dev environment
cd dev/

# Option A: Use local state (for testing)
# Skip this step - Terraform will use local state by default

# Option B: Configure Azure Storage Backend (for production)
# Copy the example backend configuration
cp backend.tf.example backend.tf

# Edit backend.tf with your Azure Storage Account details
# You'll need to create these resources first or use existing ones
To set up Azure Storage Backend:

# Create resource group for Terraform state
az group create --name "rg-terraform-state" --location "East US"

# Create storage account (name must be globally unique)
STORAGE_ACCOUNT_NAME="tfstate$(date +%s)"
az storage account create \
  --resource-group "rg-terraform-state" \
  --name "$STORAGE_ACCOUNT_NAME" \
  --sku "Standard_LRS" \
  --encryption-services blob

# Create storage container
az storage container create \
  --name "tfstate" \
  --account-name "$STORAGE_ACCOUNT_NAME"

# Update backend.tf with your values
echo "Update backend.tf with these values:"
echo "resource_group_name  = \"rg-terraform-state\""
echo "storage_account_name = \"$STORAGE_ACCOUNT_NAME\""
echo "container_name       = \"tfstate\""
echo "key                  = \"dev/terraform.tfstate\""
Step 3: Deploy Development Environment
# Initialize Terraform (this will configure the backend if using remote state)
terraform init

# Review what will be created
terraform plan

# Deploy infrastructure (takes 5-10 minutes)
terraform apply -auto-approve
What Terraform Deploys Automatically:

✅ AKS cluster with auto-scaling
✅ ArgoCD installation via Helm
✅ Azure AD integration and RBAC
✅ Network policies and security groups
✅ Sample guestbook application via GitOps
Step 4: Configure kubectl Access
# Get cluster credentials using admin access
az aks get-credentials \
  --resource-group $(terraform output -raw resource_group_name) \
  --name $(terraform output -raw aks_cluster_name) \
  --admin \
  --overwrite-existing

# Verify cluster connectivity
kubectl get nodes
kubectl get namespaces
Step 5: Verify ArgoCD Installation
# Check if ArgoCD is running
kubectl get pods -n argocd

# Check ArgoCD service
kubectl get svc -n argocd
🔐 Step 6: Configure Key Vault Integration for ArgoCD Applications
IMPORTANT: After deploying your infrastructure, you need to update your ArgoCD application manifests with the actual Key Vault details. The infrastructure creates dynamic values that must be configured in your GitOps repository.

6.1 Get Key Vault Information from Terraform
# Get the Key Vault name created by Terraform
terraform output key_vault_name

# Get the Azure tenant ID
az account show --query tenantId -o tsv

# Get the Key Vault Secrets Provider managed identity client ID (CORRECT METHOD)
az aks show --resource-group $(terraform output -raw resource_group_name) \
  --name $(terraform output -raw aks_cluster_name) \
  --query "addonProfiles.azureKeyvaultSecretsProvider.identity.clientId" -o tsv

# Alternative: Get it from terraform show output
terraform show | grep -A 5 "key_vault_secrets_provider" | grep "client_id"
6.2 Update GitOps Repository with Key Vault Configuration
Required Updates in Your GitOps Repository:
File: 3tire-configs/key-vault-secrets.yaml

apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: postgres-secrets-provider
  namespace: 3tirewebapp-dev
spec:
  provider: azure
  parameters:
    usePodIdentity: "false"
    useVMManagedIdentity: "true"
    userAssignedIdentityID: "REPLACE_WITH_KUBELET_CLIENT_ID"    # ← Update this
    keyvaultName: "REPLACE_WITH_KEY_VAULT_NAME"                 # ← Update this  
    tenantId: "REPLACE_WITH_AZURE_TENANT_ID"                    # ← Update this
    objects: |
      array:
        - |
          objectName: postgres-username
          objectType: secret
          objectVersion: ""
        - |
          objectName: postgres-password
          objectType: secret
          objectVersion: ""
        - |
          objectName: postgres-database
          objectType: secret
          objectVersion: ""
        - |
          objectName: postgres-connection-string
          objectType: secret
          objectVersion: ""
  secretObjects:
  - secretName: postgres-credentials-from-kv
    type: Opaque
    data:
    - objectName: postgres-username
      key: POSTGRES_USER
    - objectName: postgres-password
      key: POSTGRES_PASSWORD
    - objectName: postgres-database
      key: POSTGRES_DB
    - objectName: postgres-connection-string
      key: DATABASE_URL
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: key-vault-config
  namespace: 3tirewebapp-dev
data:
  KEY_VAULT_NAME: "REPLACE_WITH_KEY_VAULT_NAME"                 # ← Update this
  KEY_VAULT_SECRET_POSTGRES_USERNAME: "postgres-username"
  KEY_VAULT_SECRET_POSTGRES_PASSWORD: "postgres-password"
  KEY_VAULT_SECRET_POSTGRES_DATABASE: "postgres-database"
  KEY_VAULT_SECRET_CONNECTION_STRING: "postgres-connection-string"
6.3 Automated Script for Key Vault Configuration
Create this script to automate the Key Vault configuration:

# Create update-keyvault-config.sh
cat > update-keyvault-config.sh << 'EOF'
#!/bin/bash

echo "🔐 Updating Key Vault configuration in GitOps repository..."

# Get values from Terraform
KEY_VAULT_NAME=$(terraform output -raw key_vault_name)
TENANT_ID=$(az account show --query tenantId -o tsv)

# Get Key Vault Secrets Provider managed identity client ID (CORRECT METHOD)
KV_SECRETS_PROVIDER_CLIENT_ID=$(az aks show --resource-group $(terraform output -raw resource_group_name) \
  --name $(terraform output -raw aks_cluster_name) \
  --query "addonProfiles.azureKeyvaultSecretsProvider.identity.clientId" -o tsv)

echo "📋 Configuration values:"
echo "  Key Vault Name: $KEY_VAULT_NAME"
echo "  Tenant ID: $TENANT_ID"  
echo "  Key Vault Secrets Provider Client ID: $KV_SECRETS_PROVIDER_CLIENT_ID"

# Path to your GitOps repository (update this path)
GITOPS_REPO_PATH="/path/to/your/gitops-configs"  # ← Update this path

if [ ! -d "$GITOPS_REPO_PATH" ]; then
  echo "❌ GitOps repository not found at: $GITOPS_REPO_PATH"
  echo "   Please update GITOPS_REPO_PATH in this script"
  exit 1
fi

# Update the key-vault-secrets.yaml file
KEY_VAULT_FILE="$GITOPS_REPO_PATH/3tire-configs/key-vault-secrets.yaml"

if [ -f "$KEY_VAULT_FILE" ]; then
  echo "🔄 Updating $KEY_VAULT_FILE..."
  
  # Create backup
  cp "$KEY_VAULT_FILE" "$KEY_VAULT_FILE.backup"
  
  # Replace placeholders with actual values
  sed -i "s/REPLACE_WITH_KUBELET_CLIENT_ID/$KV_SECRETS_PROVIDER_CLIENT_ID/g" "$KEY_VAULT_FILE"
  sed -i "s/REPLACE_WITH_KEY_VAULT_NAME/$KEY_VAULT_NAME/g" "$KEY_VAULT_FILE"
  sed -i "s/REPLACE_WITH_AZURE_TENANT_ID/$TENANT_ID/g" "$KEY_VAULT_FILE"
  
  echo "✅ Key Vault configuration updated successfully!"
  echo "🚀 Next steps:"
  echo "   1. Review the changes: git diff"
  echo "   2. Commit and push: git add . && git commit -m 'Update Key Vault configuration' && git push"
  echo "   3. ArgoCD will automatically sync the changes"
else
  echo "❌ Key Vault secrets file not found: $KEY_VAULT_FILE"
  echo "   Make sure your GitOps repository is properly set up"
fi
EOF

chmod +x update-keyvault-config.sh
6.4 Step-by-Step Manual Configuration
Step 1: Get the Required Values

# From your Terraform directory (e.g., dev/)
cd dev/

# Get Key Vault name
KEY_VAULT_NAME=$(terraform output -raw key_vault_name)
echo "Key Vault Name: $KEY_VAULT_NAME"

# Get Azure tenant ID  
TENANT_ID=$(az account show --query tenantId -o tsv)
echo "Tenant ID: $TENANT_ID"

# Get Key Vault Secrets Provider managed identity client ID (CORRECT METHOD)
KV_SECRETS_PROVIDER_CLIENT_ID=$(az aks show --resource-group $(terraform output -raw resource_group_name) \
  --name $(terraform output -raw aks_cluster_name) \
  --query "addonProfiles.azureKeyvaultSecretsProvider.identity.clientId" -o tsv)
echo "Key Vault Secrets Provider Client ID: $KV_SECRETS_PROVIDER_CLIENT_ID"
Step 2: Update Your GitOps Repository

# Navigate to your GitOps repository
cd /path/to/your/gitops-configs

# Edit the key-vault-secrets.yaml file
vim 3tire-configs/key-vault-secrets.yaml

# Replace these placeholders:
# REPLACE_WITH_KUBELET_CLIENT_ID     → Use the Key Vault Secrets Provider client ID from above
# REPLACE_WITH_KEY_VAULT_NAME        → Use the Key Vault name from above  
# REPLACE_WITH_AZURE_TENANT_ID       → Use the tenant ID from above
Step 3: Commit and Push Changes

# Review your changes
git diff

# Add and commit the changes
git add 3tire-configs/key-vault-secrets.yaml
git commit -m "Configure Key Vault integration with actual values

- Updated userAssignedIdentityID with kubelet client ID
- Updated keyvaultName with actual Key Vault name
- Updated tenantId with Azure tenant ID"

# Push to repository
git push origin main
Step 4: Verify ArgoCD Sync

# Check ArgoCD application status
kubectl get applications -n argocd

# Force sync if needed
kubectl patch application 3tirewebapp-dev -n argocd --type merge \
  --patch '{"operation":{"sync":{"syncStrategy":{"force":true}}}}'

# Verify Key Vault integration is working
kubectl get secretproviderclass -n 3tirewebapp-dev
kubectl describe secretproviderclass postgres-secrets-provider -n 3tirewebapp-dev
6.5 Troubleshooting Key Vault Issues
Common Issues and Solutions:
1. "tenantId is not set" Error

# Ensure tenantId is properly set in SecretProviderClass
kubectl get secretproviderclass postgres-secrets-provider -n 3tirewebapp-dev -o yaml | grep tenantId
2. "Multiple user assigned identities" Error

# Ensure userAssignedIdentityID is specified
kubectl get secretproviderclass postgres-secrets-provider -n 3tirewebapp-dev -o yaml | grep userAssignedIdentityID
3. "403 Forbidden" Key Vault Access Error

# Check if kubelet identity has Key Vault access
az keyvault show --name $(terraform output -raw key_vault_name) \
  --query "properties.accessPolicies[?objectId=='$(az aks show --resource-group $(terraform output -raw resource_group_name) --name $(terraform output -raw aks_cluster_name) --query "identityProfile.kubeletidentity.objectId" -o tsv)']" -o table

# If empty, the access policy is missing (this should be fixed by the updated Terraform)
4. Pod Stuck in "ContainerCreating"

# Check pod events for CSI mount errors
kubectl describe pod -l app=postgres -n 3tirewebapp-dev

# Check CSI driver logs
kubectl logs -n kube-system -l app=secrets-store-csi-driver
6.6 Important Terraform Configuration Notes
Kubelet Identity Access Policy (Fixed in This Version)
Background: Previous versions of this Terraform configuration had a missing access policy for the kubelet identity, causing "403 Forbidden" errors when the CSI driver tried to access Key Vault secrets.

Fix Applied: The dev/main.tf file now includes this access policy:

# Access policy for AKS Kubelet Identity (Node Agent Pool)
# This is needed when using userAssignedIdentityID in SecretProviderClass
access_policy {
  tenant_id = data.azurerm_client_config.current.tenant_id
  object_id = azurerm_kubernetes_cluster.main.kubelet_identity[0].object_id

  secret_permissions = [
    "Get", "List"
  ]
  
  certificate_permissions = [
    "Get", "List"
  ]
}
Why This Matters: When you specify userAssignedIdentityID in your SecretProviderClass to use the kubelet identity, that identity must have proper Key Vault access permissions. Without this access policy, the CSI driver cannot retrieve secrets from Key Vault.

Alternative Approach: You could also use the dedicated Key Vault Secrets Provider identity instead:

# In SecretProviderClass, use this instead of kubelet identity:
userAssignedIdentityID: "<key-vault-secrets-provider-client-id>"
# Create validate-keyvault-integration.sh
cat > validate-keyvault-integration.sh << 'EOF'
#!/bin/bash

echo "🔍 Validating Key Vault integration..."

# Check if SecretProviderClass exists and is configured
echo "1. Checking SecretProviderClass..."
kubectl get secretproviderclass postgres-secrets-provider -n 3tirewebapp-dev > /dev/null 2>&1
if [ $? -eq 0 ]; then
  echo "   ✅ SecretProviderClass exists"
  
  # Check if required fields are configured
  TENANT_ID=$(kubectl get secretproviderclass postgres-secrets-provider -n 3tirewebapp-dev -o jsonpath='{.spec.parameters.tenantId}')
  USER_ID=$(kubectl get secretproviderclass postgres-secrets-provider -n 3tirewebapp-dev -o jsonpath='{.spec.parameters.userAssignedIdentityID}')
  KV_NAME=$(kubectl get secretproviderclass postgres-secrets-provider -n 3tirewebapp-dev -o jsonpath='{.spec.parameters.keyvaultName}')
  
  if [ "$TENANT_ID" != "" ]; then echo "   ✅ Tenant ID configured: $TENANT_ID"; else echo "   ❌ Tenant ID missing"; fi
  if [ "$USER_ID" != "" ]; then echo "   ✅ User Assigned Identity ID configured: $USER_ID"; else echo "   ❌ User Assigned Identity ID missing"; fi
  if [ "$KV_NAME" != "" ]; then echo "   ✅ Key Vault name configured: $KV_NAME"; else echo "   ❌ Key Vault name missing"; fi
else
  echo "   ❌ SecretProviderClass not found"
fi

# Check if pods are running
echo "2. Checking pod status..."
kubectl get pods -n 3tirewebapp-dev --no-headers | while read line; do
  POD_NAME=$(echo $line | awk '{print $1}')
  POD_STATUS=$(echo $line | awk '{print $3}')
  if [ "$POD_STATUS" = "Running" ]; then
    echo "   ✅ $POD_NAME: $POD_STATUS"
  else
    echo "   ⚠️  $POD_NAME: $POD_STATUS"
  fi
done

# Check if Key Vault secret is created
echo "3. Checking Key Vault secret creation..."
kubectl get secret postgres-credentials-from-kv -n 3tirewebapp-dev > /dev/null 2>&1
if [ $? -eq 0 ]; then
  echo "   ✅ Key Vault secret successfully synced to Kubernetes"
else
  echo "   ❌ Key Vault secret not found - CSI driver may not be working"
fi

echo "🎉 Validation complete!"
EOF

chmod +x validate-keyvault-integration.sh
🏢 Multi-Environment Setup
This repository provides three fully configured environments with progressive resource allocation:

Environment Specifications
Environment	Location	VM Size	Node Count	Auto-Scaling	OS Disk	Use Case
Dev	East US	Standard_D2s_v3	2	1-5 nodes	30GB	Development & Testing
Test	East US 2	Standard_D4s_v3	3	2-8 nodes	50GB	Integration Testing
Prod	West US 2	Standard_D8s_v3	5	3-10 nodes	100GB	Production Workloads
Deployment Instructions
Deploy Development Environment
cd dev/
terraform init
terraform plan
terraform apply -auto-approve
Deploy Test Environment
cd test/
terraform init
terraform plan
terraform apply -auto-approve
Deploy Production Environment
cd prod/
terraform init
terraform plan
terraform apply -auto-approve
Environment-Specific Features
Development Environment
Purpose: Local development and experimentation
Resources: Minimal resource allocation for cost efficiency
Monitoring: Basic logging enabled
ArgoCD: Single replica with standard resource limits
Test Environment
Purpose: Integration testing and staging
Resources: Enhanced VM sizes and node count for testing workloads
Monitoring: Standard monitoring with extended log retention
ArgoCD: Enhanced resource limits for better performance
Production Environment
Purpose: Production workloads with high availability
Resources: High-performance VMs with maximum scalability
Monitoring: Full monitoring suite with 90-day log retention
ArgoCD: High availability with multiple replicas and production-grade resource allocation
Backend State Management
Each environment has its own Terraform state file:

Dev: dev/terraform.tfstate
Test: test/terraform.tfstate
Prod: prod/terraform.tfstate
Configure remote state backend for each environment using the respective backend.tf.example:

# For each environment
cp backend.tf.example backend.tf
# Update with your Azure Storage Account details
🌐 Accessing ArgoCD WebUI
Get ArgoCD Access Information
# Get the LoadBalancer external IP
ARGOCD_IP=$(kubectl get svc argocd-server -n argocd -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
echo "ArgoCD URL: http://$ARGOCD_IP"

# Get the admin password
ARGOCD_PASSWORD=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
echo "Username: admin"
echo "Password: $ARGOCD_PASSWORD"
Access ArgoCD WebUI
Open your web browser and navigate to: http://<EXTERNAL-IP>

Login credentials:

Username: admin
Password: Use the password from the command above
First-time setup:

Change the default admin password
Explore the ArgoCD interface
Check the "Applications" section
Alternative: Port Forward (if LoadBalancer IP is not available)
# Port forward ArgoCD service to localhost
kubectl port-forward svc/argocd-server -n argocd 8080:80

# Access via: http://localhost:8080
# Use same credentials as above
🎯 Quick Application Access
Manual Steps
# 1. Check your deployed applications
kubectl get applications -n argocd

# 2. For the 3-tier web application, start port forwarding to frontend
kubectl port-forward svc/frontend -n 3tirewebapp-dev 3000:3000

# 3. Open your browser and navigate to:
# http://localhost:3000
🎉 Your 3-tier application is now accessible! This includes a React frontend, Node.js backend, and PostgreSQL database.

📋 Application Access Summary
🎯 Quick Access (Recommended)
# Port forward to frontend service for immediate access
kubectl port-forward svc/frontend -n 3tirewebapp-dev 3000:3000
🔗 All Access Methods for Your 3-Tier Application
Method	Use Case	Prerequisites	Command/Steps	Access URL
Port Forward	Development, Testing	kubectl access	kubectl port-forward svc/frontend -n 3tirewebapp-dev 3000:3000	http://localhost:3000
Ingress (Built-in)	Production-like, Domain Access	NGINX Ingress Controller + /etc/hosts	Install NGINX Ingress + configure hosts file	http://3tirewebapp-dev.local
LoadBalancer	External Cloud Access	Azure LoadBalancer support	Patch service to LoadBalancer type	http://<EXTERNAL-IP>:3000
NodePort	Direct Node Access	Node IP access	Patch service to NodePort type	http://<NODE-IP>:<NodePort>
🏆 Recommended Access Methods by Environment
Development: Port Forward (fastest setup)
Testing/Staging: Built-in Ingress (production-like)
Production: Ingress with real domain + TLS
Demo/External: LoadBalancer (public access)
🌐 Using the Built-in Ingress (Recommended for Production-like Testing)
Your manifest already includes an Ingress configuration! Here's how to use it:

📋 Your Ingress Configuration
Your frontend.yaml manifest includes this built-in Ingress resource:

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: frontend-ingress
  namespace: 3tirewebapp-dev
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
    - host: 3tirewebapp-dev.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: frontend
                port:
                  number: 3000
Key Features:

Host: 3tirewebapp-dev.local (customizable domain for local testing)
Path: / (root path routing to frontend)
Target Service: frontend service on port 3000
Ingress Class: nginx (requires NGINX Ingress Controller)
Rewrite Target: Root path rewriting for clean URLs
Path Type: Prefix matching for flexible routing
Step 1: Install NGINX Ingress Controller
# Install NGINX Ingress Controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml

# Wait for ingress controller to be ready
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s

# Check ingress controller service
kubectl get svc -n ingress-nginx
Step 2: Configure Local Domain (for local testing)
# Get the ingress external IP
INGRESS_IP=$(kubectl get svc ingress-nginx-controller -n ingress-nginx -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
echo "Ingress IP: $INGRESS_IP"

# Add to /etc/hosts for local domain resolution
echo "$INGRESS_IP 3tirewebapp-dev.local" | sudo tee -a /etc/hosts

# Verify the ingress is working
kubectl get ingress -n 3tirewebapp-dev
Step 3: Access via Domain
# Open your browser to:
# http://3tirewebapp-dev.local

# Or test with curl
curl -H "Host: 3tirewebapp-dev.local" http://$INGRESS_IP
🚀 Alternative Access Methods
Method 1: LoadBalancer (External Cloud Access)
# Patch the frontend service to use LoadBalancer type
kubectl patch svc frontend -n 3tirewebapp-dev -p '{"spec":{"type":"LoadBalancer"}}'

# Wait for external IP assignment (may take 2-5 minutes)
echo "Waiting for external IP..."
kubectl get svc frontend -n 3tirewebapp-dev --watch

# Get the external IP and access your application
EXTERNAL_IP=$(kubectl get svc frontend -n 3tirewebapp-dev -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
echo "Access your application at: http://$EXTERNAL_IP:3000"

# To revert back to ClusterIP:
kubectl patch svc frontend -n 3tirewebapp-dev -p '{"spec":{"type":"ClusterIP"}}'
Method 2: NodePort (Direct Node Access)
# Patch the frontend service to use NodePort type
kubectl patch svc frontend -n 3tirewebapp-dev -p '{"spec":{"type":"NodePort"}}'

# Get the NodePort and Node IP
NODE_PORT=$(kubectl get svc frontend -n 3tirewebapp-dev -o jsonpath='{.spec.ports[0].nodePort}')
NODE_IP=$(kubectl get nodes -o jsonpath='{.items[0].status.addresses[0].address}')

echo "Access your application at: http://$NODE_IP:$NODE_PORT"

# To revert back to ClusterIP:
kubectl patch svc frontend -n 3tirewebapp-dev -p '{"spec":{"type":"ClusterIP"}}'
🔍 Your 3-Tier Application Architecture
Your deployed application consists of:

Frontend (React Application)
Service: frontend on port 3000 (ClusterIP)
Container: itsbaivab/frontend:v2
Features: React-based web interface with Express.js proxy server
Ingress: Pre-configured with domain 3tirewebapp-dev.local
Health Checks: HTTP probes on / endpoint with liveness/readiness checks
Resources: 100m CPU request, 200m CPU limit, 128Mi-256Mi memory
Backend (Node.js API)
Service: backend on port 8080 (ClusterIP)
Container: itsbaivab/backend:latest
Database Connection: Connects to PostgreSQL via ConfigMap/Secret settings
API Endpoints: Health check on /health, business logic APIs
Frontend Integration: Backend URL configured as http://backend:8080
Resources: 100m CPU request, 200m CPU limit, 128Mi-256Mi memory
Database (PostgreSQL)
Service: postgres on port 5432 (ClusterIP)
Container: postgres:15
Persistence: Uses postgres-pvc persistent volume for data storage
Database: goalsdb with user postgres
Configuration: Environment variables managed via ConfigMaps and Secrets
Test Your 3-Tier Application
# Method 1: Frontend Browser Testing (Recommended)
# 1. Use port forwarding: kubectl port-forward svc/frontend -n 3tirewebapp-dev 3000:3000
# 2. Open browser to: http://localhost:3000
# 3. Test the web interface functionality
# 4. Verify frontend-backend communication through UI interactions
# 5. Check database interactions via application features

# Method 2: API Testing (Backend Direct)
kubectl port-forward svc/backend -n 3tirewebapp-dev 8080:8080 &
echo "Testing backend health endpoint..."
curl -X GET http://localhost:8080/health
echo "Testing backend API endpoints..."
curl -X GET http://localhost:8080/api/goals  # or your specific API endpoints
kill %1  # Stop background port forwarding

# Method 3: Database Connection Testing (Internal)
echo "Testing database connectivity from backend pod..."
kubectl exec -it deployment/backend -n 3tirewebapp-dev -- \
  psql -h postgres -U postgres -d goalsdb -c "SELECT version();"

# Stop background port forwards
kill %1 %2
Verify Application Health & Communication
# Check all pods are running and ready
kubectl get pods -n 3tirewebapp-dev
kubectl describe pods -n 3tirewebapp-dev

# Check application logs
kubectl logs deployment/frontend -n 3tirewebapp-dev --tail=50
kubectl logs deployment/backend -n 3tirewebapp-dev --tail=50
kubectl logs deployment/postgres -n 3tirewebapp-dev --tail=50

# Verify service endpoints
kubectl get endpoints -n 3tirewebapp-dev

# Test internal service communication
kubectl run debug-pod --image=curlimages/curl --rm -it --restart=Never -- \
  curl -s http://frontend.3tirewebapp-dev.svc.cluster.local:3000

kubectl run debug-pod --image=curlimages/curl --rm -it --restart=Never -- \
  curl -s http://backend.3tirewebapp-dev.svc.cluster.local:8080/health
Troubleshooting Application Access
Common Issues and Solutions
1. Frontend Port Forward Connection Refused

# Check if frontend service exists and has endpoints
kubectl get svc,endpoints frontend -n 3tirewebapp-dev

# Verify frontend pod is running and ready
kubectl get pods -l app=frontend -n 3tirewebapp-dev
kubectl logs deployment/frontend -n 3tirewebapp-dev

# Try different local port
kubectl port-forward svc/frontend -n 3tirewebapp-dev 3001:3000
2. Application Shows Backend Connection Error

# Check backend service connectivity
kubectl get svc backend -n 3tirewebapp-dev
kubectl get pods -l app=backend -n 3tirewebapp-dev

# Verify backend configuration
kubectl describe configmap frontend-config -n 3tirewebapp-dev
kubectl describe configmap backend-config -n 3tirewebapp-dev

# Test backend API directly
kubectl port-forward svc/backend -n 3tirewebapp-dev 8080:8080 &
curl -v http://localhost:8080/health
kill %1
3. Database Connection Issues

# Check PostgreSQL pod and service
kubectl get pods -l app=postgres -n 3tirewebapp-dev
kubectl get svc postgres -n 3tirewebapp-dev

# Check database credentials and configuration
kubectl describe secret postgres-secret -n 3tirewebapp-dev
kubectl describe configmap postgres-config -n 3tirewebapp-dev

# Test database connection from backend pod
kubectl exec -it deployment/backend -n 3tirewebapp-dev -- \
  nc -z postgres 5432 && echo "Database reachable" || echo "Database unreachable"
4. Ingress Domain Not Resolving

# Check if ingress controller is running
kubectl get pods -n ingress-nginx
kubectl get svc -n ingress-nginx

# Verify ingress resource
kubectl get ingress -n 3tirewebapp-dev
kubectl describe ingress frontend-ingress -n 3tirewebapp-dev

# Check /etc/hosts entry
grep "3tirewebapp-dev.local" /etc/hosts

# Test with curl using Host header
curl -H "Host: 3tirewebapp-dev.local" http://<INGRESS-IP>
✅ Verification
Automated Validation Script
# Run the deployment validation script
chmod +x validate-deployment.sh
./validate-deployment.sh
Manual Verification Steps
# 1. Check cluster status
kubectl get nodes
kubectl cluster-info

# 2. Verify ArgoCD components
kubectl get pods -n argocd
kubectl get svc -n argocd

# 3. Check applications
kubectl get applications -n argocd

# 4. Test application access (if Goal Tracker is deployed)
kubectl get pods -n goal-tracker
kubectl get svc -n goal-tracker
Health Indicators ✅
Healthy deployment should show:

All nodes in "Ready" state
All ArgoCD pods "Running"
ArgoCD LoadBalancer has external IP
Applications show "Synced" and "Healthy" status
🔧 Troubleshooting
Common Issues and Solutions
1. kubectl Connection Issues
# Reset kubectl configuration
az aks get-credentials --resource-group <rg-name> --name <cluster-name> --admin --overwrite-existing

# Test connectivity
kubectl get nodes --request-timeout=30s
2. ArgoCD UI Not Accessible
# Check LoadBalancer service
kubectl get svc argocd-server -n argocd

# Use port forwarding as backup
kubectl port-forward svc/argocd-server -n argocd 8080:80
# Access: http://localhost:8080
3. Terraform Apply Fails
# Check Azure authentication
az account show

# Re-initialize Terraform
terraform init -reconfigure

# Check for resource conflicts
terraform plan
4. Application Sync Issues
# Check ArgoCD application status
kubectl describe application <app-name> -n argocd

# Force sync via CLI
kubectl patch application <app-name> -n argocd --type merge --patch '{"operation":{"sync":{"syncStrategy":{"force":true}}}}'
🧹 Clean Up
Remove Applications
# Delete all ArgoCD applications
kubectl delete applications --all -n argocd
Remove Infrastructure
# Destroy the environment
terraform destroy -auto-approve
Clean Up Resources
# Remove kubectl configuration
kubectl config delete-context <cluster-context>

# Clean up local files
rm -f ~/.kube/config.backup
📝 Configuration Files
Key Configuration Files
terraform.tfvars - Environment-specific variables
main.tf - AKS cluster configuration
kubernetes-resources.tf - ArgoCD and Kubernetes resources
provider.tf - Terraform provider configuration
Default Settings
Environment: Development
Location: East US
Node Count: 2 (auto-scaling: 1-5)
VM Size: Standard_D2s_v3
ArgoCD: LoadBalancer with insecure mode (demo)
🎯 Next Steps
Explore ArgoCD WebUI - Navigate through applications and sync policies
Deploy Your Applications - Add your own Git repositories
Set Up Monitoring - Configure Log Analytics and Azure Monitor
Enable TLS - Configure HTTPS for ArgoCD in production
Scale to Test/Prod - Deploy test and production environments
📞 Support
For issues or questions:

Check the troubleshooting section above
Review Terraform and kubectl logs
Validate Azure permissions and quotas
Ensure all prerequisites are met
🎉 Congratulations! You now have a fully functional AKS GitOps platform!

⚠️ IMPORTANT: Key Vault Identity Configuration
Common Mistake: Using Wrong Managed Identity
CRITICAL ERROR TO AVOID: Do NOT use the kubelet identity for the SecretProviderClass. This is a common mistake that causes "403 Forbidden" errors.

❌ WRONG - Don't Use This Command:
# This gets the kubelet identity - WRONG for SecretProviderClass
az aks show --resource-group $(terraform output -raw resource_group_name) \
  --name $(terraform output -raw aks_cluster_name) \
  --query "identityProfile.kubeletidentity.clientId" -o tsv
✅ CORRECT - Use This Command:
# This gets the Key Vault Secrets Provider identity - CORRECT for SecretProviderClass
az aks show --resource-group $(terraform output -raw resource_group_name) \
  --name $(terraform output -raw aks_cluster_name) \
  --query "addonProfiles.azureKeyvaultSecretsProvider.identity.clientId" -o tsv
Why This Matters:
Key Vault Secrets Provider Identity: Dedicated identity specifically for accessing Key Vault secrets
Kubelet Identity: Node pool identity for general cluster operations
Using the wrong identity results in access denied errors even with proper access policies


every time need mention that

📋 Prerequisites
Azure free account or subscription, follow this video
Azure Fundamentals Video Link
Visual Studio Code or preferred IDE
Git installed and working knowledge of it
Linux or Mac or WSL(Windows Subsystem for Linux)
Linux and Shell scripting
Basic understanding of YAML and JSON
Networking Fundamentals
IP Addressing Video Link
Docker and Kubernetes Fundamentals Playlist Link
📚 Course Curriculum
Module 1: Core Concepts
Day1: Introduction to Terraform
Understanding Infrastructure as Code (IaC)
Why we need IaC
What is Terraform and its benefits
Challenges with the traditional approach
Terraform Workflow
Installing Terraform
Code Sample
Day2: Terraform Provider
Terraform Providers
Provider version v/s Terraform core version
Why version matters
Version constraints
Operators for versions
Code Sample
Day3: Resource Group and Storage Account
Authentication and Authorization to Azure resources
Creating resource groups
Storage account management
Understanding dependencies
Code Sample
Day4: State file management - Remote Backend
How Terraform updates Infra
Terraform state file
State file best practices
Remote backend setup
State management
Code Sample
Day5: Variables
Input variables
Output variables
Locals
Variable precedence
Variable files (tfvars)
Code Sample
Day6: File Structure
Terraform file organization
Sequence of file loading
Best practices for structure
Code Sample
Video 7: Type constraints in Terraform
String, number, bol
Map, set, list, Tuple, Objects
Code Sample
Video 8: Meta-arguments
Understanding count
for_each loop
for loop
Practical examples
Code Sample
Video 9: The Lifecycle meta-arguments
create before destroy
prevent destroy
ignore changes
replace triggered by
customer condition
Code Sample
Video 10: Dynamic Blocks and expressions
Dynamic blocks
Conditional expressions
Splat Expressions
practical examples
Code Sample
Video 11: Functions in Terraform
Built-in functions
Practical examples
tasks for practice
Code Sample
Video 12: Functions in Terraform(Continue..)
Built-in functions
Practical examples
tasks for practice
Code Sample
Video 13: Data Sources
Using data sources
Practical examples
Code Sample
Module 2: Azure resources using Terraform
Video 14: High available/scalable Infrastructure Deployment ( Mini Project 1 )
Creating Virtual Machines
VM Scale Sets
Network Security Groups
Loadbalancer, Nat Gateway, Public IP , Autoscaling rules etc
Code Sample
Video 15: VNET and Peering ( Mini Project 2 )
Virtual Network Creation
VNet peering setup
Code Sample
Video 16: Azure AD Authentication ( Mini Project 3 )
Authentication methods
Service principals
Managed identities
Code Sample
Video 17: Azure Web Apps ( Mini Project 4 )
App Service creation
Configuration
Deployment
Code Sample
Video 18: Azure Functions ( Mini Project 5 )
Function App setup
Configuration
Code Sample
Video 19: Terraform Provisioners ( Mini Project 6 )
What are provisioners and their use case
Local vs remote vs file provisioners
Demo of all three provisioners
Code Sample
Video 20: AKS Cluster ( Real-time Project 1)
Kubernetes cluster setup
Custom module usage
Custom module creation for AKS, KeyVault, SPN etc
Code Sample
Video 21: Azure Policy and Governance ( Mini Project 7 )
Policy creation
Governance setup
Code Sample
Video 22: Azure SQL Database ( Mini Project 8 )
Database creation
Configuration
Code Sample
Video 23: Azure Monitoring ( Mini Project 9 )
Metrics alerts
Action Groups
Log analytics workspace
Log alerts
Code Sample
Module 3: Advanced Concepts
Video 24: Terraform Import (Real-time project 2)
Different ways of importing Azure resource to Terraform
Terraform Import
Importing a live website to Terraform using Terraform Import
AZExport
Importing a live website to Terraform using AZExport
Terraformer
Code Sample
Video 25: Terraform Cloud and Workspaces
Cloud setup
Workspace management
Code Sample
Video 26: Azure DevOps with Terraform (Real-time project 3)
CI/CD pipeline setup
Automation
Code Sample
Video 27: 3-Tier Architecture (Real-time project 4)
Complete architecture setup
Best practices
Code Sample
Video 28: Terraform end-to-end Project with AKS and GitOps (Real-time project 5)
Diagram and flow
Code walkthrough
Implementation
Code Sample
📂 Repository Structure
├── lessons/
│   ├── day01/
│   ├── day02/
│   └── ...
├── docs/
│   ├── setup.md
│   └── troubleshooting.md
└── README.md
🎓 Learning Path
Follow videos in sequence
Complete hands-on exercises
Implement projects
Practice with provided code samples
📝 License
MIT License - see the LICENSE file for details.

🔗 Resources
Terraform Documentation
Azure Documentation
Course Support Forum
