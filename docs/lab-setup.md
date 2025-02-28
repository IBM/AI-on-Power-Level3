# Lab setup

## Provisioning the environment

This hands-on lab uses Red Hat OpenShift on Power10 on-premises environment which is optimized for AI workloads and hosted on IBM TechZone.

Follow these steps:

1. Open [this](https://techzone.ibm.com/collection/generative-ai-demos-on-ibm-power/environments){target="_blank"} TechZone collection and provision the environment named "OpenShift ready for AI on IBM Power10 (Container PaaS)" by clicking **Reserve** **(A)** and submitting the resulting form (select **Education** as purpose)
   
     ![image](https://github.com/user-attachments/assets/9c5c4702-509b-4757-b91e-2098ae818454)
   
3. Watch your email for updates from TechZone and wait for your environment to be provisioned.

    !!! note "TechZone provisioning can take time"

        It can take up to 3 hours for the provisioning to happen. Allow adequate time for its completion and maintain patience during this period. If you get into provisioning issues, refer to the [Support](https://ibm.github.io/AI-on-Power-Level3/support/){target="_blank"} section for assistance.
   
5. After provisioning is complete, go to [my reservations](https://techzone.ibm.com/my/reservations){target="_blank"} page to verify it's in "Ready" **(A)** state.

     ![image](https://github.com/user-attachments/assets/46640f65-545e-4dca-aa7a-6c0b6ca771f8)

    !!! tip "Extend your reservation"

        You can extend your reservation up to a maximum of 6 days. Click the three dots that are seen on the upper right of your reservation and select the **Extend** option.

## Accessing the environment

1. This lab uses an on-premises environment. Verify that you are connected to the IBM Virtual Private Network (VPN) to access the environment. Refer to [this](https://github.com/IBM/itz-support-public/blob/main/IBM-On-premise/IBM-On-premise-Runbooks/configure-vpn.md){target="_blank"} link for more details.

    !!! warning "VPN connectivity issues and solution"

        Some IBM folks from EU have reported that the TechZone environment is not accessible when using Mac OS specific VPN profiles.
        For example: "EUROPE-MEA (macOS)" or "AMERICA (macOS) are not working.
        Solution is to use the non Mac OS VPN profiles. For example: "EUROPE-MEA" or "AMERICA" seem to work      
   
3. In TechZone, open the [My Reservations](https://techzone.ibm.com/my/reservations){target="_blank"} page.
4. Click your reservation, which opens up the details page.
5. Scroll to the "Reservation Details" section of the page which has information on how to connect to the Red Hat OpenShift console.
   ![image](https://github.com/user-attachments/assets/9e7df820-6a8b-4cc6-9ca3-b2a8cdc7decb)

### Accessing Red Hat OpenShift console

1. In the "Reservation Details" section of the TechZone environment details page, click the **Red Hat OpenShift Console URL** **(A)**.
     ![image](https://github.com/user-attachments/assets/68c271a0-0077-4cf7-9080-b031b2070cb6)
   
3. This action opens a new browser tab or window and navigates to the Red Hat OpenShift console login page.
      - If you encounter any security exception, navigate to the bottom of the browser page, accept the exception under Advanced and continue. This action is acceptable since this is a lab or demo environment and it uses self-signed certificates.
4. Using the user account (cecuser) and password from your reservation details page, login by using the **htpasswd** **(A)** option.
     ![image](https://github.com/user-attachments/assets/61c1015c-41ab-4400-9b83-ad8f6e2cba10)
   
    !!! tip "How to copy user password?"

        Click the **copy icon** **(A)** provided under 'User Password' in the reservation details page to copy the password and paste it in the Red Hat OpenShift console window.

     ![image](https://github.com/user-attachments/assets/33fe60d7-c285-4d55-a830-73bac9e8b032)     

7. You are logged in to the Red Hat OpenShift cluster successfully by using the console. The dashboard of the Red Hat OpenShift console is now be visible (or the page you were on, before you logged off). You land in **Administrator** profile (or **Developer** profile if that was the last profile you were in when you logged off).
   
    !!! tip "Tip - Maintain 2 browser windows, one each for Administrator and Developer persona"
   
        It is a good idea to have 2 browser windows (or tabs per your preference) for Red Hat OpenShift console access - one with Administrator profile and another with Developer profile because in the hands-on labs you are switching between these profiles and it's easier and efficient to do so with 2 browser windows.
   
         - To do so, copy the URL from the browser address bar. Open a new browser window (or tab) and paste the URL there. It opens up one more Red Hat OpenShift console in the new window (or tab). In the new window/tab, switch to the Developer profile (also known as Persona) by going to the upper left and clicking **Administrator** and selecting **Developer** (or vice-versa) in the drop down menu. In short, verify that you have 2 browser windows, one each with Administrator and Developer profile (also known as Persona). These two windows are referred to as the Red Hat OpenShift Administrator console and Developer console.

    <video style="width:100%" muted="true" autoplay="true" loop="true" controls="" alt="type:video">
       <source src="https://github.com/user-attachments/assets/a622a195-00a6-4950-b2e5-686b04fa3401" type="video/mp4">
    </video>

### Reauthenticating to the Red Hat OpenShift console

!!! note "REAUTHENTICATING in case you lose console access"
   
    In case you lose access to the Red Hat OpenShift cluster and need to reauthenticate to the console, which is possible in case your reservation expires and/or your console authentication timed-out, follow the steps in the preceding section to reauthenticate to your Red Hat OpenShift console.

### Accessing Red Hat OpenShift Command Line Interface (CLI)

Red Hat OpenShift CLIs are accessed by using the `oc` command

1. Navigate to the "Reservation Details" section of your TechZone environment details page and make a note of the Bastion server's **hostname** **(A)**, **IP address** **(B)** and the **user account** **(C)** that is needed to login by using SSH client.

      ![image](https://github.com/user-attachments/assets/ecef8ba6-9790-4c75-94e0-3be409bff4ea)
   
3. Open a terminal window and use the SSH client to connect to the Bastion node of the Red Hat OpenShift cluster.
      - `ssh -l cecuser <your bastion hostname/IP as provided in the Reservation Details section>`.
      - If `ssh` presents a warning, type `yes` and continue.
      - When prompted for password, copy the password from Reservation Details page by clicking the copy icon and pasting it in the ssh terminal window
4. You have now logged in to the bastion node of your Red Hat OpenShift cluster.
      - Keep this terminal window open as you are using it frequently to run CLI commands.
            ![image](https://github.com/user-attachments/assets/576d86f0-8873-492c-8b13-9433c9f25604)
        
5. `oc` CLI is preinstalled on the bastion node. Verify by running `oc version` **(A)** command.
      - Ignore the error part of the `oc version` for now. It is as expected since you have not yet logged in to the cluster from the CLI.  
            ![image](https://github.com/user-attachments/assets/b0e21c56-3757-4771-bb47-42d3aaf5591f)

### Logging in to Red Hat OpenShift Cluster by using `oc` CLI
Login to the Red Hat OpenShift cluster by using the `oc` CLI. This login is needed to run some CLI commands as part of the lab steps.

1. In the Red Hat OpenShift console, upper right section, click **cecuser** **(A)** and select **Copy login command** **(B)** option.
   
     ![image](https://github.com/user-attachments/assets/52c8e57b-d507-4b48-9eb8-d3843fc9d3d4)
   
3. A new browser window (or tab, depending on your browser setting) opens up.
      - If you encounter any security exception, navigate to the bottom of the browser page, accept the exception under Advanced and continue. This action is acceptable since this is a lab or demo environment and it uses self-signed certificates.
4. You are presented with another login screen. Click **htpasswd** **(A)** option
   ![image](https://github.com/user-attachments/assets/1fb91841-e243-4d56-a4f1-1068fb3058df)

5. Use Username: `cecuser` and Password: `<as provided in the TechZone Reservation Details page>`.
      - TIP: Click the copy icon that is provided under 'User Password' in the Reservation Details page to copy the password and paste it in the Red Hat OpenShift console window.
6. Click **Display token** **(A)**.
   
     ![image](https://github.com/user-attachments/assets/45865182-61f6-4bfd-86ec-4b5f43e99706)
   
7. Copy the `oc login --token=...` CLI and paste it into the bastion node’s terminal window.
     ![image](https://github.com/user-attachments/assets/75ad62a0-d0a0-45f6-8797-fedad6e5877a)
     ![image](https://github.com/user-attachments/assets/a2753a4c-86d6-49ca-96c8-54f3ed7dbac5)
8. You are logged in to the Red Hat OpenShift cluster successfully by using the `oc` CLI.
9. In case you lose access to the `oc` CLI, you get an error as below, in which case you need to reauthenticate.<br>
   Refer to the following section on how to reauthenticate in CLI.

     ![image](https://github.com/user-attachments/assets/a1e8d00c-64d0-41ab-997c-540378df0544)
   
### Reauthenticating for CLI access

!!! note "REAUTHENTICATING in case you lose CLI access"
      
    In case you lose access to the Red Hat OpenShift cluster and need to reauthenticate by using the CLI, which is possible in case your reservation expires and/or your CLI window gets terminated for some reason, follow the steps in the preceding section to get back your `oc` CLI authenticated to the Red Hat OpenShift cluster.

### Summary
Efforts are made to keep the lab instructions simple and easy to follow to cater to an audience of all skill levels.
Both the Red Hat OpenShift CLI and Red Hat OpenShift web console are used in the lab guide as not all Red Hat OpenShift capabilities are available in the Red Hat OpenShift web console.
