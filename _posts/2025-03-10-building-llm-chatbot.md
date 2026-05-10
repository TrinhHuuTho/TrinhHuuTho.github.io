---
layout: post
title: "Building a Production-Ready RAG Chatbot with LangChain & FastAPI"
date: 2025-03-10
category: LLM
tags: [llm, nlp, tutorial]
tech_tags: [LangChain, FastAPI, RAG]
excerpt: "A step-by-step guide to building an enterprise-grade chatbot using Retrieval-Augmented Generation, from document ingestion to deployment."
description: "A step-by-step guide to building an enterprise-grade RAG chatbot with LangChain and FastAPI, from document ingestion to production deployment."
subtitle: "A step-by-step guide to building an enterprise-grade chatbot using Retrieval-Augmented Generation, from document ingestion to deployment."
read_time: 8
emoji: "🤖"
cover_image: "https://images.unsplash.com/photo-1677442135703-1787eea5ce01?w=1200&auto=format&fit=crop&q=80"
---

![Building a RAG Chatbot with LangChain](https://images.unsplash.com/photo-1677442135703-1787eea5ce01?w=1200&auto=format&fit=crop&q=80)
*Production-ready retrieval-augmented generation with LangChain and FastAPI*


Retrieval-Augmented Generation (RAG) has become the go-to architecture for building domain-specific chatbots that need to answer questions based on private knowledge bases. In this article, I'll walk you through building a production-ready RAG system using LangChain, FAISS, and FastAPI.

## Why RAG?

Large Language Models (LLMs) are trained on general internet data. When you need a chatbot to answer questions about your company's internal documentation, product manuals, or proprietary data, the model simply doesn't have that information. You have three main options:

- **Fine-tuning**: Expensive, time-consuming, and requires frequent updates
- **Prompting with full context**: Limited by context window size
- **RAG**: Retrieve relevant chunks dynamically, cost-effective, always up-to-date ✅

## Architecture Overview

Our system has two main phases:

1. **Indexing Pipeline**: Load documents → Chunk → Embed → Store in vector DB
2. **Query Pipeline**: User query → Embed → Retrieve → Augment prompt → LLM → Response

## Setting Up the Environment

```bash
pip install langchain langchain-openai langchain-community faiss-cpu fastapi uvicorn python-dotenv
```

## Step 1: Document Ingestion Pipeline

```python
from langchain.document_loaders import DirectoryLoader, PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_openai import OpenAIEmbeddings
from langchain.vectorstores import FAISS
import os

def build_knowledge_base(docs_path: str, index_path: str = "faiss_index") -> FAISS:
    """Load documents, chunk them, and create a FAISS vector store."""
    # Load all PDFs from directory
    loader = DirectoryLoader(docs_path, glob="**/*.pdf", loader_cls=PyPDFLoader)
    documents = loader.load()
    print(f"Loaded {len(documents)} document pages")

    # Split into chunks
    splitter = RecursiveCharacterTextSplitter(
        chunk_size=1000,
        chunk_overlap=200,
        separators=["\n\n", "\n", ".", " ", ""]
    )
    chunks = splitter.split_documents(documents)
    print(f"Created {len(chunks)} chunks")

    # Create embeddings and vector store
    embeddings = OpenAIEmbeddings(model="text-embedding-3-small")
    vectorstore = FAISS.from_documents(chunks, embeddings)
    vectorstore.save_local(index_path)
    print(f"Vector store saved to {index_path}")
    return vectorstore
```

## Step 2: The RAG Chain

```python
from langchain_openai import ChatOpenAI
from langchain.chains import ConversationalRetrievalChain
from langchain.memory import ConversationBufferWindowMemory

def build_rag_chain(vectorstore: FAISS) -> ConversationalRetrievalChain:
    """Build the RAG chain with conversation memory."""
    llm = ChatOpenAI(
        model="gpt-4-turbo-preview",
        temperature=0.1,
    )

    retriever = vectorstore.as_retriever(
        search_type="mmr",          # Maximal Marginal Relevance
        search_kwargs={"k": 5, "fetch_k": 20}
    )

    memory = ConversationBufferWindowMemory(
        memory_key="chat_history",
        return_messages=True,
        k=5  # Keep last 5 turns
    )

    chain = ConversationalRetrievalChain.from_llm(
        llm=llm,
        retriever=retriever,
        memory=memory,
        return_source_documents=True,
        verbose=False
    )
    return chain
```

## Step 3: FastAPI Backend

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import Optional
import uvicorn

app = FastAPI(title="RAG Chatbot API", version="1.0.0")

# Initialize on startup
vectorstore = None
rag_chain = None

@app.on_event("startup")
async def startup_event():
    global vectorstore, rag_chain
    from langchain_openai import OpenAIEmbeddings
    from langchain.vectorstores import FAISS

    embeddings = OpenAIEmbeddings(model="text-embedding-3-small")
    vectorstore = FAISS.load_local("faiss_index", embeddings)
    rag_chain = build_rag_chain(vectorstore)

class ChatRequest(BaseModel):
    message: str
    session_id: Optional[str] = "default"

class ChatResponse(BaseModel):
    answer: str
    sources: list[str]

@app.post("/chat", response_model=ChatResponse)
async def chat(request: ChatRequest):
    try:
        result = rag_chain.invoke({"question": request.message})
        sources = [
            doc.metadata.get("source", "Unknown")
            for doc in result.get("source_documents", [])
        ]
        return ChatResponse(
            answer=result["answer"],
            sources=list(set(sources))  # Deduplicate
        )
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))

if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

## Step 4: Dockerizing the Application

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

## Production Considerations

> "A demo is not a product. The distance from 80% to 99% reliable is greater than from 0% to 80%."

Key things to address before going to production:

- **Streaming responses**: Use `astream` for better UX
- **Rate limiting**: Protect your OpenAI API budget
- **Caching**: Cache embeddings and frequent queries with Redis
- **Evaluation**: Use RAGAS to measure faithfulness, answer relevancy, and context recall
- **Monitoring**: Log all queries, latencies, and token usage to detect issues

## Results

After deploying this system for a customer support use case, we saw a **40% reduction** in support tickets and a customer satisfaction score increase from 3.2 to 4.1/5. The average response latency is under 2 seconds for 95% of queries.

## Conclusion

RAG is a powerful pattern that combines the knowledge retrieval capabilities of search systems with the language understanding of LLMs. The setup I've described here is production-grade but still simple enough to adapt quickly.

The full code is available on [GitHub](https://github.com/TrinhHuuTho/rag-chatbot). Feel free to open issues or PRs!
