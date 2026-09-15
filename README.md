# 🤖 AI Assistant

A modern, responsive **AI conversational assistant** that combines an instant FAQ engine with **Google Gemini generative AI** for intelligent, open-ended conversations.

The application uses a two-layer response architecture:

* ⚡ **FAQ Engine** — instant, deterministic responses for common questions.
* 🧠 **Gemini AI** — generative responses for questions that require natural-language understanding and contextual reasoning.
* 💾 **MongoDB Atlas** — cloud database for storing conversation data and chat history.
* 🌐 **Flask** — lightweight Python backend serving the web application and REST APIs.

---

## ✨ Features

### 🎨 Modern UI / UX

* **Dark & Light Mode** with user preference stored in `localStorage`
* **Rich Markdown Formatting** for:

  * Bold text
  * Bullet points
  * Links
  * Code blocks
* **Dynamic 3-Dot Typing Indicator**
* **Quick Suggestion Chips** for common prompts
* **Speech-to-Text Voice Input** using the Web Speech API
* **Conversation Actions**

  * Copy individual messages
  * Export conversation as `.txt`
  * Clear conversation
* **Live Server Status** using the `/health` endpoint
* **Responsive Design** for desktop, tablet and mobile devices

---

## 🧠 AI Architecture

The assistant uses a **dual-engine response pipeline**.

### Layer 1 — FAQ Engine

The FAQ engine in:

```text
chatbot/responses.py
```

uses pattern matching to identify common questions and return predefined responses immediately.

Advantages:

* ⚡ Very fast
* 💰 Zero AI API cost
* 🎯 Deterministic responses
* 🌐 Works without calling the generative AI model

### Layer 2 — Gemini Generative AI

When a question does not match the FAQ knowledge base, the request is passed to:

```text
chatbot/ai.py
```

The application uses Google's Gemini API through the `google-genai` SDK.

The configured model is:

```text
gemini-3.5-flash-lite
```

Gemini 3.5 Flash-Lite is designed for low-latency and cost-efficient high-volume applications.

### Context Memory

Recent conversation history can be supplied to the AI engine so users can ask follow-up questions without repeating the entire context.

Example:

```text
User: What is Python?

AI: Python is a programming language...

User: What can I build with it?

AI: You can build web applications, AI systems...
```

---

## 🏛️ Architecture

```text
                 User Input
              Text / Voice
                    │
                    ▼
          ┌─────────────────────┐
          │      Web UI         │
          │ HTML/CSS/JavaScript │
          └──────────┬──────────┘
                     │
                     │ POST /chat
                     ▼
          ┌─────────────────────┐
          │    Flask Backend    │
          │       app.py        │
          └──────────┬──────────┘
                     │
              ┌──────┴───────┐
              │              │
              ▼              ▼
       ┌─────────────┐  ┌────────────────┐
       │ FAQ Engine  │  │ Gemini AI      │
       │ responses.py│  │    ai.py       │
       └──────┬──────┘  └───────┬────────┘
              │                 │
              │    Fallback     │
              └────────┬────────┘
                       ▼
              ┌─────────────────┐
              │ JSON Response   │
              │ reply + source  │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │ MongoDB Atlas   │
              │ Chat History    │
              └─────────────────┘
```

---

## 📁 Project Structure

```text
CodeAlpha_Chatbot/
│
├── app.py
├── requirements.txt
├── Dockerfile
├── .env
├── .gitignore
│
├── chatbot/
│   ├── __init__.py
│   ├── ai.py
│   ├── db.py
│   └── responses.py
│
├── templates/
│   └── index.html
│
├── static/
│   ├── script.js
│   └── style.css
│
└── README.md
```

### File Description

| File                   | Purpose                                                       |
| ---------------------- | ------------------------------------------------------------- |
| `app.py`               | Flask application and API routes                              |
| `chatbot/ai.py`        | Gemini AI integration and response generation                 |
| `chatbot/responses.py` | FAQ pattern-matching engine                                   |
| `chatbot/db.py`        | MongoDB Atlas connection and chat-history operations          |
| `templates/index.html` | Chat application interface                                    |
| `static/script.js`     | Frontend interaction, chat handling, voice input and UI logic |
| `static/style.css`     | Application styling and responsive design                     |
| `requirements.txt`     | Python dependencies                                           |
| `Dockerfile`           | Container configuration                                       |
| `.env`                 | Local environment configuration                               |

