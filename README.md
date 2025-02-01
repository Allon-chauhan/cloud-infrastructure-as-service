# ☁️ Cloud & Infrastructure as a Service (IaaS) Basics

This guide walks you through the process of hosting a Java-React application on a remote server.

### 🛠️ Technologies Used:
1. DigitalOcean
2. Linux
3. Java
4. Gradle

## ✅ Prerequisites
1. A DigitalOcean account with available credits to launch a droplet.
2. Java SDK (preferably **OpenJDK 8**) installed on your local machine.
3. SSH key generated on your local machine for secure server access.
4. Gradle installed locally to build the Java application.

### ⚙️ Setting Up and Configuring a Server on DigitalOcean
1. Create a small, cost-effective droplet in your nearest region.
2. Choose **SSH** as the authentication method.
3. Create a new SSH key, copy the public key from your local machine, and save the changes.
4. Once the droplet is ready, note down the **public IPv4** address.
5. Connect to the server via terminal:
   ```bash
   ssh root@<droplet-ipv4>
   ```

*If SSH is configured correctly, you'll be logged in as the root user.*

### ☕ Installing Java on the Remote Server
1. Use the **APT** package manager to install OpenJDK 8:
   ```bash
   apt update
   apt install openjdk-8-jre-headless
   ```
2. Verify the installation:
   ```bash
   java -version
   ```

### 👤 Creating and Configuring a New Linux User
1. As the root user, create a new user:
   ```bash
   adduser <username>
   ```
2. Add the new user to the **sudo** group:
   ```bash
   usermod -aG sudo <username>
   ```
3. Switch to the new user:
   ```bash
   su - <username>
   ```
4. To enable SSH access for this user, add your public SSH key to the `authorized_keys` file:
   ```bash
   mkdir -p ~/.ssh
   nano ~/.ssh/authorized_keys
   ```
5. Paste your public SSH key, save, and exit. Set the correct permissions:
   ```bash
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

*You should now be able to access the server using:*
```bash
ssh <username>@<droplet-ipv4>
```

### 🚀 Deploying and Running a Java Gradle Application on the Droplet
*Using a demo Java-React application from [this repository](https://gitlab.com/twn-devops-bootcamp/latest/05-cloud/java-react-example.git):*

1. Clone the project locally:
   ```bash
   git clone https://gitlab.com/twn-devops-bootcamp/latest/05-cloud/java-react-example.git
   ```
2. Build the application:
   ```bash
   gradle build
   ```
   *Note: It is recommended to use **OpenJDK 8** due to deprecated Gradle features in this project.*
3. Locate the JAR file in the `/build/libs` directory.
4. Transfer the JAR file to your server using **SCP**:
   ```bash
   scp /path/to/your.jar <username>@<droplet-ipv4>:/home/<username>
   ```
5. SSH into the server and run the application:
   ```bash
   java -jar /home/<username>/your.jar
   ```

*The application will run on port **7071**.*

**🌐 Access the application by navigating to `http://<droplet-ipv4>:7071` in your browser.**

### 🔒 Additional Security Measures (Optional)
To enhance security:
1. Configure your droplet's firewall to allow SSH connections **only** from your **public IPv4** address.
2. Allow inbound connections on port **7071** from all IPs to access the application.

*This helps protect your server from unauthorized access.*
