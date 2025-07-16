
# Project Overview: Hybrid Search over GitHub Repositories

This project implements a hybrid retrieval pipeline for exploring and ranking GitHub repositories using both semantic and keyword-based search methods. It combines dense embeddings, lexical scoring (BM25), cross-encoder reranking, and LLM-based evaluation.

## 1. Data Structure
description: Treated as free text for semantic search using embeddings.

topics: Used as structured keyword fields for lexical (BM25) search.

## 2. Semantic Search
Model: BGE-M3 (from sentence-transformers) – chosen for its strong performance on technical texts.

Chunking: Descriptions are split into 500-token chunks with overlapping context (X tokens overlap).

Embedding and Similarity:

User queries are embedded.

Similarity is computed via cosine similarity between the query embedding and the document chunks.

## 3. Keyword-Based Search (BM25)
Lexical Matching over the topics field.

Constructed an inverted index to match user query terms with repository topics.

Implemented using ChromaDB with LangChain, even though Chroma is typically used for dense search.

## 4. Hybrid Retrieval
Combined results from:

Semantic search (based on cosine similarity)

Lexical search (BM25)

Merging strategy:

Union based on scores or document IDs.

Aimed to improve recall and precision over single-method approaches.

## 5. Reranking with CrossEncoder
Model: cross-encoder/ms-marco-MiniLM-L6-en-de-v1 (based on BERT).

Input: Pairs of (user query, candidate document).

Output: Relevance score in the range [0, 1].

Final ranking is determined based on these scores.

## 6. LLM-as-a-Judge Evaluation
Used a language model (GPT) to automatically evaluate the quality of the retrieved answers.

Each answer is scored along three dimensions:

Relevance: How well the answer matches the query.

Fluency: Coherence and clarity of the text.

Faithfulness: Whether the answer stays true to the original document content.

