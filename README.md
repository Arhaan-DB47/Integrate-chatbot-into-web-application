# 🌐 Integrate Chatbot into Web Application

> A full-stack web chatbot powered by Facebook's BlenderBot LLM, built with a Flask backend and a custom HTML/CSS/JS frontend — containerized with Docker.

Built as part of **Course 8 – Building Generative AI-Powered Applications with Python** in the [IBM AI Developer Professional Certificate](https://www.coursera.org/professional-certificates/applied-artifical-intelligence-ibm-watson-ai).

---

## 📌 Overview

This project takes the conversational chatbot from the [Simple Chatbot Using LLM](https://github.com/Arhaan-DB47/Simple-chatbot-using-LLM) project and integrates it into a **full web application**. Instead of chatting in the terminal, users interact through a polished browser-based chat interface — complete with user/bot avatars, loading animations, and a responsive layout.

### Architecture

```
┌─────────────────────────────────────────────┐
│                  Browser                     │
│  ┌─────────────────────────────────────────┐ │
│  │  index.html + style.css + script.js     │ │
│  │  (Chat UI with avatars & animations)    │ │
│  └──────────────┬──────────────────────────┘ │
│                 │ POST /chatbot              │
│                 │ { "prompt": "..." }         │
└─────────────────┼───────────────────────────┘
                  ▼
┌─────────────────────────────────────────────┐
│          Flask Backend (app.py)              │
│  ┌─────────────────────────────────────────┐ │
│  │  BlenderBot-400M-distill                │ │
│  │  (Hugging Face Transformers)            │ │
│  │  + Conversation History Management      │ │
│  └─────────────────────────────────────────┘ │
└─────────────────────────────────────────────┘
```

---

## 📂 Project Structure

```
Integrate-chatbot-into-web-application/
└── LLM_application_chatbot/
    ├── app.py                    # Flask server + BlenderBot inference
    ├── Dockerfile                # Docker container configuration
    ├── requirements.txt          # Python dependencies
    ├── templates/
    │   └── index.html            # Chat interface HTML template
    └── static/
        ├── css/
        │   └── style.css         # Chat UI styling & animations
        ├── script.js             # Frontend logic — message handling & API calls
        ├── Bot_logo.png          # Bot avatar image
        ├── user.jpeg             # User avatar image
        ├── Error.png             # Error state avatar
        └── favicon.ico           # Browser tab icon
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- pip
- Docker *(optional, for containerized deployment)*

### Option 1: Run Locally

```bash
# Clone the repository
git clone https://github.com/Arhaan-DB47/Integrate-chatbot-into-web-application.git
cd Integrate-chatbot-into-web-application/LLM_application_chatbot

# Install dependencies
pip install -r requirements.txt

# Start the Flask server
python app.py
```

The app will be available at `http://127.0.0.1:5000`.

### Option 2: Run with Docker

```bash
cd Integrate-chatbot-into-web-application/LLM_application_chatbot

# Build the Docker image
docker build -t llm-chatbot-app .

# Run the container
docker run -p 5000:5000 llm-chatbot-app
```

---

## 📖 How It Works

### Backend — `app.py`

The Flask server exposes two routes:

| Route | Method | Description |
|-------|--------|-------------|
| `/` | GET | Serves the chat interface (`index.html`) |
| `/chatbot` | POST | Accepts a JSON prompt, generates a response using BlenderBot, and returns it |

**Key implementation details:**
- Loads **BlenderBot-400M-distill** (Seq2Seq model) at startup
- Maintains a **conversation history** list — both user messages and bot responses are appended to preserve context across turns
- Uses `tokenizer.encode_plus()` to encode the conversation history alongside the new input
- CORS is enabled via `flask_cors` for cross-origin requests

### Frontend — `index.html` + `script.js` + `style.css`

The chat interface provides a rich, interactive experience:

- **Chat bubbles** — User messages float right (light purple), bot responses float left (white)
- **Avatars** — Each message is accompanied by a user or bot avatar image
- **Loading animation** — A spinning loader with "Loading....Please wait" text appears while the model generates a response
- **Fade-in animations** — Bot responses and error messages animate in from the bottom
- **Error handling** — Failed requests display with a red error bubble and error icon
- **Responsive design** — Layout adapts to smaller screens via CSS media queries

### Message Flow

1. User types a message and clicks ➤ (or presses submit)
2. `script.js` adds the user message to the chat window with a user avatar
3. A loading animation appears while waiting for the response
4. A `POST` request is sent to `/chatbot` with the message as JSON
5. The Flask backend tokenizes the input + conversation history, runs BlenderBot inference, and returns the response
6. The loading animation is removed and the bot response fades in with a bot avatar

---

## 🐳 Docker Configuration

The included `Dockerfile` creates a production-ready container:

```dockerfile
FROM python:3.10
WORKDIR /LLM_application_chatbot
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

---

## 🧠 Model Used

- **Model**: [facebook/blenderbot-400M-distill](https://huggingface.co/facebook/blenderbot-400M-distill)
- **Architecture**: Seq2Seq (Encoder-Decoder)
- **Size**: ~400M parameters (distilled)
- **Strength**: Purpose-built for open-domain multi-turn conversation

---

## 🛠️ Technologies & Libraries

| Technology | Purpose |
|------------|---------|
| [Flask](https://flask.palletsprojects.com/) | Python web framework — serves the UI and API |
| [Flask-CORS](https://flask-cors.readthedocs.io/) | Cross-origin resource sharing support |
| [Hugging Face Transformers](https://huggingface.co/docs/transformers) | BlenderBot model loading and text generation |
| [PyTorch](https://pytorch.org/) | Deep learning backend for model inference |
| [Docker](https://www.docker.com/) | Containerized deployment |
| HTML / CSS / JavaScript | Custom chat interface with animations |

---

## 🔗 Related Projects

This project is part of a series built during the IBM AI Developer Professional Certificate (Course 8):

| Project | Description |
|---------|-------------|
| [Image Captioning with BLIP & Gradio](https://github.com/Arhaan-DB47/Img_captioning) | AI-powered image captioning — from local inference to automated web scraping |
| [Simple Chatbot Using LLM](https://github.com/Arhaan-DB47/Simple-chatbot-using-LLM) | Terminal-based chatbots using BlenderBot and SmolLM2 |
| **Integrate Chatbot into Web App** *(this repo)* | Full-stack web chatbot with Flask + custom UI + Docker |

---

## 📜 License

This project was created for educational purposes as part of the IBM AI Developer Professional Certificate on Coursera.

---

## 👤 Author

**Arhaan Khan** — [@Arhaan-DB47](https://github.com/Arhaan-DB47)
