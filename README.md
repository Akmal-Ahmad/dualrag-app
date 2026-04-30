# DualRAG - Frontend

Modern chat interface for RAG (Retrieval-Augmented Generation) systems.

---

## Installation & Deployment

```bash
# Clone the repository
git clone https://github.com/Akmal-Ahmad/dualrag-app.git

# Navigate to project folder
cd dualrag-app

# Install dependencies
npm install

# Start development server
npm run dev
```

The app will open at **http://localhost:5173**

## Backend API Requirements

Your backend server must expose the following endpoints:

| Method   | Endpoint              | Purpose                |
| -------- | --------------------- | ---------------------- |
| `GET`    | `/api/health`         | Health check           |
| `GET`    | `/api/documents`      | List indexed documents |
| `POST`   | `/api/upload`         | Upload a document      |
| `DELETE` | `/api/documents/{id}` | Remove a document      |
| `POST`   | `/api/query`          | Query the RAG system   |

---

### 1. Health Check

```
GET /api/health
```

**Response:** HTTP `200 OK`

```json
{
  "status": "healthy"
}
```

---

### 2. List Documents

```
GET /api/documents
```

**Response:**

```json
{
  "documents": [
    {
      "id": "doc-123",
      "filename": "report.pdf",
      "chunks": 5,
      "size": 1024000,
      "created_at": "2024-01-15 10:30:00"
    }
  ]
}
```

---

### 3. Upload Document

```
POST /api/upload
Content-Type: multipart/form-data
```

**Form Field:** `file` (the document file)

**Response:**

```json
{
  "message": "Document uploaded successfully",
  "doc_id": "doc-123",
  "filename": "report.pdf",
  "chunks": 5
}
```

---

### 4. Delete Document

```
DELETE /api/documents/{doc_id}
```

**Response:**

```json
{
  "message": "Document deleted successfully"
}
```

---

### 5. Query RAG System

```
POST /api/query
Content-Type: application/json
```

**Request Body:**

```json
{
  "query": "What did the report say about Q4 earnings?",
  "top_k": 3,
  "stream": false,
  "model": "gpt-4"
}
```

**Standard Response (Non-Streaming):**

```json
{
  "answer": "According to the report, Q4 earnings increased by 15%...",
  "sources": [
    {
      "filename": "earnings-report.pdf",
      "id": "doc-123",
      "chunk": 2,
      "relevance_score": 0.94
    }
  ]
}
```

**Streaming Response (SSE):**

```
Content-Type: text/event-stream
```

```
data: {"text": "According "}
data: {"text": "to the "}
data: {"text": "report..."}
data: {"sources": [{"filename": "report.pdf", "id": "doc-123"}]}
data: [DONE]
```

---

## Important Notes

### CORS Configuration

Your backend **must** enable CORS to allow the frontend to connect:

```python
# Python/FastAPI
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### API Authentication

The frontend sends API keys via headers:

```
Authorization: Bearer YOUR_API_KEY
X-API-Key: YOUR_API_KEY
```

### Configuration

Users can change the API URL and settings through the ⚙️ Settings button in the app.

---

## Testing Your Backend

```bash
# Test health endpoint
curl http://localhost:8000/api/health

# Test document upload
curl -X POST http://localhost:8000/api/upload \
  -F "file=@document.pdf"

# Test query endpoint
curl -X POST http://localhost:8000/api/query \
  -H "Content-Type: application/json" \
  -d '{"query": "test query", "top_k": 2}'
```

---

## Project Structure

```
dualrag-app/
├── index.html          # Main HTML file
├── package.json        # Project dependencies
├── README.md          # This file
└── src/
    ├── main.js        # Application logic
    └── style.css      # Application styles
```

---

## Tech Stack

- **Vite** - Build tool and dev server
- **Vanilla JavaScript** - No frameworks, pure JS
- **Font Awesome** - Icons
- **LocalStorage** - Data persistence

---

## License

MIT
