# 🏥 Healthcare Chatbot

A smart, adaptive healthcare chatbot built with Flask and powered by GPT-3.5. It automatically detects the user's medical knowledge level and tailors its responses accordingly — from simple plain-language explanations for beginners to precise clinical terminology for healthcare professionals. Voice input in English and Turkish is also supported.

---

## 🛠️ Built With

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

---

## ✨ Features

### 🧠 Smart Knowledge Level Detection
The chatbot automatically classifies each user's question into one of four knowledge levels and adapts its response language accordingly:

| Level | Description | Response Style |
|---|---|---|
| `new` | First-time users | Welcoming, simple, encouraging |
| `beginner` | Basic health knowledge | Plain language, no jargon |
| `intermediate` | Some medical background | Mixed terminology with explanations |
| `advanced` | Healthcare professionals | Clinical terms, evidence-based detail |

### ⚡ Hybrid Classification Engine
Questions are routed through a three-tier system for maximum speed and accuracy:

1. **Rule-based** — fast keyword matching for short/simple questions (free, instant)
2. **ML model** — Random Forest trained on classified questions for common medical topics (used when confidence ≥ 70%)
3. **AI (GPT-3.5)** — OpenAI for complex or specialized questions where rules and ML aren't confident enough

### 💾 LRU Question Cache
Repeated questions are served instantly from an in-memory cache (up to 1,000 entries) — no redundant API calls.

### 👤 Per-User Profiles
Each user has a profile that tracks their knowledge level history. If a user consistently asks advanced questions, they are automatically promoted to the advanced tier.

### 📣 Feedback Loop
Users can give feedback after each response. The chatbot adjusts the user's knowledge level based on whether the response felt too easy, too hard, or just right.

### 🎙️ Voice Input (English & Turkish)
`speechrec.py` provides voice input support with automatic language detection using `langid`. Speaks responses back using gTTS (Turkish) or pyttsx3 (English).

### 🌐 Web Interface
Clean chat UI built in HTML/CSS/JavaScript with:
- Animated "Typing..." indicator while waiting for a response
- Yes/No feedback buttons after each bot response
- Knowledge level selector (Beginner / Intermediate / Advanced)
- Smooth auto-scroll to latest message

---

## 📁 Project Structure

```
healthcare-chatbot/
├── app.py                        # Flask backend — chatbot logic, classification, API routes
├── speechrec.py                  # Voice input — English/Turkish speech recognition + TTS
├── best_classifier.py            # BestMedicalClassifier — OpenAI-powered question classifier
├── templates/
│   └── index.html                # Frontend chat interface
├── data/
│   └── classified_questions.json # Training data for the local ML model
└── requirements.txt
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- An OpenAI API key

### Installation

```bash
git clone https://github.com/your-username/healthcare-chatbot.git
cd healthcare-chatbot
pip install -r requirements.txt
```

### Environment Variables

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=your_openai_api_key
```

### Run the App

```bash
python app.py
```

Then open your browser at `http://localhost:5001`

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Serves the chat UI |
| `POST` | `/chatbot` | Send a message, get a response |
| `POST` | `/feedback` | Submit feedback on a response |
| `GET` | `/user_profile/<user_id>` | Get a user's knowledge profile |
| `POST` | `/classify` | Classify a question (for testing) |
| `GET` | `/classification_stats` | View classification method usage stats |
| `POST` | `/clear_cache` | Clear the question cache |
| `POST` | `/train_model` | Manually retrigger ML model training |

### Example Request

```bash
curl -X POST http://localhost:5001/chatbot \
  -d "message=What is hypertension?&knowledge_level=beginner&user_id=user123"
```

### Example Response

```json
{
  "output": "Hypertension, or high blood pressure, is when the force of blood pushing against your artery walls is consistently too high...",
  "detected_level": "beginner",
  "confidence": 0.85,
  "classification_method": "ml",
  "interaction_count": 3
}
```

---

## 🎙️ Voice Mode

To use the voice interface, run:

```bash
python speechrec.py
```

Speak your health query in English or Turkish — the assistant detects the language automatically, fetches a response, and reads it back to you.

Additional dependencies for voice mode:
```bash
pip install SpeechRecognition pyttsx3 gTTS langid pyaudio
```


## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push and open a Pull Request

---

## 📄 License

This project is open source. See `LICENSE` for details.

---

> ⚠️ **Disclaimer**: This chatbot is for informational purposes only and does not constitute medical advice. Always consult a qualified healthcare professional for medical decisions.
