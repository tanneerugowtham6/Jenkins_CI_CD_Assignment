# Jenkins CI/CD Pipeline for Python(Flask) based Student Registration System Application

---

Set up a Jenkins pipeline that automates the testing and deployment of a Student Registration System Python web application.

---

## Project Execution Overview

This project is executed in **4 phases**, each containing a set of clear deployment tasks required to fully host and scale the Travel Memory MERN application on AWS.

### Phases of Deployment

- **Phase 1:** Instance Setup & Environment Preparation
- **Phase 2:** Application Deployment (Backend + Frontend)
- **Phase 3:** Domain Integration and SSL Configuration
- **Phase 4:** Scaling & Load Balancing

---

## Environment

### Cloud Platform
- **Provider:** AWS EC2

### Operating System
- **OS:** Ubuntu 20.04 LTS

### AWS Services Used
- EC2
- AMI
- Launch Template
- Auto Scaling Group (ASG)
- Amazon Certificate Manager (ACM)
- Application Load Balancer (ALB)
- Security Groups
- VPC

### Database
- MongoDB Atlas

## Technology Stack

- Python

---

## Phase 1: Instance Setup & Environment Preparation 

### Task-1: Launching EC2 Instance

#### Steps:

1. Login to your AWS Account and search for EC2 service

    <img width="949" height="187" alt="image" src="https://github.com/user-attachments/assets/c899ca60-e19f-4ba9-9e5c-30015013e660" />

2. Click on `Launch Instance`

    <img width="297" height="138" alt="image" src="https://github.com/user-attachments/assets/bdcf3af9-b39b-49dc-9535-0809f7018e24" />

3. Enter the name of the instance, select `Ubuntu Server 24.04 LTS` AMI and select the type of the instance

    <img width="881" height="581" alt="image" src="https://github.com/user-attachments/assets/fd775572-1e93-496f-8507-ba911dafd5c2" />

4. Create a new keypair (Optional) or choose an existing keypair

    <img width="494" height="472" alt="image" src="https://github.com/user-attachments/assets/f62ded24-64f3-4713-bbaa-30cec9b1227a" />

5. Under Networks Settings, click on `edit`. Select the required VPC and a Public Subnet

    <img width="785" height="273" alt="image" src="https://github.com/user-attachments/assets/9b0c4bfe-0569-4345-bc9f-6d4044cb189d" />

6. Under the **Firewall (security groups)**, select **Create security group**. Enter the required details, **Security group name, Description** or use the existing **Securtiy group** with the required **Inbound rules**

    <img width="872" height="816" alt="image" src="https://github.com/user-attachments/assets/8d8cb150-df91-4e7e-9511-da869fcc88ad" />

7. Allow the ports `80, 8080, 3000` or `HTTP, Custom TCP` as below using **Add security group rule** button

    <img width="795" height="820" alt="image" src="https://github.com/user-attachments/assets/602e22a2-21f1-4578-a5ff-3319bda2d0e9" />

8. Click on the `Launch instance` button, which initiates the instance. Refer below for the confirmation

    <img width="622" height="182" alt="image" src="https://github.com/user-attachments/assets/e40b6b0b-554c-4d5d-ba0f-196cf4d032c5" />

### Task-2: Installing Jenkins and other required Software

#### Steps:

1. Go to the EC2 Instance and copy the Public IPv4 or Public DNS
2. Launch the Terminal (macOS), Command Prompt (Windows)
3. Navigate to the folder where your `.pem` file is downloaded and assign the executable permissions to the file

    ```sh
    cd <.pem_file_location>
    chmod 700 skilltestkeypair.pem
    ```

    <img width="579" height="68" alt="image" src="https://github.com/user-attachments/assets/3c9db7a4-9f2d-4eba-b9db-65ea9973c85b" />

4. Connect to the EC2 Instance using the `ssh` command

    ```sh
    ssh -i "<.pem_file_name>" <username>@<public_dns or ipv4 of the instance>
    ```

    <img width="906" height="643" alt="image" src="https://github.com/user-attachments/assets/e0268094-098a-4611-9739-0c17a4d1c678" />
5. Update and upgrade the available packages

    ```sh
    sudo apt update
    sudo apt upgrade -y
    ```

    <img width="853" height="953" alt="image" src="https://github.com/user-attachments/assets/70609570-2cb6-4921-b8e3-b89b1bf6d805" />
    <img width="1312" height="313" alt="image" src="https://github.com/user-attachments/assets/a47e70ea-b596-4464-8527-c78a7b7440e1" />
    <img width="817" height="365" alt="image" src="https://github.com/user-attachments/assets/002d9cac-1a41-46a9-92e5-510453322056" />

6. Install Java (Pre-requisite for Jenkins)

   ```sh
   sudo apt install openjdk-17-jdk -y
   java -version
   ```

   <img width="1545" height="370" alt="image" src="https://github.com/user-attachments/assets/6f8d4c79-7d12-4016-95aa-1ef3269eaac4" />
   <img width="941" height="370" alt="image" src="https://github.com/user-attachments/assets/5b4ebb42-a43a-4dc9-81bc-36bc201c42f9" />
   <img width="570" height="80" alt="image" src="https://github.com/user-attachments/assets/b5f32024-3702-4c9f-bad8-508acd6f6843" />

7. Install Jenkins by following the instructions mentioned `https://www.jenkins.io/doc/book/installing/linux/#debianubuntu`

    ```sh
    curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null

    sudo apt update
    sudo apt install jenkins -y
    ```

    <img width="1507" height="254" alt="image" src="https://github.com/user-attachments/assets/cdc8ac5c-7e48-45bb-b3f9-897ca101d9c3" />
    <img width="639" height="256" alt="image" src="https://github.com/user-attachments/assets/b84ac8c9-0ca2-4d4b-9b78-36907ed3ae78" />
    <img width="862" height="660" alt="image" src="https://github.com/user-attachments/assets/a12a6374-ab1e-4b6c-8928-1cbe27f8af48" />

