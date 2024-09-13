# Provisioning the environment

For the hands-on labs, we will be using the OpenShift on Power10 on-prem environment, optimized for AI workloads and hosted on IBM TechZone.

Follow the steps below:

1. Head to [this](https://techzone.ibm.com/collection/generative-ai-demos-on-ibm-power/environments) TechZone collection and provision the environment named "OpenShift 4.14 ready for AI on IBM Power10 (Container PaaS)" by clicking on **Reserve** and submitting the resulting form (select **Education** as purpose)
   ![](_attachments/tz-env-reserve.png)
   
2. Watch your email for updates from TechZone and wait for your environment to be provisioned.
3. Once provisioned, head to [my reservations](https://techzone.ibm.com/my/reservations) page to ensure its ready.
   ![](https://github.com/user-attachments/assets/e4ef1ccc-70e3-42b8-8e06-caab6ed87d52)

# Accessing the environment

1. Since this is an on-prem environment, make sure you are connected to the IBM network to access the environment.(TODO: add details on VPN)
2. In TechZone, click on your provisioned environment under "My Reservations" which will open up the details page.
3. Scroll to the "Reservation Details" section of the page which has information on how to connect to OpenShift console
   ![image](https://github.com/user-attachments/assets/9e7df820-6a8b-4cc6-9ca3-b2a8cdc7decb)

## Accessing OpenShift GUI/console

1. In the "Reservation Details" section of the TechZone environment details page, click on the OpenShift console link
2. This will open up a new browser tab/window and opens up the OpenShift console login page
   - If you encounter any security exception, navigate to the bottom of the browser page, acccept the exception under Advanced and continue.
3. On the OpenShift console page, select the **htpasswd** login option.
4. Use Username: `cecuser` and Password: `<as provided in the TechZone Reservation Details page>`
    - TIP: Click on the copy icon provided under 'User Password' in the Reservation Details page to copy the password and paste it in the OpenShift console window

   ![image](https://github.com/user-attachments/assets/b31a361a-b69a-4872-b5a7-a71db2f8f52f)
   

