# Generate SQL query using code LLM on IBM Power10 - Lab guide

## Lab-ready check

Make sure you have the following items ready:

  - In your browser, you have logged in as cecuser in the OpenShift GUI/console.
  - In your terminal, you have logged into the bastion server and authenticated to the OpenShift cluster using the OpenShift CLI oc.

!!! warning "Warning"
    Do not proceed further unless the items mentioned above are ready. Please refer to "Lab setup instructions" section (see left hand side menu) to setup your browser and terminal windows.

## Lab guide

!!! warning "Pre-requisite"
    
    This lab assumes you have finished Lab1. This lab uses the OpenShift resources deployed in Lab1 to optimize the usage of TechZone resources and to avoid re-deploying the same resources and re-learning the same concepts already taught in Lab1!
    
!!! note "Image zoom functionality"

    Feel free to click on the images in the lab guide below to a view larger image

1. In this lab, we will deploy IBM's granite code LLM which is available on HF. Navigate to OpenShift developer profile window and ensure you are in **lab1-demo** project. Click **Topology** and ensure that your application is healthy and running (has a blue circle) before proceeding further.
   ![image](https://github.com/user-attachments/assets/999accf3-e5b2-4a38-85d3-458ec024247c)

2. Select **ConfigMaps** and click **model-params**
   ![image](https://github.com/user-attachments/assets/c00aaf7f-caf5-4e95-8706-895ade37aee5)

3. Click on **Actions** -> **Edit ConfigMap**
   ![image](https://github.com/user-attachments/assets/e900c672-ee49-4f9d-94f2-f28bbe1aef20)
   
5. In the resulting form, the key/value fields for MODEL_NAME and MODEL_URL as below and click **Save**
   
     - Key: MODEL_NAME
     - Value: granite-8b-code-instruct.Q4_K_M.gguf
     ---
     - Key: MODEL_URL
     - Value: https://huggingface.co/ibm-granite/granite-8b-code-instruct-4k-GGUF/resolve/main/granite-8b-code-instruct.Q4_K_M.gguf     
     
     ![image](https://github.com/user-attachments/assets/395d95b2-bd4f-4163-980a-536acf9c4877)
   
   !!! note "ConfigMap update does not restart pods"
   
       The existing pod won't see the changes right away as changing values of a ConfigMap doesn't cause a deployment (and hence pod) to restart. It needs to be done manually.

7. Let's go to the deployment view. Click **Topology**, then click "**D lab1-demo**" part of the application icon, which will open up the deployment details pane (on the right hand side of the browser window). Click "**D lab1-demo**" in that pane which will then open up the deployment details view for lab1-demo deployment.
   ![image](https://github.com/user-attachments/assets/c1c50473-0673-48e9-a2b8-54611fd67ead)
8. Click **Actions** and select **Restart rollout**. This will restart the deployment which results in re-deployment of the pod. This ensure the pod will now see the new ConfigMap changes.
   

   
