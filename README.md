# RAG Chatbot with Conversational Memory

A Retrieval-Augmented Generation (RAG) chatbot built with LangChain that supports conversational memory for follow-up questions. Features FastAPI backend and Streamlit frontend with document upload capabilities.

---

## 🚀 Features

- **Document Upload**: Supports PDF, DOCX, HTML files (200MB limit per file)
- **Conversational Memory**: Maintains context for follow-up questions
- **Multiple Models**: Choose between GPT-4o-mini and other model options
- **Vector Storage**: Efficient document retrieval with ChromaDB
- **Interactive API**: FastAPI backend with Swagger documentation
- **LangSmith Integration**: Built-in tracing and monitoring

---

## ⚙️ Configuration

Create a `.env` file in the root with the following variables:

```env
OPENAI_API_KEY=your_openai_api_key
LANGSMITH_API_KEY=your_langsmith_api_key
````

---

## 📦 Project Structure

```
RAG-CHATBOT/
├── api/                         # FastAPI backend server
│   ├── __pycache__/             # Python bytecode cache
│   ├── chroma_db/               # ChromaDB vector storage
│   ├── app.log                  # Logging file
│   ├── chroma_utils.py          # ChromaDB utilities
│   ├── db_utils.py              # Chat history & metadata DB logic
│   ├── langchain_utils.py       # LangChain RAG pipeline
│   ├── main.py                  # FastAPI entry point
│   ├── pydantic_models.py       # Request/response validation
│   └── rag_app.db               # SQLite DB
├── app/                         # Streamlit frontend
│   ├── __pycache__/             # Python bytecode cache
│   ├── api_utils.py             # FastAPI client utils
│   ├── chat_interface.py        # Chat UI
│   ├── sidebar.py               # File upload & model switch
│   └── streamlit_app.py         # Streamlit app
├── docs/                        # Sample documents
├── documentation/               # Guides & screenshots
│   ├── screenshots/             # UI screenshots
│   ├── api_reference.md         # API docs
│   └── user_guide.md            # Manual
├── .env                         # Env variables
├── .gitignore                   # Git ignore rules
├── LICENSE                      # MIT License
├── notes.txt                    # Dev notes
├── rag_fastapi.ipynb            # Notebook for testing
├── README.md                    # This file
└── requirements.txt             # Dependencies
```

---

## ⚡ Quick Start

### 🔧 Prerequisites

* Python 3.9+
* OpenAI API Key
* LangSmith API Key (optional)

### 🛠 Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/YOUR-USERNAME/rag-chatbot.git
   cd rag-chatbot
   ```

2. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```

3. **Configure environment**

   Create a `.env` file using the sample and add your keys:

   ```bash
   cp .env.example .env
   ```

4. **Run the FastAPI backend**

   ```bash
   cd api
   uvicorn main:app --reload --port 8000
   ```

5. **Run the Streamlit frontend**

   In a new terminal:

   ```bash
   cd app
   streamlit run streamlit_app.py --server.port 8500
   ```

6. **Access the application**

   * Streamlit UI: [http://localhost:8500](http://localhost:8500)
   * Swagger Docs: [http://localhost:8000/docs](http://localhost:8000/docs)

---

## 📚 Usage Guide

### 📤 Upload Documents

1. Open the Streamlit UI
2. Use the sidebar to upload PDF, DOCX, or HTML files
3. Uploaded docs are indexed into ChromaDB

### 💬 Chat with Documents

1. Ask a question related to the uploaded content
2. Ask follow-up questions — context is remembered
3. Switch models via the sidebar dropdown

---

## 🧪 API Usage Example (Python)

```python
import requests

# Upload a document
files = {'file': open('document.pdf', 'rb')}
upload_res = requests.post('http://localhost:8000/upload-doc', files=files)

# Chat with the document
chat_payload = {
    "message": "What is this document about?",
    "session_id": "user123"
}
chat_res = requests.post('http://localhost:8000/chat', json=chat_payload)
print(chat_res.json())
```

---

## 🔌 API Endpoints

| Endpoint      | Method | Description                  |
| ------------- | ------ | ---------------------------- |
| `/chat`       | POST   | Chat with uploaded documents |
| `/upload-doc` | POST   | Upload and index documents   |
| `/list-docs`  | GET    | List all uploaded documents  |
| `/delete-doc` | POST   | Delete a specific document   |

---

## 🧠 Architecture Overview

1. **Document Ingestion**: Files are split into text chunks
2. **Embedding**: Text is embedded using OpenAI embeddings
3. **Storage**: Embeddings are stored in ChromaDB
4. **Retrieval**: Relevant chunks are fetched for user queries
5. **Generation**: LangChain passes context and query to LLM
6. **Memory**: Session IDs preserve conversation history

---

## 🛠 Environment Setup

1. **Copy template and configure**

   ```bash
   cp .env.example .env
   ```

2. **Edit `.env` and add keys**

   * Get your OpenAI API key from: [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys)
   * (Optional) Get LangSmith API key from: [https://smith.langchain.com/](https://smith.langchain.com/)

3. **Keep `.env` private**

   `.env` is included in `.gitignore` to avoid committing secrets.

---

## 🔑 Required API Keys

* **OPENAI\_API\_KEY**: Required for embeddings and completions
* **LANGSMITH\_API\_KEY**: For request tracing and logging

---

## 🖼 Screenshots

Screenshots are located in:

```
documentation/screenshots/
```

---

## 🤝 Contributing

We welcome contributions!

1. Fork the repository
2. Create a feature branch
3. Make your changes and commit
4. Open a pull request

---

## 📜 License

This project is licensed under the MIT License.

```
MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:
...
```

See the full [LICENSE](LICENSE) file for more details.

---

## 🙏 Acknowledgments

* Built with [LangChain](https://www.langchain.com/)
* Vector storage by [ChromaDB](https://www.trychroma.com/)
* UI by [Streamlit](https://streamlit.io/)
* Backend by [FastAPI](https://fastapi.tiangolo.com/)
* Observability via [LangSmith](https://smith.langchain.com/)

---

## 📬 Contact

Have feedback, issues, or ideas?

* Open an issue on GitHub
* Submit a pull request
* Contact the maintainer (replace with actual email/contact info)