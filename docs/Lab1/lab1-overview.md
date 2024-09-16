# Deploy a Large Language Model (LLM) on IBM Power10 - Lab Education

Goal of this lab is to showcase the basics (and the ease) of deploying a LLM (aka foundation models) on Power and how easy it is to switch to a different LLM.
But before we do that, let's understand the ecosystem around LLMs (aka foundation models or Generative AI) in the context of IBM Power.

In this lab, we will deploy 2 LLMs from [Hugging Face (HF)](https://huggingface.co/) on IBM Power10 using the on-prem lab environment that has been provisioned.

## What is Hugging Face?

Hugging Face is an AI company that built an incredibly popular AI community around open source libraries, models, and data sets.
It's an open source community with a large collection of AI models and datasets that are available for sharing and collaboration.
The Hugging Face hub now hosts more than 950K models and over 210K data sets, and those numbers are growing quickly.

!!! note "What is Hugging Face?"

    Hugging Face is an AI company that has become a major hub for Natural Language Processing (NLP) and Machine Learning (ML) tools. It is widely known for its open-source library, Transformers, and its collaborative platform, which provides a vast collection of pre-trained models and datasets. Hugging Face has made it easy for developers, researchers, and businesses to access, fine-tune, and deploy state-of-the-art machine learning models, especially those related to NLP tasks like text classification, translation, summarization, and more.

We use HF to access LLMs in this lab as it has a vast collection of pre-trained models which are hosted publicly and can be accessed for free.

## What is IBM watsonx?

Enterprise clients of IBM will use the [IBM watsonx platform](https://www.ibm.com/watsonx), an enterprise-ready AI and data platform.
Watsonx is designed for the enterprise and is targeted for business domains, empowering value creators to transform business applications into AI-first applications.

!!! note "What is IBM watsonx?"

    IBM Watsonx is IBM’s next-generation AI and data platform, designed to help enterprises build, train, fine-tune, and deploy large-scale AI models efficiently. Watsonx enables businesses to harness the power of artificial intelligence and machine learning (AI/ML) for various tasks like generative AI, predictive analytics, and decision-making. It integrates advanced capabilities for handling large language models (LLMs), machine learning workflows, and AI governance.

!!! danger "IBM watsonx support on IBM Power"

    **NOTE**: At the time of writing this lab, IBM watsonx is not available to run on IBM Power yet. However, clients can use foundation models (either hosted by watsonx on IBM Cloud or running stand-alone on-premise on IBM Power) to infuse and harness the power of (Gen)AI into their on-prem applications running on IBM Power.

## IBM watsonx Vs Hugging Face

IBM Watsonx and Hugging Face both offer AI tools, but they serve different purposes:

- **IBM watsonx**: An enterprise-grade AI platform designed for large-scale AI model training, fine-tuning, and deployment. It focuses on governance, compliance, and custom AI models for business use, offering robust tools for data management and AI governance. Ideal for enterprises needing secure, scalable AI with industry-specific solutions.

- **Hugging Face**: A community-driven platform offering pre-trained models (especially in NLP) and datasets for rapid experimentation and development. It's known for its open-source library and easy access to state-of-the-art models, making it great for research, startups, and developers.

In essence, IBM watsonx focuses on enterprise-grade AI with strong governance and scalability, while Hugging Face is designed for flexible, community-driven AI development with a focus on rapid prototyping and open access to pre-trained models.

## IBM watsonx & Hugging Face

With IBM watsonx, clients can run not just IBM-trained foundation models, but also open source models and models from Hugging Face as well!

IBM watsonx and Hugging Face can work together by combining IBM's enterprise AI capabilities with Hugging Face's vast collection of pre-trained models and tools for rapid AI development:

- **Model Access**: IBM watsonx users can leverage Hugging Face's pre-trained models (like GPT, BERT, etc.) from its model hub to fine-tune or deploy in enterprise environments using Watsonx’s scalable infrastructure.
- **Fine-Tuning and Customization**: Businesses can use Hugging Face models in Watsonx to fine-tune them with proprietary data while benefiting from watsonx’s AI governance and compliance tools.
- **Deployment**: Hugging Face models can be integrated into IBM watsonx to deploy at scale on hybrid cloud or on-premise environments, ensuring enterprise-level security and performance.

Together, Hugging Face provides the models, and watsonx offers the enterprise-ready infrastructure for secure, large-scale, compliant deployment.
Read [this blog](https://developer.ibm.com/blogs/awb-hugging-face-and-ibm-working-together-in-open-source/) to understand more about how IBM and Hugging Face are working to bring open source communities together for enterprise AI.

## AI and watsonx with IBM Power

Here is a quick 1-slider on what you can do with AI and watsonx on IBM Power, today.
![image](https://github.com/user-attachments/assets/f3e6a66d-e418-4e3c-8315-08e125ad8149)

Clients can get started with AI and watsonx with IBM Power today. 
IBM has made it simple by aligning the common use cases around 4 key areas that we see within our pilots and client activations.

- **Pattern 1**: Securely tune, deploy and manage foundation models. When it comes to task-specific use cases, it is a good idea to work with large open-source models in your own workspace that are under your control. Hugging Face is a large repository with over 950K pre-trained ML models and a platform where the AI ecosystem collaborates on models, datasets and applications. Download any model from Hugging Face and securely deploy at scale on IBM Power. Then, use best-of-breed software to help you tune, deploy and manage as many models as you need. Some examples of what enterprises can use this capability for: Customer service, knowledge workers to augment staff and fraud reporting.
 
- **Pattern 2**: There are many new and existing business apps that are using foundation models integrated into the workflows. Deploy your foundation models anywhere, on Power, x86, cloud, and use the watsonx.ai software development kit (SDK) available in Python and embed directly into applications. On Power, enterprises can do this quickly so that services can be released to customers faster on a resilient 24/7 environment. Some examples of clients can embed AI into apps are generating the first draft of reports, citizen services for government end-clients and knowledge management.
 
- **Pattern 3**: The ecosystem is important to enterprises that have long-standing investments in software that drives their core business workflows. Consume watsonx services from customers customized ecosystem apps. Enterprises can generate code for Ansible playbooks for IBM i or AIX to enhance the Ansible IT management experience. Additionally, SAP applications can be embedded with watsonx services within the SAP ABAP environments. These custom apps help clients deliver services much faster while taking advantage of existing and familiar investments, making this an attractive proposition for many. Some example use cases include asset management, code generation, accounting automation. 
 
- **Pattern 4**: Lastly, we are bringing a full suite of AI capabilities to the Power platform to help clients train, tune and deploy models without purchasing GPUs. The lead time for GPUs is somewhere around a year and clients will miss out on opportunity if they can’t get started today. Some of the popular use cases that enterprises will use are fraud detection, risk underwriting, and demand forecasting.



