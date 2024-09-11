# Jenkins

# Jenkins Docker Compose Configuration

The Docker Compose configuration is designed to set up Jenkins and a Jenkins agent with SSH connectivity.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Usage

1. Set your Jenkins agent SSH public key in `.env` file :

   ```env
   JENKINS_AGENT_SSH_PUBKEY=<your_ssh_public_key>
   ```

   Replace `<your_ssh_public_key>` with your actual SSH public key.

2. Run Docker Compose:

   ```bash
   docker-compose up -d
   ```

   This will start the Jenkins master and Jenkins agent containers in the background.

3. Access Jenkins:

   Open your browser and go to [http://localhost:8080](http://localhost:8080). Follow the Jenkins setup wizard using the initial admin password obtained from the Jenkins container logs:

   ```bash
   docker logs jenkins  # Find the initial admin password
   ```

   Continue with the Jenkins setup, installing recommended plugins, and creating an admin user.



## Additional Information

- The Jenkins master is available at [http://localhost:8080](http://localhost:8080).
- The SSH port for the Jenkins agent is mapped to port `2200`.



# Jenkins SSH Agent Setup

## Overview

here is step-by-step instructions on creating SSH credentials in Jenkins and setting up a Jenkins agent using the Jenkins web interface with SSH connection.

## Prerequisites

- **Jenkins Server:** A running instance of Jenkins accessible from your web browser.

## Create SSH Credentials

1. **Login to Jenkins:**
   - Open your web browser and navigate to your Jenkins server URL.
   - Log in with your credentials.

2. **Navigate to Credentials:**
   - Click on "Manage Jenkins" in the Jenkins dashboard.
   - Select "Manage Credentials."

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img4.png)


3. **Add Credentials:**
   - In the Credentials page, click on "(global)" and then "Add Credentials."
   - Choose "SSH Username with private key" as the kind.

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img1.png)

4. **Enter Credentials:**
   - Fill in the following details:
     - **Username:** Your SSH username.
     - **Private Key:** Enter the private key directly or choose "From the Jenkins master ~/.ssh" if your private key is on the Jenkins server.

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img8.png)

5. **Save Credentials:**
   - Click "OK" to save the SSH credentials.

## Set Up SSH Agent

1. **Navigate to Manage Jenkins:**
   - Click on "Manage Jenkins" in the Jenkins dashboard.

2. **Manage Nodes and Clouds:**
   - Select "Manage Nodes and Clouds" from the options.

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img6.png)

3. **Create a New Node:**
   - Click on the "New Node" or "New Agent" button.

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img7.png)

4. **Enter Node Details:**
   - Provide a name for your agent in the "Node name" field.
   - Choose "Permanent Agent" as the node type.

5. **Configure Node:**
   - Set the number of executors (build slots) based on your requirements.
   - Choose the appropriate operating system for the agent.

6. **Launch Method:**
   - Select "Launch agent via SSH" as the launch method.

7. **Credentials:**
   - Choose the SSH credentials you created in the previous steps.

8. **Host:**
   - Enter the hostname or IP address of the machine where the agent will run.

9. **Host Key Verification:**
   - Choose the "Non verifying Verification Strategy" option for host key verification.

10. **Save Node Configuration:**
    - Click "Save" to save the node configuration.

    ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img3.png)

11. **Launch Agent:**
    - Jenkins will automatically attempt to launch the agent via SSH using the provided credentials. Ensure that the agent machine is reachable from the Jenkins server.

    ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img2.png)



## Conclusion

Your Jenkins agent with SSH connection is now set up and ready to execute builds. If you encounter any issues, refer to the Jenkins documentation or seek assistance from the Jenkins administrator.


# Jenkins Integration with GitLab

This section provides step-by-step instructions on integrating Jenkins with GitLab.

## Prerequisites

- **Jenkins Server:** A running instance of Jenkins accessible from your web browser.
- **GitLab Account:** Access to a GitLab account with the repository you want to integrate.

## Integration Steps

1. **Install Required Plugins:**
   - In Jenkins, navigate to "Manage Jenkins" > "Manage Plugins."
   - Install the necessary plugins for GitLab integration.

    ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img5.png)

    ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img9.png)


2. **Generate GitLab Personal Access Token:**
   - In your GitLab account, generate a Personal Access Token and save it  at any safe place.

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img10.png)

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img11.png)


3. **Configure GitLab Credentials in Jenkins:**
   - In Jenkins, navigate to "Manage Jenkins" > "Manage Credentials."
   - Add a new credential of type "Gitlab API token" with the Personal Access Token generated from GitLab.

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img12.png)

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img13.png)


4. **Configure Global Jenkins Settings:**
   - Navigate to "Manage Jenkins" > "System."
   - Look for the GitLab configuration section
   - Configure the source code management settings with your GitLab repository URL and the credentials you added.

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img14.png)

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img15.png)


5. **Configure ssh in GitLab:**
   - Create an SSH key in Gitlab account.
   - Add ‘id_rsa.pub’ key to Gitlab account

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img16.png)

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img17.png)


6. **Configure Credentials in Jenkins:**
   - Now Create Credentials and ‘id_rsa’ private key to Jenkins.

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img18.png)

   - add your gitlab credentials (user,password)

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img19.png)


7. **Create a project in Gitlab:**
   - create a project in Gitlab and push your file in the repository.

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img20.png)

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img21.png)

8. **inetgrate Jenkins wiht Gitlab:**
   - Run the build now in jenkins to create a workspace similar to your Gitlab repository.

   ![Alt Text](jenkins-route-traefik/jenkins/-/raw/dev/img/img22.png)

## Conclusion

Your Jenkins server should now be integrated with GitLab, allowing you to automate builds triggered by GitLab events.