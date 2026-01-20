# AI Voice-to-Voice Interviewer 🎙️🤖

An intelligent AI-powered interview platform that conducts real-time voice interviews, analyzes candidate responses, and provides comprehensive assessments. Built with LangChain, OpenAI, Deepgram, and modern web technologies.

## 🌟 Overview

This project is a full-stack AI interviewing assistant that leverages cutting-edge language models and voice technology to conduct realistic, context-aware interviews. The system parses resumes, generates tailored questions, conducts voice-based interviews in real-time, and provides detailed candidate assessments.

### Key Features

- 🎤 **Real-Time Voice Interviews**: WebSocket-based voice interviews with live speech-to-text transcription using Deepgram
- 🧠 **AI-Powered Question Generation**: Dynamic question generation based on resume analysis using LangChain and OpenAI
- 📄 **Resume Intelligence**: Advanced resume parsing and semantic analysis with vector embeddings (FAISS)
- 📊 **Comprehensive Assessment**: Automated candidate evaluation with detailed scoring across multiple dimensions
- 💼 **Job Matching**: Intelligent job recommendation using semantic similarity and embeddings
- 📈 **ATS Score Analysis**: Applicant Tracking System compatibility scoring for resumes
- 📚 **Interview History**: Track and review past interviews with detailed analytics
- 🔐 **Secure Authentication**: JWT-based authentication and user management

## 🏗️ Architecture

### System Flow

![Interview Flow](Flowchart.jpg)

The system follows this workflow:

1. **User Registration & Resume Upload**: User creates account and uploads resume (PDF/DOCX)
2. **Resume Processing**: Backend extracts text, generates embeddings, and stores in vector database
3. **Interview Initialization**: LangChain agent loads resume context and determines interview approach
4. **Voice Interview Loop**: Real-time WebSocket connection streams audio, transcribes speech, evaluates responses
5. **AI Question Generation**: Agent dynamically generates follow-up questions based on candidate answers
6. **Assessment Generation**: Final evaluation with technical proficiency, communication, and improvement recommendations
7. **Results Storage**: Interview data and metrics stored in MongoDB for analytics

### Tech Stack

#### Backend
- **Framework**: FastAPI (Python) - High-performance async web framework
- **AI/LLM Stack**:
  - LangChain - Orchestration framework for LLM applications
  - OpenAI GPT-4 - Language model for question generation and assessment
  - FAISS - Vector database for semantic search
  - LangChain Text Splitters - Document chunking and processing
- **Voice Processing**:
  - Deepgram - Real-time speech-to-text streaming API
  - WebSocket - Bidirectional audio streaming
- **Database**: MongoDB (Motor - async driver)
- **Authentication**: JWT (PyJWT)
- **Additional Tools**:
  - PyPDF - PDF resume parsing
  - Pydantic - Data validation and settings management
  - python-dotenv - Environment configuration

#### Frontend
- **Framework**: React 19.2 with Vite
- **State Management**: Redux Toolkit + Redux Saga
- **Routing**: React Router DOM
- **Styling**: TailwindCSS 4.1
- **Real-time Communication**: Socket.IO Client
- **HTTP Client**: Axios
- **UI Components**: Lucide React icons

## 📂 Project Structure