---

## 🧰 Technology Stack

| Category               | Technology              |
| ---------------------- | ----------------------- |
| Frontend               | HTML5, CSS3, JavaScript |
| Backend                | Python                  |
| Web Framework          | Flask 3.0.3             |
| Generative AI          | Google Gemini API       |
| Gemini SDK             | `google-genai`          |
| AI Model               | Gemini 3.5 Flash-Lite   |
| Database               | MongoDB Atlas           |
| Database Driver        | PyMongo                 |
| Environment Management | python-dotenv           |
| Markdown               | marked.js               |
| Sanitization           | DOMPurify               |
| Voice Input            | Web Speech API          |
| Containerization       | Docker                  |
| Deployment             | Google Cloud Run        |

---

# 🚀 Local Setup

## 1. Clone the repository

```bash
git clone https://github.com/senthilvelanprabakaran-alt/CodeAlpha_Chatbot.git
cd CodeAlpha_Chatbot
```

---

## 2. Create a virtual environment

### Windows

```powershell
python -m venv venv
venv\Scripts\activate
```

### Linux / macOS

```bash
python3 -m venv venv
source venv/bin/activate
```

---

## 3. Install dependencies

```bash
pip install -r requirements.txt
```

The current project uses the Google GenAI SDK rather than the previous OpenRouter/OpenAI-compatible client.

---

## 4. Configure environment variables

Create a `.env` file in the project root.

```env
GEMINI_API_KEY=your_gemini_api_key
GEMINI_MODEL=gemini-3.5-flash-lite

MONGODB_URI=your_mongodb_atlas_connection_string
MONGODB_DB=your_database_name
MONGODB_COLLECTION=your_collection_name

PORT=5000
```

### Example

```env
GEMINI_API_KEY=YOUR_API_KEY
GEMINI_MODEL=gemini-3.5-flash-lite

MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/
MONGODB_DB=chatbot_db
MONGODB_COLLECTION=chat_history

PORT=5000
```

> ⚠️ **Never commit `.env` or your Gemini API key to GitHub.**

---

## 5. Run the application

```bash
python app.py
```

Open:

```text
http://localhost:5000
```

---

# 🤖 Gemini Configuration

The AI backend is controlled using:

```env
GEMINI_API_KEY=your_api_key
GEMINI_MODEL=gemini-3.5-flash-lite
```

The model can therefore be changed without modifying the Python source code.

For example:

```env
GEMINI_MODEL=gemini-3.5-flash-lite
```

Google's current Gemini documentation lists Gemini 3.5 Flash-Lite as a supported model and describes it as a cost-efficient option for high-volume use.

---

# 💾 MongoDB Atlas

The application uses **MongoDB Atlas** as its cloud database.

The database layer is implemented in:

```text
chatbot/db.py
```

MongoDB is used for persistent conversation-related data rather than storing the chat only in browser memory.

Required configuration:

```env
MONGODB_URI=your_mongodb_connection_string
MONGODB_DB=your_database_name
MONGODB_COLLECTION=your_collection_name
```

### MongoDB Atlas Connection

Your Atlas connection string normally follows this structure:

```text
mongodb+srv://<username>:<password>@<cluster>.mongodb.net/
```

Make sure your application's server IP/network access is permitted in MongoDB Atlas.

---

# 📡 API Reference

## `POST /chat`

Sends a message to the assistant.

### Request

```json
{
  "message": "What can you help me with?",
  "history": [
    {
      "role": "user",
      "content": "Hello"
    },
    {
      "role": "assistant",
      "content": "Welcome to AI Assistant!"
    }
  ]
}
```

### Response

```json
{
  "reply": "I can help you with programming, AI, data science and more.",
  "source": "ai"
}
```

Possible `source` values include:

```text
faq
ai
warning
```

### Response Flow

```text
User Message
     │
     ▼
FAQ Pattern Matching
     │
 ┌───┴────┐
 │        │
Match    No Match
 │        │
 ▼        ▼
FAQ     Gemini
 │        │
 └───┬────┘
     ▼
JSON Response
```

---

## `GET /health`

Checks whether the Flask server is running.

Example:

```json
{
  "status": "ok"
}
```

This endpoint can be used by deployment platforms and monitoring systems.

