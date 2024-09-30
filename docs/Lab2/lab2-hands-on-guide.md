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
    
  - Deploy a jupyter notebook where we will learn about:
      - Index the milvus DB with a sample PDF
      - Query the DB to get relevant documents for the question asked.
      - Create a prompt based on the relevant documents gotten from the previous step and send it to the LLM (granite model we deploy in Lab1) to get a domain specific answer.


  
### TEST
