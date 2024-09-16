# Deploy a Large Language Model (LLM) on IBM Power10

In this lab, we will deploy 2 LLMs from [Hugging Face (HF)](https://huggingface.co/) on IBM Power10 using the on-prem lab environment that has been provisioned.
Goal of this lab is to showcase the basics (and the ease) of deploying a LLM (aka foundation models) on Power and how easy it is to switch to a different LLM.

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

## IBM Watsonx Vs Hugging Face

IBM Watsonx and Hugging Face both offer AI tools, but they serve different purposes:

- **IBM Watsonx**: An enterprise-grade AI platform designed for large-scale AI model training, fine-tuning, and deployment. It focuses on governance, compliance, and custom AI models for business use, offering robust tools for data management and AI governance. Ideal for enterprises needing secure, scalable AI with industry-specific solutions.

- **Hugging Face**: A community-driven platform offering pre-trained models (especially in NLP) and datasets for rapid experimentation and development. It's known for its open-source library and easy access to state-of-the-art models, making it great for research, startups, and developers.

In essence, IBM Watsonx focuses on enterprise-grade AI with strong governance and scalability, while Hugging Face is designed for flexible, community-driven AI development with a focus on rapid prototyping and open access to pre-trained models.