8. Start Jenkins Service

    ```sh
    sudo systemctl enable jenkins
    sudo systemctl start jenkins
    sudo systemctl status jenkins
    ```

    <img width="1491" height="390" alt="image" src="https://github.com/user-attachments/assets/76f97af8-7ec6-46f0-a32a-fa499fefd790" />

9. Now try to access Jenkins on `http://<public-ip_or_public-dns>:8080`

    <img width="1546" height="1067" alt="image" src="https://github.com/user-attachments/assets/08354c91-9dd7-42fa-a3eb-3d19a9800bd6" />

10. Install other required tools or software, like Python, which is required for our Flask application

    ```sh
    sudo apt install python3 python3-pip -y
    ```

    <img width="1546" height="397" alt="image" src="https://github.com/user-attachments/assets/5226706d-9a8d-4fa5-a4c7-acf852a87e78" />
    <img width="820" height="397" alt="image" src="https://github.com/user-attachments/assets/5549aa81-7f98-4750-8dd3-810a57145e50" />

11. Verify the installations

    ```sh
    python3 --version
    pip3 --version
    ```

    <img width="450" height="76" alt="image" src="https://github.com/user-attachments/assets/cb857d17-26a0-4b6f-ad1d-0687b1a207f4" />

### Task-2: Conigure Jenkins

#### Steps:

1. Open Jenkins URL `http://<public-ip_or_public-dns>:8080`

    <img width="1546" height="1067" alt="image" src="https://github.com/user-attachments/assets/08354c91-9dd7-42fa-a3eb-3d19a9800bd6" />
    
2. Unlock the server by using the key on the Jenkins server

    ```sh
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

    <img width="582" height="47" alt="image" src="https://github.com/user-attachments/assets/59416bba-5c39-45c1-aa90-4c6238571382" />

3. Copy the password and paste it in the Administrator Password Section, then click on **Continue**

    <img width="1295" height="1064" alt="image" src="https://github.com/user-attachments/assets/0aa0fd2f-d2b4-46e2-b093-c8fe53f40a7b" />

4. Select **Install suggested plugins**

    <img width="825" height="414" alt="image" src="https://github.com/user-attachments/assets/e2996c46-f6c2-4602-af04-a8d738fd6aac" />
    <img width="825" height="819" alt="image" src="https://github.com/user-attachments/assets/320605ea-d17d-46ab-bffe-193ed35489c9" />

5. Create an Admin User, fill in the details, and click on **Save and continue**

    <img width="825" height="959" alt="image" src="https://github.com/user-attachments/assets/597a7ec1-6c80-4b7d-95b3-3ba91454b77b" />

6. Confirm the Instance Configuration URL, then click on **Save and Finish**

    <img width="825" height="959" alt="image" src="https://github.com/user-attachments/assets/be5f04e3-df07-4630-9002-8d294439a11f" />

7. Confirm the configuration by clicking on **Start using Jenkins**

    <img width="825" height="247" alt="image" src="https://github.com/user-attachments/assets/34f041b9-056a-4ca3-bc66-73225df87a5b" />

8. On Jenkins Homepage, click on the Settings icon

    <img width="1709" height="564" alt="image" src="https://github.com/user-attachments/assets/0a96c906-b19e-4bf4-9c4a-7bc51e21648d" />

9. Install the Jenkins plugins by navigating to the Plugins sections

    <img width="1709" height="1071" alt="image" src="https://github.com/user-attachments/assets/efbeae92-c9a5-478d-9abe-1a08fc6fbc05" />

10. Click on Available plugins, search for the **ShiningPanda & Email Extension Plugin** plugin, and click on **Install** and select **Restart Jenkins when installation is complete and no jobs are running**

    <img width="1709" height="455" alt="image" src="https://github.com/user-attachments/assets/d30e8a3d-891b-47ad-9842-594afd576460" />
    <img width="447" height="87" alt="image" src="https://github.com/user-attachments/assets/2e8e3f33-2eeb-47eb-bfc7-45a9a20072a7" />
    <img width="1710" height="379" alt="image" src="https://github.com/user-attachments/assets/bb9127ec-26f0-4b98-8c33-31af087abdf2" />

11. Once restarted, log back in to the Jenkins page

---

## Phase 2: Create Jenkins pipeline

### Task-1: Create a Job in Jenkins

1. Open Jenkins Homepage, click on **New item**

    <img width="1710" height="558" alt="image" src="https://github.com/user-attachments/assets/f9b3fbcc-b310-4af1-b5a1-91e407dd4823" />

2. Enter an item name as `python_ci_cd_pipeline`, select the item type as `Pipeline`, and click on `OK`

    <img width="824" height="687" alt="image" src="https://github.com/user-attachments/assets/30c9f9f5-b91a-4ec6-a14b-ecd7cffd324f" />

3. Enter Description and scroll down to the **Pipeline** section and select Definition as **Pipeline script from SCM**
4. Select SCM as **Git** and enter the **Repository URL**

    <img width="1168" height="506" alt="image" src="https://github.com/user-attachments/assets/ab554649-95ab-4b64-bd77-e61c28f45796" />

5. Under **Credentials**, click on **Add** and select **Global**
6. Select **GitHub App**, click on **Next**

    <img width="441" height="514" alt="image" src="https://github.com/user-attachments/assets/8aa5a1e6-fbf2-4d67-8630-8ac6c52279c9" />

7. ds
8. v
9. ds
10. v
11. dsf
12. 
