# Getting Started with FARMLY - The Grocery App

Farmly is a grocery app designed to provide users with a convenient and efficient way to shop for their groceries online. Here is a brief description of its features and functionality:

Farmly: Your Online Grocery Shopping Companion
Overview:
Farmly is a modern grocery app that brings the convenience of online shopping to your fingertips. Whether you're planning your weekly grocery run or need to quickly replenish your pantry, Farmly ensures a seamless and enjoyable shopping experience.

Key Features:

1. User-Friendly Interface:
     + Intuitive Navigation: Easily browse through various categories and products with a clean and organized interface.
     + Search and Filter: Quickly find specific items using the powerful search function and filter options.

2. Wide Range of Products:
     + Fresh Produce: Access a variety of fresh fruits, vegetables, and herbs sourced from local farms.

3. Personalized Shopping Experience:

     + Recommendations: Receive personalized product recommendations based on your shopping history and preferences.
     + Favorites: Save your favorite items and create custom shopping lists for easy access.

4. Convenient Delivery Options:

     + Flexible Scheduling: Choose a delivery time that suits your schedule, with same-day or next-day delivery options.
     + Order Tracking: Track your order in real-time and receive notifications on its status.

5. Secure and Easy Payment:

     + Multiple Payment Methods: Pay securely using credit/debit cards, digital wallets, or cash on delivery.
     + Promotions and Discounts: Take advantage of special offers, discounts, and loyalty rewards.

4. Customer Support:

     + 24/7 Assistance: Get help anytime with dedicated customer support available through chat, email, or phone.
     + Order Issues: Quickly resolve any issues with your orders, including returns and refunds.

Why Choose Farmly?

Quality Assurance: Farmly partners with trusted suppliers to ensure the highest quality of products.
Time-Saving: Skip the lines and the hassle of physical grocery shopping by ordering from the comfort of your home.
Eco-Friendly: Support sustainable practices with eco-friendly packaging and local sourcing of products.
Farmly aims to revolutionize the way you shop for groceries by combining convenience, quality, and personalized service into one seamless app experience.

# STEP 1 : Launch an Ubuntu(22.04) T2 Large Instance



# Step 2 —

# Step 2 (A) - Install Jenkins

     sudo apt update -y

     sudo apt upgrade -y 

     sudo apt install openjdk-17-jre -y

     curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
     /usr/share/keyrings/jenkins-keyring.asc > /dev/null
     echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
     https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
     /etc/apt/sources.list.d/jenkins.list > /dev/null
     sudo apt-get update -y 
     sudo apt-get install jenkins -y


Go to AWS EC2 Security Group and open Inbound Port to all traffic 

<EC2 Public IP Address:8080>

    sudo cat /var/lib/jenkins/secrets/initialAdminPassword

UNLOCK JENKINS



Create a user click on save and continue.



# Step 2 (B) — Install Docker and setup Sonarqube

    sudo apt update -y

    sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" -y

    sudo apt update -y

    sudo apt-get install docker-ce docker-ce-cli containerd.io

    apt-cache policy docker-ce -y

    sudo apt install docker-ce -y

    sudo systemctl status docker

    sudo chmod 777 /var/run/docker.sock

    sudo docker --version

    # Remove old versions
    sudo apt-get remove docker docker-engine docker.io containerd runc
    
    sudo apt-get update
    
    sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
    
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -


    sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"

    sudo apt-get update
    
    sudo apt-get install docker-ce docker-ce-cli containerd.io

    sudo systemctl status docker
    
After the docker installation, we created a Sonarqube container


    docker run -d --name sonar -p 9000:9000 sonarqube:lts-community



Now our Sonarqube is up and running

Enter username and password, click on login and change password

username admin / password admin

Update New password, This is Sonar Dashboard.



# Step 2 (C) — Install Trivy [ IAC ]

     sudo apt-get install wget apt-transport-https gnupg lsb-release
     wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
     echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
     sudo apt-get update
     sudo apt-get install trivy

# Step 3 — Install Plugins like JDK, Sonarqube Scanner,

# Step 3 (A) — Install Plugin in jenkins server

Go to Manage Jenkins →Plugins → Available Plugins →
Install below plugins

1 → Eclipse Temurin Installer

2 → Sonarqube Scanner 

3 → OWASP Dependancy Check

4 → Docker

5 → Docker commons

6 → Docker Pipeline

7 → Docker Api

8 → Docker Built Steps

9 → Slack Notification

10 → Sonar Quality Gates

# Step 3 (B) — Configure Java in Global Tool Configuration

Goto Manage Jenkins → Tools → Install JDK(17) and NodeJs(16)→
Click on Apply and Save



# Step 3 (C) — Create a Job

create a job as Devsecops_demo Name, select pipeline and click ok.

# Step 4 — Configure Sonar Server in Manage Jenkins

Grab the Public IP Address of your EC2 Instance, Sonarqube works on
Port 9000, so <Public IP>:9000.

Goto your Sonarqube Server.

Click on Administration → Security → Users → Click on Tokens and
Update Token → Give it a name → and click on Generate Token

copy Token

Goto Jenkins Dashboard → Manage Jenkins → Credentials → Add Secret
Text. It should look like this

Now, go to Dashboard → Manage Jenkins → System and Add like the
below image.

Click on Apply and Save

The Configure System option is used in Jenkins to configure different
server

Global Tool Configuration is used to configure different tools that we
install using Plugins

We will install a sonar scanner in the tools.

In the Sonarqube Dashboard add a quality gate also

Administration → Configuration →Webhooks

Add details

Let’s go to our Pipeline and add the script in our Pipeline Script.

https://github.com/tohidhanfi20/DEPLOY-THE-REACTJS-APP-IN-KUBERNETES-WITH-DEVSECOPS-CICD-PIPELINE/blob/main/Jenkinsfile1

Click on Build now, you will see the stage view like this


# Step 5 — Install OWASP Dependency Check Plugins

Go to Dashboard → Manage Jenkins → Plugins → OWASP
Dependency-Check. Click on it and install it without restart.


First, we configured the Plugin and next, we had to configure the Tool

Go to Dashboard → Manage Jenkins → Tools →



Now go configure → Pipeline and add OWASP and TRIVY stage to your
pipeline and build.



You will see that in status, a graph will also be generated and
Vulnerabilities.



# Step 6 — Docker Image Build and Push

We need to install the Docker tool in our system,

Goto Dashboard → Manage Plugins → Available plugins → Search
for Docker and install these plugins

     Docker

     Docker Commons

     Docker Pipeline

     Docker API

     Docker-build-step

Now, goto Dashboard → Manage Jenkins → Tools →



Add Docker Hub Username and Password under Global Credentials





Now Run the container to see if the game coming up or not by adding
below stage

image

<Jenkins-public-ip:3000>


# terminate the instance if you don't you will begger in no time :> 
