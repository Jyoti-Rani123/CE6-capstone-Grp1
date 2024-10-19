<h1>Capstone Project: CI/CD Pipeline for Web App Deployment on AWS ECS</h1>

<h3>Project Team</h3>
<ul>
    <li><strong>Jyoti</strong> Rani</li>
    <li><strong>Keren</strong> Priya</li>
    <li>Bakthavachalam<strong> Saravanan</strong></li>
    <li>Vedarethinam<strong> Vennila</strong></li>
</ul>
<p>Students of PaCE@NTU enrolled in the SCTP Cloud Infrastructure Engineering course.</p>

<hr>

<h3>Project Overview</h3>
<ul>
    <li><strong>Project Name:</strong> CI/CD Pipeline for Web App Deployment on AWS ECS</li>
    <li><strong>Repository:</strong> GitHub Repository - https://github.com/Saravanannus/ce6-capstone-grp1</li>
</ul>
<p>This project automates the deployment of a Node.js application using Docker on Amazon ECS, utilizing GitHub Actions for Continuous Integration and Continuous Deployment (CI/CD) and HCP Terraform for infrastructure provisioning.</p>

<hr>

<h3>Project Objectives</h3>
<ul>
    <li>Implement a CI/CD pipeline using GitHub Actions for automated building, testing, and deployment of a Dockerized application to AWS ECS.</li>
    <li>Provision and manage AWS infrastructure using HCP Terraform, ensuring reliable and scalable application deployment.</li>
</ul>

<hr>

<h3>Architecture Diagram</h3>
<p>The architecture diagram below depicts the project's structure and interactions between components:</p>

