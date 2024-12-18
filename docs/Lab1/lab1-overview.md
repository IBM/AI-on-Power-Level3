# Deploy a large language model (LLM) on IBM Power10 - Lab Education

The goal of this lab is to showcase the basics (and the ease) of deploying an LLM (also known as foundation models) on Power and how easy it is to switch to a different LLM.
Before proceeding, it is important to understand the ecosystem around LLMs (also known as foundation models or generative AI) in the context of IBM Power.

This lab involves deploying two LLMs from [Hugging Face (HF)](https://huggingface.co/){target="_blank"} on IBM Power10 within the provisioned on-premises lab environment.

## What is Hugging Face?

Hugging Face is an AI company that built an incredibly popular AI community around open source libraries, models, and datasets.
It's an open source community with a large collection of AI models and datasets that are available for sharing and collaboration.
The Hugging Face hub now hosts more than 950 K models and over 210 K datasets, and those numbers are growing quickly.

!!! note "What is Hugging Face?"

    Hugging Face is an AI company that serves as a major hub for Natural Language Processing (NLP) and machine learning (ML) tools. It is widely known for its open source library, Transformers, and its collaborative platform, which provides a vast collection of pre-trained models and datasets. Hugging Face simplifies access, fine-tuning, and deployment of state-of-the-art machine learning models for developers, researchers, and businesses, particularly for NLP tasks such as text classification, translation, summarization, and more.

HF is used to access LLMs in this lab due to its extensive collection of publicly hosted pre-trained models that are available for free.

## What is IBM watsonx?

Enterprise clients of IBM use [IBM watsonx platform](https://www.ibm.com/watsonx){target="_blank"}, an enterprise-ready AI, and data platform.
IBM watsonx is designed for the enterprise and is targeted for business domains, empowering value creators to transform business applications into AI-first applications.

!!! note "What is IBM watsonx?"

    IBM watsonx is IBM’s next-generation AI and data platform, which is designed to help enterprises build, train, fine-tune, and deploy large-scale AI models efficiently. IBM watsonx enables businesses to harness the power of artificial intelligence and machine learning (AI/ML) for various tasks like generative AI, predictive analytics, and decision-making. It integrates advanced capabilities for handling large language models (LLMs), machine learning workflows, and AI governance.

!!! danger "IBM watsonx support on IBM Power"

    **NOTE**: At the time of writing, IBM watsonx is not yet available to run on IBM Power Systems. However, the LLMs that support its functions are fully compatible with IBM Power. Clients can leverage foundation models, including IBM-developed and open source options, to integrate and use (gen)AI capabilities within their on-premises applications running on IBM Power. This course's labs demonstrate the use of such LLMs.

## IBM watsonx Vs Hugging Face

IBM watsonx and Hugging Face both offer AI tools, but they serve different purposes:

- **IBM watsonx**: An enterprise-grade AI platform designed for large-scale AI model training, fine-tuning, and deployment. It focuses on governance, compliance, and custom AI models for business use, offering robust tools for data management and AI governance. Ideal for enterprises needing secure, scalable AI with industry-specific solutions.

- **Hugging Face**: A community-driven platform offering pre-trained models (especially in NLP) and datasets for rapid experimentation and development. It is known for its open source library and easy access to state-of-the-art models, making it great for research, startups, and developers.

In essence, IBM watsonx focuses on enterprise-grade AI with strong governance and scalability, while Hugging Face is designed for flexible, community-driven AI development with a focus on rapid prototyping and open access to pre-trained models.

## IBM watsonx and Hugging Face

With IBM watsonx, clients can run not only IBM-trained foundation models, but also open source models and models from Hugging Face.

IBM watsonx and Hugging Face can work together by combining IBM's enterprise AI capabilities with Hugging Face's vast collection of pre-trained models and tools for rapid AI development:

- **Model Access**: IBM watsonx users can leverage Hugging Face's pre-trained models from its model hub to fine-tune or deploy in enterprise environments by using watsonx’s scalable infrastructure.
- **Fine-Tuning and Customization**: Businesses can use Hugging Face models in watsonx to fine-tune them with proprietary data while benefiting from watsonx’s AI governance and compliance tools.
- **Deployment**: Hugging Face models can be integrated into IBM watsonx to deploy at scale on hybrid cloud or on-premises environments, ensuring enterprise-level security and performance.

Together, Hugging Face provides the models, and watsonx offers the enterprise-ready infrastructure for secure, large-scale, compliant deployment.
Read [this blog](https://developer.ibm.com/blogs/awb-hugging-face-and-ibm-working-together-in-open-source/){target="_blank"} to understand more about how IBM and Hugging Face are working to bring open source communities together for enterprise AI.

## AI and watsonx with IBM Power

This 1-slide overview highlights the capabilities of AI and watsonx on IBM Power available today.
![image](https://github.com/user-attachments/assets/f3e6a66d-e418-4e3c-8315-08e125ad8149)

Clients can get started with AI and watsonx with IBM Power today. 
IBM simplifies the process by aligning common use cases with four key areas that are observed in pilots and client activations.

- **Pattern 1**: Securely tune, deploy, and manage foundation models. When it comes to task-specific use cases, it is a good idea to work with large open source models in your own workspace that are under your control. Hugging Face is a large repository with over 950 K pre-trained ML models and a platform where the AI ecosystem collaborates on models, datasets, and applications. Download any model from Hugging Face and securely deploy at scale on IBM Power. Then, use high-quality software to help you tune, deploy, and manage as many models as you need. Some examples of what enterprises can use this capability for: Customer service, knowledge workers to augment staff and fraud reporting.
 
- **Pattern 2**: There are many new and existing business apps that are using foundation models that are integrated into the workflows. Deploy your foundation models anywhere, on Power, x86, cloud, and use the watsonx.ai software development kit (SDK) available in Python and embed directly into applications. On Power, enterprises can do this quickly so that services can be released to clients faster on a resilient 24/7 environment. Some examples of clients can embed AI into apps are generating the first draft of reports, citizen services for government end-clients and knowledge management.
 
- **Pattern 3**: The ecosystem is important to enterprises that have long-standing investments in software that drives their core business workflows. Consume watsonx services from clients customized ecosystem apps. Enterprises can generate code for Ansible playbooks for IBM i or AIX to enhance the Ansible IT management experience. Also, SAP applications can be embedded with watsonx services within the SAP ABAP (Advanced Business Application Programming) environments. These custom apps enable faster service delivery, leverage existing investments, and offer an appealing solution for many. Some example use cases include asset management, code generation, accounting automation. 
 
- **Pattern 4**: A comprehensive suite of AI capabilities is being introduced to the Power platform, enabling clients to train, tune, and deploy models without the need for purchasing GPUs. The lead time for GPUs is somewhere around a year and clients can miss an opportunity if they can’t get started today. Some of the popular use cases that enterprises use are fraud detection, risk underwriting, and demand forecasting.

## Choosing a foundation model

There are many factors to consider when you choose a foundation model to use for inferencing from a generative AI project.
Determine which factors are most important for you and your organization.

- Tasks the model can do
- Languages supported
- Tuning options for customizing the model
- License and IP indemnity terms
- Model attributes, such as size, architecture, and context window length

After you have a short list of models that best fit your needs, you can test the models to see which ones consistently return the results you want.

For more details on choosing a foundation model that supports your use case / language / other factors, refer to [this document](https://www.ibm.com/docs/en/watsonx/saas?topic=models-choosing-model){target="_blank"}. Although this document is part of the watsonx.ai product documentation, the information that is provided includes both IBM-trained models and open source models as watsonx.ai support both types of models.
