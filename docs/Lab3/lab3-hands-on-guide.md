# Use natural language to generate code using code LLM on IBM Power10 - Lab guide

## Lab-ready check

Make sure you have the following items ready:

  - In your browser, you have logged in as cecuser in the OpenShift GUI/console.
  - In your terminal, you have logged into the bastion server and authenticated to the OpenShift cluster using the OpenShift CLI oc.

!!! warning "Warning"
    Do not proceed further unless the items mentioned above are ready. Please refer to "Lab setup instructions" section (see left hand side menu) to setup your browser and terminal windows.

## Lab guide

!!! note "Image zoom functionality"

    Feel free to click on the images in the lab guide below to a view larger image

### Deploy granite code LLM

!!! warning "Pre-requisite"
    
    This lab assumes you have finished Lab1. This lab uses the OpenShift resources deployed in Lab1 to optimize the usage of TechZone resources and to avoid re-deploying the same resources and re-learning the same concepts already taught in Lab1!
    
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

6. Use the deployment resource to restart the pods.
         
    !!! note "ConfigMap update does not restart pods automatically"
   
        The existing pod won't see the ConfigMap changes right away as changing values of a ConfigMap doesn't cause a deployment (and hence pod) to restart automatically. It needs to be done manually.
      
8. Navigate to the deployment view. Click **Topology**, then click "**D lab1-demo**" part of the application icon, which will open up the deployment details pane (on the right hand side of the browser window). Click "**D lab1-demo**" in that pane which will then open up the deployment details view for lab1-demo deployment.
   ![image](https://github.com/user-attachments/assets/c1c50473-0673-48e9-a2b8-54611fd67ead)
   
9. Click **Actions** and select **Restart rollout**. This will restart the deployment which results in re-deployment of the pod. This ensure the pod will now see the new ConfigMap changes.
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
14. In the log window, scroll upwards to see the name of the model against the attribute **llm_load_print_meta: general.name**
    
    ![image](https://github.com/user-attachments/assets/a84291e2-c952-4710-a2cd-88caed8b4dd2)

    This verifies that we have indeed deployed granite code LLM.

15. Let's access our model. As we did in Lab1, head to **Topology** view, click on the **Open URL** icon of your application.
    ![image](https://github.com/user-attachments/assets/d48c3a0e-9d52-4a24-8fbf-8776b38ead5f)
16. A new browser window/tab where you will be able to interact with your newly deployed LLM. You should see a screen like this:
    ![image](https://github.com/user-attachments/assets/2237409c-7160-471f-aafa-f0e1254c5a53)
17. Scroll all the way down to the input field "Say something..." where you can interact with the LLM. You can ask any question you like!
    ![image](https://github.com/user-attachments/assets/82196cf5-d4c2-459d-af7e-c24650f1f6ce)

    !!! note "Experimenting with model parameters"

        You can see a lot of model parameters or tunables (eg: Predictions, Temperature, etc.). Feel free to google and learn about them and experiment with it. You may want change some parameters, ask the same question and check how the response changes. We will not cover these parameters in this lab as its outside the scope of the lab

### Generate python code

Now let's use the granite code model to generate python code using natural language queries.

1. Generating python code for printing the fibonacci series
   ![image](https://github.com/user-attachments/assets/031c1d4a-d700-4f2a-89ec-87e49b1cc8f3)
   
     - I ran this code on my local python environment and it ran without errors!
     
     - Note that I spelled fibonacci incorrectly, yet it understood my question correctly.
     
     - Also note that the answer it gave uses recursion (function fib is being called recursively).

3. Now let's try asking it to generate the same code without using recursion.
   ![image](https://github.com/user-attachments/assets/74f1ce36-7aee-40ac-8444-8e212402745b)
   
     - As expected, it did give the code snippet that doesn't use recursion, so it did well there.
     
     - Generated code is not 100% correct. I ran this code on my local python environment and it ran into some issues and had to fix some code to make it generate the right fibonacci sequence.
     
     - The above shows that code LLMs can generate almost perfect code and in some cases it might need developer intervention to make it perfect!

    !!! info "Accuracy of code LLMs"

         - LLMs excel at generating simple or boilerplate code, often producing highly accurate results for common tasks such as sorting algorithms, basic input/output operations, or template-based functions.
         - For routine tasks and widely-used languages like Python or JavaScript, accuracy rates can be high, sometimes upwards of 80-90% for straightforward problems.
         - When dealing with more complex algorithms, nuanced edge cases, or multi-step logic, LLMs may struggle. The model can produce syntactically correct code that might not solve the problem as intended or might have logic errors.
         - For complex use cases, accuracy can drop significantly, often requiring human intervention to correct the output.

   Feel free to explore and try out more scenarios!

### Generate C code

Now let's use the granite code model to generate C code using natural language queries.

1. Generate C code for sorting a list of numbers.
   ![image](https://github.com/user-attachments/assets/57ae7cf6-2d8f-4b20-8618-5c19fe2b833b)

     - I ran this code on my local C environment and it ran without errors! Ofcourse I had to fix the first line (which was incomplete) as `#include <stdio.h>`

2. Generate C code for swapping 2 numbers without using a temporary variable
   ![image](https://github.com/user-attachments/assets/86fe9b4b-0684-45c0-a3da-c159121384ef)

     - I ran this code on my local C environment and it ran without errors! Ofcourse I had to fix the first line (which was incomplete) as `#include <stdio.h>`

   Feel free to explore and try out more scenarios!

### Generate SQL query

Generating SQL query using natural langguage is a little different than C or python code since SQL is a language to query Databases (DBs). One needs to give enough context to the code LLM for it to understand the DB table schemas for which you want it to generate the SQL query.

The context is given as a prompt to the code LLM which preceeds the natural language query and the code LLM answers the query using the context given in the prompt. The best way to understand is looking at some examples as depicted below.

Let's take an super simple example of a bank which has information stored in 2 tables in a DB:

* USERS - This which has user specific info (user_id, name, age, dob etc)
* ACCOUNTS - This holds the account balance of each user with user_id being the common field between the 2 tables

1. Given the above DB example, the prompt (aka context) we need to provide to the code LLM is as below:
   ```
   You are a developer writing SQL queries given natural language questions. The database contains a set of 2 tables. The schema of each table with description of the attributes is given. Write the SQL query given a natural language statement with names being not case sensitive

    Here are the 2 tables :
  
    (1) Database Table Name: USERS
    Table Schema:
    Column Name # Meaning
    user_id # unique identifier of the user
    user_name # name of the user
    usertypeid # user is '\''employee'\'', '\''customer'\''
    gender_id # user'\''s gender is 1 for female, 2 for male and 3 for other
    dob # date of birth of the user
    address # adress of the user
    state # state of the user
    country # country of residence of the user
    
    (2) Database Table Name: ACCOUNTS
    Table Schema:
    Column Name # Meaning
    acc_id # account number or account id of the user
    user_id # user id of the user
    balance # available balance in the account
   
   ```
2. Natural language query can be something like:
   
     ```
     With the above schema, please generate sql query to list all users whose balance is > 2000
     ```
   
4. Let's send the "Prompt + Query" to the code LLM and see how it responds. Feel free to copy and paste it in your LLM application window.
   
     ```
     You are a developer writing SQL queries given natural language questions. The database contains a set of 2 tables. The schema of each table with description of the attributes is given. Write the SQL query given a natural language statement with names being not case sensitive
  
      Here are the 2 tables :
    
      (1) Database Table Name: USERS
      Table Schema:
      Column Name # Meaning
      user_id # unique identifier of the user
      user_name # name of the user
      usertypeid # user is '\''employee'\'', '\''customer'\''
      gender_id # user'\''s gender is 1 for female, 2 for male and 3 for other
      dob # date of birth of the user
      address # adress of the user
      state # state of the user
      country # country of residence of the user
      
      (2) Database Table Name: ACCOUNTS
      Table Schema:
      Column Name # Meaning
      acc_id # account number or account id of the user
      user_id # user id of the user
      balance # available balance in the account
    
      With the above schema, please generate sql query to list all users whose balance is > 2000
     ```

     <br/>
     
     ![image](https://github.com/user-attachments/assets/445f9928-074f-4a58-a5b4-a1f757910c11)

     - The SQL query generated seems correct.
     - Its joining both the tables using `user_id` as the key and selecting all records where the user's account balance is > 2000
     - For the sake of people who may want to analyse further, pasting the SQL query that was generated:  `SELECT * FROM USERS u, ACCOUNTS a WHERE u.user_id = a.user_id AND a.balance > 2000; `
       
6. Interestingly, code LLM works both ways! Given a SQL query, you can ask code LLM to explain what it does. To do that I have formed the query as below. Feel free to copy the query and post it in your LLM application window.
   
    ```
    What does the below SQL query do ?
    SQL Query:
    SELECT * FROM USERS u, ACCOUNTS a WHERE u.userid = a.userid AND a.balance > 2000;
    ```

    <br/>
    
    ![image](https://github.com/user-attachments/assets/8576ca30-42b8-4647-bd92-e092d0c67aa8)

    That's a decent explanation of the SQL query!
   
8. Let's try one more example. Here I give it 2 conditions to match in the query.<br>
   NOTE: You don't have to repeat the whole schema in the prompt. LLMs can remember context.
   
     ```
     Using the schema given above, generate SQL query to list all users whose account balance > 2000 and user is of type employee
     ```

     <br/>
     
     ![image](https://github.com/user-attachments/assets/2be00e6a-8d79-4f1b-8844-b6768b63d86a)
  
     Response received as below:

     ```
     Here's the SQL query to list all users whose account balance is greater than 2000 and they are of type employee from the USERS and ACCOUNTS tables:
     SELECT * FROM USERS u, ACCOUNTS a WHERE u.user_id = a.user_id AND a.balance > 2000 AND usertypeid='employee';
     ```
     <br/>
     
     - The response is not 100% correct.
     - The last part of the SQL query `usertypeid='employee'` is ambiguous as the DB won't know which `usertypeid` column to reference.
     - The correct SQL query would have `u.usertypeid='employee'` so that the DB knows that its part of the USERS (aliased as `u` in the query) table.

9. Re-iterating some of the points we learned in this lab:
   
       - Code LLMs are not 100% correct, yet they can be immensely helpful for a developer as they can help generate near perfect code which can then be analyzed & tweaked to perfection by the developer.

       - There are many code LLMs available in HF with varied degree of accuracy and clients can choose the right one by experimenting with them in the context of their specific use case.

       - IBM also offers enterprise grade code LLMs via its watsonx Code Assistant (WCA) family of product offerings.

10. IBM Power servers are best used as system of record (SoR) servers which means they hold a lot of enterprise specific data in different DBs (e.g.: Oracle on AIX, DB2 on AIX / IBM i, PostgreSQL on Linux etc). From an IBM Power solution point of view, an end to end solution to query DB records using natural language can be easily implemented which can help clients immensely. They don't need to depend on experts to generate DB reports. Even executives (with authorized access to the DB) can generate reports and/or view data using natural language queries.
    - Check this ~2min short [video demo](https://mediacenter.ibm.com/media/Infusing+AI+into+mission+critical+workloads+with+PowerVS+and+watsonx.ai/1_fzqutamr) on "Infusing AI into mission critical workloads with PowerVS and watsonx.ai" which uses natural language to query fraudulent transactions and list high value customers. Although this demo is based on PowerVS use case the same is applicable to on-premise as well.

**Summary**

Code LLMs democratize coding by making it more accessible to non-coders, accelerating the development process for smaller teams, and assisting both beginners and experienced developers in learning and improving productivity. By lowering technical barriers, they are transforming who can engage in software development and how innovation happens across industries.