```
ai-interviewer/
├── backend/
│   ├── app/
│   │   ├── main.py                 # FastAPI application entry point
│   │   ├── config.py               # Configuration and environment variables
│   │   ├── routers/
│   │   │   ├── auth.py             # Authentication endpoints
│   │   │   ├── interview.py       # Text-based interview endpoints
│   │   │   ├── voice_interview.py # WebSocket voice interview handler
│   │   │   ├── resume.py           # Resume upload and parsing
│   │   │   ├── results.py          # Interview results and analytics
│   │   │   ├── jobs.py             # Job board and matching
│   │   │   └── ats.py              # ATS scoring endpoints
│   │   ├── services/
│   │   │   ├── ai_agent_client.py  # LangChain AI agent integration
│   │   │   ├── realtime_stt.py     # Deepgram speech-to-text service
│   │   │   ├── voice_session_manager.py # Voice interview session management
│   │   │   ├── job_embeddings.py   # Job matching with embeddings
│   │   │   └── Similarity_Jobs.py  # Semantic job similarity
│   │   ├── utils/
│   │   │   ├── ats_scorer.py       # ATS compatibility scoring
│   │   │   ├── jwt_utils.py        # JWT token utilities
│   │   │   └── auth_middleware.py  # Authentication middleware
│   │   ├── schemas/                # Pydantic request/response schemas
│   │   └── db/
│   │       └── mongo_clients.py    # MongoDB connection management
│   └── ai-agent/
│       ├── app.py                  # Standalone LangChain agent service
│       ├── requirements.txt        # Python dependencies
│       └── README.md               # AI agent documentation
├── frontend/
│   └── Chat_Agent/
│       ├── src/
│       │   ├── App.jsx             # Main application component
│       │   ├── main.jsx            # React entry point
│       │   ├── pages/
│       │   │   ├── LoginPage.jsx
│       │   │   ├── SignupPage.jsx
│       │   │   ├── Dashboard.jsx
│       │   │   ├── ResumeUpload.jsx
│       │   │   ├── ResumeManagement.jsx
│       │   │   ├── VoiceInterviewPage.jsx # Real-time voice interview UI
│       │   │   ├── InterviewPage.jsx      # Text-based interview
│       │   │   ├── InterviewResults.jsx
│       │   │   ├── InterviewHistory.jsx
│       │   │   ├── JobBoard.jsx
│       │   │   ├── JobDetail.jsx
│       │   │   ├── ATSScore.jsx
│       │   │   └── Analytics.jsx
│       │   └── components/
│       │       ├── AudioWaveform.jsx      # Audio visualization
│       │       ├── CircularWaveform.jsx
│       │       └── DeepgramHoop.jsx
│       ├── package.json
│       └── vite.config.js
├── Flowchart.jpg                   # System architecture diagram
└── README.md                       # This file
```

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- Node.js 18+
- MongoDB (local or Atlas)
- OpenAI API Key
- Deepgram API Key

### Backend Setup

1. **Navigate to backend directory**:
```bash
cd ai-interviewer/backend
```

2. **Install Python dependencies**:
```bash
pip install -r ai-agent/requirements.txt
```

3. **Configure environment variables**:

Create `.env` file in the `backend/` directory:

```env
# OpenAI Configuration
OPENAI_API_KEY=your_openai_api_key_here

# Deepgram Configuration (for voice interviews)
DEEPGRAM_API_KEY=your_deepgram_api_key_here
STT_PROVIDER=deepgram

# MongoDB Configuration
MONGODB_URI=mongodb://localhost:27017
MONGODB_DB_NAME=ai_interviewer

# JWT Configuration
JWT_SECRET=your_secret_key_here
JWT_ALGORITHM=HS256
JWT_EXPIRATION_HOURS=24

# Server Configuration
PORT=8000
ENVIRONMENT=development
```

4. **Run the backend server**:
```bash
cd app
uvicorn main:app --reload --port 8000
```

The API will be available at `http://localhost:8000`

### AI Agent Service Setup (Optional Standalone)

The AI agent can also run as a separate service:

```bash
cd backend/ai-agent
python app.py
```

This runs the LangChain agent on port 5000 with API documentation at `http://localhost:5000/docs`

### Frontend Setup

1. **Navigate to frontend directory**:
```bash
cd ai-interviewer/frontend/Chat_Agent
```

2. **Install dependencies**:
```bash
npm install
```

3. **Configure API endpoints**:

Update API base URL in your frontend configuration if needed (default: `http://localhost:8000`)

4. **Run the development server**:
```bash
npm run dev
```

The app will be available at `http://localhost:5173`

### Building for Production

