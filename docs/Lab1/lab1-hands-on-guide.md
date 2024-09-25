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
In OpenShift, you first request for the storage (aka PersistentVolumeClaim or PVC) and the actual storage (aka PersistentVolume or PV) gets alloted based on your request and the availability in the storage pool (NFS in this case).

1. Go to OpenShift Administrator profile, click on **Storage** -> **PersistentVolumeClaims** and click **Create PersistentVolumeClaim**
   ![image](https://github.com/user-attachments/assets/e8d43c1e-2174-4976-b02b-e05ebfe37cc2)
2. In the resulting form, enter PVC name: **model-storage** and Size: **20** GB. Leave other fields as defaults and click **Create**
   ![image](https://github.com/user-attachments/assets/56931cb0-f697-4a11-8038-db15f451168c)
3. Note that it shows PVC status as bound, which means storage was allotted
   ![image](https://github.com/user-attachments/assets/ea19ae1f-899d-4ff5-a5e0-f97df1e97ea2)
4. Navigate to **Storage** -> **PersistentVolumes** and view the actual storage (PV) bound to your storage request (PVC = **model-storage**)
   ![image](https://github.com/user-attachments/assets/9fbc41be-f900-4052-8ac0-810edf6bd17e)

