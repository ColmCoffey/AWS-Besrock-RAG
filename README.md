# Building a Retrieval-Augmented Generation (RAG) System with Amazon Bedrock

![rag_system_architecture](https://github.com/user-attachments/assets/c27f927e-434a-41aa-910a-d2c91514f388)


## Executive Summary

This guide provides step-by-step instructions for implementing a Retrieval-Augmented Generation (RAG) system using Amazon Bedrock. It highlights the setup process, configuration of a Knowledge Base, and integration with Amazon's tools for efficient query handling. RAG enables large language models (LLMs) to access real-time, domain-specific, and up-to-date information, bridging the limitations of static training data.

Amazon Bedrock's Knowledge Base feature simplifies embedding management, vector store setup, and document retrieval without requiring extensive coding, making it ideal for scalable, AI-driven applications.

---

## Services and Skills Used

- **Languages and Frameworks:**
  - Python (for API calls and integrations)

- **Cloud Services:**
  - Amazon Bedrock
  - Amazon S3 (for document storage)
  - Amazon OpenSearch Serverless (for vector storage)

- **Deployment Tools:**
  - RetrieveAndGenerate API
  - AWS Management Console

- **Security Features:**
  - AWS Identity and Access Management (IAM) for role-based access control

---

## Prerequisites

1. An AWS account with access to Amazon Bedrock.
2. Documents relevant to your application domain uploaded to an S3 bucket.
3. Familiarity with Python and basic AWS services.

---

## Step-by-Step Guide to Build a RAG System Using Amazon Bedrock

### Step 1: Request Access to Amazon Bedrock

To begin, request access to Amazon Bedrock through your AWS account. This service allows you to use LLMs like Meta's Llama or Amazon's Titan models. If you encounter access delays, creating a temporary EC2 instance can help expedite the process for new accounts.

---

### Step 2: Create a Knowledge Base

#### 1. Upload Documents:
- Store your documents in an S3 bucket. For this guide, imagine uploading diagnostic tool documentation for movement disorders, such as PDFs or clinical research papers.

#### 2. Configure the Knowledge Base:
- Navigate to the Amazon Bedrock console and select **Create Knowledge Base**.
- Provide the path to your S3 bucket and configure the embeddings model. Titan Text Embeddings v2 is an excellent choice for most use cases.

#### 3. Set Up the Vector Store:
- Use Amazon OpenSearch Serverless for scalable, high-speed retrieval.

---

### Step 3: Test the Knowledge Base

Once your Knowledge Base is ready:

1. Use the **RetrieveAndGenerate API** to query the Knowledge Base.
2. The API converts queries into embeddings, searches the vector store, and uses the results to augment LLM-generated responses.

#### Example: Query for Recent Diagnostic Tools
```python
response = bedrock_agent_runtime_client.retrieve_and_generate(
    input={'text': "What are the recent diagnostic tools for movement disorders?"},
    retrieveAndGenerateConfiguration={
        'type': 'KNOWLEDGE_BASE',
        'knowledgeBaseConfiguration': {
            'knowledgeBaseId': 'your-knowledge-base-id',
            'modelArn': 'model-arn'
        }
    },
)
print(response['output']['text'])