**Backend**:
```bash
# Run with production settings
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

**Frontend**:
```bash
npm run build
npm run preview
```

## 🔌 API Endpoints

### Authentication
- `POST /api/auth/signup` - User registration
- `POST /api/auth/login` - User login
- `GET /api/auth/me` - Get current user

### Resume Management
- `POST /api/resume/upload` - Upload and parse resume
- `GET /api/resume/{user_id}` - Get user's resume
- `PUT /api/resume/{resume_id}` - Update resume
- `DELETE /api/resume/{resume_id}` - Delete resume

### Interviews
- `POST /api/interview/init` - Initialize text-based interview
- `POST /api/interview/answer` - Submit answer and get next question
- `POST /api/interview/complete` - Complete interview and get assessment

### Voice Interviews
- `WS /api/ws/voice-interview/{session_id}` - WebSocket for real-time voice interview
- `GET /api/voice-interview/status/{session_id}` - Get session status

### Results & Analytics
- `GET /api/results/{user_id}` - Get user's interview results
- `GET /api/results/interview/{interview_id}` - Get specific interview details

### Job Matching
- `GET /api/jobs` - Get available jobs
- `GET /api/jobs/{job_id}` - Get job details
- `POST /api/jobs/match` - Get job recommendations based on resume
- `POST /api/jobs/{job_id}/apply` - Apply to a job

### ATS Scoring
- `POST /api/ats/score` - Get ATS compatibility score for resume

## 🎯 How It Works

### 1. Resume Analysis with LangChain

When a user uploads their resume:
1. PDF/DOCX is parsed into text
2. Text is chunked using LangChain's text splitters
3. Embeddings are generated using OpenAI embeddings
4. Vectors are stored in FAISS for semantic search
5. Resume profile (name, skills, experience, seniority) is extracted using LLM

### 2. AI Interview Agent

The LangChain-powered agent:
- Loads resume context from vector store
- Determines candidate's field of study and difficulty level
- Generates contextually relevant questions
- Evaluates responses for technical accuracy, clarity, and relevance
- Adapts follow-up questions based on previous answers
- Stores conversation history with metrics (confidence, clarity, relevance)

### 3. Real-Time Voice Interview

The WebSocket-based voice interview:
1. Client connects via WebSocket with session ID
2. Browser captures audio from microphone
3. Audio chunks stream to backend
4. Deepgram streams real-time transcription
5. On speech completion (utterance end), AI agent:
   - Evaluates the transcribed answer
   - Generates the next question
   - Sends question back to client
6. Cycle repeats for 3-5 questions
7. Final assessment generated with detailed feedback

### 4. Assessment Generation

Final assessment includes:
- **Overall Rating**: Score out of 10
- **Technical Proficiency**: Evaluation of technical knowledge
- **Communication Score**: Clarity and articulation
- **Role Fit Analysis**: Suitability for position
- **Strengths**: Key positive points
- **Weaknesses**: Areas needing improvement
- **Personalized Recommendations**: Specific advice for improvement

### 5. Job Matching Algorithm

Semantic job matching:
1. Job descriptions are converted to embeddings
2. Resume is converted to embedding
3. Cosine similarity computed between resume and all jobs
4. Jobs ranked by similarity score
5. Top matches returned with relevance scores

## 🔐 Security Features

- **JWT Authentication**: Secure token-based authentication
- **Password Hashing**: Secure password storage (if implemented)
- **CORS Protection**: Configurable CORS middleware
- **Input Validation**: Pydantic schemas validate all requests
- **Environment Variables**: Sensitive data stored in `.env`

## 🧪 Testing

Run the backend API:
```bash
# Check health endpoint
curl http://localhost:8000/

