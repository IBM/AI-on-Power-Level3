# Deploy a LLM on Power10 - Hands-on lab guide

## Lab-ready check

Make sure you have the following items ready:

- In your browser, you have logged in as `cecuser` in the OpenShift GUI/console.
- In your terminal, you have logged into the bastion server and authenticated to the OpenShift cluster using the OpenShift CLI `oc`.
  
!!! warning "Warning"
    Do not proceed further unless the items mentioned above are ready. Please refer to "Lab setup instructions" section (see left hand side menu) to setup your browser and terminal windows.

## Lab guide

### Create project
1. Go to OpenShift Administrator profile, click on **Home** -> **Projects** and click **Create Project**
   ![image](https://github.com/user-attachments/assets/27a3b2e6-414b-4ec9-b0b1-be90e0b3858f)
2. Enter a project name: **lab1-demo** & click **Create**
   ![image](https://github.com/user-attachments/assets/6e6f471c-43e5-491b-8bcb-8700dbe4b320)

### Setup storage
Let's setup storage for this lab which is needed for storing the downloaded AI models. This environment comes with NFS (file storage) pre-configured. 
In OpenShift, you first request for the storage (aka PersistentVolumeClaim or PVC) and the actual storage (aka PersistentVolume or PV) gets alloted based on your request and the storage availability in the storage pool (NFS in this case).

1. Go to OpenShift Administrator profile, click on **Storage** -> **PersistentVolumeClaims** and click **Create PersistentVolumeClaim**
   ![image](https://github.com/user-attachments/assets/e8d43c1e-2174-4976-b02b-e05ebfe37cc2)
2. In the resulting form, enter PVC name: **model-storage** and Size: **20** GB. Leave other fields as defaults and click **Create**
   ![image](https://github.com/user-attachments/assets/56931cb0-f697-4a11-8038-db15f451168c)
3. Note that it shows PVC status as bound, which means storage was allotted
   ![image](https://github.com/user-attachments/assets/ea19ae1f-899d-4ff5-a5e0-f97df1e97ea2)
4. Navigate to **Storage** -> **PersistentVolumes** and view the actual storage (PV) bound to your storage request (PVC = **model-storage**)
   ![image](https://github.com/user-attachments/assets/9fbc41be-f900-4052-8ac0-810edf6bd17e)

This completes the storage setup.

### Setup ConfigMap

In OpenShift, a ConfigMap is an object used to manage configuration data for applications. It allows you to decouple configuration details from application logic, which makes your applications more portable and easier to manage across different environments. Instead of hardcoding configuration values in your application, you store them in a ConfigMap and inject them into your application at runtime.

We will use ConfigMap to store the model URL and model name, both of which will be used when we deploy the model. Using ConfigMap will help us switch to a different model very easily.

1. Navigate to **Workloads** -> **ConfigMaps** and click **Create ConfigMap**
   ![image](https://github.com/user-attachments/assets/11dea9ae-a2cb-4b6a-ae33-0d4a80c168f7)
2. In the resulting form, enter a name: **model-params**, add Key: **MODEL_NAME** & Value: **tinyllama-1.1b-chat-v1.0.Q8_0.gguf** and click **Add key/value** which will open up one more Key/Value box.
   ![image](https://github.com/user-attachments/assets/68614a1f-7a47-425d-8bd4-ebd291b7ee32)
3. In the newly created Key/Value box, enter: Key: **MODEL_URL** and Value: **https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF/resolve/main/tinyllama-1.1b-chat-v1.0.Q8_0.gguf** and click **Create**
   ![image](https://github.com/user-attachments/assets/24be3e90-eab3-4961-8f7c-03980c95721a)

This completes the ConfigMap setup.

### Deploy first model

1. Navigate to the OpenShift Developer profile console window.

!!! note

       If you followed the TIP given in the "Lab setup instructions" page, you should already have a browser window/tab with the Developer profile. In case you didn't, go to top left corner of your console, click **Administrator** and select **Developer**.

2. **IMP**: Ensure you are in the **lab1-demo** project in the Developer profile window. If not, goto **Project** and select **lab1-demo**
   
   <video style="width:100%" muted="true" autoplay="true" loop="true" controls="" alt="type:video">
        <source src="./_attachments/switch-to-lab1-demo-project.mp4" type="video/mp4">
     </video>
