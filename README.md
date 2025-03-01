# 🐳 Deploying Nexus as a Docker Container

## Description
This demo project is part of Module 7: Containers with Docker from the Nana DevOps Bootcamp. It focuses on deploying a Nexus repository on Digital Ocean using Docker Containers.
<br />

## 🚀 Technologies Used

- <b>Docker for containerization.</b>
- <b>Linux: Ubuntu for Server configuration and management.</b>
- <b>Nexus: Private docker repository hosted on DigitalOcean.</b>
- <b>Digital Ocean: Cloud provider for hosting Nexus repository.</b>
  

## 🎯 Features

- <b>Create a Digital Ocean droplet to host Nexus</b>
- <b>Persist Nexus data using Docker Volumes.</b>
- <b>Deploy Nexus Repository as a Docker container.</b>


## 🏗 Project Architecture
<img src="" width=800 />

## ⚙️ Project Configuration
### Creating a Digital Ocean Droplet to host Nexus
1. Sign up for an account on [DigitalOcean](https://www.digitalocean.com).
2. Create a droplet:<br>
   a) Select **Create Droplet** from the Digital Ocean dashboard. <br>

      <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/create%20a%20droplet%201.png?raw=true"/>
    
   b) Select a region: Select the region closest to your location.<be>

      <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/Droplet%20region.png?raw=true"/>

   c) Select an image: Select the Ubuntu image.<br>

      <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/CreatingDropletChooseImage.png?raw=true"/>
     
     - Choose Size: <br>
       * Droplet type: Select Basic.<br>
       * CPU options: Select Regular SSD and the 8GB option.<br>

   
   d) Select the SSH key  as the Authentication Method. <br>
    - Generate a New SSH Key (if required): If you do not have an existing SSH key, follow DigitalOceans's guide to create a new pair.
    - Use an existing SSH Key: if you already have a public SSK key pair, navigate to the .ssh folder in your local directory. Copy the public key and paste it into the appropriate field in DigitalOCean's interface.<br>

      <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/DropletSSH.png?raw=true"/>

   e) Select **Create Droplet**

3. Configuring Firewall on Digital Ocean: Following security best practices, configure the firewall's inbound and outbound rules. In this case, you only allow inbound SSH access from your machine to the Droplet and allowing access to Nexus port. restricting all other unnecessary connections.

4. Select the **Networking** option from the left panel, then choose **Firewall**.

   <img src="https://github.com/lala-la-flaca/deploy-java-app-digitalocean/blob/main/resources/Img/SettingUpFirewall.png?raw=true"/>

5. Click on **Create Firewall**
  
6. Set the firewall rules for incoming traffic.<br>
   Allow SSH access from your machine by adding an inbound rule that allows traffic from the public IP address of your machine on port 22 and Nexus port 8081 and Nexus Docker adapter 8083 .

   <img src=""/>
   
7. SSH into the droplet to verify that everything works as expected.

   <img src="" width=800/>

### Deploying Nexus Repository as a Docker Container
1. Connect to the droplet using SSH.

   ```bash
   ssh root@64.227.16.46
   ```
   <img src="" width=800/>
      
2. Update the package manager, install net tools, and install Docker.

   ```bash
   apt update
   apt install net-tools
   snap install docker
   ```

   <img src="" width=800/>

3. Navigate to GitHub and find the Offical Nexus Image

   <img src="" width=800/>
   
4. Set up a Docker Volume to store Nexus Data.

   ```bash
   docker volume create --name nexus-data
   ```

   <img src="" width=800/>
   
5. Run the Nexus container using docker.

   ```bash
   docker run -d \
   -p 8081:8081 \
   --name nexus \
   -v nexus-data:/nexus-data \
   sonatype/nexus3
   ```

   <img src="" width=800/>

      
6. Ensure that the Nexus container is running.

    ```bash
    docker ps
    ```

    <img src="" width=800/>

7. Open a web browser and navigate to the Nexus Repository URL.

    [Nexus repository](HTTP://64.227.16.46:8081)

    <img src="" width=800/>
    
8. Validate the user by accessing the docker container.

    ```bash
    docker ps
    docker exec -it eadd42683cc5 /bin/bash
    whoami
    exit 
    ```
    <img src="" width=800/>

    
9. Check persistent Data by navigating to the Nexus directory on the droplet. The docker inspect allows us to know the location of the data.

    ```bash
    docker volume ls
    docker inspect nexus-data
    ```
    <img src="" width=800/>
