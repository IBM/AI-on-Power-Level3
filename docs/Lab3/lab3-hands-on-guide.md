# Use natural language to generate code by using code LLM on IBM Power10 - Lab guide

## Lab-ready check

Make sure that you have the following items ready:

  - In your browser, you are logged in as cecuser in the Red Hat OpenShift console.
  - In your terminal, you are logged in to the bastion server and authenticated to the Red Hat OpenShift cluster by using the Red Hat OpenShift CLI oc.

!!! warning "Warning"
    Proceed only once the items mentioned earlier are ready. Refer to "Lab setup instructions" section (see left side menu) to setup your browser and terminal windows.

## Lab guide

!!! note "Image zoom functionality"

    Click the images in the following lab guide to view a larger image.

### Deploy IBM Granite code LLM

!!! warning "Pre-requisite"
    
    This lab assumes that Lab 1 is completed. This lab uses the Red Hat OpenShift resources that are deployed in Lab 1 to optimize the usage of TechZone resources and to avoid redeploying the same resources and relearning the same concepts that have been taught in Lab 1!
    
1. Deploy IBM Granite code LLM which is available on Hugging Face (HF). Navigate to the Red Hat OpenShift developer profile window and make sure that you are in **lab1-demo** **(A)** project. Click **Topology** **(B)** and make sure that your application is healthy and running (has a blue circle) before proceeding further.
   ![image](https://github.com/user-attachments/assets/e64ca98b-4abc-4ba4-b483-57e38482073a)

2. Select **ConfigMaps** **(A)** and click **model-params** **(B)**.
   ![image](https://github.com/user-attachments/assets/59e35af8-0b8a-4c3a-a085-4d6e3d6841f1)

3. Click **Actions** **(A)** -> **Edit ConfigMap** **(B)**.
   ![image](https://github.com/user-attachments/assets/687cce4c-6d96-4693-883b-432b1d3bcae2)
   
5. In the resulting form, enter the key/value fields for MODEL_NAME and MODEL_URL as below and click **Save** **(E)**.
   
     - **Key**: `MODEL_NAME` **(A)**
     - **Value**: `granite-8b-code-instruct.Q4_K_M.gguf` **(B)**
     ---
     - **Key**: `MODEL_URL` **(C)**
     - **Value**: `https://huggingface.co/ibm-granite/granite-8b-code-instruct-4k-GGUF/resolve/main/granite-8b-code-instruct.Q4_K_M.gguf` **(D)**
     ---
     ![image](https://github.com/user-attachments/assets/ed94d17e-283e-4f90-9cf5-9b11c6a4adb1)
     ---

6. Use the deployment resource to restart the pods.
         
    !!! note "ConfigMap update does not restart pods automatically"
   
        The existing pod cannot see the ConfigMap changes right away as changing values of a ConfigMap does not cause a deployment (and hence the pod) to restart automatically. It needs to be done manually.
      
8. Navigate to the deployment view. Click **Topology** **(A)**, then click "**D lab1-demo**" **(B)** part of the application icon, which opens up the deployment details pane (on the right side of the browser window). Click "**D lab1-demo**" **(C)** in that pane which opens up the deployment details view for lab1-demo deployment.
   ![image](https://github.com/user-attachments/assets/07715c25-5b84-44d4-8e74-a1a989581d80)
   
9. Click **Actions** **(A)** and select **Restart rollout** **(B)**. This restarts the deployment which results in the redeployment of the pod. It ensures that the pod will now see the new ConfigMap changes.
   ![image](https://github.com/user-attachments/assets/bca10454-1bd3-4e7a-ad7d-e3340aaedc73)
   
10. Click **Pods** **(A)** tab, where you see a new pod instantiated. The new pod downloads the model and then starts it. Since the configmap points to the IBM Granite code model, it is downloaded from HuggingFace and then started. When that happens the new pod's status changes to Running and the existing pod will be terminated.
    ![image](https://github.com/user-attachments/assets/4299ccb0-9233-45f8-904e-9fdb4960ad12)
   
    !!! info "Model download takes time"

        This process takes a few minutes (in this case it took around 3-4 mins as it is a fairly large model compared to tinyllama) and your results might vary. Remember, this is a demo environment and the models are a few GBs in size. Models that are once downloaded will not be downloaded again if you are using the same storage (PV).

11. Provided the previous step is completed without any errors, you will see just 1 pod running.
    ![image](https://github.com/user-attachments/assets/32fb028f-fef6-4abe-996c-b1bdfbe80489)

12. Verify that the model running is IBM Granite code LLM! Click the pod name **(A)** to enter the pod details view/page.
    ![image](https://github.com/user-attachments/assets/f7ae35e7-808e-4a92-bbad-804a5284fffa)

13. In the pod details page, click the **Logs** **(A)** tab to see the pod logs.
    ![image](https://github.com/user-attachments/assets/9e03dd0f-a3c4-457a-b735-03a8cfce48a2)

14. In the log window, scroll upwards to see the name of the model against the attribute **llm_load_print_meta: general.name**.
    
    ![image](https://github.com/user-attachments/assets/a84291e2-c952-4710-a2cd-88caed8b4dd2)

    This confirms that the IBM Granite code LLM is successfully deployed.

15. The next step is to access the model. As done in Lab 1, head to **Topology** **(A)** view, click the **Open URL** **(B)** icon of your application.
    ![image](https://github.com/user-attachments/assets/40a959ed-e233-4597-b904-5ea4dd9eb96d)

16. A new browser window/tab where you are able to interact with your newly deployed LLM. You will see a screen like this:
    ![image](https://github.com/user-attachments/assets/2237409c-7160-471f-aafa-f0e1254c5a53)
17. Scroll all the way down to the input field "Say something..." where you can interact with the LLM. You can ask any question that you like!
    ![image](https://github.com/user-attachments/assets/82196cf5-d4c2-459d-af7e-c24650f1f6ce)

    !!! note "Experimenting with model parameters"

        You can see numerous model parameters or tunables (for example: Predictions, Temperature, and so on). Visit the model-specific documentation and learn about them and experiment with it. You might want to change some parameters, ask the same question, and check how the response changes. These parameters are not covered in this lab as it is outside the scope of the lab.

### Generate Python code

Now, let's use the IBM Granite code model to generate Python code by using natural language queries.

1. Generating Python code for printing the fibonacci series.<br> The input provided was: `Python for finonacci sequence`.
   ![image](https://github.com/user-attachments/assets/031c1d4a-d700-4f2a-89ec-87e49b1cc8f3)
   
     - The preceding code snippet was run on a local Python environment and it ran without errors.
     
     - Notice that I spelled the fibonacci incorrectly, yet it understood the question correctly.
     
     - Also the answer it gave uses recursion (the function fib is being called recursively).

3. Now, let's try asking it to generate the same code without using recursion.<br> The input provided was: `give me Python code for generating fibonacci sequence without using recursion`.
   ![image](https://github.com/user-attachments/assets/74f1ce36-7aee-40ac-8444-8e212402745b)
   
     - As expected, it did give the code snippet that doesn't use recursion, so it did well there.
     
     - Generated code is not 100% correct. The preceding code snippet was run on a local Python environment and it ran into some issues and had to fix some code to make it generate the right fibonacci sequence.
     
     - This demonstrates that code LLMs can generate highly accurate code, though developer intervention might occasionally be required to ensure its perfection.

    !!! info "Accuracy of code LLMs"

         - LLMs excel at generating simple or boilerplate code, often producing highly accurate results for common tasks such as sorting algorithms, basic input/output operations, or template-based functions.
         - For routine tasks and widely-used languages like Python or JavaScript, accuracy rates can be high, sometimes upwards of 80-90% for straightforward problems.
         - When dealing with more complex algorithms, nuanced edge cases, or multi-step logic, LLMs can struggle. The model can produce syntactically correct code that might not solve the problem as intended or might have logic errors.
         - For complex use cases, accuracy can drop significantly, often requiring human intervention to correct the output.
         - AI-generated content can vary and cannot always provide consistent answers. Your results might vary.

   Feel free to explore and try out more scenarios!

### Generate C code

Now, let's use the IBM Granite code model to generate C code by using natural language queries.

1. Generate C code for sorting a list of numbers.<br> The input provided was: `C code to sort a list of numbers`.
   ![image](https://github.com/user-attachments/assets/57ae7cf6-2d8f-4b20-8618-5c19fe2b833b)

     - The preceding code snippet was run on a local C environment and it ran without errors! I had to fix the first line (which was incomplete) as `#include <stdio.h>`.

2. Generate C code for swapping 2 numbers without using a temporary variable.<br> The input provided was: `C code for swapping 2 numbers`.
   ![image](https://github.com/user-attachments/assets/86fe9b4b-0684-45c0-a3da-c159121384ef)

     - The preceding code snippet was run on a local C environment and it ran without errors! I had to fix the first line (which was incomplete) as `#include <stdio.h>`.

     **NOTE**: AI-generated content can vary and cannot always provide consistent answers. Your results can vary.
   Feel free to explore and try out more scenarios!

### Generate SQL query

Generating SQL query by using natural language is a little different than C or Python code since SQL is a language to query Databases (DBs). One needs to give enough context to the code LLM for it to understand the DB table schemas for which you want it to generate the SQL query.

The context is given as a prompt to the code LLM which preceded the natural language query and the code LLM answers the query by using the context that is given in the prompt. The best way to understand is looking at some examples as depicted below.

Let's take a super simple example of a bank which has information that is stored in 2 tables in a DB:

* USERS - This table has user-specific info (user_id, name, age, dob, and so on).
* ACCOUNTS - This table holds the account balance of each user with user_id being the common field between the 2 tables.

1. Given the preceding DB example, the prompt (also known as context) to provide to the code LLM is as follows:
   ```
   You are a developer writing SQL queries given natural language questions. The database contains a set of 2 tables. The schema of each table with description of the attributes is given. Write the SQL query given a natural language statement with names being not case-sensitive.

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
2. A natural language query can be something like:
   
     ```
     With the above schema, please generate sql query to list all users whose balance is > 2000.
     ```
   
3. Let's send the "Prompt + Query" to the code LLM and see how it responds. Feel free to copy and paste it in your LLM application window.
   
     ```
     You are a developer writing SQL queries given natural language questions. The database contains a set of 2 tables. The schema of each table with description of the attributes is given. Write the SQL query given a natural language statement with names being not case-sensitive.
  
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

     - The SQL query that is generated seems correct.
     - Its joining both the tables by using `user_id` as the key and selecting all records where the user's account balance is > 2000.
     - For the sake of people who want to analyze further, pasting the SQL query that was generated:  `SELECT * FROM USERS u, ACCOUNTS a WHERE u.user_id = a.user_id AND a.balance > 2000; `
       
4. Interestingly, code LLM works both ways! Given an SQL query, you can ask the code LLM to explain what it does. To do that, the query formed is as follows. Feel free to copy the query and post it in your LLM application window.
   
    ```
    What does the below SQL query do ?
    SQL Query:
    SELECT * FROM USERS u, ACCOUNTS a WHERE u.userid = a.userid AND a.balance > 2000;
    ```

    <br/>
    
    ![image](https://github.com/user-attachments/assets/8576ca30-42b8-4647-bd92-e092d0c67aa8)

    That is a decent explanation of the SQL query.
   
5. Let's try one more example. Here I give it 2 conditions to match in the query.<br>
   NOTE: You don't have to repeat the whole schema in the prompt. LLMs can remember context.
   
     ```
     Using the schema given above, generate SQL query to list all users whose account balance > 2000 and the user is of type employee.
     ```

     <br/>
     
     ![image](https://github.com/user-attachments/assets/2be00e6a-8d79-4f1b-8844-b6768b63d86a)
  
     The response received is as follows:

     ```
     Here's the SQL query to list all users whose account balance is greater than 2000 and they are of type employee from the USERS and ACCOUNTS tables:
     SELECT * FROM USERS u, ACCOUNTS a WHERE u.user_id = a.user_id AND a.balance > 2000 AND usertypeid='employee';
     ```
     <br/>
     
     - The response is not 100% correct.
     - The last part of the SQL query `usertypeid='employee'` is ambiguous as the DB cannot know which `usertypeid` column to reference.
     - The correct SQL query will have `u.usertypeid='employee'` so that the DB knows that its part of the USERS (aliased as `u` in the query) table.

     **NOTE**: AI-generated content can vary and cannot always provide consistent answers. Your results can vary.<br>
   
6. Reiterating some of the points learned in this lab:
   
       - Code LLMs are not 100% correct, yet they can be immensely helpful for a developer as they can help generate near perfect code which can then be analyzed and tweaked to perfection by the developer.

       - Many code LLMs are available in HF with varied degree of accuracy and clients can choose the right one by experimenting with them in the context of their specific use case.

       - IBM also offers enterprise grade code LLMs through its watsonx Code Assistant (WCA) family of product offerings.

7. IBM Power servers are best used as system of record (SoR) servers which means they hold numerous enterprise-specific data in different DBs (for example: Oracle on AIX, Db2 on AIX / IBM i, PostgreSQL on Linux, and so on). From an IBM Power solution point of view, an end to end solution to query DB records by using natural language can be easily implemented which can help clients immensely. They don't need to depend on experts to generate DB reports. Even executives (with authorized access to the DB) can generate reports and/or view data by using natural language queries.
    - Check this ~2 min short [video demo](https://mediacenter.ibm.com/media/Infusing+AI+into+mission+critical+workloads+with+PowerVS+and+watsonx.ai/1_fzqutamr){target="_blank"} on "Infusing AI into mission-critical workloads with PowerVS and watsonx.ai" which uses natural language to query fraudulent transactions and list high-value customers. Although, this demo is based on PowerVS use case the same is applicable to on-premises as well.

**Summary**

Code LLMs democratize coding by making it more accessible to noncoders, accelerating the development process for smaller teams, and assisting both beginners and experienced developers in learning and improving productivity. By reducing technical barriers, they are transforming who can engage in software development and how innovation happens across industries.
