# Introduction

Welcome to the AI on Power - Level 3 course - seller enablement demonstration.
This Level 3 course provides hands-on enablement on how to use generative AI (gen AI) models on Power10 and covers several use cases to help understand the art of the possible.

This course has 4 main parts as you can see on the left window:

* Lab setup - How to provision and setup the lab environment for the hands-on demos.

* Lab 1 – Deploy a large language model (LLM) on Power10 and switch between LLMs.

* Lab 2 – Deploy Retrieval-Augmented Generation (RAG) on Power10 and use it for domain-specific question and answers

* Lab 3 – Deploy code LLM on Power10 - Generate Python, C code, and Sequential Query Language (SQL) query by using natural language.

## Generative AI and LLMs overview

Large language models (LLMs) and gen AI has revolutionized industries by enabling machines to understand, generate, and interact with human language in ways that are never seen before. It has disrupted the market by:

* **Boosting productivity**: LLMs streamline workflows by handling complex tasks, like writing, summarizing, and translating, allowing professionals to focus on more strategic, creative efforts.
  
* **Automating content creation**: From drafting articles, reports, and code to generating creative content like music and artwork, gen AI has drastically reduced time and costs in creative industries.

* **Enhancing customer experience**: LLM-powered chatbots and virtual assistants are offering more personalized, human-like interactions, reshaping customer service and support.

* **Transforming decision-making**: With advanced data processing and natural language understanding, businesses are using AI for smarter, faster insights and predictive analytics. 

In short, LLMs and gen AI are reshaping industries by enhancing efficiency, creativity, and customer engagement, making them indispensable tools in the modern business landscape.

## Why IBM Power for generative AI
IBM Power platform is a high-performance, enterprise-grade computing solution that is designed to handle demanding workloads such as AI, analytics, cloud, and mission-critical applications. With unmatched processing power, scalability, and reliability, IBM Power enables organizations to optimize performance, lower costs, and ensure security.

### Key benefits

* **Unmatched performance**: Optimized for data-intensive applications, AI, and large-scale analytics, delivering faster insights and better decision-making.

* **Scalability**: Seamlessly scale workloads across on-premises, cloud, and hybrid environments to meet evolving business needs.

* **Enterprise-grade security**: Built-in security features provide robust protection for sensitive data and critical operations.

* **Efficiency and flexibility**: Designed for diverse environments, supporting Linux, AIX, IBM i, and containerized applications, making it versatile for a range of industries.

IBM Power is the platform of choice for businesses seeking to future-proof their IT infrastructure, handle complex workloads, and accelerate innovation.

## IBM Power10 differentiation

IBM Power10 offers several advantages for gen AI use cases, providing the computational power, security, and efficiency that is required to support the demanding workloads of AI-driven applications.

### Key advantages

1. **AI acceleration:** IBM Power10 comes with several features that accelerate gen AI workloads by using high memory bandwidth and on-chip acceleration:
   
    * **High-bandwidth data-path:** As data fuels AI, a fast access to big data for training and inferencing is required. Most AI workloads are memory bandwidth bound. IBM Power10's large memory capacity that, in general exceeds limited Graphics Processing Unit (GPU) memory and IBM Power10’s high memory bandwidth are optimal for such scenarios. In addition, the Double Data Rate 5 (DDR5) memory and faster input/output (I/O) bandwidth provide a significant performance boost, enabling large-scale gen AI models to run more efficiently.
      
    * **4 Matrix Math Accelerator (MMA) engines per core:** MMA does matrix math and helps accelerate matrix multiplications, which are required for training, fine-tuning, and inferencing of AI models such as neural networks and foundation models. IBM is seeing strong evidence that it supersedes GPUs and improves consolidation when deploying AI at point of use. 

    * **8 Single Instruction Multiple Data (SIMD) engines per core:** SIMD does vector math. Vectors are used in most AI algorithms. Vector processing can be highly parallelized by using SIMD engines. A single inference request is often “just” a vector that is “send through” the LLM using SIMD acceleration. By batching multiple inference requests together, the workload can become MMA-bound where a whole matrix is "send through" the LLM, leading to improved throughputs.

2. **Optimized AI software:** AI software with IBM Power10 is optimized down to the core.
    *  AI software running on top of IBM Power10 hardware fully leverages the AI acceleration features, without requiring data scientists to alter their code – optimization comes right away.
    * The optimized AI software portfolio spans from enterprise options to supported open source options; even hybrid approaches that mix enterprise and supported open source are possible.
    * This flexibility allows solution architects to adapt the AI software portfolio to the unique requirements of their company. For example, if a company’s data scientists already use a set of open source tools, they can continue to do so while benefitting from all optimizations in IBM Power10 and paving the road to integrations with the enterprise portfolio.

3. **Support for AI ecosystems:** IBM Power10 works seamlessly with AI frameworks like TensorFlow, PyTorch, and ONNX runtime, making it easier for developers to build, train, and deploy gen AI models without needing major infrastructure changes.
    * These AI frameworks leverage some of the popular math libraries like OpenBLAS, libAten, Eigen, and MLAS which provide reusable functions for matrix multiplication.
    * IBM has already integrated Power10's hardware acceleration capabilities into these math libraries, thus allowing AI workloads by using these frameworks to automatically get the IBM Power10 speed-up without any code changes.
    * Support for container orchestration platforms like Red Hat OpenShift and Kubernetes allows efficient orchestration of AI workloads.
      
4. **Supersede GPUs:** Integrating GPU clusters into computing centers is a daunting and complex task. CUDA drivers need to be installed and managed. Hardware faults regularly cause systems to crash. GPUs are expensive and consume lots of energy. Using multiple GPUs and distributed training on GPUs is complicated and buggy. By consolidating on IBM Power10, these are all problems of the past while maintaining low latencies and a high throughput for AI workloads. 

5. **Energy efficiency:** Power10 processors are highly energy-efficient, offering more performance per watt compared to previous generations. This reduces operational costs and is especially beneficial for the resource-intensive training and deployment of gen AI models.

6. **Security:** IBM Power10 comes with end-to-end encryption and advanced memory protection features, ensuring that sensitive AI workloads are secure from potential breaches. This is important in industries like healthcare and finance, where gen AI applications need strong data protection. IBM's support for quantum-safe encryption helps future-proof AI workloads against quantum computing threats.

7. **Flexible cloud and hybrid deployments:** Power10 integrates smoothly with hybrid cloud environments, allowing enterprises to run gen AI workloads both on-premises and in the cloud. This flexibility is key for scaling AI applications while maintaining control over data and infrastructure.

## **Summary**

IBM Power10 is designed to meet the specific needs of gen AI use cases by offering high-performance processing, memory scalability, security, and energy efficiency. It empowers enterprises to handle complex AI workloads efficiently while maintaining flexibility and security, making it ideal for industries that rely on AI-driven innovation.
