# Deploy a Large Language Model (LLM) on IBM Power10

In this lab, we will deploy 2 LLMs from [Hugging Face (HF)](https://huggingface.co/) on IBM Power10 using the on-prem lab environment that has been provisioned.
Goal of this lab is to showcase the basics (and the ease) of deploying a LLM (aka foundation models) on Power and how easy it is to switch to a different LLM.

!!! note "What is Hugging Face?"

    Hugging Face is an AI company that has become a major hub for Natural Language Processing (NLP) and Machine Learning (ML) tools. It is widely known for its open-source library, Transformers, and its collaborative platform, which provides a vast collection of pre-trained models and datasets. Hugging Face has made it easy for developers, researchers, and businesses to access, fine-tune, and deploy state-of-the-art machine learning models, especially those related to NLP tasks like text classification, translation, summarization, and more.

We use HF to access LLMs in this lab as it has a vast collection of pre-trained models which are hosted publicly and can be accessed for free.

Enterprise clients of IBM will use the [IBM watsonx platform](https://www.ibm.com/watsonx), an enterprise-ready AI and data platform. IBM watsonx is a robust, enterprise-grade platform tailored for large-scale AI deployments with a focus on governance, security, and industry-specific solutions. It's ideal for businesses that require custom AI model training, hybrid cloud integration, and responsible AI management.

!!! note "What is IBM watsonx?"

    IBM Watsonx is IBM’s next-generation AI and data platform, designed to help enterprises build, train, fine-tune, and deploy large-scale AI models efficiently. Watsonx enables businesses to harness the power of artificial intelligence and machine learning (AI/ML) for various tasks like generative AI, predictive analytics, and decision-making. It integrates advanced capabilities for handling large language models (LLMs), machine learning workflows, and AI governance.

!!! danger "IBM watsonx support on IBM Power"

    **NOTE**: At the time of writing this lab, IBM watsonx is not available to run on IBM Power yet. However, clients can use foundation models (either hosted by watzonx on IBM Cloud or running stand-alone on-premise on IBM Power) to infuse AI into their applications.

Here is a quick 
