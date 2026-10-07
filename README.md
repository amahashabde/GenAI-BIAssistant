# InsightForge: AI Business Intelligence Assistant

InsightForge turns a business spreadsheet into answers. Upload a CSV or Excel file, then ask questions in plain English, get AI-written insights and recommendations, and generate charts, without needing a BI tool or a data analyst.

It's built on a **retrieval-augmented generation (RAG)** pipeline with LangChain, OpenAI embeddings and a FAISS vector database, wrapped in an interactive Gradio app.

## Why

Many small and mid-sized businesses collect lots of data but lack the tools or people to turn it into decisions. InsightForge shows how an LLM, grounded in a company's own data through RAG, can act as a lightweight business analyst.

## Features

| Tab | What it does |
|---|---|
| **1. API Key** | Enter your OpenAI key at runtime (never stored in code) |
| **2. Upload Data** | Load a CSV or Excel file, preview it, and index it for search |
| **3. Data Summary** | Rows, columns, data types, missing values, and descriptive statistics (Pandas) |
| **4. Ask AI** | Ask any question about your data and get a grounded answer through RAG |
| **5. Generate Chart** | Histogram for numeric columns, top-10 bar chart for categories (Matplotlib) |
| **6. Business Insights** | One-click AI analysis: summary, trends, problems, opportunities, recommendations, and suggested charts |

## How it works

```
                 ┌──────────────── Indexing ────────────────┐
 CSV / Excel ──► Pandas DataFrame ──► text ──► chunks ──► OpenAI embeddings ──► FAISS
                                        (1,000 chars, 100 overlap)

                 ┌──────────────── Question answering ──────┐
 User question ──► FAISS retriever (top 4 chunks) ──► prompt + context ──► gpt-4o-mini ──► answer
```

**Grounded answers.** The Ask AI prompt tells the model to use *only* the retrieved business data and not to guess beyond it. Temperature is set to 0 for consistent output. Each answer follows a fixed structure:

1. Direct answer
2. Key insight
3. Reason
4. Business recommendation

**Business Insights** sends a sample of the dataset (first 50 rows) to the LLM and returns a structured report with summary, trends, problems, opportunities, recommendations and suggested charts.

## Tech stack

- **Python**: Pandas, Matplotlib
- **LangChain**: prompt templates, LCEL chains (`prompt | llm | parser`), text splitter
- **OpenAI**: `gpt-4o-mini` for generation, OpenAI embeddings for vector search
- **FAISS**: in-memory vector database
- **Gradio**: multi-tab web interface
- **Google Colab**: runtime

## Getting started

1. Open `BI-Assistant.ipynb` in **Google Colab**.
2. Run all cells. Dependencies install automatically:
   ```
   pandas openpyxl matplotlib gradio langchain langchain-openai langchain-community faiss-cpu
   ```
3. Open the Gradio link that appears at the bottom.
4. Paste your **OpenAI API key** in the *API Key* tab.
5. Upload a CSV or Excel file (sales, customers, inventory, etc.) and explore.

**Example questions**

- "Which region had the highest sales?"
- "What product category is underperforming?"
- "Are there any trends in customer purchases over time?"

## Limitations and next steps

This is a prototype. Ideas for a production version:

- **Smarter indexing:** chunk by row or record instead of raw text, so retrieval keeps table structure intact
- **Exact math:** route numeric questions (totals, averages) to Pandas or SQL instead of the LLM
- **Full-dataset insights:** move beyond the 50-row sample with aggregation before analysis
- **Evaluation:** add a test set of questions with expected answers to measure retrieval and answer accuracy
- **Persistence:** save the FAISS index so files don't need re-embedding each session

## Author

**Ashish Mahashabde**: engineer and AI builder
