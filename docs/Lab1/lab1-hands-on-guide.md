# Deploy an LLM on Power10 - Hands-on lab guide

## Lab-ready check

Make sure that you have the following items ready:

- In your browser, you are logged in as `cecuser` within the Red Hat OpenShift console.
- In your terminal, you are logged in to the bastion server and authenticated to the Red Hat OpenShift cluster by using the Red Hat OpenShift CLI `oc`.
  
!!! warning "Warning"
    Proceed only once the items mentioned earlier are ready. Refer to "Lab setup instructions" section (see left side menu) to setup your browser and terminal windows.

## Lab guide

!!! note "Image zoom functionality"

    Click the images in the following lab guide to view them in a larger format.
    
### Create a project
1. Navigate to the Red Hat OpenShift Administrator profile. Click **Home** **(A)** -> **Projects** **(B)** and click **Create Project** **(C)**.
   ![image](https://github.com/user-attachments/assets/87c5fec4-e76d-4ca3-9845-77d25b8c7d46)

2. Enter a project name: **lab1-demo** **(A)** and click **Create** **(B)**.
   ![image](https://github.com/user-attachments/assets/6d1b6e5d-3cf6-4c3b-811a-6bd40a7a715e)

### Setup storage
Setup storage for this lab which is needed for storing the downloaded AI models. This environment comes with NFS (file storage) pre-configured. 
In Red Hat OpenShift, you first request for the storage (also known as PersistentVolumeClaim or PVC) and the actual storage (also known as PersistentVolume or PV) gets allotted based on your request and the storage availability in the storage pool (NFS in this case).

1. Navigate to the Red Hat OpenShift Administrator profile. Click **Storage** **(A)** -> **PersistentVolumeClaims** **(B)** and click **Create PersistentVolumeClaim** **(C)**.
   ![image](https://github.com/user-attachments/assets/23b17459-0bd3-4abd-8cd2-4bfd572efa0f)
2. In the resulting form, enter the PVC name: **model-storage** **(A)** and Size: **20** **(B)** GB. Leave other fields as defaults and click **Create** **(C)**.
   ![image](https://github.com/user-attachments/assets/98571147-31a2-4e37-a063-c2d1c3929a64)
3. It shows PVC status as bound, which means storage was allotted.
   ![image](https://github.com/user-attachments/assets/ea19ae1f-899d-4ff5-a5e0-f97df1e97ea2)
4. Navigate to **Storage** **(A)** -> **PersistentVolumes** **(B)** and view the actual storage (PV) bound to your storage request (PVC = **model-storage** **(C)**).
   ![image](https://github.com/user-attachments/assets/4209536d-a21a-42d0-b847-cc6ad15cd64d)

    The storage setup is now complete.

### Setup ConfigMap

In Red Hat OpenShift, a ConfigMap is an object that is used to manage configuration data for applications. It helps to decouple configuration details from application logic, which makes your applications more portable and easier to manage across different environments. Instead of hardcoding configuration values in your application, you store them in a ConfigMap and inject them into your application at run time.

ConfigMap is used to store the model URL and model name, both of which are required during model deployment. Using ConfigMap allows for easy switching to a different model.

1. Navigate to **Workloads** **(A)** -> **ConfigMaps** **(B)** and select **lab1-demo** **(C)** as the active Project.
   ![image](https://github.com/user-attachments/assets/4b0f77a2-f5f5-4821-a073-bbc67ded39cf)

5. Click **Create ConfigMap** **(A)**.
   ![image](https://github.com/user-attachments/assets/793a95ad-953a-49d9-8cec-8c7e321947cd)

6. In the resulting form, make sure the **Form view** **(A)** option is selected.
   ![image](https://github.com/user-attachments/assets/f0c6b9e0-f60d-4b92-b3c5-45252351e539)

8. Enter a name: **model-params** **(A)**, and fill the Key and Value fields as below:
   
     - **Key**: `MODEL_NAME` **(B)**

     - **Value**: `tinyllama-1.1b-chat-v1.0.Q8_0.gguf` **(C)**
   
     Click **Add key/value** **(D)** which opens up one more Key/Value box.
   
     ![image](https://github.com/user-attachments/assets/be119bf2-f37d-45f8-b9df-c8d4d7045e63)
   
9. In the newly created Key/Value box, enter the values as below:

     - **Key**: `MODEL_URL` **(A)**
     
     - **Value**: `https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF/resolve/main/tinyllama-1.1b-chat-v1.0.Q8_0.gguf` **(B)**
     
     Click **Create** **(C)**.
     
     ![image](https://github.com/user-attachments/assets/5abc809b-0f3b-4356-9429-2d5159a561e7)

     The ConfigMap setup is now complete.

### Deploy first model

2. Navigate to the Red Hat OpenShift Developer profile console window.
   
     - **Note**: If you followed the TIP given in the "Lab setup instructions" page, you already have a browser window/tab with the Developer profile. In case you didn't, go to the upper left section of your console, click **Administrator** and select **Developer**.

4. **IMP**: Make sure that you are in the **lab1-demo** project in the Developer profile window. If not, goto **Project** and select **lab1-demo**.
   
    ![type:video](./_attachments/switch-to-lab1-demo-project.mp4)

5. Click **+Add** **(A)** and select **Import YAML** **(B)** option.
   ![image](https://github.com/user-attachments/assets/72e28c16-590c-4acd-9c29-5dc91bc031d7)

6. In the resulting window, copy and paste the following deployment YAML into it and click **Create** **(A)**.
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
    ![image](https://github.com/user-attachments/assets/35195937-4df3-4263-9d4a-755b9bd67fce)

7. A quick explanation of the deployment YAML follows.

    ??? info "Deployment YAML explanation"
     
        - **Line 2** says its a deployment YAML  
        - **Line 4** shows the name of this deployment resource (`lab-demo`)         
        - **Lines 7-9**: Defines the label selector to match Pods with the "app: lab1-demo" label         
        - **Lines 11-13**: Specifies the labels that the Pods have.  
        - `spec` section starting on **Line 14** contains the pod specification. This deployment has 1 pod which contains 2 containers.   
            - **initContainer** named `fetch-model-data` (**Lines 15-32**) which fetches the model from HF and saves it in the underlying storage. The shell script embedded under the `command` section makes sure that the model is downloaded only if it is not already present. `volumeMounts` section (**Lines 18-20**) has the name of the volume `llama-models` this container uses for disk storage and the path where the storage is mounted. This initContainer container uses the standard RHEL8 UBI (Universal Base Image) image and exits after the model is downloaded.
            - **Container** named `llama-cpp` (**Lines 33-42**) which is the main workload container that serves the LLM. This container uses the docker image `quay.io/daniel_casali/llama.cpp-mma:sep2024` which is a custom-built image for ppc64le architecture with MMA optimized libraries. This image provides a runtime environment based on the open source [llama.cpp](https://github.com/ggerganov/llama.cpp){target="_blank"} project which enables LLM inference with minimal setup and state-of-the-art performance on a wide variety of hardware.
                - You may ask... What's the need for a model runtime?                   
                    - A model runtime serves as the infrastructure needed to deploy, manage, and run these models in production.                      
                    - It helps manage system resources such as CPU, GPU, memory, and network resources that are critical for deploying models.                     
                    - Advanced runtimes may include optimizations for specific hardware, including accelerators like TPUs or MMAs (Matrix Math Accelerators), for faster computation.                      
                    - It allows models to be scaled for real-world applications, particularly in cloud or distributed computing environments.                      
                    - It enables handling multiple requests simultaneously while keeping latency low, which is vital in production systems.                      
                - By facilitating the efficient execution and management of machine learning models, runtimes are essential for operationalizing AI and machine learning solutions in production.   
            - **NOTE**: Container `llama-cpp` uses the same storage volume `llama-models` as container `initContainer` for underlying storage and hence can access the models that are downloaded by container `initContainer`. **Lines 43-46** specifies the PVC (Persistent Volume Claim) `model-storage` used as the source of storage for the volume. Recall that this PVC was created at the beginning of this lab!

7. You are now in the Deployment details window. Click **Pods** **(A)** tab and you see the Pod erring out. This behavior is expected as the YAML references MODEL_URL and MODEL_NAME environment variables which aren’t supplied yet! These environment variables aren’t in the ConfigMap, and will be injected in the next step.
   ![image](https://github.com/user-attachments/assets/d6032ff5-03ba-4c2f-b733-ffb8a83fdf30)
8. Navigate to **Environment** **(A)** tab, select **fetch-model-data** **(B)** container and select **model-params** **(C)** ConfigMap and click **Save** **(D)**.
    ![image](https://github.com/user-attachments/assets/ef491f57-4908-427d-bf8b-c7e4a20c5a4e)

    !!! info "About LLama and tinyLLama models"
    
        The ConfigMap currently points to the tinyLLaMa model. Meta (formerly Facebook) developed the LLaMA (Large Language Model Meta AI) model, a family of state-of-the-art large language models, which are designed to perform various natural language processing tasks. TinyLLaMA is a compact variant of the LLaMA (Large Language Model Meta AI) model, which is designed for efficiency and accessibility, especially when deployed in smaller environments. Available in Hugging Face (HF), it focuses on reducing the size of large-scale language models while retaining strong performance across various natural language processing tasks.
    
9. Switch back to the **Pods** **(A)** tab and observe that a new pod is started by Red Hat OpenShift, as the pod's environment was modified when the ConfigMap was added.
    ![image](https://github.com/user-attachments/assets/74c8a1f9-fa9d-42b8-a26b-f8ea873d1116)
10. The new pod downloads the model and then starts it. Since the configmap points to a tinyllama model, it is downloaded from HuggingFace and then started. When that happens the pod's status changes to Running.

    !!! info "Model download takes time"

        This process takes a few minutes (in this case, it took around 1 to 1.5 minutes) and your results might vary. Remember, this is a demo environment and the models are a few GBs in size. Models that are once downloaded are not downloaded again if you are using the same storage (PV).
    
    ![image](https://github.com/user-attachments/assets/06801c61-7ec4-46c6-b5d7-1f9f4af660dc)

    ** Well done!** The LLM is successfully deployed on Power10.

11. Verify that the model running is indeed tinyllama! Click the pod name **(A)** to enter the pod details view/page.
    ![image](https://github.com/user-attachments/assets/5a6e4078-9047-4d2f-ab84-942f938b46b7)
12. In the pod details page, click the **Logs** **(A)** tab to see the pod logs.
    ![image](https://github.com/user-attachments/assets/c7bb75d5-6768-45fa-b51f-37fef4132218)
13. In the log window, scroll upwards to see the name of the model against the attribute **llm_load_print_meta: general.name**.
    ![image](https://github.com/user-attachments/assets/89536022-644a-497c-9225-9a08d68de52a)

    This confirms that tinyllama is successfully deployed.
     
14. The next step is to access the model and interact with it. In Red Hat OpenShift, you need to create a service and a route which generates the cluster internal and publicly accessible HTTP endpoints, respectively. To do that, navigate to the Red Hat OpenShift Administrator profile and click **Networking** **(A)** -> **Services** **(B)** and click **Create Service** **(C)**.
     
    !!! note ""
      
        If you are switching browser window/tab, make sure that you are in the **lab1-demo** project in the new window/tab.
  
    ![image](https://github.com/user-attachments/assets/a4f691e2-f597-42c8-951f-6872d02d7713)

15. In the resulting Create Service YAML window, select all, and delete everything. Then copy the following service YAML, paste it in the YAML window and click **Create** **(A)**.
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
    ![image](https://github.com/user-attachments/assets/c53a1eaf-57c7-4fcd-8148-b0ab9fba77e4)

16. A brief explanation of the Service YAML is provided below.

    ??? info "Service YAML explanation"
    
        - **Line 2** shows that it is a service YAML
          
        - **Line 4** shows the name of the service (`lab1-service`)
          
        - **Line 8** shows that the type of this service is `ClusterIP`. ClusterIP is the default service type that is used to expose a set of Pods internally within the cluster. It creates a virtual IP (ClusterIP) for the service, which other services or Pods within the same Kubernetes cluster can use to access it.
          
        - **Lines 9-13** has the port details. `port: 8080` refers to the port for the incoming traffic of the service. `targetPort: 8080` refers to the port on the pod. This means that incoming traffic for the service on port 8080 is forwarded to the pod on port 8080.
          
        - **Lines 14-15** specifies the pod selector label. It defines which Pods the service routes traffic to. In this case, it matches Pods that have the label `app: lab1-demo`, which is the label that was assigned in the deployment YAML.
      
18. You are in service details view/page. You can see the ClusterIP which is accessible from inside the cluster only.
    ![image](https://github.com/user-attachments/assets/5b8b3385-44a7-4dec-bbfa-f384c5f784fb)

19. Navigate to **Networking** **(A)** -> **Routes** **(B)** and click **Create Route** **(C)**.
      ![image](https://github.com/user-attachments/assets/4bf9bb4f-b017-43fe-ae12-99db103d001f)

20. In the resulting Create Route window, select **YAML view** **(A)** and clear everything from the YAML window. Copy the below YAML and paste it in the YAML window and click **Create** **(B)**.
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
    ![image](https://github.com/user-attachments/assets/9f313dfd-c1a3-4ec7-8ebe-b1e2595b8ae6)

21. A brief explanation of the Route YAML is being provided here. In Red Hat OpenShift, a Route exposes a service to external clients by mapping an external URL to an internal Red Hat OpenShift service.

    ??? info "Route YAML explanation"
    
        - **Line 1**: This line defines the creation of a Red Hat OpenShift Route resource.
          
        - **Lines 3-6**: The `name` field defines the name of the route (`lab1-route`), and the `labels` field associates labels with the route, which can be used for tracking or organizing the route in Red Hat OpenShift.
          
        - **Lines 7-10**: The `to` field under `spec` defines the target resource for the route. In this case, it targets a Service that is named `lab1-service`, which is already defined and exposes Pods internally by using ClusterIP resource.
          
        - **Line 11**: TLS is set to `null`, which means that SSL/TLS encryption that is not configured for this route. For secure routes (HTTPS), you'd configure this to specify certificates and encryption protocols.
          
        - **Lines 12-13**: The `targetPort` field under `port` specifies which port on the service (or Pods) the route will forward requests to. In this case, it refers to `lab1-port`, which was defined as port 8080 in our service configuration.
      
23. You are in the route details view. The URL mentioned under **Location** is the externally accessible URL of your application (which hosts the tinyllama LLM).
    ![image](https://github.com/user-attachments/assets/abfcc310-985e-4208-9e56-d2f39e4f2c13)
24. Click the external URL in the route details view to access your model. A new browser window/tab where you are able to interact with your newly deployed LLM. You see a screen like this:      
      
    ![image](https://github.com/user-attachments/assets/2237409c-7160-471f-aafa-f0e1254c5a53)
      
19. Scroll all the way down to the input field "Say something..." where you can interact with the LLM. You can ask any question that you like, but keep in mind you're using a small model and many more powerful models out there are available for general conversation.
    ![image](https://github.com/user-attachments/assets/82196cf5-d4c2-459d-af7e-c24650f1f6ce)

    !!! note "Experimenting with model parameters"

        You can see numerous model parameters or tunables (for example: Predictions, Temperature, and so on). Visit the model-specific documentation and learn about them and experiment with it. You can try changing some parameters, ask the same question and check how the response changes. These parameters are not covered in this lab as it is outside the scope of the lab.

20. Presented below are the questions and their corresponding responses.
    
    !!! warning "Accuracy of LLM responses"

        - Large language models (LLMs) are trained on vast amounts of text data, but that training is limited to a **specific cutoff date**. It means that the model can only answer questions based on the information available up to that point in time. It cannot access real-time data or understand events, trends, or new information that occurred after the cutoff date. Therefore, the knowledge on which they were trained limits their ability to provide accurate answers.
        - AI-generated content varies and does not always provide consistent answers. Your response might be different.

    ![image](https://github.com/user-attachments/assets/c0c4b3ca-dbc6-4f9f-8d29-115c11486843)

    ![image](https://github.com/user-attachments/assets/c9fde420-3c8c-40bc-a48c-0ece97fc2248)

    **Well done!**, you successfully deployed an LLM model and provisioned a public-facing API endpoint to access it.
    In the next step, you will learn how to deploy a different LLM.

### Deploy second model

In this section, IBM's Granite model is deployed.

!!! info "About IBM's Granite LLM"

    The IBM Granite model family is designed as enterprise-grade AI models tailored for business applications. Granite models are available both as open source models on platforms like Hugging Face and through IBM's watsonx.ai for more enterprise-specific needs.

1. Navigate to your Red Hat OpenShift developer profile window. Click **ConfigMaps** **(A)**, then click **model-params** **(B)**. This opens up the ConfigMap details page.
   ![image](https://github.com/user-attachments/assets/3f611c0a-e4b0-47fc-90e5-3fbedce627f3)
2. In the model-params ConfigMap details page, click **Action** **(A)** and select **Edit ConfigMap** **(B)**.
   ![image](https://github.com/user-attachments/assets/2e7d312c-fb9c-44a4-ae39-9c958b72403e)
3. In the resulting form, edit the key/value fields for MODEL_NAME and MODEL_URL as below and click **Save** **(E)**.
   
     - **Key**: `MODEL_NAME` **(A)**
     - **Value**: `granite-7b-lab.Q4_K_M.gguf` **(B)**
     - ---------
     -  **Key**: `MODEL_URL` **(C)**
     -  **Value**: `https://huggingface.co/RichardErkhov/instructlab_-_granite-7b-lab-gguf/resolve/main/granite-7b-lab.Q4_K_M.gguf` **(D)**

    ![image](https://github.com/user-attachments/assets/9173fc4e-7dde-4afd-917b-aec289c42b5b)

4. The existing pod cannot see the changes right away as changing values of a ConfigMap does not cause a deployment (and hence the pod) to restart. It needs to be done manually.
   
5. Click **Topology** **(A)**, then click "**D lab1-demo**" **(B)** part of the application icon, which opens the deployment details pane (on the right side of the browser window). Click **D lab1-demo** **(C)** in that pane which opens the deployment details view for lab1-demo deployment.
   
    ![image](https://github.com/user-attachments/assets/26ab79ec-d0f2-4d58-a1d3-e744326b4b91)
   
6. Click **Actions** **(A)** and select **Restart rollout** **(B)**. This restarts the deployment which results in the redeployment of the pod.
   
    ![image](https://github.com/user-attachments/assets/b12bf560-6689-4002-8c34-2bb936ef5931)
   
7. Click **Pods** **(A)** tab, where you see a new pod instantiated. The new pod downloads the model and then starts it. Since the configmap points to a Granite model, it is downloaded from HuggingFace and then started. When that happens the new pod's status changes to Running and the existing pod is terminated.
    
    ![image](https://github.com/user-attachments/assets/5d1d9b22-dfcd-4c6a-91cf-fbfc4a8a3384)

    !!! info "Model download takes time"

        This process takes a few minutes (in this case that it took around 3-4 mins as it is a fairly large model compared to tinyllama) and your results might vary. Remember, this is a demo environment and the models are a few GBs in size. Models that are downloaded are not downloaded again if you are using the same storage (PV).
   
8. If the previous step is completed successfully, only one pod is running.
    
    ![image](https://github.com/user-attachments/assets/854703df-2ca4-42ce-825c-30c49a94a050)
   
9. Verify that the model that is running is an IBM Granite model. Click the pod name **(A)** to enter the pod details view/page.
    
    ![image](https://github.com/user-attachments/assets/4c7c0139-3920-413c-bb20-c8ddeeab6fa3)
    
10. In the pod details page, click the **Logs** **(A)** tab to see the pod logs.
    
    ![image](https://github.com/user-attachments/assets/4bec6d4d-8dd3-4b5f-823b-99342ca633a3)
    
11. In the log window, scroll upwards to see the name of the model against the attribute **llm_load_print_meta: general.name**.
    
    ![image](https://github.com/user-attachments/assets/5ca4f0b1-8655-41df-85d2-8d740627989c)

    This verifies that the IBM Granite (granite-7b-lab) model is successfully deployed.   

12. Access the model by locating the external public endpoint, as demonstrated in the previous section of this lab. The beauty of Red Hat OpenShift is that the endpoint remains the same despite the pod being restarted. So, either you can refresh the earlier page (if you have it opened in the browser) or follow the steps below to access the public URL of your application by using the Topology view.

    !!! note "Multiple ways to access the public URL of your application"

        If you are in the Administrator profile, you can navigate to **Networking** -> **Routes** to access your route resource and click the URL to open your application. Alternatively, if you are in the Developer profile, you can go to **Topology** view and access the URL of your application as well, which is the method that is used here:

    - In the Developer profile window, click **Topology** **(A)** and you see the icon representing your deployed application.
      ![image](https://github.com/user-attachments/assets/4dd6037e-1bfd-49b6-b190-f0096aae28b4)
    - Click the arrow (at the upper right of the icon) **(A)** that says "Open URL".
      ![image](https://github.com/user-attachments/assets/5b7ecd28-8491-4b37-b64d-629563c7ae82)
    - A new browser window/tab where you are able to interact with your newly deployed LLM. You see a screen like this:
      ![image](https://github.com/user-attachments/assets/2237409c-7160-471f-aafa-f0e1254c5a53)
    - Scroll all the way down to the input field "Say something..." where you can interact with the LLM. You can ask any question that you like!
      ![image](https://github.com/user-attachments/assets/82196cf5-d4c2-459d-af7e-c24650f1f6ce)

    !!! note "Experimenting with model parameters"

        You can see numerous model parameters or tunables (for example: Predictions, Temperature, and so on). Visit the model-specific documentation and learn about them and experiment with it. You might want to change some parameters, ask the same question, and check how the response changes. These parameters are not covered in this lab as it is outside the scope of the lab.

13. Presented below are the questions and their corresponding responses.

    !!! warning "Accuracy of LLM responses"

        - Large language models (LLMs) are trained on vast amounts of text data, but that training is limited to a **specific cutoff date**. It means that the model can answer questions based on the information available up to that point in time. It cannot access real-time data or understand events, trends, or new information that occurred after the cutoff date. Therefore, the knowledge on which they were trained limits their ability to provide accurate answers.
        - AI-generated content varies and does not always provide consistent answers. Your response might be different.

    ![image](https://github.com/user-attachments/assets/4201cafb-9f11-4f82-945b-1f843713426e)

    ![image](https://github.com/user-attachments/assets/e0a23318-e2bc-4778-8c12-84643a8f530c)

    **Well done!** In this lab, you learned how straightforward it is to transition to a different LLM, access it, and use its capabilities.

    This concludes the lab.
