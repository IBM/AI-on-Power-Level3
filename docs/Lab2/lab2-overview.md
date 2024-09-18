# Deploy Retrieval-Augmented Generation (RAG) on IBM Power10 - Lab education

Goal of this lab is to get hands-on experience in deploying a RAG pattern on IBM Power10.
Before we do that, lets understand what RAG is, its advantages, key usecases and how it fits in the IBM Power landscape.

## What is RAG?

RAG (Retrieval-Augmented Generation) is an advanced technique that combines retrieval-based and generation-based approaches to improve the performance of large language models, especially in question-answering and knowledge-intensive tasks. In layman terms, clients can use RAG pattern to generate factually accurate output from LLMs, that is grounded in information in a knowledge base.

Key Components of RAG:

1. **Retrieval**: The model first retrieves relevant documents or information from a large knowledge base (like Wikipedia or custom databases) based on the user's query. This helps provide the model with factual and up-to-date information that it may not have learned during training.
2. **Augmented Generation**: Once the relevant documents are retrieved, the model then generates a response based on the query and the retrieved documents. This ensures the generated answer is both relevant and factual, combining the reasoning power of the language model with the accuracy of external knowledge.

## How RAG works?

RAG is a technique that uses vector databases to retrieve relevant information and improve the accuracy of Large Language Models (LLMs):

1. **Vector database storage**: Text data is converted into vector embeddings using pre-trained models like BERT or GPT. These embeddings are then stored in a vector database.
2. **Query conversion**: When a query is posed to the AI system, it is also converted into a vector
3. **Vector search**: The vector database performs a vector search to find relevant embeddings from the stored dataset
4. **Information retrieval**: The retrieved information is then integrated into the LLM's query input
5. **Response generation**: The augmented query is sent to the LLM to generate an accurate answer.

!!! info "Why use vector database in RAG?"

    Vector databases are used in RAG because they store data in a way that makes it easy to search and retrieve. Vector search techniques go beyond keyword matching and focus on semantic relationships, which improves the quality of the retrieved information.

## Need & Advantages of RAG

The need for Retrieval-Augmented Generation (RAG) arises from the limitations of current large language models (LLMs) and the growing demands for factual accuracy and knowledge scalability in AI applications. 

Here are the key reasons why RAG is necessary and its associated advantages:

1. **Handling Knowledge Gaps**
    - **LLMs are static**: Traditional language models, once trained, cannot access new or external information. They can only generate text based on the data they were trained on, which means they might miss important or up-to-date knowledge.
    - **RAG dynamically retrieves information**: By incorporating a retrieval step, RAG can pull in relevant, up-to-date documents from external sources to complement the model's output, making it more accurate and current.
2. **Reducing Hallucinations**
    - **LLMs sometimes "hallucinate"**: LLMs can generate convincing but incorrect or fabricated answers because they are predicting text based on patterns rather than verifying facts.
    - **RAG grounds responses in real data**: Since RAG retrieves factual documents before generating a response, it ensures that the output is based on real, verifiable information, reducing the risk of false or misleading content.
3. **Scalability in Knowledge**
    - **LLMs are limited by training data**: Even the largest models have limitations on how much they can remember from their training data, which might become outdated or incomplete.
    - **RAG scales with external data**: By leveraging vast external knowledge bases or documents, RAG allows for almost unlimited knowledge expansion without retraining the model. This is particularly useful for enterprises or specific domains where continuous data updates are essential.
4. **Improved Performance in Specific Domains**
    - **Specialized knowledge is often needed**: Many applications require access to niche or domain-specific information, such as legal texts, scientific papers, or proprietary company data.
    - **RAG retrieves domain-specific documents**: The retrieval step allows RAG to pull in domain-specific or proprietary documents, making the output more relevant for specialized tasks.
5. **Efficiency and Adaptability**
    - **Model retraining is costly**: Constantly retraining LLMs with new data is computationally expensive and time-consuming.
    - **RAG avoids retraining**: By using real-time retrieval, RAG can access the latest information or new content without the need to retrain the entire model, making it more adaptable and cost-efficient.
6. **Better Results for Open-Domain Question Answering**
    - **Complex queries require precise answers**: In tasks like open-domain question answering, general models might struggle to provide precise answers for complex or rare questions.
    - **RAG enhances accuracy**: By combining retrieval with generation, RAG can provide more accurate, context-rich answers, drawing from a wide range of documents.

In summary, RAG addresses limitations in current LLMs by improving factual accuracy, scalability, and adaptability, making it particularly useful for knowledge-intensive tasks and dynamic environments.

## Common usecases of RAG

Below are some of the common usecases for RAG:

