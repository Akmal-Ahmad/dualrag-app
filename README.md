# DualRAG - Frontend

Modern chat interface for RAG (Retrieval-Augmented Generation) systems.

## Installation & Deployment


# Clone the repository
git clone https://github.com/Akmal-Ahmad/dualrag-app.git
cd dualrag-app

# Install dependencies
npm install

# Start development server
npm run dev
The app will open at http://localhost:5173

Backend API Requirements
Your backend must run on a server and expose these endpoints:

Required Endpoints
Method		Endpoint			Purpose
GET		/api/health			Health check
GET		/api/documents			List indexed documents
POST		/api/upload			Upload a document
DELETE		/api/documents/{id}		Remove a document
POST		/api/query			Query the RAG system

