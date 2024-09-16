# Deploy Retrieval-Augmented Generation (RAG) on IBM Power10 - Lab education

Goal of this lab is to get hands-on experience in deploying a RAG pattern on IBM Power10.
Before we do that, lets understand what RAG is, its advantages and key usecases.

## What is RAG?

RAG (Retrieval-Augmented Generation) is an advanced technique that combines retrieval-based and generation-based approaches to improve the performance of language models, especially in question-answering and knowledge-intensive tasks.

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

## Advantages of RAG

Below are the key advantages of RAG:
1. **Improved Accuracy**: By retrieving relevant data in real-time, RAG can provide more accurate and up-to-date information.
2. **Factual Consistency**: It reduces the risk of the model "hallucinating" incorrect facts, as it relies on verified external sources.
3. **Scalability**: Can work with very large databases, enabling the model to scale its knowledge without requiring retraining.

## Common usecases of RAG

Below are some of the common usecases for RAG:
1. **Open-domain question answering**: Where models need to answer questions about a wide range of topics, potentially beyond the training data.
2. **Customer support**: Providing accurate answers by retrieving relevant documents from knowledge bases.
3. **Enterprise AI**: Helping businesses with information retrieval, knowledge management, and research by retrieving and summarizing relevant documents.

To summarize, RAG allows models to perform better in knowledge-intensive tasks by combining the strengths of both retrieval systems and generative language models.