1. **Open-domain question answering**: Where models need to answer questions about a wide range of topics, potentially beyond the training data.
2. **Customer support**: Providing accurate answers by retrieving relevant documents from knowledge bases.
3. **Enterprise AI**: Helping businesses with information retrieval, knowledge management, and research by retrieving and summarizing relevant documents.

To summarize, RAG allows models to perform better in knowledge-intensive tasks by combining the strengths of both retrieval systems and generative language models.

## IBM Power and RAG

### IBM Power Systems as Systems of Record
IBM Power Systems are renowned for their performance, reliability, and scalability, making them ideal for handling systems of record. A system of record (SOR) refers to a trusted source of truth that stores essential business data and transactions, such as financial systems, customer data, and inventory management. IBM Power Systems are often used for mission-critical applications in industries like banking, healthcare, and government due to their ability to manage large volumes of secure, transactional data.

### How IBM Power Systems Plays Well with RAG
Retrieval-Augmented Generation (RAG) is a generative AI framework that enhances the performance of AI models by retrieving relevant documents from external knowledge bases before generating a response. This retrieval step ensures that the generated output is more accurate and fact-based, as it is grounded in real-time data from a reliable source.

IBM Power Systems play exceptionally well with RAG due to the following reasons:

1. **High-Performance Data Handling**
      - Power Systems are designed for high-volume data transactions and processing. This makes them perfect for storing and managing systems of record, which RAG relies on to retrieve real-time, relevant information.
      - When RAG retrieves data from a system of record stored on IBM Power, it benefits from the fast data access and throughput that Power Systems offer, ensuring quick and efficient retrieval of documents for AI processing.
2. **Data Security and Compliance**
      - Power Systems are known for their robust security features, including end-to-end encryption and advanced data protection, making them ideal for storing sensitive data such as customer information, financial records, or healthcare data.
      - In a RAG scenario, where retrieved data is used to generate answers, the ability of IBM Power Systems to ensure data privacy and regulatory compliance (e.g., HIPAA, GDPR) is crucial, especially for enterprises dealing with sensitive or regulated data.
3. **Scalability and Reliability**
      - RAG applications require scalable infrastructure to handle varying levels of computational demand, especially when dealing with large-scale document retrieval and real-time AI processing. IBM Power Systems are built to scale seamlessly, allowing RAG to handle larger datasets and more complex queries without performance degradation.
      - Reliability is critical for systems of record, and Power Systems have a proven track record of uptime and resilience, ensuring that the data RAG retrieves is always available when needed, without risk of downtime affecting the retrieval process.
4. **Integration with AI Workloads**
      - IBM Power Systems are optimized for AI workloads, with features like accelerated AI processing (e.g., Power10's Matrix Math Accelerator (MMA)) that boost the performance of both retrieval and generation tasks in RAG.
      - By running RAG-based applications on Power10, enterprises can take advantage of faster AI inference and improved data handling, resulting in more responsive and accurate AI systems.
5. **Efficient Handling of Structured and Unstructured Data**
      - Power Systems can efficiently handle both structured data (like databases) and unstructured data (such as documents and records), making them ideal for RAG, where both types of data may be retrieved from systems of record.
      - RAG can retrieve structured data for quick reference (e.g., customer records or transactions) and unstructured data (e.g., reports, emails) for more complex, context-driven AI responses. Power Systems' capability to manage both types ensures efficient, accurate information retrieval.
6. **Real-Time Analytics**
      - Real-time data analytics capabilities in IBM Power Systems enable quick access to up-to-date information, which is essential for RAG when generating responses based on current data.
      - This feature allows the RAG model to provide contextualized answers based on the latest transactions or data updates from the system of record, improving the relevance of AI-driven outputs.
        
**Summary**

IBM Power Systems provide the speed, security, and reliability needed to store and manage systems of record that RAG models depend on for data retrieval. Power Systems’ advanced capabilities in data handling, AI optimization, scalability, and security make them an ideal infrastructure for supporting RAG-based AI applications, ensuring that retrieved data is accurate, current, and secure, thus enhancing the quality of generative AI outputs.

### RAG + IBM Power10 - solution architecture

Here is the high level solution architecture of a typical RAG use case on IBM Power10. This example is deployed on IBM Power10 end-to-end. The foundation model is simply downloaded from watsonx.ai or open-source repositories such as the Hugging Face model hub. The model does not require fine-tuning thanks to a domain adaptation technique called Retrieval Augmented Generation (RAG). 

![image](https://github.com/user-attachments/assets/3476cad1-a743-474f-8535-b70806d8c09f)

1. User asks a domain specific question in natural language.
2. Q&A app looksup in the knowledge base repository.
3. Documents relevant to the question is retrieved from the repository.
4. "Question + Documents" is passed as the context in a prompt to LLM.
5. LLM generates the domain-specific answer.

