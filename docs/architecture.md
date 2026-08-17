# Architecture

## Ingestion and indexing

```mermaid
flowchart LR
    A[PDF / TXT / MD] --> B[loaders.py]
    B --> C[LangChain Document]
    C --> D[RecursiveCharacterTextSplitter]
    D --> E[Chunks]
    E --> F[Ollama nomic-embed-text]
    F --> G[(SingleStore Vector DB)]
```

## Retrieval and generation

```mermaid
flowchart LR
    A[User Question] --> B[SingleStore Retriever]
    B --> C[Top-K Relevant Chunks]
    C --> D[Context]
    D --> E[ChatPromptTemplate]
    E --> F[Ollama LLM]
    F --> G[Streaming Answer]
    C --> H[Source Snippets]
```

## Module responsibilities

- `app.py` — UI, indexing orchestration, retrieval, prompting and streaming response.
- `loaders.py` — converts uploaded files into LangChain `Document` objects.
- `rag_singlestore.py` — chunking, embeddings, SingleStore vector store and source formatting.
- `prompts.py` — grounding instructions used by the generation step.
