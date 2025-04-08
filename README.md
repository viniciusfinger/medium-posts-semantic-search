# Semantic Search on Medium Posts 🔍🧠

Run a **semantic search** powered by **embedding vectors** and **Pinecone** on Medium post titles and subtitles with precision, returning the most relevant results for your query.

## Features

- **Embeddings with Transformers**: Generates vector representations using `all-MiniLM-L6-v2` sentence-transformer.
- **Indexing with Pinecone**: Stores vectors in Pinecone for lightning-fast real-time vector search.
- **Data Preprocessing**: Cleans up incomplete or truncated data to improve search quality.
- **Natural Querying**: Search using natural language and retrieve the most semantically relevant titles.

## Prerequisites

Before running the application, make sure you have the following installed/pre-configured:

- Python 🐍
- [Pinecone](https://www.pinecone.io/)
- `sentence-transformers` library

## How It Works

1. **Load the Data**  
   Load 10,000 Medium post titles from a CSV file using `pandas`.

2. **Clean & Prepare**  
   - Remove rows with missing values or truncated subtitles.  
   - Combine title and subtitle into a single extended string.

3. **Generate Embeddings**  
   Use the popular `all-MiniLM-L6-v2` model from `sentence-transformers` to convert text into 384-dimensional vectors.

4. **Initialize Pinecone**
   Create an index if it doesn't exist, with cosine similarity as the distance metric.

5. **Upsert Data**
   Insert the embeddings into Pinecone, along with metadata including the title, subtitle, and category.

6. **Query the Index**
   Search with a natural language question (e.g., “What is the best way to learn Machine Learning?”) and retrieve the top 10 most relevant posts.

## Example Query Output

```python
results = index.query(
    top_k=10,
    include_metadata=True,
    vector=model.encode("What is the best way to learn Machine Learning?").tolist()
)

for result in results["matches"]:
    print(f"{round(result['score'], 4)}: {result['metadata']['title']}")
```

## Datasource of Medium Posts

Kaggle: https://www.kaggle.com/datasets/nulldata/medium-post-titles