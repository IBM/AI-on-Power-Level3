# Introduction

Welcome to the AI on Power - Level 3 course - seller enablement demonstration.
This Level 3 course will provide hands-on enablement on how to use generative AI (genAI) models on Power10 and will cover few use-cases to help understand the art of the possible.

There are 4 main parts to this demonstration as you can see on the left panel:

* Lab setup - How to provision and setup the lab env for the hands-on demos

* Lab 1 – Deploy a Large Language Model (LLM) on Power10 - Deploy a LLM on Power10 and switch to a different LLM

* Lab 2 – Deploy Retrieval-Augmented Generation (RAG) on Power10 - RAG chatbot for domain specific question and answers

* Lab 3 – Deploy code LLM Power10 - Generate python, C code and Sequential Query Language (SQL) query using natural language

## Generative AI and LLMs overview

Large language models (LLMs) and generative AI (genAI) have revolutionized industries by enabling machines to understand, generate, and interact with human language in ways never seen before. They have disrupted the market by:

* **Boosting productivity**: LLMs streamline workflows by handling complex tasks, like writing, summarizing, and translating, allowing professionals to focus on more strategic, creative efforts.
  
* **Automating content creation**: From drafting articles, reports, and code to generating creative content like music and artwork, genAI has drastically reduced time and costs in creative industries.

* **Enhancing customer experience**: LLM-powered chatbots and virtual assistants are offering more personalized, human-like interactions, reshaping customer service and support.

* **Transforming decision-making**: With advanced data processing and natural language understanding, businesses are leveraging AI for smarter, faster insights and predictive analytics. 

In short, LLMs and genAI are reshaping industries by enhancing efficiency, creativity, and customer engagement, making them indispensable tools in the modern business landscape.

## Why IBM Power for genAI
IBM Power platform is a high-performance, enterprise-grade computing solution designed to handle demanding workloads such as AI, analytics, cloud, and mission-critical applications. With unmatched processing power, scalability, and reliability, IBM Power enables organizations to optimize performance, lower costs, and ensure security.

### Key benefits

* **Unmatched performance**: Optimized for data-intensive applications, AI, and large-scale analytics, delivering faster insights and better decision-making.

* **Scalability**: Seamlessly scale workloads across on-prem, cloud, and hybrid environments to meet evolving business needs.

* **Enterprise-grade security**: Built-in security features provide robust protection for sensitive data and critical operations.

* **Efficiency and flexibility**: Designed for diverse environments, supporting Linux, AIX, IBM i, and containerized applications, making it versatile for a range of industries.

IBM Power is the platform of choice for businesses seeking to future-proof their IT infrastructure, handle complex workloads, and accelerate innovation

## IBM Power10 differentiation

IBM Power10 offers several advantages for generative AI (genAI) use cases, providing the computational power, security, and efficiency required to support the demanding workloads of AI-driven applications.

### Key advantages

1. **AI acceleration:** IBM Power10 comes with several features that accelerate genAI workloads using high memory bandwith and on-chip acceleration:
   
    * **High bandwidth data-path:** As data fuels AI, a fast access to big data for training and inferencing is required. Most AI workloads are memory bandwith bound. IBM Power10's large memory capacity that, in general exceeds limited Graphics Processing Unit (GPU) memory and IBM Power10’s high memory bandwidth are optimal for such scenarios. In addition, the Double Data Rate 5 (DDR5) memory and faster Input/Output (I/O) bandwidth provide a significant performance boost, enabling large-scale genAI models to run more efficiently.
      
    * **4 Matrix Math Accelerator (MMA) engines per core:** MMA does matrix math and helps accelerate matrix multiplications, which are required for training, fine-tuning, and inferencing of AI models such as Neural Networks and foundation models. IBM is seeing strong evidence that it supersedes GPUs and improves consolidation when deploying AI at point of use. 

    * **8 Single Instruction Multiple Data (SIMD) engines per core:** SIMD does vector math. Vectors are used in most AI algorithms. Vector processing can be highly parallelized using SIMD engines. A single inference request is often “just” a vector that is “send through” the LLM using SIMD acceleration. By batching multiple inference requests together, the workload can become MMA-bound where a whole matrix is "send through" the LLM, leading to improved throughputs.

2. **Optimized AI sofware:** AI software with IBM Power10 is optimized down to the core.
    *  AI software running on top of IBM Power10 hardware fully leverages the above mentioned acceleration features, without requiring data scientists to alter their code – optimization comes out-of-the-box!
    * The optimized AI software portfolio spans from enterprise options to supported open-source options; even hybrid approaches that mix enterprise and supported open-source are possible.
    * This flexibility allows solution architects to adapt the AI software portfolio to the unique requirements of their company. For example, if a company’s data scientists already use a set of open-source tools, they can continue to do so while benefitting from all optimizations in IBM Power10 and paving the road to integrations with the enterprise portfolio.

3. **Support for AI ecosystems:** IBM Power10 works seamlessly with AI frameworks like TensorFlow, PyTorch and ONNX runtime, making it easier for developers to build, train, and deploy genAI models without needing major infrastructure changes.
    * These AI frameworks leverage some of the popular math libraries like OpenBLAS, libAten, Eigen and MLAS which provide reusable function for matrix multiplication.
    * IBM has already integrated Power10's hardware acceleration capabilities into these math libraries, thus allowing AI workloads using these frameworks to automatically get the IBM Power10 speed-up without any code changes.
    * Support for container orchestration platforms like OpenShift and Kubernetes allows efficient orchestration of AI workloads.
      
4. **Supersede GPUs:** Integrating GPU clusters into computing centers is a daunting and complex task. CUDA drivers need to be installed and managed. Hardware faults regularly cause systems to crash. GPUs are expensive and consume lots of energy. Using multiple GPUs and distributed training on GPUs is complicated and buggy. By consolidating on IBM Power10, these are all problems of the past while maintaining low latencies and a high throughput for AI workloads. 

5. **Energy efficiency:** Power10 processors are highly energy-efficient, offering more performance per watt compared to previous generations. This reduces operational costs and is especially beneficial for the resource-intensive training and deployment of genAI models.

6. **Security:** IBM Power10 comes with end-to-end encryption and advanced memory protection features, ensuring that sensitive AI workloads are secure from potential breaches. This is particularly important in industries like healthcare and finance, where genAI applications need strong data protection. IBM's support for quantum-safe encryption helps future-proof AI workloads against quantum computing threats.

7. **Flexible cloud and hybrid deployments:** Power10 integrates smoothly with hybrid cloud environments, allowing enterprises to run genAI workloads both on-premises and in the cloud. This flexibility is key for scaling AI applications while maintaining control over data and infrastructure.

## **Summary**

IBM Power10 is designed to meet the specific needs of genAI use cases by offering high-performance processing, memory scalability, security, and energy efficiency. It empowers enterprises to handle complex AI workloads efficiently while maintaining flexibility and security, making it ideal for industries that rely on AI-driven innovation.
