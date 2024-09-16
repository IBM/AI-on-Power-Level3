# Deploy Retrieval-Augmented Generation (RAG) on IBM Power10 - Lab education

Goal of this lab is to get hands-on experience in deploying a RAG pattern on IBM Power10.
Before we do that, lets understand what RAG is, its advantages and key usecases.

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
