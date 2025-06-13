# 📘 API Reference

This document provides detailed information about the FastAPI endpoints available in the RAG Chatbot.

---

## 🌐 Base URL

```

[http://localhost:8000](http://localhost:8000)

````

---

## 🔐 Authentication

No authentication is required for local development. All endpoints are publicly accessible when running locally.

---

## 🧭 Endpoints Overview

| Endpoint        | Method | Description                            |
|-----------------|--------|----------------------------------------|
| `/chat`         | POST   | Send a message to the chatbot          |
| `/upload-doc`   | POST   | Upload and process a document          |
| `/list-docs`    | GET    | List all uploaded documents            |
| `/delete-doc`   | POST   | Delete a specific document             |
| `/health`       | GET    | Check API health status                |

---

## 🔍 Detailed Endpoint Documentation

### 🗨️ POST `/chat`

Send a message to the chatbot and receive a response based on uploaded documents.

**Request Body (JSON):**

```json
{
  "message": "What is this document about?",
  "session_id": "user123"
}
````

**Parameters:**

* `message` (string, required): The user's question or message
* `session_id` (string, required): Unique identifier for the session

**Response Example:**

```json
{
  "response": "This document discusses machine learning techniques...",
  "session_id": "user123",
  "timestamp": "2025-06-13T14:45:00Z",
  "sources": [
    {
      "filename": "ml_guide.pdf",
      "page": 1,
      "chunk_id": "chunk_001"
    }
  ]
}
```

**Error Response:**

```json
{
  "error": "No documents found. Please upload documents first.",
  "status_code": 400
}
```

---

### 📤 POST `/upload-doc`

Upload and process a document for the chatbot.

**Request:**

* `Content-Type`: `multipart/form-data`
* Field: `file` (file upload)

**Supported File Types:**

* `.pdf`
* `.docx`
* `.html`

**File Size Limit:** 200MB

**Response Example:**

```json
{
  "message": "Document uploaded and processed successfully",
  "filename": "document.pdf",
  "file_size": 1024000,
  "chunks_created": 15,
  "processing_time": 2.5,
  "document_id": "doc_12345"
}
```

**Error Response:**

```json
{
  "error": "File type not supported. Please upload PDF, DOCX, or HTML files.",
  "status_code": 400
}
```

---

### 📄 GET `/list-docs`

Retrieve a list of all uploaded documents.

**Response Example:**

```json
{
  "documents": [
    {
      "document_id": "doc_12345",
      "filename": "ml_guide.pdf",
      "upload_date": "2025-06-13T14:45:00Z",
      "file_size": 1024000,
      "chunks": 15,
      "status": "processed"
    },
    {
      "document_id": "doc_12346",
      "filename": "ai_basics.docx",
      "upload_date": "2025-06-13T15:30:00Z",
      "file_size": 512000,
      "chunks": 8,
      "status": "processing"
    }
  ],
  "total_documents": 2
}
```

---

### 🗑️ POST `/delete-doc`

Delete a specific document.

**Request Body:**

```json
{
  "document_id": "doc_12345"
}
```

**Response Example:**

```json
{
  "message": "Document deleted successfully",
  "document_id": "doc_12345",
  "filename": "ml_guide.pdf"
}
```

**Error Response:**

```json
{
  "error": "Document not found",
  "status_code": 404
}
```

---

### 💓 GET `/health`

Check the health status of the API.

**Response Example:**

```json
{
  "status": "healthy",
  "timestamp": "2025-06-13T14:45:00Z",
  "version": "1.0.0",
  "database_status": "connected",
  "vector_store_status": "ready"
}
```

---

## 🚨 Error Handling

All endpoints return appropriate HTTP status codes:

* `200`: Success
* `400`: Bad Request
* `404`: Not Found
* `413`: Payload Too Large
* `422`: Unprocessable Entity
* `500`: Internal Server Error

**Error Response Format:**

```json
{
  "error": "Description of the error",
  "status_code": 400,
  "timestamp": "2025-06-13T14:45:00Z"
}
```

---

## 📉 Rate Limiting

No rate limiting is implemented for local development. For production environments, implement limits based on user ID or IP.

---

## 🧪 Interactive API Documentation

Visit [http://localhost:8000/docs](http://localhost:8000/docs) for Swagger UI to:

* Test endpoints directly
* View request/response formats
* See live examples and schema definitions

---

## 💻 Code Examples

### 🐍 Python Example

```python
import requests

# Upload a document
def upload_document(file_path):
    with open(file_path, 'rb') as f:
        files = {'file': f}
        response = requests.post('http://localhost:8000/upload-doc', files=files)
    return response.json()

# Chat with the bot
def chat_with_bot(message, session_id="default"):
    payload = {
        "message": message,
        "session_id": session_id
    }
    response = requests.post('http://localhost:8000/chat', json=payload)
    return response.json()

# Example usage
upload_result = upload_document('my_document.pdf')
print(f"Upload result: {upload_result}")

chat_response = chat_with_bot("What is this document about?")
print(f"Bot response: {chat_response['response']}")
```

---

### 🌐 JavaScript Example

```javascript
// Upload a document
async function uploadDocument(file) {
  const formData = new FormData();
  formData.append('file', file);

  const response = await fetch('http://localhost:8000/upload-doc', {
    method: 'POST',
    body: formData
  });

  return await response.json();
}

// Chat with the bot
async function chatWithBot(message, sessionId = 'default') {
  const response = await fetch('http://localhost:8000/chat', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      message: message,
      session_id: sessionId
    })
  });

  return await response.json();
}
```