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

    chmod 777 /var/run/docker.sock
    
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

# Step 3 (B) — Configure Tools in jenkins

Go to Manage Jenkins → Tools →

Configure JDK

+ Name - jdk17
+ Install from - adobtium.net
+ version - jdk-17.0.8.1+1
+  + Click on Apply and Save
        
Configure Sonarqube Scanner

+ Name - sonar-scanner
+ Install automatically - Ver - 5.0.1.3006

Configure OWASP Dependency Check

+ Name - DP-Check
+ Install from - Github.com
+ Version - 6.5.1

Configure Docker

+ Name - Docker
+ Install from - github.com
+ Version - Latest

# Step 4 — Let's Set-up Credentials in jenkins

1) Docker Credentials (using username & pass)
+ Username - tohidaws (example)
+ Password - ********
+ ID - docker
+ Description - docker

2) Sonar Credentials ( Using secret text)
   
For sonar token 
+ navigate to sonarqube dashboard
+ go to administrator tab
+ then naviagate to security
+ Enter name , Type , and expiry of token an generate it and copy token

NOW navigate to manage jenkins → Credentials 

+ Kind - secret text
+ secret - Paste what you copied
+ ID - Sonar-token

3) Slack Credentials ( Using secret text)

For Slack secret text

Navigate to → Slack website → more → Automation → Search Jenkins CI → Configuration → Add to Slack →
Choose a channel to get updates about your deployment → Take integration Credential ID → Secret text

NOW navigate to manage jenkins → Credentials  

+ Kind - secret text
+ secret - Paste what you copied
+ ID - slack-jenkins-ci

# Step 4 — Configure System settings in Manage Jenkins

1) Sonarqube Installations
   + Name - sonar-server
   + Server URL - http://your_server_ip:9000
   + Server Auth token - You will see (sonar-token) that we configured in the credentials

2) Slack
   + Workspace -- will be found on slack-jenkins integation page -- With a name of (TEAM SUB DOMAIN) -- Copy and paste HERE
   + Credentials - You will see slack-jenkins-ci -- configured in credentials
   + default channel - #general as saved in integeration page


# Let's add webhook between Jenkins server and Sonarqube

NOW navigate to soanrqube Dashboard → Administration → Configuration → webhooks → Create

+ Name - (Your Wish)
+ URL - http://Your_machine_ip:8080/sonarqube/webhook/
+ Create

# NOW Let's Build Our pipeline

NOW navigate to Jenkins Dashboard → New item → Name it → Select Pipeline → Click OK

Go to Pipeline and paste it from 
https://github.com/tohidhanfi20/Farmly---The_Grocery_App/blob/main/jenkinsfile

NOTE : Do changes in all steps ex - Docker Credentials , Slack Credentials etc.

SAVE THE PIPELINE

BUILD NOW

HERE ARE SOME SCREENSHOTS


# terminate the instance if you don't you will begger in no time :> 
