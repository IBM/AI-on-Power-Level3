# Query a DB using LLM on IBM Power10 - Lab education

In this lab, we will use a code LLM to convert a natural language query into a SQL statement and use that to query a DB.
But before we do that, lets understand what code LLM is, its benefits, key use cases and IBM products around code LLMs.

## What is code LLM?

A Code LLM (Large Language Model) is a type of AI model specifically trained to understand, generate, and manipulate programming code.
These models, built on architectures like GPT or similar transformers, are trained on vast datasets of code from various programming languages, enabling them to assist with coding tasks such as generating code snippets, debugging, refactoring, and even writing documentation.

## Key Features of Code LLM

  - **Code Generation**: Code LLMs can generate new code based on prompts, such as writing functions, classes, or even entire programs.
  - **Code Completion**: Similar to how text-based LLMs suggest completions for sentences, code LLMs can suggest code completions in real-time, aiding developers in writing code faster.
  - **Bug Detection and Fixes**: These models can detect bugs or potential issues in code and suggest corrections.
  - **Multi-Language Support**: Code LLMs are typically trained on multiple programming languages, allowing them to work across various tech stacks (e.g., Python, Java, JavaScript, C++, etc.).
  - **Code Explanation**: Some models can explain how a piece of code works, making them useful for learning, documentation, and debugging.

## Use Cases

  - **Autocompletion in IDEs**: Code LLMs like GitHub Copilot, powered by OpenAI's Codex, assist developers by offering code suggestions and completions.
  - **Code Refactoring**: LLMs can suggest improvements or optimizations to existing code, helping improve performance or readability.
  - **Debugging and Error Resolution**: Code LLMs can help identify issues in code and suggest potential fixes, streamlining the debugging process.
  - **Automated Documentation**: They can generate comments or documentation for code, reducing the manual effort required for explaining code logic.

## Examples of Code LLMs
  
  Below are few examples of code LLMs:
  
  - **OpenAI Codex**: The LLM that powers GitHub Copilot, trained specifically on a large corpus of programming languages and capable of generating code.
  - **AlphaCode**: DeepMind's LLM designed for competitive programming tasks.
  - **CodeT5**: A transformer model from Hugging Face fine-tuned for code generation and understanding tasks.
  - **Code Llama**: It is a large language model developed by Meta, optimized for coding tasks such as code generation, completion, and debugging, supporting multiple programming languages for enhanced developer productivity.
  - **IBM Granite code models**: A family of open foundation models for code intelligence. It is an enterprise-focused large language model developed by IBM Research, designed to enhance productivity in software development by enabling code generation, debugging, and refactoring with a strong emphasis on security and performance.

## Benefits of code LLMs

  - **Boosts Developer Productivity**: By automating repetitive coding tasks, Code LLMs help developers write code faster and reduce errors.
  - **Assists with Learning**: Code LLMs are valuable for learners by explaining how code works and suggesting how to fix bugs.
  - **Cross-Language Compatibility**: Supporting multiple languages, Code LLMs help developers work across different coding environments without needing expertise in all languages.
    
In essence, Code LLMs bring AI-driven enhancements to the software development process, making it faster, more accurate, and more accessible.

## IBM watsonx Code Assistant

IBM watsonx™ Code Assistant (WCA) is a family of offerings from IBM that leverages generative AI to accelerate development while maintaining the principles of trust, security and compliance at its core. Developers and IT Operators can speed up application modernization efforts and generate automation to rapidly scale IT environments. 

WCA is powered by the IBM Granite foundation models that include state-of-the-art large language models designed for code, geared to help IT teams create high-quality code using AI-generated recommendations based on natural language requests or existing source code. Built on the Watsonx AI platform, this tool leverages IBM’s expertise in AI and machine learning to enhance productivity across various industries.

??? danger "IBM watsonx Code Assistant (WCA) support on IBM Power"

    NOTE: At the time of writing this lab, IBM WCA is not available to run on IBM Power yet. However, clients can use code specific foundation models (either hosted by WCA on IBM Cloud or running stand-alone on-premise on IBM Power) to infuse and harness the power of code LLMs into their on-prem applications running on IBM Power.

### IBM WCA architecture & offerings

Here is a high level architecture of IBM WCA:
![image](https://github.com/user-attachments/assets/71decd7c-fc8e-46a0-8765-ecfc38b897d4)
At the bottom-most layer, WCA is powered by IBM Granite code models, specifically trained and fine-tuned by IBM for different programming languages.

At the time of writing this lab, there are 2 products available under the IBM WCA offering family:
- **IBM watsonx Code Assistant for Red Hat Ansible Lightspeed**: The Ansible-tuned model forms the basis of this product. This is fine-tuned for Ansible use cases. In addition to x86 endpoints, this supports generating Ansible tasks/playbooks for IBM Power endpoints (running AIX, IBM i and Linux) as well.
- **IBM watsonx Code Assistant for Z**: The Cobol to Java-tuned model forms the basis of this product. This is fine-tuned for COBOL to Java conversion. It can help with enterprise use cases around IBM Z application modernization.

More and more programming languages support is being added to WCA over time.

### Key Features of IBM WCA

1. **Code Generation**: Automatically generates code snippets or complete functions based on natural language prompts or specific programming needs.
2. **Debugging Assistance**: Helps identify bugs and suggests fixes, improving the efficiency of the development process.
3. **Multi-Language Support**: Supports a variety of programming languages, making it versatile for different development environments.
4. **Security and Compliance**: Designed with a strong focus on enterprise-level security, ensuring that generated code adheres to industry standards and compliance regulations.
5. **Customization**: Tailors AI recommendations to the specific needs of a project, enabling more accurate code suggestions and personalized developer assistance.

### Benefits of IBM WCA

1. **Increased Developer Productivity**: Automates routine coding tasks, allowing developers to focus on higher-level problem-solving.
2. **Enhanced Code Quality**: Improves the accuracy of code with AI-powered insights, reducing the chances of errors or vulnerabilities.
3. **Enterprise-Grade Security**: Ensures that code generation is secure and compliant with regulations, making it suitable for industries like finance and healthcare.