![image](https://github.com/user-attachments/assets/0d0bbf04-9a8c-40a9-bfb2-f2a212f50d2b)


<p>The architecture consists of a Node.js application running in Docker containers on Amazon ECS, with a CI/CD pipeline configured through GitHub Actions.</p>

<hr>

<h3>Getting Started</h3>
<h4>Tools and Technologies Used:</h4>

<h5>Setup Terraform Cloud:</h5>
<ul>
    <li>Link Terraform Cloud with your repository.</li>
    <li>Define workspaces for dev and prod.</li>
</ul>

<h5>List of AWS Resources used in this Project:</h5>
<ul>
    <li><strong>GitHub Actions</strong> – Automates the CI/CD pipeline for building, testing, and deploying the application.
    <li><strong>IAM Roles and Policies</strong> – Manages access and permissions for AWS resources between ECS and GitHub Actions, ensuring secure access.
    <li><strong>Infrastructure as Code (IaC)</strong> – 
    Terraform Cloud manages the infrastructure definitions for both environments.
    <li><strong>Amazon ECR</strong> – Stores Docker images for different environments (dev, prod).
    <li><strong>Amazon ECS Fargate</strong> – Runs containerized services without managing infrastructure.
    <li><strong>Networking and Security</strong> – Each ECS cluster is deployed within a VPC using public subnets and security groups. 
    The security group allows HTTP traffic on port 8080 to access the running containers.
    <li><strong>VPC and Public Subnets</strong> – Provides networking and connectivity for the ECS tasks.
</ul>

<h4>Dependencies:</h4>
<ul>
    <li>index.js</li>
    <li>Express</li>
    <li>Jest (for testing)</li>
    <li>Docker</li>
</ul>

<h4>Application Structure</h4>
<pre>
/ce6-capstone-grp1
|-- terraform
    |-- dev
        |-- infra
            |-- main.tf
            |-- provider.tf
            |-- version.tf
    |-- prod
        |-- infra
            |-- main.tf
            |-- provider.tf
            |-- version.tf
|-- .gitignore
|-- Dockerfile
|-- README.md
|-- index.js
|-- package.json
|-- package-lock.json
|-- sum.test.js
</pre>

<hr>

<h3>Project Development Details</h3>
<h4>Steps Overview</h4>
<ol>
    </li>
    <li><strong>Set up AWS credentials globally in GitHub Secrets:</strong>
        <ul>
            <li>AWS_ACCESS_KEY_ID</li>
            <li>AWS_SECRET_ACCESS_KEY</li>
        </ul>
    </li>
    <li><strong>Set Up GitHub Variables:</strong> Ensure that the following environment-specific variables are set in GitHub Repository Settings:
        <ul>
            <li>ECR_REPOSITORY (for both dev and prod)</li>
            <li>ECS_SERVICE (for both dev and prod)</li>
            <li>ECS_CLUSTER (for both dev and prod)</li>
            <li>AWS_REGION (for both dev and prod)</li>
        </ul>
    <li><strong>Push Changes and Merge Feature Branch into Main:</strong>
        <ul>
            <li>Develop the feature in a feature branch and push the changes.</li>
            <li>Create a Pull Request (PR) to merge the feature branch into the main branch.</li>
            <li>Merge the PR into the main branch.</li>
        </ul>
    </li>
    <li><strong>Run Terraform Plan and Apply using HCP Terraform:</strong>
        <ul>
            <li>After changes are merged into the main branch, use HCP Terraform to plan and apply infrastructure changes.</li>
            <li>HCP Terraform should be integrated into the workflow to:
                <ul>
                    <li>Plan changes to AWS resources like ECS services, ECR repositories, etc.</li>
                    <li>Apply the infrastructure changes if the plan looks good.</li>
                </ul>
            </li>
        </ul>
    </li>
    <li><strong>Trigger GitHub Actions Manually:</strong>
        <ul>
            <li>Once the infrastructure is updated, trigger the GitHub Action workflow manually to deploy the ECS application.</li>
            <li>The GitHub Action should:
                <ul>
                    <li>Pull the latest image from the ECR_REPOSITORY.</li>
                    <li>Update the ECS service in the target environment.</li>
                </ul>
            </li>
        </ul>
    </li>
</ol>

<hr>

<h3>Docker Setup for the Application</h3>
<p>The application is containerized using Docker. Below is the Dockerfile that describes how to build the application image.</p>

<h4>Dockerfile</h4>
<pre>
FROM node:16-alpine
WORKDIR /my-app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8080
CMD ["node", "index.js"]
</pre>

<h4>Application Code (index.js)</h4>
<pre>
'use strict';
const express = require('express');
const PORT = 8080;
const HOST = '0.0.0.0';
const app = express();

app.get('/', (req, res) => {
  res.send('Hello from ce6-capstone-grp1!');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
</pre>

<hr>

<h3>CI/CD Pipeline</h3>
<h4>GitHub Actions Workflow for Continuous Deployment</h4>
<p>This workflow automates building, testing, and deploying a Dockerized application to Amazon ECS. It consists of two main jobs: one for building the Docker image and pushing it to Amazon ECR, and the other for deploying the application to the ECS cluster in both development and production environments.</p>

<h4>AWS ECR (Elastic Container Registry):</h4>
<p>Purpose: Stores Docker images that are tagged with version identifiers such as latest or specific commit SHAs (e.g., :abc123).</p>
<p>Integration: GitHub Actions authenticates with ECR and pushes images to designated repositories based on the environment (dev or prod).</p>

AWS Fargate Tasks & Services
ECS services run containerized applications inside Fargate tasks, each exposed on port 8080.

Networking Components
VPC with Public Subnets: Each cluster resides inside a Virtual Private Cloud (VPC) with public-facing subnets.
Security Groups: Allow inbound HTTP traffic on port 8080 and permit all outbound traffic.

<h3>ci-cd.yml Workflow File</h3>
<p><strong>Trigger:</strong> The workflow is triggered manually (<code>workflow_dispatch</code>), allowing for on-demand deployments.</p>
<p><strong>Environment Variables:</strong> Set for easier reuse of names and identifiers like container names, regions, and repository names.</p>

<h4>Jobs Overview:</h4>
<ul>
    <li><strong>build-image:</strong> This job builds a Docker image, pushes it to Amazon ECR, and stores the image tag as an artifact.
        <ul>
            <li><strong>Steps:</strong>
                <ol>
                    <li>Clone the Repository.</li>
                    <li>Checkout the code.</li>
                    <li>Install dependencies (Node.js and npm).</li>
                    <li>Run tests with <code>npm test</code>.</li>
                    <li>Configure AWS credentials using GitHub secrets.</li>
                    <li>Build the Docker image, tag it, and push it to Amazon ECR.</li>
                    <li>Save the image information for the deployment job.</li>
                </ol>
            </li>
        </ul>
    </li>
    <li><strong>deploy-dev and deploy-prod:</strong> These jobs deploy the Docker image to ECS for the dev and prod environments, respectively.
        <ul>
            <li><strong>Steps:</strong>
                <ol>
                    <li>Download the image information artifact.</li>
                    <li>Output the image URL.</li>
                    <li>Retrieve the ECS task definition.</li>
                    <li>Update the task definition with the new Docker image.</li>
                    <li>Deploy the updated task definition to ECS.</li>
                </ol>
            </li>
        </ul>
    </li>
</ul>

<hr>

<h3>Terraform for AWS (ECS) Infrastructure</h3>
<p>Terraform is used to provision and configure AWS infrastructure for the ECS service. The environment folders (<strong>dev/infra</strong> and <strong>prod/infra</strong>) contain configuration files to set up ECS clusters, security groups, and ECS services.</p>

<h4>Key AWS (ECS) Resources by Terraform:</h4>
<ul>
    <li>AWS ECS Cluster</li>
    <li>AWS ECS Task Definitions</li>
    <li>AWS ECS Services</li>
    <li>Security Groups</li>
</ul>
<hr>
<h3>Output Samples:</h3>

<h3>Github Workflow - Waiting for approval before deploying to production environment</h3>
<img width="465" alt="image" src="https://github.com/user-attachments/assets/0f3f5d5b-e753-451b-b57f-1a184e06043c">
<hr>
<h3>Github Workflow - After deployed to production environment:</h3>
<img width="465" alt="image" src="https://github.com/user-attachments/assets/30fb60a3-1a99-4e77-8249-dfe5cbc9cb70">

<hr>

<h3>In Console - Images stored on ECR</h3>
<img src="image-1.png" alt="Images stored on ECR" />

<hr>

<h3>In Console - ECS Clusters in development and production environments</h3>
<img src="image-2.png" alt="Dev and Prod Clusters in ECS" />

<hr>

<h3>Application Access</h3>
<p>Once the application is successfully deployed, it can be accessed locally at <strong>http://localhost:8080</strong> in a web browser.</p>
<img width="539" alt="Application Access" src="https://github.com/user-attachments/assets/5609e39f-7f7b-459d-b6fb-23ee7e86862f">

<hr>

<h3>Conclusion</h3>
<p>This capstone project showcases our ability to deploy an application using best practices in CI/CD automation, infrastructure-as-code with HCP Terraform, and cloud service management with AWS ECS. We have successfully met the project objectives, and our implementation allows for future scaling and optimization.</p>

</body>
</html>
