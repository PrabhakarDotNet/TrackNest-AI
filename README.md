# TrackNest-AI

An AI-powered microservice built with Python FastAPI, integrated with TrackNest API to provide intelligent expense management through natural language queries, semantic search, and RAG (Retrieval-Augmented Generation).

---

## Features

### AI Chat Assistant
* Natural language expense queries ("Show my food expenses last month")
* Smart query routing — aggregation queries routed to SQL, semantic queries to ChromaDB
* Confirmation flow before executing add/delete actions
* "List all" keyword bypass for direct data retrieval
* Expense context injection for accurate AI responses

### RAG (Retrieval-Augmented Generation)
* ChromaDB as vector store for semantic expense search
* LangChain for RAG pipeline orchestration
* Groq LLaMA 3.3 70B as the AI language model
* `/ingest` endpoint to sync expenses into ChromaDB
* k=5 semantic retrieval for relevant expense context

### Query Routing
* Aggregation queries (total, count, sum) → directly handled via SQL
* Semantic queries (find, show, search) → ChromaDB vector search → LLaMA response
* Keyword-based bypass for broad queries ("list all expenses")

### Cloud & DevOps
* Deployed on Azure App Service
* Environment-based configuration (no credentials in code)
* GitHub Actions CI/CD pipeline (auto-deploy on push)
* CORS configured for Angular frontend integration

---

## Technology Stack

* Python 3.11
* FastAPI
* LangChain
* ChromaDB
* Groq LLaMA 3.3 70B
* Azure App Service
* GitHub Actions
* Uvicorn
* Python-dotenv

---

## Project Structure

```text
TrackNest-AI
│
├── main.py                  ← FastAPI app, /chat and /ingest endpoints
├── requirements.txt         ← Python dependencies
├── .env                     ← Local environment variables (gitignored)
├── .gitignore
├── chroma_db/               ← ChromaDB vector store (gitignored)
└── .github/
    └── workflows/
        └── deploy.yml       ← GitHub Actions CI/CD
```

---

## API Endpoints

| Method | Endpoint  | Description                                      |
| ------ | --------- | ------------------------------------------------ |
| POST   | /chat     | Natural language expense query via RAG           |
| POST   | /ingest   | Sync expenses from TrackNest API into ChromaDB   |

### /chat Request Body
```json
{
  "message": "Show my food expenses last month",
  "expenses": [
    {
      "id": 1,
      "amount": 500,
      "category": "Food",
      "description": "Lunch",
      "expenseDate": "2026-06-01"
    }
  ]
}
```

### /ingest Request Body
```json
{
  "expenses": [
    {
      "id": 1,
      "amount": 500,
      "category": "Food",
      "description": "Lunch",
      "expenseDate": "2026-06-01"
    }
  ]
}
```

---

## AI Chat Flow

1. Angular sends user query + expense context to `/chat`
2. FastAPI checks query type:
   - **Aggregation** (total, count, sum) → SQL via TrackNest API
   - **Semantic** → ChromaDB vector search → top 5 relevant expenses retrieved
3. Retrieved context passed to Groq LLaMA 3.3 70B
4. AI generates natural language response
5. Response returned to Angular chat widget
6. For add/delete actions → confirmation card shown before execution

---

## Environment Variables

| Variable               | Description                        |
| ---------------------- | ---------------------------------- |
| GROQ_API_KEY           | Groq API key for LLaMA 3.3 70B     |
| DB_CONNECTION_STRING   | Azure SQL connection string        |
| CHROMA_PERSIST_DIR     | ChromaDB persistence path          |

### Local `.env` file
```env
GROQ_API_KEY=your_groq_api_key
DB_CONNECTION_STRING=your_connection_string
CHROMA_PERSIST_DIR=./chroma_db
```

### Azure App Service Configuration
```env
GROQ_API_KEY=your_groq_api_key
DB_CONNECTION_STRING=your_azure_sql_connection_string
CHROMA_PERSIST_DIR=/home/chroma_db
```

---

## Getting Started

### Clone Repository
```bash
git clone https://github.com/your-username/TrackNest-AI.git
cd TrackNest-AI
```

### Install Dependencies
```bash
pip install -r requirements.txt
```

### Set Environment Variables
```bash
cp .env.example .env
# Add your GROQ_API_KEY and DB_CONNECTION_STRING
```

### Run Locally
```bash
uvicorn main:app --reload
```

### Access Swagger Docs
```text
http://localhost:8000/docs
```

---

## Key Concepts Demonstrated

* FastAPI REST API design
* RAG (Retrieval-Augmented Generation)
* ChromaDB vector store + semantic search
* LangChain RAG pipeline
* Groq LLaMA 3.3 70B integration
* Smart query routing (aggregation vs semantic)
* Azure App Service deployment
* GitHub Actions CI/CD
* Environment-based secure configuration
* Microservice architecture (integrates with .NET TrackNest API)

---

## Related Repositories

* [TrackNest](https://github.com/your-username/TrackNest) — ASP.NET Core 8 REST API
* [TrackNest-Angular](https://github.com/your-username/TrackNest-Angular) — Angular 17 Frontend

---

## Author

**Prabhakar Koranga**
Full Stack Developer | .NET | Angular | Python | AI Integration

**GitHub:** [github.com/your-username](https://github.com/your-username)
