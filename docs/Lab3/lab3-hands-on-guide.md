# Generate SQL query using code LLM on IBM Power10 - Lab guide

## Lab-ready check

Make sure you have the following items ready:

  - In your browser, you have logged in as cecuser in the OpenShift GUI/console.
  - In your terminal, you have logged into the bastion server and authenticated to the OpenShift cluster using the OpenShift CLI oc.

!!! warning "Warning"
    Do not proceed further unless the items mentioned above are ready. Please refer to "Lab setup instructions" section (see left hand side menu) to setup your browser and terminal windows.

## Lab guide

### Deploy granite code LLM

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
     ---
     ![image](https://github.com/user-attachments/assets/395d95b2-bd4f-4163-980a-536acf9c4877)
     ---
   
     !!! note "ConfigMap update does not restart pods"
   
       The existing pod won't see the changes right away as changing values of a ConfigMap doesn't cause a deployment (and hence pod) to restart. It needs to be done manually.
   
7. Let's go to the deployment view. Click **Topology**, then click "**D lab1-demo**" part of the application icon, which will open up the deployment details pane (on the right hand side of the browser window). Click "**D lab1-demo**" in that pane which will then open up the deployment details view for lab1-demo deployment.
   ![image](https://github.com/user-attachments/assets/c1c50473-0673-48e9-a2b8-54611fd67ead)
8. Click **Actions** and select **Restart rollout**. This will restart the deployment which results in re-deployment of the pod. This ensure the pod will now see the new ConfigMap changes.
   ![image](https://github.com/user-attachments/assets/51476c1a-52a2-4fbe-ae28-9dca9348ac4f)
   
10. Click on **Pods** tab, where you will see a new pod instantiated. The new pod will download the model and then start it. Since the configmap points to granite code model, it will be downloaded from HuggingFace and then started. When that happens the new pod's status will change to Running and the existing pod will be terminated.
    ![image](https://github.com/user-attachments/assets/3eb442b4-08a5-4bec-8e24-c4f2e1b57926)
   
    !!! info "Model download will take time - Have patience!!"

        This process will take a few minutes (in my case it took around 3-4 mins as this is a fairly large model compared to tinyllama) and your mileage may vary! Remember, this is a demo environment and models are few GBs in size. Models once downloaded won't be downloaded again as long as you are using the same storage (PV).

11. If all goes well, you should see just 1 pod running.
    ![image](https://github.com/user-attachments/assets/32fb028f-fef6-4abe-996c-b1bdfbe80489)

12. Let's verify that the model running is granite code LLM!. Click on the pod to enter the pod details view/page.
    ![image](https://github.com/user-attachments/assets/74893247-6c4b-444f-9163-97ec3eb3e934)

13. In the pod details page, click on the Logs tab to see the pod logs.
    ![image](https://github.com/user-attachments/assets/ac49a5bb-bbf0-4ac1-9050-a88327185c18)
14. In the log window, scroll upwards to see the name of the model against the attribute llm_load_print_meta: general.name
    ![image](https://github.com/user-attachments/assets/a84291e2-c952-4710-a2cd-88caed8b4dd2)

    This verifies that we have indeed deployed granite code LLM.



