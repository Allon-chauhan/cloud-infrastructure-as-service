# Cloud & Infrastructure as Service Basics

This folder contains data and information required to host a Java-React application on a remote server!

### Technologies used:
1. DigitalOcean
2. Linux
3. Java
4. Gradle

## Pre-requisites
1. DigitalOcean account with some available credits to launch a droplet.
2. JavaSDK (preferably OPENJDK@8) available and installed on your local computer.
3. SSH key generated on your local computer to access the server via SSH.
4. Gradle installed on your local computer to build Java application locally.

### Setting and configuring a server on DigitalOcean
1. Create a small and affordable droplet in your nearest location.
2. To access this droplet, we'll be using SSH (Secure Shell). So, select SSH as authentication method.
3. Create a new SSH key and copy the public key from your local computer and save the changes.
4. Once, the droplet is ready - note down the public IPv4.
5. Using your terminal, connect to the server - `$ ssh root@droplet-ipv4`

(If SSH is configured correctly, you should be able to log into your server as root user.)

### Installing Java on remote server to run the application
1. Using APT package manager, we can install openjdk version 8
2. In the CLI, enter command `$ apt install openjdk-8-jre-headless`
3. Once done, you can verify it using command `$ java -version`

### Create and configure a new linux user on the Droplet
1. Once you are in the server as root user, using command `$ adduser [username]` - you can create a new user.
2. Now we need to add the newly created user to sudo group, use command for so `$ usermod -aG sudo [username]`.
3. You can use command `$ su - [username]` to switch user.
4. To access this user directly via SSH, we need to add the public SSH key to the authorized_keys file.
5. Since this is a new user, we need to create the authorized_keys file in .ssh folder.

(If configured correctly, you should be able to access your user using command - `$ ssh [username]@droplet-ipv4` )

### Deploy and run a Java Gradle application on Droplet
(I used a demo java-react application available [here](https://gitlab.com/twn-devops-bootcamp/latest/05-cloud/java-react-example.git))
1. Clone the gitlab project locally in your directory.
2. Build the application using command - `$ grable build` (keep in mind, it is recommended to use openjdk@8 to build this project because of the deprecated Gradle features used in this application)
3. Once built, you can access the JAR file in /build/libs directory
4. Now we can use command - $ SCP (secure copy), to transfer the JAR from local computer to the server (command - `$ SCP [jar-file-path] [username]@droplet-ipv4:/root`)
5. When done, SSH into the server and run the application using command `$ java -jar [jar-file-path]`

(this application will run on port **7071**)

**To access the application, go to your browser and type in - `droplet-ipv4-address:7071`, you should be able to access your application**

### Additional security measures (optional)
For your remote server, you can configure the inbound firewall rules to allow SSH **only** from your **public IPv4 address** and port **7071** connections (from all destination IPs). This will ensure that we are not exposing our remote server to unknown inbound connections.
