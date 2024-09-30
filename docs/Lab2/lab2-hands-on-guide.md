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

Since our focus is to provide hands-on using jupyter notebook we will use `oc` CLI to quickly deploy milvus DB using the pre-created yamls available in our github repository

1. Navigate to the terminal window where we had setup `oc` CLI on the bastion node.

   !!! warning "Ensure `oc` CLI is authenticated with the cluster"

       Run a simple command: `oc project` and if it throws error you need re-authenticate with the cluster. Follow the steps mentioned in [lab instructions](https://ibm.github.io/AI-on-Power-Level3/lab-setup/#logging-in-to-openshift-cluster-using-oc-cli) to ensure `oc` is authenticated with the cluster.

3. Clone the github repository.
   
4.
5.
6. Switch to **lab2-demo** project

