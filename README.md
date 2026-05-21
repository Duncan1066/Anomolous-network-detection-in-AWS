Network Anomaly Detection in AWS
This repository contains the Infrastructure as Code (IaC) and automation scripts for deploying a machine learning pipeline to detect anomalous network traffic.

The project utilizes the IoT-23 dataset and automatically provisions, executes, and tears down the computing environment to ensure strict cost control.

🏗️ Architecture Overview
The entire infrastructure is written in Terraform and can be executed via a Jupyter Notebook for easy, repeatable deployment.

To maintain a strict project budget (under $100) while still meeting high compute requirements, this architecture bypasses expensive managed machine learning services in favor of a heavily automated, ephemeral EC2 setup.

Key components include:

Compute: An m5.xlarge EC2 instance (Amazon Linux 2023) acts as the ML engine.

Automation: An injected bash script (ml_bootstrap.sh) automatically installs dependencies (Pandas, Scikit-learn, Boto3), clones the experiment repository, and executes the ML models.

Cost Optimization: Once run_experiments.py completes its classification tasks, the EC2 instance automatically issues a shutdown -h now command to halt billing immediately.

Monitoring: An IAM Instance Profile is attached to the EC2 instance, granting it permissions to stream execution and experiment logs directly to AWS CloudWatch.

🚀 Deployment Guide
Prerequisites
AWS CLI installed and configured (aws configure) with appropriate permissions.

Terraform installed.

Execution Steps
Clone this repository to your local machine.

Navigate to the directory containing the Terraform files (main.tf and ml_bootstrap.sh).

Initialize the Terraform backend:
terraform init

Review the deployment plan:
terraform plan

Deploy the infrastructure (type yes when prompted):
terraform apply

Post-Deployment
Once deployed, Terraform will output the ml_engine_instance_id. You can monitor the progress of the machine learning pipeline by checking the system logs in AWS CloudWatch. The instance will terminate itself upon completion.