---

# 🧪 Diagnostics

The project includes diagnostic functionality for checking the AI/database configuration.

Use the project's diagnostic script if available:

```bash
python diagnose.py
```

The diagnostics can be used to identify configuration problems involving:

* Gemini API configuration
* Model configuration
* MongoDB Atlas connectivity
* Environment variables

---

# 🐳 Docker Deployment

The application includes a Dockerfile for containerized deployment.

### Build the image

```bash
docker build -t ai-chatbot .
```

### Run the container

```bash
docker run -p 8080:8080 --env-file .env ai-chatbot
```

Then open:

```text
http://localhost:8080
```

The container listens on port `8080`.

---

# ☁️ Google Cloud Run Deployment

Google Cloud Run supports deployment of containerized applications and provides the `asia-south1` Mumbai region.

## 1. Authenticate

```bash
gcloud auth login
```

## 2. Select your project

```bash
gcloud config set project YOUR_PROJECT_ID
```

## 3. Build and submit the container

```bash
gcloud builds submit \
  --tag gcr.io/YOUR_PROJECT_ID/ai-chatbot
```

## 4. Deploy to Cloud Run

```bash
gcloud run deploy ai-chatbot \
  --image gcr.io/YOUR_PROJECT_ID/ai-chatbot \
  --platform managed \
  --region asia-south1 \
  --allow-unauthenticated
```

### Environment Variables

For production, configure the Gemini and MongoDB credentials through Cloud Run environment/secret configuration rather than committing them to the repository.

Required values:

```text
GEMINI_API_KEY
GEMINI_MODEL
MONGODB_URI
MONGODB_DB
MONGODB_COLLECTION
```

---

# 🔐 Security

The project follows several basic security practices:

* API credentials are loaded from environment variables
* `.env` should not be committed to Git
* MongoDB credentials are kept outside source code
* Gemini API keys are kept outside source code
* Markdown output is sanitized on the frontend using DOMPurify
* Database connectivity is handled through the backend
* The Gemini API key is never intended to be exposed to browser-side JavaScript

### ⚠️ Never do this

```javascript
const GEMINI_API_KEY = "your-secret-key";
```

API keys belong on the server.

---

# 📈 Response Strategy

The application is designed to minimize unnecessary AI API calls.

```text
                    User Question
                         │
                         ▼
                ┌─────────────────┐
                │ FAQ Pattern      │
                │ Matching         │
                └────────┬────────┘
                         │
                ┌────────┴────────┐
                │                 │
              Match            No Match
                │                 │
                ▼                 ▼
         ⚡ Instant FAQ      🤖 Gemini AI
                │                 │
                └────────┬────────┘
                         ▼
                  Final Response
```

This approach provides:

* Faster answers for common questions
* Reduced AI API usage
* Deterministic responses where appropriate
* Generative intelligence for open-ended queries

---

# 🎯 Future Enhancements

Possible future improvements include:

* Streaming Gemini responses
* User authentication
* Per-user conversation history
* Conversation search
* File/document upload
* RAG-based knowledge retrieval
* Voice output / text-to-speech
* Multilingual conversations
* Admin analytics dashboard
* Conversation export as PDF
* Advanced MongoDB conversation management
* Gemini Interactions API integration
* Cloud monitoring and centralized logging

Google currently recommends the **Interactions API** for new Gemini projects, while the older `generateContent` API remains supported.

---

# 📊 Project Highlights

| Feature              | Status |
| -------------------- | ------ |
| Responsive Chat UI   | ✅      |
| Dark / Light Mode    | ✅      |
| FAQ Engine           | ✅      |
| Gemini AI            | ✅      |
| Multi-turn Context   | ✅      |
| Voice Input          | ✅      |
| Markdown Rendering   | ✅      |
| Message Copy         | ✅      |
| Chat Export          | ✅      |
| MongoDB Atlas        | ✅      |
| Health Check         | ✅      |
| Docker Support       | ✅      |
| Cloud Run Deployment | ✅      |

---

# 👨‍💻 Author

**Senthilvelan Prabakaran**

GitHub:

`https://github.com/senthilvelanprabakaran-alt`

---

# 📄 License

This project currently does not specify an open-source license.

If you intend to distribute the project publicly, consider adding an appropriate license such as the MIT License.
