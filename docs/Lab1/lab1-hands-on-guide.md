# Deploy a LLM on Power10 - Hands-on lab guide

## Lab-ready check

Make sure you have the following items ready:

- In your browser, you have logged in as `cecuser` in the OpenShift GUI/console.
- In your terminal, you have logged into the bastion server and authenticated to the OpenShift cluster using the OpenShift CLI `oc`.
  
!!! warning "Warning"
    Do not proceed further unless the items mentioned above are ready. Please refer to "Lab setup instructions" section (see left hand side menu) to setup your browser and terminal windows.

## Lab guide

!!! note "Image zoom functionality"

    Feel free to click on the images in the lab guide below to a view larger image
    
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

   !!! info "Note"
   
       If you followed the TIP given in the "Lab setup instructions" page, you should already have a browser window/tab with the Developer profile.
       In case you didn't, go to top left corner of your console, click Administrator and select Developer.

3. **IMP**: Ensure you are in the **lab1-demo** project in the Developer profile window. If not, goto **Project** and select **lab1-demo**.
   
    ![type:video](./_attachments/switch-to-lab1-demo-project.mp4)

4. Click on **+Add** and select **Import YAML** option.
   ![image](https://github.com/user-attachments/assets/1f49bdcb-bf92-420b-993f-509a52446462)

5. In the resulting window, copy and paste the below deployment yaml into it and click **Create**
   ``` yaml linenums="1"
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: lab1-demo
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: lab1-demo
      template:
        metadata:
          labels:
            app: lab1-demo
        spec:
          initContainers:
            - name: fetch-model-data
              image: ubi8
              volumeMounts:
                - name: llama-models
                  mountPath: /models
              command:
                - sh
                - '-c'
                - |
                  if [ ! -f /models/$MODEL_NAME ] ; then
                    curl -L $MODEL_URL --output /models/$MODEL_NAME
                  else
                    echo "model /models/$MODEL_NAME already present"
                  fi
                  rm -f /models/mymodel
                  ln -sf /models/$MODEL_NAME /models/mymodel
              resources: {}
          containers:
            - name: llama-cpp
              image: quay.io/daniel_casali/llama.cpp-mma:sep2024
              args: ["-m", "/models/mymodel", "-c", "4096", "--host", "0.0.0.0"]
              ports:
                - containerPort: 8080
                  name: http
              volumeMounts:
                - name: llama-models
                  mountPath: /models
          volumes:
            - name: llama-models
              persistentVolumeClaim:
                claimName: model-storage
   ```
    ![image](https://github.com/user-attachments/assets/84a47fac-c4a6-49bc-b69d-3b89266b4d61)

 6. You should land in the Deployment details window. Click on **Pods** tab and you should see the Pod erroring out. This is expected as the yaml references MODEL_URL and MODEL_NAME environment variables which we haven't supplied yet! Remember we do have those in ConfigMap, so we use inject that in the next step.
   ![image](https://github.com/user-attachments/assets/e25f5f53-0aa7-4f81-a51b-b3dee3bb7cf9)
 7. Navigate to **Environment** tab, select **fetch-model-data** container and select **model-params** ConfigMap and click **Save**
    ![image](https://github.com/user-attachments/assets/0b42cb07-97a2-4f70-82a2-9abc9c6113aa)
 8. Switch back to **Pods** tab and you should see that a new Pod has been launched by OpenShift as we updated the Pods's environment via ConfigMap.
    ![image](https://github.com/user-attachments/assets/55ba8b7c-3760-45f8-813b-99b90e026daf)
 9. The new pod will download the model and then start it. Since the configmap points to tinyllama model, it will be downloaded from HuggingFace and then started. When that happens the pod's status will change to Running.

    !!! info "Model download will take time - Have patience!!"

        This process will take a few minutes (in my case it took around 1-1.5 mims) and your mileage may vary! Remember, this is a demo environment and models are few GBs in size. Models once downloaded won't be downloaded again as long as you are using the same pod instance.
    
    ![image](https://github.com/user-attachments/assets/06801c61-7ec4-46c6-b5d7-1f9f4af660dc)

    **Congratulations!**, you have successfully deployed a LLM on Power10.

 10. Let's verify that the model running is tinyllama!. Click on the pod to enter the pod details view/page.
    ![image](https://github.com/user-attachments/assets/b96d318e-f6e8-48ea-9b19-88ca2813d0ca)
11. In the pod details page, click on the **Logs** tab to see the pod logs
    ![image](https://github.com/user-attachments/assets/d01b6b9e-b1d7-4f5a-8dd1-a2bd54882aff)
12. In the log window, scroll upwards to see the name of the model against the attribute **llm_load_print_meta: general.name**
    ![image](https://github.com/user-attachments/assets/89536022-644a-497c-9225-9a08d68de52a)

    This verifies that we have indeed deployed tinyllama.
     
13. Let's access our model and interact with it. In OpenShift, you need to create a service and a route which generates the cluster internal and publicly accessible HTTP endpoints, respectively. To do that, navigate to the OpenShift Administrator profile, click **Networking** -> **Services** and click **Create Service**
     
    !!! note ""
      
        If you are switching browser window/tab, make sure you are in the **lab1-demo** project in the new window/tab.
  
    ![image](https://github.com/user-attachments/assets/d901813f-96f5-4c91-a5d5-1455adafff09)

14. In the resulting Create Service yaml window, select all & delete everything. Then copy the below service yaml, paste it in the yaml window and click **Create**
    ``` yaml linenums="1"
    apiVersion: v1
    kind: Service
    metadata:
      name: "lab1-service"
      labels:
        app: "lab1-service"
    spec:
      type: "ClusterIP"
      ports:
        - name: lab1-port
          port: 8080
          protocol: TCP
          targetPort: 8080
      selector:
        app: "lab1-demo"
    ```
    ![image](https://github.com/user-attachments/assets/6e76ac2d-bf80-41c2-895b-53050d4cbbbf)

15. You should land in service details view/page. You can see the ClusterIP which is accessible from inside the cluster only.
    ![image](https://github.com/user-attachments/assets/5b8b3385-44a7-4dec-bbfa-f384c5f784fb)

16. Navigate to **Networking** -> **Routes** and click **Create Route**
      ![image](https://github.com/user-attachments/assets/7240eef0-b4af-4fde-abed-faae0234e343)
17. In the resulting Create Route window, select **YAML view** and clear everything from the yaml window. Copy the below yaml and paste it in the yaml window and click **Create**
    ``` yaml linenums="1"
    kind: Route
    apiVersion: route.openshift.io/v1
    metadata:
      name: lab1-route
      labels:
        app: lab1-route
    spec:
      to:
        kind: Service
        name: lab1-service
      tls: null
      port:
        targetPort: lab1-port
    ```
    ![image](https://github.com/user-attachments/assets/f100a3dc-1286-4c6f-bd60-b534f1e84090)

18. You should land in the route details view. The URL mentioned under **Location** is the externally accessible URL of your application (which hosts the tinyllama LLM).
    ![image](https://github.com/user-attachments/assets/abfcc310-985e-4208-9e56-d2f39e4f2c13)
19. Click on the external URL in the route details view to access your model. A new browser window/tab where you will be able to interact with your newly deployed LLM. You should see a screen like this:      
      
    ![image](https://github.com/user-attachments/assets/2237409c-7160-471f-aafa-f0e1254c5a53)
      
20. Scroll all the way down to the input field "Say something..." where you can interact with the LLM. You can ask any question you like, but keep in mind you're using a small model and there are more powerful models out there for general conversation.
    ![image](https://github.com/user-attachments/assets/82196cf5-d4c2-459d-af7e-c24650f1f6ce)

    !!! note "Experimenting with model parameters"

        You can see a lot of model parameters or tunables (eg: Predictions, Temperature, etc.). Feel free to google and learn about them and experiment with it. You may want change some parameters, ask the same question and check how the response changes. We will not cover these parameters in this lab as its outside the scope of the lab

21. Here are some questions I asked and the responses I got.
    
    !!! warning "Accuracy of LLM responses"

        Large Language Models (LLMs), are trained on vast amounts of text data, but that training is limited to a **specific cutoff date**. This means that the model can only answer questions based on the information available up to that point in time. It cannot access real-time data or understand events, trends, or new information that occurred after the cutoff date. Consequently, their ability to provide accurate answers is constrained by the knowledge they were trained on.

    ![image](https://github.com/user-attachments/assets/c0c4b3ca-dbc6-4f9f-8d29-115c11486843)

    ![image](https://github.com/user-attachments/assets/c9fde420-3c8c-40bc-a48c-0ece97fc2248)

    **Congratulations**, you were able to deploy a LLM, create a public endpoint to access it and take it for a run!
    In the next step, let's learn how to deploy a different LLM.
