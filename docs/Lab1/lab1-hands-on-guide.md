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
2. In the resulting form, enter a name: **model-params**, and fill the Key and Value fields as below:
   
     - **Key**: `MODEL_NAME`

     - **Value**: `tinyllama-1.1b-chat-v1.0.Q8_0.gguf`
   
     Click **Add key/value** which will open up one more Key/Value box.
   
     ![image](https://github.com/user-attachments/assets/68614a1f-7a47-425d-8bd4-ebd291b7ee32)
   
4. In the newly created Key/Value box, enter the values as below:

     - **Key**: `MODEL_URL`
     
     - **Value**: `https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF/resolve/main/tinyllama-1.1b-chat-v1.0.Q8_0.gguf`
     
     Click **Create**
     
     ![image](https://github.com/user-attachments/assets/24be3e90-eab3-4961-8f7c-03980c95721a)

     This completes the ConfigMap setup.

### Deploy first model

2. Navigate to the OpenShift Developer profile console window.
   
     - **Note**: If you followed the TIP given in the "Lab setup instructions" page, you should already have a browser window/tab with the Developer profile. In case you didn't, go to top left corner of your console, click **Administrator** and select **Developer**.

4. **IMP**: Ensure you are in the **lab1-demo** project in the Developer profile window. If not, goto **Project** and select **lab1-demo**.
   
    ![type:video](./_attachments/switch-to-lab1-demo-project.mp4)

5. Click on **+Add** and select **Import YAML** option.
   ![image](https://github.com/user-attachments/assets/1f49bdcb-bf92-420b-993f-509a52446462)

6. In the resulting window, copy and paste the below deployment yaml into it and click **Create**
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

7. Here is a quick explanation of the deployment yaml

    ??? info "Deployment yaml explanation"
     
        - **Line 2** says its a deployment yaml  
        - **Line 4** shows the name of this deployment resource (`lab-demo`)         
        - **Lines 7-9**: Defines the label selector to match Pods with the "app: lab1-demo" label         
        - **Lines 11-13**: Specifies the labels that the Pods will have.  
        - `spec` section starting on **Line 14** contains the pod specification. This deployment has 1 pod which contains 2 containers.   
            - **initContainer** named `fetch-model-data` (**Lines 15-32**) which fetches the model from HF and saves it in the underlying storage. The shell script embedded under the `command` section ensures that the model is downloaded only if its not already present. `volumeMounts` section (**Lines 18-20**) has the name of the volume `llama-models` this container will use for disk storage and the path where the storage should be mounted. This initContainer uses the standard RHEL8 UBI (Universal Base Image) image and exits after downloading the model.
            - **Container** named `llama-cpp` (**Lines 33-42**) which is the main workload container that will serve the LLM. This container uses the docker image `quay.io/daniel_casali/llama.cpp-mma:sep2024` which is a custom built image for ppc64le architecture with MMA optimized libraries. This image provides a runtime environment based on the open-source [llama.cpp](https://github.com/ggerganov/llama.cpp) project which enables LLM inference with minimal setup and state-of-the-art performance on a wide variety of hardware.
                - You may ask... What's the need for a model runtime?                   
                    - A model runtime serves as the infrastructure needed to deploy, manage, and execute these models in production.                      
                    - It helps manage system resources such as CPU, GPU, memory, and network resources that are critical for deploying models.                     
                    - Advanced runtimes may include optimizations for specific hardware, including accelerators like TPUs or MMAs (Matrix Math Accelerators), for faster computation.                      
                    - It allows models to be scaled for real-world applications, particularly in cloud or distributed computing environments.                      
                    - It enables handling multiple requests simultaneously while keeping latency low, which is vital in production systems.                      
                - By facilitating the efficient execution and management of machine learning models, runtimes are essential for operationalizing AI and machine learning solutions in production.   
            - **NOTE**: Container `llama-cpp` uses the same volume `llama-models` as initContainer for underlying storage and hence can access the model(s) downloaded by initContainer. **Lines 43-46** specifies the PVC (Persistent Volume Claim) `model-storage` used as the source of storage for the volume. Recall that we created this PVC at the beginning of this lab!

7. You should land in the Deployment details window. Click on **Pods** tab and you should see the Pod erroring out. This is expected as the yaml references MODEL_URL and MODEL_NAME environment variables which we haven't supplied yet! Remember we do have those in ConfigMap, so we use inject that in the next step.
   ![image](https://github.com/user-attachments/assets/e25f5f53-0aa7-4f81-a51b-b3dee3bb7cf9)
8. Navigate to **Environment** tab, select **fetch-model-data** container and select **model-params** ConfigMap and click **Save**
    ![image](https://github.com/user-attachments/assets/0b42cb07-97a2-4f70-82a2-9abc9c6113aa)

    !!! info "About LLama and tinyLLama models"
    
        The ConfigMap currently points to the tinyLLaMa model. The LLaMA (Large Language Model Meta AI) model is a family of state-of-the-art large language models developed by Meta (formerly Facebook), specifically designed to perform various natural language processing tasks. TinyLLaMA is a compact variant of the LLaMA (Large Language Model Meta AI) model, designed for efficiency and accessibility, especially when deployed in smaller environments. Available via Hugging Face (HF), it focuses on reducing the size of large-scale language models while retaining strong performance across various natural language processing tasks.
    
9. Switch back to **Pods** tab and you should see that a new pod has been launched by OpenShift as we changed the pod's environment, when we added ConfigMap.
    ![image](https://github.com/user-attachments/assets/55ba8b7c-3760-45f8-813b-99b90e026daf)
10. The new pod will download the model and then start it. Since the configmap points to tinyllama model, it will be downloaded from HuggingFace and then started. When that happens the pod's status will change to Running.

    !!! info "Model download will take time - Have patience!!"

        This process will take a few minutes (in my case it took around 1-1.5 mims) and your mileage may vary! Remember, this is a demo environment and models are few GBs in size. Models once downloaded won't be downloaded again as long as you are using the same storage (PV)
    
    ![image](https://github.com/user-attachments/assets/06801c61-7ec4-46c6-b5d7-1f9f4af660dc)

    **Congratulations!**, you have successfully deployed a LLM on Power10.

11. Let's verify that the model running is tinyllama!. Click on the pod to enter the pod details view/page.
    ![image](https://github.com/user-attachments/assets/b96d318e-f6e8-48ea-9b19-88ca2813d0ca)
12. In the pod details page, click on the **Logs** tab to see the pod logs
    ![image](https://github.com/user-attachments/assets/d01b6b9e-b1d7-4f5a-8dd1-a2bd54882aff)
13. In the log window, scroll upwards to see the name of the model against the attribute **llm_load_print_meta: general.name**
    ![image](https://github.com/user-attachments/assets/89536022-644a-497c-9225-9a08d68de52a)

    This verifies that we have indeed deployed tinyllama.
     
14. Let's access our model and interact with it. In OpenShift, you need to create a service and a route which generates the cluster internal and publicly accessible HTTP endpoints, respectively. To do that, navigate to the OpenShift Administrator profile, click **Networking** -> **Services** and click **Create Service**
     
    !!! note ""
      
        If you are switching browser window/tab, make sure you are in the **lab1-demo** project in the new window/tab.
  
    ![image](https://github.com/user-attachments/assets/d901813f-96f5-4c91-a5d5-1455adafff09)

15. In the resulting Create Service yaml window, select all & delete everything. Then copy the below service yaml, paste it in the yaml window and click **Create**
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

16. Here is a quick explanation of the Service yaml

    ??? info "Service yaml explanation"
    
        - **Line 2** shows its a service yaml
          
        - **Line 4** shows the name of the service (`lab1-service`)
          
        - **Line 8** shows the type of this service is `ClusterIP`. ClusterIP is the default service type used to expose a set of Pods internally within the cluster. It creates a virtual IP (ClusterIP) for the service, which other services or Pods within the same Kubernetes cluster can use to access it.
          
        - **Lines 9-13** has the port details. `port: 8080` refers to the port for the incoming traffic of the service. `targetPort: 8080` refers to the port on the pod. This means that incoming traffic for the service on port 8080 will be forwarded to the pod on port 8080.
          
        - **Lines 14-15** specifies the pod selector label. It defines which Pods the service will route traffic to. In this case, it matches Pods that have the label `app: lab1-demo`, which is the label we assigned in the deployment yaml.
      
18. You should land in service details view/page. You can see the ClusterIP which is accessible from inside the cluster only.
    ![image](https://github.com/user-attachments/assets/5b8b3385-44a7-4dec-bbfa-f384c5f784fb)

19. Navigate to **Networking** -> **Routes** and click **Create Route**
      ![image](https://github.com/user-attachments/assets/7240eef0-b4af-4fde-abed-faae0234e343)
20. In the resulting Create Route window, select **YAML view** and clear everything from the yaml window. Copy the below yaml and paste it in the yaml window and click **Create**
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

21. Here is a quick explanation of the Route yaml. In OpenShift, a Route exposes a service to external clients by mapping an external URL to an internal OpenShift service.

    ??? info "Route yaml explanation"
    
        - **Line 1**: This defines that you're creating an OpenShift Route resource.
          
        - **Lines 3-6**: The `name` field defines the name of the route (`lab1-route`), and the `labels` field associates labels with the route, which can be used for tracking or organizing the route in OpenShift.
          
        - **Lines 7-10**: The `to` field under `spec` defines the target resource for the route. In this case, it targets a Service named `lab1-service`, which has already been defined and exposes Pods internally via ClusterIP.
          
        - **Line 11**: TLS is set to `null`, meaning there is no SSL/TLS encryption configured for this route. For secure routes (HTTPS), you'd configure this section to specify certificates and encryption protocols.
          
        - **Lines 12-13**: The `targetPort` field under `port` specifies which port on the service (or Pods) the route should forward requests to. In this case, it refers to `lab1-port`, which was defined as port 8080 in our service configuration.
      
23. You should land in the route details view. The URL mentioned under **Location** is the externally accessible URL of your application (which hosts the tinyllama LLM).
    ![image](https://github.com/user-attachments/assets/abfcc310-985e-4208-9e56-d2f39e4f2c13)
24. Click on the external URL in the route details view to access your model. A new browser window/tab where you will be able to interact with your newly deployed LLM. You should see a screen like this:      
      
    ![image](https://github.com/user-attachments/assets/2237409c-7160-471f-aafa-f0e1254c5a53)
      
19. Scroll all the way down to the input field "Say something..." where you can interact with the LLM. You can ask any question you like, but keep in mind you're using a small model and there are more powerful models out there for general conversation.
    ![image](https://github.com/user-attachments/assets/82196cf5-d4c2-459d-af7e-c24650f1f6ce)

    !!! note "Experimenting with model parameters"

        You can see a lot of model parameters or tunables (eg: Predictions, Temperature, etc.). Feel free to google and learn about them and experiment with it. You may want change some parameters, ask the same question and check how the response changes. We will not cover these parameters in this lab as its outside the scope of the lab

20. Here are some questions I asked and the responses I got.
    
    !!! warning "Accuracy of LLM responses"

        Large Language Models (LLMs), are trained on vast amounts of text data, but that training is limited to a **specific cutoff date**. This means that the model can only answer questions based on the information available up to that point in time. It cannot access real-time data or understand events, trends, or new information that occurred after the cutoff date. Consequently, their ability to provide accurate answers is constrained by the knowledge they were trained on.

    ![image](https://github.com/user-attachments/assets/c0c4b3ca-dbc6-4f9f-8d29-115c11486843)

    ![image](https://github.com/user-attachments/assets/c9fde420-3c8c-40bc-a48c-0ece97fc2248)

    **Congratulations**, you were able to deploy a LLM, create a public endpoint to access it and take it for a run!
    In the next step, let's learn how to deploy a different LLM.

### Deploy second model

In this section, let's deploy IBM's granite model.

!!! info "About IBM's granite LLM"

    The IBM Granite model family is designed as enterprise-grade AI models tailored for business applications. Granite models are available both as open-source models on platforms like Hugging Face and through IBM's watsonx.ai for more enterprise-specific needs.

1. Navigate to your OpenShift developer profile window. Click **ConfigMaps**, then click **model-params**. This should open up the ConfigMap details page.
   ![image](https://github.com/user-attachments/assets/a26e875b-02d0-438e-84f3-9ed86347e650)
2. In the model-params ConfigMap details page, click **Action** and select **Edit ConfigMap**
   ![image](https://github.com/user-attachments/assets/a0d66be8-8ca3-4f18-b360-ee35a7843862)
3. In the resulting form, edit the key/value fields for MODEL_NAME and MODEL_URL as below and click **Save**
   
     - Key: MODEL_NAME
     - Value: granite-7b-lab.Q4_K_M.gguf
     - ---------
     -  Key: MODEL_URL
     -  Value: https://huggingface.co/RichardErkhov/instructlab_-_granite-7b-lab-gguf/resolve/main/granite-7b-lab.Q4_K_M.gguf

    ![image](https://github.com/user-attachments/assets/7347b269-c597-4570-9d5d-509474212052)
4. The existing pod won't see the changes right away as changing values of a ConfigMap doesn't cause a deployment (and hence pod) to restart. It needs to be done manually.
   
6. Let's go to the deployment view. Click **Topology**, then click "**D lab1-demo**" part of the application icon, which will open up the deployment details pane (on the right hand side of the browser window). Click **D lab1-demo** in that pane which will then open up the deployment details view for lab1-demo deployment.
   
   ![image](https://github.com/user-attachments/assets/ecbbca24-682c-4480-a989-7a72f7398958)
   
8. Click **Actions** and select **Restart rollout**. This will restart the deployment which results in redeployment of the pod.
   
   ![image](https://github.com/user-attachments/assets/51476c1a-52a2-4fbe-ae28-9dca9348ac4f)
   
10. Click on **Pods** tab, where you will see a new pod instantiated. The new pod will download the model and then start it. Since the configmap points to granite model, it will be downloaded from HuggingFace and then started. When that happens the new pod's status will change to Running and the existing pod will be terminated.
    
    ![image](https://github.com/user-attachments/assets/b80de8ad-04b0-414b-bc2c-247dd83b089b)

    !!! info "Model download will take time - Have patience!!"

        This process will take a few minutes (in my case it took around 3-4 mins as this is a fairly large model compared to tinyllama) and your mileage may vary! Remember, this is a demo environment and models are few GBs in size. Models once downloaded won't be downloaded again as long as you are using the same storage (PV).
   
11. If all goes well, you should see just 1 pod running.
    
    ![image](https://github.com/user-attachments/assets/854703df-2ca4-42ce-825c-30c49a94a050)
   
12. Let's verify that the model running is granite!. Click on the pod to enter the pod details view/page.
    
    ![image](https://github.com/user-attachments/assets/8a50c71b-4490-4bfe-8aaf-eafa25fdc05f)
    
13. In the pod details page, click on the Logs tab to see the pod logs.
    
    ![image](https://github.com/user-attachments/assets/bd5aa047-9b86-4996-abb5-28c1ec091d75)
    
15. In the log window, scroll upwards to see the name of the model against the attribute **llm_load_print_meta: general.name**.
    
    ![image](https://github.com/user-attachments/assets/5ca4f0b1-8655-41df-85d2-8d740627989c)

    This verifies that we have indeed deployed granite.   

16. Let's access the model now!. As we did in the previous section of this lab, we need to find the external public endpoint. The beauty of OpenShift is that the endpoint remains same inspite of the pod being restarted. So either you can refresh the earlier page (if you have it opened in the browser) or follow the steps below to access the public URL of your application via the **Topology** view.

    !!! note "Multiple ways to access public URL of your application"

        If you are in Administrator profile, you can navigate to **Networking** -> **Routes** to access your route resource and click on the URL to open your application, as we did in the previous section of this lab where we deployed our first model. Alternatively, if you are in Developer profile, you can go to **Topology** view and access the URL of your application as well. Let's use that method here...

    - In Developer profile window, click **Topology** and you should see the icon representing your deployed application.
      ![image](https://github.com/user-attachments/assets/a34df273-e29b-44e0-9cf3-90e94d8fdafb)
    - Click on the arrow (in top right corner of the icon) that says "Open URL"
      ![image](https://github.com/user-attachments/assets/7366146b-cf82-4dc3-9327-d51aa5944778)
    - A new browser window/tab where you will be able to interact with your newly deployed LLM. You should see a screen like this:
      ![image](https://github.com/user-attachments/assets/2237409c-7160-471f-aafa-f0e1254c5a53)
    - Scroll all the way down to the input field "Say something..." where you can interact with the LLM. You can ask any question you like!
      ![image](https://github.com/user-attachments/assets/82196cf5-d4c2-459d-af7e-c24650f1f6ce)

    !!! note "Experimenting with model parameters"

        You can see a lot of model parameters or tunables (eg: Predictions, Temperature, etc.). Feel free to google and learn about them and experiment with it. You may want change some parameters, ask the same question and check how the response changes. We will not cover these parameters in this lab as its outside the scope of the lab

17. Here are some questions I asked and the responses I got.

    !!! warning "Accuracy of LLM responses"

        Large Language Models (LLMs), are trained on vast amounts of text data, but that training is limited to a **specific cutoff date**. This means that the model can only answer questions based on the information available up to that point in time. It cannot access real-time data or understand events, trends, or new information that occurred after the cutoff date. Consequently, their ability to provide accurate answers is constrained by the knowledge they were trained on.

    ![image](https://github.com/user-attachments/assets/4201cafb-9f11-4f82-945b-1f843713426e)

    ![image](https://github.com/user-attachments/assets/e0a23318-e2bc-4778-8c12-84643a8f530c)

    **Congratulations**, in this section you learned how easy it is to switch to a different LLM, access it and take it for a run!
    This completes the lab.


