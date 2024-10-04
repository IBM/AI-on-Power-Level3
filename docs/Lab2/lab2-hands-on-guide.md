# Deploy Retrieval-Augmented Generation (RAG) on IBM Power10 - lab guide

## Lab-ready check

Make sure you have the following items ready:

  - In your browser, you have logged in as cecuser in the OpenShift GUI/console.
  - In your terminal, you have logged into the bastion server and authenticated to the OpenShift cluster using the OpenShift CLI oc.

!!! warning "Warning"
    Do not proceed further unless the items mentioned above are ready. Please refer to "Lab setup instructions" section (see left hand side menu) to setup your browser and terminal windows.

## Lab guide

!!! note "Image zoom functionality"

    Feel free to click on the images in the lab guide below to a view larger image

In this lab, we will focus on the below:

  - Deploy a vector DB - milvus
    
  - Deploy a jupyter notebook where we will implement RAG pattern and learn about:
      - Index the milvus DB with a sample PDF
      - Query the DB to get relevant documents for the question asked.
      - Create a prompt based on the relevant documents gotten from the previous step and send it to the LLM (granite model we deployed in Lab1) to get a domain specific answer.

### Create project

1. Let's create a new OpenShift project that will hold all resources of this lan. Navigate to your browser window/tab which has the **Administrator** profile. Select **Home** -> **Projects** and click **Create Project**
   ![image](https://github.com/user-attachments/assets/ec396478-05eb-4fa9-8862-c49c9321f21e)

2. In the resulting form, enter **lab2-demo** as the project name and click **Create**
   ![image](https://github.com/user-attachments/assets/6ece11da-db12-4ddb-ae79-9334004ccdd1)

### Deploy milvus

1. Navigate to the terminal window where we had setup `oc` CLI on the bastion node.

    !!! warning "Ensure `oc` CLI is authenticated with the cluster"

        Run a simple command: `oc project` and if it throws error you need re-authenticate with the cluster. Follow the steps mentioned in [lab instructions](https://ibm.github.io/AI-on-Power-Level3/lab-setup/#logging-in-to-openshift-cluster-using-oc-cli) to ensure `oc` is authenticated with the cluster.

3. Make sure you are in the home directory and then run the command below to clone the github repository. Then switch to the newly cloned repository directory.
     - `cd`
     - `git clone https://github.com/dpkshetty/bcn-lab-2084`
     - `cd bcn-lab-2084/`

       ![image](https://github.com/user-attachments/assets/87e39369-ebbc-4fbd-a130-214f04ee4212)
     
       ![image](https://github.com/user-attachments/assets/ba5c7a65-25f2-4073-878d-1fcd5d72030a)
     
       ![image](https://github.com/user-attachments/assets/65fecf4b-0328-4df4-b28a-fd255db495f5)

5. Make sure `oc CLI is pointing to the **lab2-demo** project

     `oc project lab2-demo `   
     
     ![image](https://github.com/user-attachments/assets/721a4e9d-81db-48e8-b85c-c6491e96a8a1)

7. Run the below commands to deploy milvus DB
     ```
     cd Part2-RAG/milvus-deployment

     oc create configmap milvus-config --from-file=./config/milvus.yaml

     oc apply -f .

     cd ..
     ```
     
     ![image](https://github.com/user-attachments/assets/0b632d95-3af0-4b48-a883-31085455370f)

8. Monitor deployment using the below command until all pods are in **Running** state. <br>
   Hit Ctrl-C on the keyboard to exit and come back to the shell prompt.

     `oc get pods -w`

      ![image](https://github.com/user-attachments/assets/5289e0ef-3322-4206-9da3-ef833db0608e)

### Deploy jupyter notebook

1. Run the below commands to deploy jupyter notebook (NB). Ignore any warnings if seen.

     ```
     cd nb-deployment

     oc apply -f .
     ```
     ![image](https://github.com/user-attachments/assets/6868a6eb-bcc8-4a34-8ed0-172fd536feb9)

2. Verify the notebook pod is running using the command below. Hit Ctrl-C on keyboard to exit and return back to shell prompt.
   
     `oc get pods --selector=app=cpu-notebook -w`

     ![image](https://github.com/user-attachments/assets/3b7b1764-2135-4192-854e-0144068673b0)

4. Once the notebook pod is deployed you should be able to access it using the link retrieved from the below command:
   
     `oc get route cpu-notebook -o jsonpath='{.spec.host}'`

     ![image](https://github.com/user-attachments/assets/be511e8a-15e9-44af-8e40-6079cc266af6)

     In my case, the URL was: <br>
       `cpu-notebook-lab2-demo.apps.p1279.cecc.ihost.com` <br>
     but yours can be different! <br>


    !!! note "Alternate way to get the jupyter NB URL"

        You can also goto OpenShift **Administrator** profile console window in your browser, navigate to **Networking** -> **Routes**, select **cpu-notebook** route and click on the URL mentioned under **Location** field

6. Copy and paste the URL in the browser. You should see the jupyter screen as below:
    ![image](https://github.com/user-attachments/assets/ee5cf9c5-8f3f-48d4-8741-08d7ae5617ab)

7. Now let's copy the jupyter NB (.ipynb file) present in the git repository to the NB pod. <br>
   In your `oc` CLI terminal window, navigate to the root of your git repository which has the **RAG.ipynb** file.

     `cd /home/cecuser/bcn-lab-2084`

     ![image](https://github.com/user-attachments/assets/3e7774f6-18c3-49bd-b2ff-dc36740fc121)

8. List pods and copy the name of the cpu-notebook pod.

     `oc get pods`

     ![image](https://github.com/user-attachments/assets/d6ac1e0d-408a-45e5-9262-c5217d08dd35)

9. Use **oc cp ...** command to copy the NB file from bastion server to **/tmp/notebooks/** directory of the NB pod.

     `oc cp ./RAG.ipynb cpu-notebook:/tmp/notebooks/`
    
     ![image](https://github.com/user-attachments/assets/fb351616-347f-489f-890a-258fc4bea196)

10. Go back to the jupyter NB application in your browser and hit refresh (F5 shortcut in keyboard). You should be able to see the **RAG.ipynb** file listed.

      ![image](https://github.com/user-attachments/assets/e7e52a0c-a840-4a3d-b2ad-954040ced4ad)

11. Double-click on the **RAG.ipynb** file and it should open up in the right pane of the browser
    
      ![image](https://github.com/user-attachments/assets/e5e26a7c-33d7-4c9f-acaf-fce3290d6b3d)

11. You're all set! Follow the instructions given in the NB to finish this lab.
