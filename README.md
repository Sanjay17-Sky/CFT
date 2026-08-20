AWS CloudFormation + EC2 + GitHub Actions Web Deployment

A hands-on DevOps project that uses AWS CloudFormation to provision AWS networking and an EC2 instance, followed by GitHub Actions automation to deploy a web application to the EC2 server using Docker.

🚀 Project Overview

In this project, I built and deployed a web application using Infrastructure as Code and CI/CD concepts.

The infrastructure is defined using an AWS CloudFormation template. It creates a VPC, public and private subnets, an Internet Gateway, route table, security group, and an EC2 instance.

The application deployment is automated using GitHub Actions. Whenever changes are pushed to the "main" branch, the workflow connects to the EC2 instance through SSH, pulls the latest repository changes, builds a Docker image, replaces the previous container, and starts the updated application.

🏗️ Architecture

                    ┌──────────────────┐
                    │    Developer     │
                    └────────┬─────────┘
                             │
                             │ git push
                             ▼
                    ┌──────────────────┐
                    │     GitHub       │
                    │   Repository     │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ GitHub Actions   │
                    │      CI/CD       │
                    └────────┬─────────┘
                             │
                             │ SSH
                             ▼
              ┌─────────────────────────────┐
              │          AWS EC2            │
              │                             │
              │  ┌───────────────────────┐  │
              │  │       Docker          │  │
              │  │  ┌─────────────────┐  │  │
              │  │  │   Web Container  │  │  │
              │  │  │      Port 80     │  │  │
              │  │  └─────────────────┘  │  │
              │  └───────────────────────┘  │
              └──────────────┬──────────────┘
                             │
                         Port 8080
                             │
                             ▼
                       🌐 Web Browser

☁️ AWS Infrastructure

The CloudFormation template "CFT.yaml" provisions:

- VPC — "10.0.0.0/16"
- Public subnet — "10.0.1.0/24"
- Private subnet — "10.0.2.0/24"
- Internet Gateway
- Route table
- Public subnet route-table association
- Security group
- EC2 instance
- "t3.micro" instance type

The EC2 instance is placed in the public subnet and configured with a public IP address.

🛠️ Technologies Used

Technology| Purpose
AWS CloudFormation| Infrastructure as Code
Amazon VPC| Network infrastructure
Amazon EC2| Application server
AWS Security Groups| Network access control
Git| Version control
GitHub| Source-code repository
GitHub Actions| CI/CD automation
Docker| Application containerization
Linux/Ubuntu| Server environment
SSH| Remote server connection
HTML| Web application

📁 Project Structure

CFT/
│
├── .github/
│   └── workflows/
│       └── deploy.yaml
│
├── CFT.yaml
│
└── sanjay web.html

"CFT.yaml"

Contains the AWS CloudFormation infrastructure definition.

"deploy.yaml"

Contains the GitHub Actions CI/CD workflow.

"sanjay web.html"

The web page deployed by the project.

⚙️ How It Works

1. Infrastructure Provisioning

The AWS CloudFormation template defines the required AWS resources.

The template creates the networking environment and EC2 instance instead of requiring the infrastructure to be manually created through the AWS console.

2. Source Code Management

The application source code is maintained in GitHub.

Changes are committed and pushed to the "main" branch.

3. CI/CD Trigger

The GitHub Actions workflow is configured to run when code is pushed to "main".

on:
  push:
    branches:
      - main

4. GitHub Actions Connects to EC2

The workflow uses SSH credentials stored as GitHub Secrets:

EC2_HOST
EC2_USERNAME
EC2_SSH_KEY

This keeps the connection credentials outside the workflow source code.

5. Latest Code Is Pulled

The workflow connects to the EC2 server and runs:

cd ~/CFT
git pull origin main

6. Docker Image Is Built

The application image is built using:

sudo docker build -t sanjay-web .

7. Previous Container Is Replaced

The existing container is stopped and removed:

