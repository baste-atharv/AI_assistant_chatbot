
## Overview

This AI Assistant Chatbot is an intelligent, multilingual assistant designed to provide accurate, empathetic, and context-aware responses to Bengaluru International Airport visitors. The chatbot handles both flight-related queries and general airport information using advanced NLP techniques.

## Features

- **Multi-Modal Support**: Process both text and audio inputs/outputs,
Voice AI Integration: OpenAI Whisper for speech recognition and TTS-1 for natural speech output.
- **Sentiment-Aware Responses**: Adapts tone based on user sentiment and emotion
- **Domain-Specific Knowledge**:
  - Flight information (schedules, status, bookings)
  - Airport amenities and services
  - Transportation options with direct cab booking links
- **Retrieval Augmented Generation (RAG)**: Leverages airport-specific knowledge base for accurate answers

## Technology Stack

- **LLM**: GPT-4o for natural language understanding and generation
- **Embeddings**: OpenAI text-embedding-3-small
- **Vector Database**: FAISS for efficient similarity search
- **Text-to-Speech/Speech-to-Text**: OpenAI Whisper and TTS-1
- **Backend**: Python with FastAPI
- **Natural Language Processing**:
  - Query classification
  - Intent recognition
  - Sentiment analysis
  - Language detection

## Architecture

```
User Query (Text/Audio)
   ↓
Language Detection & Translation (if needed)
   ↓
Query Classification (Flight vs. FAQ)
   ↓
Sentiment Analysis
   ↓
   ┌───────────────────┐       ┌────────────────────┐
   │ Flight Processing │  OR   │ FAQ Processing     │
   │ (Direct Lookup)   │       │ (RAG with FAISS)   │
   └───────────────────┘       └────────────────────┘
              ↓                            ↓
       Empathy Enhancement
              ↓
Translation (if needed)
              ↓
Response (Text/Audio)
```

## Core Components

### 1. Query Classification
Determines if the query is flight-related or general FAQ information using an LLM-based classifier.

### 2. Sentiment Analysis
Detects user sentiment (positive, negative, neutral) and tone to provide empathetic responses. The system uses GPT-4o with a specialized prompt, achieving 100% accuracy in tests.

### 3. Flight Information Handling
Processes flight-related queries by extracting key information (origin, destination, flight numbers) and matching against a flight database.

### 4. General Information (RAG Pipeline)
- **Retrieval**: Uses FAISS to find semantically similar information from the knowledge base
- **Augmentation**: Combines retrieved information with the query
- **Generation**: Produces natural, contextual responses with the RetrievalQA chain

### 5. Specialized Features
- **Cab Booking Integration**: Automatically adds cab booking links for transportation queries
- **Language Translation**: Supports multiple Indian languages including Hindi, Kannada, Tamil

## API Endpoints

```
POST /process-text
Request Body: {"query": "string"}
Response: {"query": "string", "response": "string"}

POST /process-audio
Request Body: Form data with audio file (.mp3 or .wav)
Response: {
  "transcription": "string",
  "language": "string",
  "response": "string",
  "translated_response": "string",
  "audio_response_path": "string"
}
```

## Getting Started

### Prerequisites
- Python 3.8+
- OpenAI API key
- LiteLLM configuration

### Installation

```bash

# Install requirements
pip install -r requirements.txt

# Set environment variables
export LITELLM_API_KEY="your-openai-api-key"

# Start the server
uvicorn BIAL_Chatbot:app --host 0.0.0.0 --port 8000 --reload
```

### Usage Examples

#### Text Query
```python
import requests
import json

response = requests.post(
    "http://localhost:8000/process-text",
    json={"query": "Where can I find a coffee shop at Terminal 2?"}
)
print(json.dumps(response.json(), indent=2))
```

#### Audio Query
```python
import requests

with open("question.mp3", "rb") as f:
    files = {"file": f}
    response = requests.post("http://localhost:8000/process-audio", files=files)
    
print(json.dumps(response.json(), indent=2))
```

## Future Enhancements

- Integration with real-time flight APIs
- Expansion of the knowledge base
- User feedback loop for continuous improvement
- Enhanced personalization based on user preferences

## License

[MIT License](LICENSE)