# Access interactive API docs
open http://localhost:8000/docs
```

Test WebSocket connection:
```javascript
const ws = new WebSocket('ws://localhost:8000/api/ws/voice-interview/test-session');
ws.onmessage = (event) => console.log('Received:', event.data);
```

## 📊 Database Schema

### MongoDB Collections

**users**:
```json
{
  "_id": "ObjectId",
  "email": "string",
  "name": "string",
  "password_hash": "string",
  "created_at": "datetime"
}
```

**resumes**:
```json
{
  "_id": "ObjectId",
  "user_id": "string",
  "resume_text": "string",
  "profile": {
    "name": "string",
    "email": "string",
    "skills": ["array"],
    "experience": "string",
    "seniority_level": "string"
  },
  "embeddings": ["array"],
  "created_at": "datetime",
  "updated_at": "datetime"
}
```

**interviews**:
```json
{
  "_id": "ObjectId",
  "session_id": "string",
  "user_id": "string",
  "type": "text|voice",
  "questions": ["array"],
  "answers": ["array"],
  "assessment": {
    "overall_rating": "number",
    "technical_score": "number",
    "communication_score": "number",
    "strengths": ["array"],
    "weaknesses": ["array"],
    "recommendations": "string"
  },
  "created_at": "datetime",
  "completed_at": "datetime"
}
```

## 🛠️ Technologies Explained

### LangChain
LangChain orchestrates the AI interview agent by:
- Managing conversation memory and context
- Chaining multiple LLM calls for complex reasoning
- Integrating vector stores for semantic resume search
- Providing structured output for consistent responses

### OpenAI GPT-4
Powers the intelligence behind:
- Resume parsing and entity extraction
- Dynamic question generation
- Answer evaluation and scoring
- Natural language assessment generation

### FAISS (Facebook AI Similarity Search)
Enables:
- Fast semantic search over resume content
- Efficient similarity matching for job recommendations
- In-memory vector storage for quick retrieval

### Deepgram
Provides:
- Real-time audio transcription with low latency
- Utterance detection for natural conversation flow
- High-accuracy speech recognition optimized for interviews

### WebSocket
Enables:
- Bidirectional audio streaming
- Real-time question delivery
- Live transcription updates
- Low-latency communication

## 📝 Development Notes

### Adding New Interview Questions

Modify the LangChain agent prompts in `backend/app/services/ai_agent_client.py` or `backend/ai-agent/app.py` to customize question generation logic.

### Customizing Assessment Criteria

Update the assessment prompt in the AI agent to include additional evaluation dimensions or modify scoring algorithms.

### Extending Job Matching

Add more sophisticated matching algorithms by modifying `backend/app/services/job_embeddings.py` to include factors beyond semantic similarity (e.g., location, experience level, salary).

## 🐛 Troubleshooting

### WebSocket Connection Failed
- Ensure backend is running on the correct port
- Check CORS configuration in `backend/app/main.py`
- Verify WebSocket URL in frontend configuration

### Deepgram Transcription Issues
- Verify `DEEPGRAM_API_KEY` is set correctly
- Check audio format (should be linear16, 16kHz, mono)
- Review Deepgram console for API usage limits

### OpenAI API Errors
- Confirm `OPENAI_API_KEY` is valid and has quota
- Check for rate limiting (consider implementing retry logic)
- Verify model availability (e.g., gpt-4, gpt-3.5-turbo)

### MongoDB Connection Issues
- Ensure MongoDB is running (local or Atlas)
- Verify `MONGODB_URI` in `.env`
- Check network connectivity and authentication

## 🚧 Future Enhancements

- [ ] Multi-language support for international candidates
- [ ] Video interview capability with facial expression analysis
- [ ] Integration with LinkedIn for resume import
- [ ] Advanced analytics dashboard with performance metrics
- [ ] Collaborative interview mode (multiple interviewers)
- [ ] Custom interview templates by role/industry
- [ ] Mobile app (React Native)
- [ ] AI-powered mock interview practice mode
- [ ] Integration with HR management systems (Workday, BambooHR)
- [ ] Enhanced ATS optimization recommendations

## 👨‍💻 Author

**Dhruv Arora**

## 📄 License

This project is available for educational and commercial use.

## 🙏 Acknowledgments

- OpenAI for GPT-4 and embedding models
- LangChain for the orchestration framework
- Deepgram for real-time speech-to-text
- FastAPI for the excellent async framework
- React team for the frontend library

---

**Note**: Remember to keep your API keys secure and never commit them to version control. Use environment variables for all sensitive configuration.
