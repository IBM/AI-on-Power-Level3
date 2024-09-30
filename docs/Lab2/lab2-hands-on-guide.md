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
     - `git clone https://github.com/mgiessing/bcn-lab-2084`
     - `cd bcn-lab-2084/`

     ![image](https://github.com/user-attachments/assets/87e39369-ebbc-4fbd-a130-214f04ee4212)
   
     ![image](https://github.com/user-attachments/assets/b5ac2239-552f-4839-900f-917f11109ab9)
   
     ![image](https://github.com/user-attachments/assets/65fecf4b-0328-4df4-b28a-fd255db495f5)

5. Make sure `oc CLI is pointing to the **lab2-demo** project
   `oc project lab2-demo `
   
   ![image](https://github.com/user-attachments/assets/a68c209f-45dd-4317-acdc-38b34d7f1394)

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

   ` oc get pods -w`

    ![image](https://github.com/user-attachments/assets/5289e0ef-3322-4206-9da3-ef833db0608e)

