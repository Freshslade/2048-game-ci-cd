# AWS CI/CD Deployment – 2048 Game 🎮

This project demonstrates a complete **CI/CD pipeline on AWS** to containerize and deploy the classic **2048 web game** using modern DevOps practices.  

It was inspired by *Tech With Lucy’s* “Advanced AWS Projects” series — I followed her tutorial closely, but also took time to troubleshoot, learn, and rebuild each step myself.  
Along the way, I used Google’s **Gemini** to debug and understand key AWS errors, helping me develop genuine hands-on understanding of cloud infrastructure.

---

## 🌐 Live Application

Here’s the deployed **2048 web game** running live on ECS Fargate:

![Live App Screenshot](./evidence/live-game.png?v=2)

*(For security reasons, the public IP is not shown here, but this was verified working during deployment.)*

---

## 🧠 What I Learned

Throughout this project, I learned how to:

- Use **Docker** to containerize a web app and push images to **Amazon ECR**.  
- Configure and deploy containers using **Amazon ECS (Fargate)**.  
- Set up a fully automated CI/CD workflow with **AWS CodePipeline** and **CodeBuild**.  
- Manage permissions and IAM roles for ECS, CodeBuild, and ECR.  
- Recreate a **default VPC**, subnets, and security groups from scratch.  
- Debug common AWS errors (region mismatches, IAM SLR issues, networking).  

This wasn’t a copy-paste exercise — I built each part manually, fixed my own mistakes, and now have a much clearer mental model of how AWS services connect.

---

## 🧩 Architecture Overview

Here’s how everything fits together — including my own screenshots from each stage of deployment:

1. **CodeBuild** – builds the Docker image, tags it, and pushes it to ECR.  
   ![CodeBuild Success](./evidence/codebuild-green.png?v=2)

2. **Amazon ECR** – stores the Docker image for ECS to use.  
   ![ECR Repository](./evidence/ecr-service.png?v=2)

3. **Amazon ECS (Fargate)** – runs the containerized 2048 app as a service.  
   ![ECS Tasks and Container URI](./evidence/ecs-tasks-container-uri.png?v=2)
   ![ECS Task Public IP](./evidence/ecs-task-public-ip.png?v=2)

4. **CodePipeline** – automates the process from GitHub → build → deploy.  
   ![CodePipeline Successful Run](./evidence/code-pipeline-green-run.png?v=2)

5. **S3** – stores the build artifact (`imagedefinitions.json`) for ECS deployment.  
   ![S3 Artifact](./evidence/s3-imagedefinitions.png?v=2)

6. **Security Groups & Networking** – opened port 80 to allow public HTTP access.  
   ![Security Group Port 80](./evidence/sg-port-80.png?v=2)

---

## 🧩 Debugging and Problem-Solving Highlights

I used **Gemini** to help me debug issues and learn *why* things broke:

- ❌ **ECR login failed (400 Bad Request)** → Fixed region mismatch between `get-login-password` and ECR URI.  
- ⚙️ **ECS cluster creation failed** → Learned about AWS Service-Linked Roles (`AWSServiceRoleForECS`).  
- 🌐 **Website not showing on Public IP** → Fixed security group inbound rules for HTTP (port 80).  
- 🧩 **Default VPC missing** → Recreated VPC, subnets, and security groups manually and via VPC wizard.  
- 🔑 **Git identity issue** → Configured `git config --global user.name` and `user.email` properly.  

These weren’t just fixes — they were lessons in AWS networking, permissions, and service dependencies.

---

## 🛠️ Technologies Used

- **AWS ECS (Fargate)**
- **AWS CodeBuild**
- **AWS CodePipeline**
- **Amazon ECR**
- **Amazon S3**
- **AWS IAM**
- **Docker**
- **Git & GitHub**

---

## 💡 Reflections

This project gave me my first real hands-on experience with **AWS DevOps tooling**.  
It showed me how infrastructure pieces like CodePipeline, CodeBuild, and ECS connect to automate deployment.  

While it began as a guided tutorial, the process of debugging and rebuilding each step on my own made me far more confident in cloud deployment and troubleshooting.

---

## 🙌 Credits

- **Tech With Lucy** – for the excellent AWS project tutorial series that guided this process.  
- **Google Gemini** – for helping explain AWS errors and command-line issues during troubleshooting.  
- **AWS Documentation** – for validating configurations and permissions.  

---

📁 *Repository structure:*
2048-game-ci-cd/
│
├── evidence/ # Screenshots and proof of deployment
│ ├── code-pipeline-green-run.png
│ ├── codebuild-green.png
│ ├── ecr-service.png
│ ├── ecs-task-public-ip.png
│ ├── ecs-tasks-container-uri.png
│ ├── live-game.png
│ ├── s3-imagedefinitions.png
│ └── sg-port-80.png
│
├── Dockerfile
├── buildspec.yml
└── README.md
