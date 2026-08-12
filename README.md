A RAG system that generates a response based on the user query by retrieving the embedded chunks of PDF documents that are saved in the FAISS vector store. 

This is entirely created on a local system with 8 GB of RAM and 4 GB of VRAM. It uses open-source models and runs on Ollama. 

## Summary of the approach:

1. Dataset - A set of some available PDFS such as company earnings reports as well as educational e-books.
2. Chunking strategy - Recursive chunking with 500 tokens and an overlap of 100 tokens
3. Embedding model - `BAAI/bge-base-en-v1.5`
4. Vector store - FAISS vector store is used to store the embeddings of the PDF documents and also enables computing similarities between the user query and the embedded documents
5. Reranking cross-encoder model - `BAAI/bge-reranker-base`
6. Retrieval of chunks - Uses a combination of the FAISS cosine search, reranking results, and metadata boost such as the tags, titles, etc. 
7. Expanding the context of the retrieved chunks - In addition to the retrieved chunks, all chunks that belong to the page are also retrieved to expand the context before passing to the model for response generation
8. LLM response - Uses `llama3.2:3b` to generate a response to the user query based on the provided context.

## Evaluation Strategy: 
The eval dataset creation is automated by creating multiple types of questions, such as conceptual, detailed, reasoning, factual, and based on multiple chunks, multiple pages, and a summary of the documents using the LLM.
While the eval set may not be the most accurate, it provides a decent starting point to evaluate the RAG system's efficacy. 

## Eval metrics: 
1. Retrieval: To test the retrieval, nDCG and Recall @k are used
2. Response: The LLM as a judge approach is used. The ground truth and the generated response are compared to evaluate the quality of the response.

## Improvement strategies: 
1. Better chunking strategies: The current approach is simplistic and is prone to missing context between pages of the same document
2. Search: Add hybrid search techniques and keyword matching methods like BM25
3. Reranking: The approach can be evaluated if its actually helping or only adding complexity
4. Eval dataset: Need more high quality questions. Maybe using a better LLM to generate more questions to test the system can be an effective way
5. Eval metrics: The qualitative metric can be diversified to include completeness, faithfulness, etc.

## Possible methodology additions: 
1. Multimodal RAG
2. Agentic RAG
