# Deploy Retrieval-Augmented Generation (RAG) on IBM Power10 - lab guide

## Lab-ready check

Make sure you have the following items ready:

  - In your browser, you have logged in as cecuser in the OpenShift console.
  - In your terminal, you have logged into the bastion server and authenticated to the OpenShift cluster using the OpenShift CLI oc.

!!! warning "Warning"
    Do not proceed further unless the items mentioned above are ready. Please refer to "Lab setup instructions" section (see left hand side menu) to setup your browser and terminal windows.

## Lab guide

!!! note "Image zoom functionality"

    Feel free to click on the images in the lab guide below to a view larger image.

In this lab, we will focus on the below:

  - Deploy a vector database (DB) - milvus.
    
  - Deploy a jupyter notebook where we will implement RAG pattern and learn about:
      - Index the milvus DB with a sample PDF.
      - Query the DB to get relevant documents for the question asked.
      - Create a prompt based on the relevant documents gotten from the previous step and send it to the LLM (IBM Granite model we deployed in Lab1) to get a domain specific answer.

### Create project

1. Let's create a new OpenShift project that will hold all resources of this lan. Navigate to your browser window/tab which has the **Administrator** profile. Select **Home** **(A)** -> **Projects** **(B)** and click **Create Project** **(C)**.
   ![image](https://github.com/user-attachments/assets/839bbfd3-e4a0-4c19-9223-c01e6ce60221)

2. In the resulting form, enter **lab2-demo** **(A)** as the project name and click **Create** **(B)**.
   ![image](https://github.com/user-attachments/assets/4bec3979-3d79-445e-88ff-3318c60123ea)

### Deploy milvus

1. Navigate to the terminal window where we had setup `oc` CLI on the bastion node.

    !!! warning "Ensure `oc` CLI is authenticated with the cluster"

        Run a simple command: `oc project` and if it throws error you need re-authenticate with the cluster. Follow the steps mentioned in [lab instructions](https://ibm.github.io/AI-on-Power-Level3/lab-setup/#logging-in-to-openshift-cluster-using-oc-cli){target="_blank"} to ensure `oc` is authenticated with the cluster.

3. Make sure you are in the home directory and then run the command below to clone the github repository. Then switch to the newly cloned repository directory.
     - `cd` **(A)**
     - `git clone https://github.com/dpkshetty/bcn-lab-2084` **(B)**
     - `cd bcn-lab-2084/` **(C)**

       ![image](https://github.com/user-attachments/assets/138af434-6b94-48ef-998f-b5bbe8fc86dd)
     
       ![image](https://github.com/user-attachments/assets/82c6187a-7163-4fe6-b6c6-c62a7e036bb2)
     
       ![image](https://github.com/user-attachments/assets/bcb3f05c-5352-4f0f-92ff-8521d6857bfe)

5. Make sure `oc` CLI is pointing to the lab2-demo project.

     `oc project lab2-demo `   
     
     ![image](https://github.com/user-attachments/assets/50330a50-d662-4967-83a3-86e5618fc1ce)

7. Run the below set of commands to deploy milvus DB.
     
     `cd Part2-RAG/milvus-deployment` **(A)**

     `oc create configmap milvus-config --from-file=./config/milvus.yaml` **(B)**

     `oc apply -f .` **(C)**

     `cd ..` **(D)**
          
     ![image](https://github.com/user-attachments/assets/38e7a10e-427d-4f22-97a7-8b421870723d)

8. Monitor deployment using the below command until all pods are in Running state. <br>
   Hit Ctrl-C on the keyboard to exit and come back to the shell prompt.

     `oc get pods -w` **(A)**

      ![image](https://github.com/user-attachments/assets/67a1498b-25f2-4e19-be32-530fec0ea62a)

### Deploy jupyter notebook

1. Run the below set of commands to deploy jupyter notebook (NB).<br>Ignore any warnings if seen.
     
     `cd nb-deployment` **(A)**

     `oc apply -f .` **(B)**
     
     ![image](https://github.com/user-attachments/assets/e668c714-2559-4df5-a595-e5c9c02bef20)

2. Verify the notebook pod is running using the command below. Hit Ctrl-C on keyboard to exit and return back to shell prompt.
   
     `oc get pods --selector=app=cpu-notebook -w` **(A)**

     ![image](https://github.com/user-attachments/assets/d188af80-225a-45ff-b28d-f37c5d257dbf)

4. Once the notebook pod is deployed you should be able to access it using the link retrieved from the below command:
   
     `oc get route cpu-notebook -o jsonpath='{.spec.host}'` **(A)**

     ![image](https://github.com/user-attachments/assets/5d89416b-ff39-49ba-aaec-1155df45b9c7)

     In my case, the URL was: <br>
       `cpu-notebook-lab2-demo.apps.p1279.cecc.ihost.com` <br>
     but yours can be different! <br>


    !!! note "Alternate way to get the jupyter NB URL"

        You can also goto OpenShift **Administrator** profile console window in your browser, navigate to **Networking** -> **Routes**, select **cpu-notebook** route and click on the URL mentioned under **Location** field.

6. Copy and paste the URL in the browser. You should see the jupyter screen as below:
    ![image](https://github.com/user-attachments/assets/ee5cf9c5-8f3f-48d4-8741-08d7ae5617ab)

7. Now let's copy the jupyter NB (.ipynb file) present in the git repository to the NB pod. <br>
   In your `oc` CLI terminal window, navigate to the root of your git repository which has the **RAG.ipynb** file.

     `cd /home/cecuser/bcn-lab-2084` **(A)**

     ![image](https://github.com/user-attachments/assets/706f1bbe-cbcc-492b-b65f-ca3c9820181b)

8. List pods and copy the name of the cpu-notebook pod.

     `oc get pods` **(A)**

     ![image](https://github.com/user-attachments/assets/b6cd888d-31b1-4212-9a2f-7911c481941d)

9. Use **oc cp ...** command to copy the NB file from bastion server to **/tmp/notebooks/** directory of the NB pod.

     `oc cp ./RAG.ipynb cpu-notebook:/tmp/notebooks/` **(A)**
    
     ![image](https://github.com/user-attachments/assets/bdfa2804-3a18-4b31-b47d-e3dd8345eea2)

10. Go back to the jupyter NB application in your browser and hit **refresh** (F5 shortcut in keyboard) **(A)**. You should be able to see the **RAG.ipynb** **(B)** file listed.

      ![image](https://github.com/user-attachments/assets/c0c44ba3-88d8-40b0-850d-f57a78f16e64)

11. **Double-click** **(A)** on the **RAG.ipynb** file and it should open up in the right pane of the browser.
    
      ![image](https://github.com/user-attachments/assets/aebc53e3-6a93-4378-b3dd-6d9b6c7ec180)

11. You're all set! Follow the instructions given in the NB to finish this lab.