sudo docker stop sanjay-web-container || true
sudo docker rm sanjay-web-container || true

8. New Container Is Started

The updated application is launched:

sudo docker run -d \
  --name sanjay-web-container \
  -p 8080:80 \
  sanjay-web

This maps EC2 port "8080" to port "80" inside the Docker container.

🔄 CI/CD Flow

1. Modify website
       ↓
2. git add / git commit
       ↓
3. git push origin main
       ↓
4. GitHub Actions starts
       ↓
5. Connect to EC2 using SSH
       ↓
6. git pull origin main
       ↓
7. Build Docker image
       ↓
8. Stop old container
       ↓
9. Remove old container
       ↓
10. Start new container
       ↓
11. Updated website is available

📸 Screenshots

AWS CloudFormation Stack
<img width="1366" height="551" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/273a77f3-e5fd-4323-8eca-db398108a095" />

GitHub Actions
  Image of Github Actions Workflow
  <img width="1067" height="225" alt="WhatsApp Image 2026-08-19 at 8 33 29 PM" src="https://github.com/user-attachments/assets/c77e5a12-49df-48a0-9375-401941e257d6" />

  Image of Automated Docker Deployment
  <img width="1066" height="212" alt="WhatsApp Image 2026-08-19 at 8 33 29 PM (1)" src="https://github.com/user-attachments/assets/e8c0449a-b196-4540-8eb3-138b728f71bb" />

Docker Container

<img width="1366" height="60" alt="WhatsApp Image 2026-08-19 at 8 42 24 PM" src="https://github.com/user-attachments/assets/9c0ede38-b270-4281-9165-d54919b2f7e4" />


Deployed Website

<img width="1366" height="683" alt="Screenshot (17)" src="https://github.com/user-attachments/assets/78677c0f-8c8f-485e-9f1b-9c3679aa41c3" />


🧩 Challenges I Faced

During the project, I worked through several real deployment and infrastructure problems, including:

- CloudFormation stack rollback and template correction
- EC2 SSH connection and key-permission issues
- Understanding public and private subnet configuration
- Managing web-server services on Linux
- Troubleshooting GitHub Actions failures
- Handling EC2 public IP changes
- Configuring GitHub Secrets for deployment
- Understanding Docker image and container deployment

These troubleshooting steps helped me understand that DevOps is not only about creating infrastructure, but also about monitoring, debugging, and fixing deployment problems.

📚 What I Learned

Through this project, I learned:

- How Infrastructure as Code works with AWS CloudFormation
- How VPC networking is structured
- The difference between public and private subnets
- How Internet Gateways and route tables provide connectivity
- How EC2 instances are configured and accessed
- How Linux services and server environments are managed
- How Git and GitHub are used in a development workflow
- How GitHub Actions can automate deployments
- How GitHub Secrets can protect deployment credentials
- The basics of Docker images and containers
- How CI/CD connects source-code changes to application deployment
- How to troubleshoot real-world deployment failures

🔐 Security Considerations

This project is intended as a learning project.

During the project, the CloudFormation security group permitted SSH (port 22) and HTTP (port 80) from 0.0.0.0/0. Since this was a learning environment, the AWS resources were deleted after testing. In a production environment, SSH access should be restricted to trusted sources.


🎯 Project Outcome

This project demonstrates a complete beginner-to-intermediate DevOps workflow:

Infrastructure as Code → Cloud Infrastructure → Version Control → CI/CD → Containerization → Automated Deployment

It combines AWS, Linux, Git, GitHub Actions, Docker, and CloudFormation into one practical project.


🧹 Resource Cleanup

After completing the deployment and testing, the AWS CloudFormation stack and associated practice resources were deleted to avoid unnecessary resource usage and charges. The GitHub repository and project documentation were retained for learning and portfolio purposes.

👨‍💻 Author

Sanjay S

GitHub: "Sanjay17-Sky" (https://github.com/Sanjay17-Sky)

---

⭐ If you found this project useful, feel free to explore the repository and the implementation.
