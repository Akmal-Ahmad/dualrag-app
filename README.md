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

## Complete Feature List

### Chat Features

- **Streaming Responses** - Real-time token-by-token AI response generation
- **Non-Streaming Fallback** - Automatically handles both streaming and standard responses
- **Regenerate Responses** - One-click regeneration of AI responses with same context
- **Copy to Clipboard** - Copy any AI response with a single click
- **Markdown Rendering** - Full support for headers, bold, italic, code blocks, lists, and more
- **Source Attribution** - Visual display of which documents contributed to each response
- **Error Handling** - Graceful error messages with recovery options
- **Connection Status** - Real-time indicator showing backend connectivity
- **Stop Generation** - Ability to cancel ongoing AI responses mid-stream

---

### Document Management

- **Multi-format Upload** - Support for PDF, TXT, DOCX, MD, CSV, JSON, PNG, JPG
- **Batch Upload** - Upload multiple files simultaneously
- **Upload Progress** - Visual progress bar during document upload
- **Document List** - View all indexed documents in dropdown panel
- **Remove Individual Documents** - Delete specific documents from the index
- **Remove All Documents** - Bulk delete with confirmation dialog
- **Refresh Documents** - Manually refresh document list from backend
- **Document Count** - Live counter showing number of indexed documents

---

### Chat History

- **Persistent Storage** - All conversations saved to localStorage
- **Multiple Conversations** - Create and switch between different chat sessions
- **Auto-titling** - Conversations automatically titled from first message
- **Active Chat Indicator** - Visual highlight showing current conversation
- **Delete Individual Chats** - Remove specific conversations
- **Clear All Data** - Wipe all conversations and settings
- **Message Persistence** - Chat messages survive page reloads
- **Empty State** - Friendly message when no conversations exist
- **Auto-scroll** - Automatically scrolls to latest messages

---

### Keyboard Shortcuts

| Shortcut       | Action                         |
| -------------- | ------------------------------ |
| `Ctrl/Cmd + N` | New conversation               |
| `Ctrl/Cmd + ,` | Open settings                  |
| `Enter`        | Send message                   |
| `Escape`       | Stop generation / Close modals |

---

### Settings & Configuration

- **API URL Configuration** - Change backend server address
- **API Key Support** - Add authentication keys (Bearer token / X-API-Key)
- **Model Selection** - Specify which AI model to use if backend supports multiple
- **Test Connection** - Verify backend connectivity from settings
- **Persistent Settings** - Settings saved to localStorage
- **Connection Monitoring** - Automatic periodic health checks (every 30s)

---

### Data & Privacy

- **Client-side Storage** - All data stays in browser localStorage
- **No External Tracking** - No analytics, no cookies, no tracking
- **API Key Masking** - Password field for API key input
- **Confirmation Dialogs** - Delete operations require confirmation

---

### Performance

- **Virtual Scrolling** - Load older messages on scroll for large chats
- **Pagination** - Messages loaded in pages (20 per page)
- **Lazy Loading** - Progressive loading of chat history
- **Optimized DOM Updates** - Fragment-based DOM manipulation
- **Efficient Rendering** - Markdown rendered client-side

---

### Developer Features

- **Hot Module Replacement** - Vite dev server with HMR
- **ES Modules** - Modern JavaScript module system
- **No Dependencies** - Zero production JavaScript dependencies
- **CORS Ready** - Works with any CORS-enabled backend
- **Streaming Support** - Server-Sent Events (SSE) support
- **Abort Controller** - Request cancellation support
- **Error State Handling** - Comprehensive try-catch blocks
- **Environment Agnostic** - Works with any RAG backend

---

### Error Handling

- **Connection Errors** - Clear message when backend unreachable
- **Upload Failures** - Per-file error reporting
- **Query Failures** - Graceful fallback with error display
- **Network Timeout** - 3-second timeout for health checks
- **JSON Parse Errors** - Safe parsing with fallbacks
- **DOM Error Recovery** - Fallback message elements on render failure

## License

MIT
