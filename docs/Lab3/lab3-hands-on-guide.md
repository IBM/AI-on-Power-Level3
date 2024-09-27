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

1. In this lab, we will deploy IBM's granite code LLM which is also available on HF. Navigate to OpenShift developer profile window and ensure you are in **lab1-demo** project. Click **Topology** and ensure that your application is healthy and running (has a blue circle) before proceeding further.
   ![image](https://github.com/user-attachments/assets/999accf3-e5b2-4a38-85d3-458ec024247c)

3. Select ConfigMaps and click **model-params**
   ![image](https://github.com/user-attachments/assets/c00aaf7f-caf5-4e95-8706-895ade37aee5)


