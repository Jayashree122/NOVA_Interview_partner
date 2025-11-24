AI Interview Practice Partner
Intelligent mock interview platform powered by Llama-3.3-70B via Groq

Project Overview
An AI-powered interview preparation platform that conducts realistic mock interviews across multiple job roles. The system uses advanced conversational AI to ask relevant questions, provide intelligent follow-ups, and deliver comprehensive feedback to help users improve their interview skills.

Tech Stack: FastAPI (Backend) + Pure HTML/CSS/JS (Frontend) + Groq API (Llama-3.3-70B)
# AI Interview Practice Partner 🎤
## 📋 Table of Contents

- [Features](#features)
- [Architecture](#architecture)
- [Setup Instructions](#setup-instructions)
- [Usage Guide](#usage-guide)
- [Design Decisions](#design-decisions)
- [API Documentation](#api-documentation)
- [Troubleshooting](#troubleshooting)
- [Future Enhancements](#future-enhancements)

---

## ✨ Features

### Core Functionality
- **Role-Specific Interviews**: 6 predefined roles (Software Engineer, Sales, Manager, Retail, Marketing, Data Analyst)
- **Intelligent Question Flow**: Dynamic follow-up questions (40% probability) for realistic interview simulation
- **AI-Powered Conversations**: Uses Groq's Llama 3.3 70B model for natural, context-aware responses
- **Comprehensive Feedback**: Detailed performance analysis with strengths, weaknesses, and actionable recommendations

### User Experience
- **Voice Input**: Speech-to-text powered by Web Speech API (Indian English)
- **Text-to-Speech**: AI responses can be played with customizable voice settings
- **Real-Time Timer**: Track interview duration
- **Calendar Integration**: Add practice sessions to Google Calendar
- **Downloadable Reports**: Export transcripts and feedback as text files

### Technical Highlights
- **In-Memory Session Management**: Fast, ephemeral storage for active interviews
- **CORS-Enabled API**: Ready for frontend/backend separation
- **Error Handling**: Graceful degradation and user-friendly error messages
- **Responsive Design**: Works seamlessly on desktop and mobile devices

---

## 🏗 Architecture

### System Overview

```
┌─────────────────┐
│   Frontend      │
│  (HTML/CSS/JS)  │
│                 │
│  - Landing Page │
│  - Interview UI │
│  - Feedback View│
└────────┬────────┘
         │
         │ HTTP/JSON
         │
┌────────▼────────┐
│   FastAPI       │
│   Backend       │
│                 │
│  - Session Mgmt │
│  - API Routes   │
│  - Validation   │
└────────┬────────┘
         │
         ├─────────────┐
         │             │
┌────────▼────────┐ ┌──▼──────────┐
│  Groq API       │ │  In-Memory  │
│  (Llama 3.3)    │ │  Sessions   │
│                 │ │             │
│  - Questions    │ │  - History  │
│  - Responses    │ │  - State    │
│  - Feedback     │ │  - Config   │
└─────────────────┘ └─────────────┘
```

### Component Breakdown

#### Backend (FastAPI)
- **`main.py`**: Core application with routes and logic
- **Session Management**: Dictionary-based storage for active interviews
- **Role Configuration**: Predefined interview parameters per role
- **AI Integration**: Groq client with fallback for Python 3.13 compatibility

#### Frontend (Vanilla JS)
- **`index.html`**: Single-page application structure
- **State Management**: Global variables for session tracking
- **Voice Features**: Web Speech API integration
- **Event Handling**: Async/await pattern for API calls

#### AI Prompting Strategy
- **System Prompts**: Role-specific instructions with behavioral guidelines
- **Conversation History**: Full context passed to maintain coherence
- **Follow-Up Logic**: Probabilistic decision-making for natural flow
- **Feedback Generation**: Structured analysis prompt for comprehensive reports

---

## 🚀 Setup Instructions

### Prerequisites

- **Python**: 3.8 or higher
- **Groq API Key**: Get one from [Groq Console](https://console.groq.com)
- **Modern Browser**: Chrome or Edge (for voice features)

### Step 1: Clone Repository

```bash
git clone <repository-url>
cd ai-interview-partner
```

### Step 2: Create Virtual Environment

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

**requirements.txt contents:**
```
fastapi==0.115.5
uvicorn==0.32.1
groq==0.9.0
python-dotenv==1.0.1
pydantic==2.10.3
httpx==0.27.0
```

### Step 4: Configure Environment

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

**To get your Groq API key:**
1. Visit [console.groq.com](https://console.groq.com)
2. Sign up or log in
3. Navigate to API Keys section
4. Create new key and copy it

### Step 5: Organize Project Structure

Ensure your project structure looks like this:

```
ai-interview-partner/
├── main.py
├── requirements.txt
├── .env
├── README.md
└── static/
    └── index.html
```

Create the `static` directory if it doesn't exist:

```bash
mkdir static
```

### Step 6: Run Application

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

**Expected output:**
```
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
INFO:     Started reloader process
INFO:     Started server process
INFO:     Waiting for application startup.
INFO:     Application startup complete.
```

### Step 7: Access Application

Open your browser and navigate to:
```
http://localhost:8000
```

---

## 📖 Usage Guide

### Starting an Interview

1. **Enter Your Name** (optional)
2. **Select Interview Role** from dropdown
3. **Review Role Details**: Questions count, estimated time, description
4. **(Optional) Schedule Session**: Add to Google Calendar
5. **Click "Start Interview Now"**

### During the Interview

**Text Input:**
- Type responses in the text area
- Press `Enter` to send
- Use `Shift+Enter` for line breaks

**Voice Input:**
- Click microphone button (🎤)
- Speak clearly when "Listening..." appears
- Voice will auto-stop after pause
- Review transcribed text before sending

**Voice Output:**
- AI responses can auto-play (toggle in settings)
- Click "Play" button on any AI message
- Adjust voice gender and speech rate in settings

**Interview Flow:**
- Answer each question thoughtfully
- Expect occasional follow-up questions
- Monitor remaining questions counter
- End early with "End Interview" button

### After Interview

**Feedback Report includes:**
- Overall Performance Score (X/10)
- Strengths with specific examples
- Areas for improvement
- Actionable recommendations
- Notable responses analysis

**Export Options:**
- Download transcript (formatted conversation)
- Download feedback (analysis report)
- Start new interview session

---

## 🎯 Design Decisions

### 1. In-Memory Session Storage

**Decision**: Use Python dictionary for session management instead of database

**Rationale**:
- Interview sessions are ephemeral (typically 30 minutes)
- No need for long-term data persistence
- Faster read/write operations
- Simpler deployment (no database setup required)
- Adequate for single-instance deployment

**Trade-offs**:
- Sessions lost on server restart
- Cannot scale horizontally without shared storage
- Not suitable for production with multiple servers

**Future**: Consider Redis for persistent sessions in production

### 2. Groq API with Llama 3.3 70B

**Decision**: Use Groq's inference platform with Llama 3.3 70B Versatile model

**Rationale**:
- Extremely fast inference (up to 800 tokens/second)
- Strong conversational capabilities
- Cost-effective compared to GPT-4
- Reliable API with good documentation
- Python 3.13 compatibility handled with httpx fallback

**Parameters**:
- `temperature: 0.8` - Balanced creativity and consistency
- `max_tokens: 500` - Prevents overly long responses
- `model: llama-3.3-70b-versatile` - Latest stable version

### 3. Probabilistic Follow-Up Questions

**Decision**: 40% chance of follow-up after candidate responses

**Rationale**:
- Real interviews don't follow up on every answer
- Prevents interview from feeling scripted
- Adds natural unpredictability
- Keeps candidate engaged
- Simulates interviewer's selective probing

**Implementation**:
```python
def should_ask_followup() -> bool:
    return random.random() < 0.4  # 40% chance
```

### 4. Web Speech API for Voice Features

**Decision**: Use browser-native Web Speech API instead of external services

**Rationale**:
- No additional API costs
- Low latency (local processing)
- Works offline for speech recognition
- Good accuracy for Indian English
- No server-side processing required

**Limitations**:
- Chrome/Edge only (webkit-based browsers)
- Requires HTTPS in production
- Limited control over voice quality

### 5. Single-Page Application (SPA)

**Decision**: Build as SPA with three distinct views instead of multi-page app

**Rationale**:
- Seamless transitions between stages
- No page reloads (better UX)
- Simpler state management
- Faster perceived performance
- Easier to maintain conversation context

**Structure**:
```
Landing Page → Interview Page → Feedback Page
```

### 6. Role-Specific Configuration

**Decision**: Hardcode role configurations instead of database-driven

**Rationale**:
- Fixed set of common interview roles
- Easy to modify in code
- No database queries needed
- Type-safe with Pydantic validation
- Clear documentation in code

**Configuration includes**:
- Role name and description
- Number of questions
- Estimated duration
- System prompt customization

### 7. Comprehensive System Prompts

**Decision**: Detailed system prompts with explicit behavioral instructions

**Rationale**:
- Ensures consistent interviewer behavior
- Prevents AI from giving feedback too early
- Guides natural conversation flow
- Handles edge cases (brief answers, confusion, off-topic)
- Maintains professional tone

**Key Instructions**:
- Ask ONE question at a time
- Random follow-ups (not after every answer)
- Redirect if off-topic
- Encourage elaboration on brief answers
- No feedback during interview

### 8. Feedback Generation Approach

**Decision**: Generate feedback after interview completion using full transcript

**Rationale**:
- Provides holistic performance analysis
- Can identify patterns across responses
- More detailed and actionable insights
- Worth the wait time for quality
- Uses full context window effectively

**Feedback Structure**:
1. Overall Performance Score
2. Strengths (with examples)
3. Areas for Improvement
4. Specific Recommendations
5. Notable Responses

### 9. Error Handling Strategy

**Decision**: Multi-layered error handling with user-friendly messages

**Levels**:
- **Backend**: Try-catch blocks with specific error types
- **Frontend**: Async/await with catch blocks
- **User Feedback**: Clear, actionable error messages
- **Graceful Degradation**: System continues working partially

**Examples**:
```python
# Backend
except Exception as e:
    raise HTTPException(status_code=500, detail=f"AI Error: {str(e)}")

# Frontend
catch (error) {
    addMessage('ai', 'I apologize, but I encountered an error. Please try again.');
}
```

### 10. No Authentication Required

**Decision**: Open access without user accounts

**Rationale**:
- Lower barrier to entry
- Faster time-to-value
- No privacy concerns (no data storage)
- Simplified architecture
- Focus on core functionality

**Considerations**:
- Add authentication for production deployment
- Implement rate limiting per IP
- Track usage for cost management

---

## 📡 API Documentation

### Base URL
```
http://localhost:8000
```

### Endpoints

#### 1. Get Available Roles
```http
GET /api/roles
```

**Response:**
```json
{
  "roles": {
    "software_engineer": {
      "name": "Software Engineer",
      "num_questions": 7,
      "estimated_time": 30,
      "description": "Technical interview focusing on..."
    },
    ...
  }
}
```

#### 2. Start Interview
```http
POST /api/interview/start
Content-Type: application/json

{
  "role": "software_engineer",
  "user_name": "John Doe"
}
```

**Response:**
```json
{
  "session_id": "session_20231124_143052_1234",
  "first_question": "Let's start with...",
  "role_info": {
    "name": "Software Engineer",
    "num_questions": 7,
    "estimated_time": 30,
    "description": "..."
  }
}
```

#### 3. Send Message
```http
POST /api/interview/message
Content-Type: application/json

{
  "session_id": "session_20231124_143052_1234",
  "message": "I have 5 years of experience..."
}
```

**Response:**
```json
{
  "response": "That's great! Can you tell me about...",
  "is_complete": false,
  "questions_remaining": 5
}
```

**When interview completes:**
```json
{
  "response": "Thank you for your time...",
  "is_complete": true,
  "feedback": "## OVERALL PERFORMANCE..."
}
```

#### 4. End Interview Early
```http
POST /api/interview/end?session_id=session_20231124_143052_1234
```

**Response:**
```json
{
  "feedback": "## OVERALL PERFORMANCE...",
  "transcript": [
    {"role": "assistant", "content": "..."},
    {"role": "user", "content": "..."}
  ]
}
```

#### 5. Get Transcript
```http
GET /api/interview/transcript/{session_id}
```

**Response:**
```json
{
  "transcript": "AI INTERVIEW PRACTICE - TRANSCRIPT\n...",
  "session_info": {
    "role": "Software Engineer",
    "user_name": "John Doe",
    "start_time": "2023-11-24T14:30:52.123456",
    "questions_asked": 7
  }
}
```

### Error Responses

**400 Bad Request:**
```json
{
  "detail": "Invalid role selected"
}
```

**404 Not Found:**
```json
{
  "detail": "Session not found"
}
```

**500 Internal Server Error:**
```json
{
  "detail": "AI Error: Connection timeout"
}
```

---

## 🔧 Troubleshooting

### Voice Recognition Issues

**Problem**: "Microphone access denied"
- **Solution**: Allow microphone permissions in browser settings
- Chrome: `chrome://settings/content/microphone`
- Enable for `localhost`

**Problem**: Voice recognition not available
- **Solution**: Use Chrome or Edge browsers
- Safari and Firefox have limited support

**Problem**: Voice cuts off too early
- **Solution**: Speak continuously without long pauses
- Recognition stops after ~3 seconds of silence

### Text-to-Speech Issues

**Problem**: No voice output
- **Solution**: Check volume settings
- Ensure voice toggle is not muted (speaker icon)
- Try different voice gender in settings

**Problem**: Voice sounds robotic
- **Solution**: Adjust speech rate to 0.8x or 0.9x
- Indian English voices may vary by browser

### API/Backend Issues

**Problem**: "Session not found" errors
- **Solution**: Server may have restarted (in-memory storage)
- Start a new interview session

**Problem**: "AI Error: Connection timeout"
- **Solution**: Check internet connection
- Verify Groq API key in `.env` file
- Check Groq API status at status.groq.com

**Problem**: Slow AI responses
- **Solution**: Groq typically responds in <2 seconds
- Check your internet speed
- Try restarting the server

### General Issues

**Problem**: Static files not loading
- **Solution**: Ensure `static/` directory exists
- Verify `index.html` is in `static/` folder
- Check console for 404 errors

**Problem**: Interview won't start
- **Solution**: Check browser console for errors
- Verify role is selected
- Refresh page and try again

**Problem**: Can't download transcript/feedback
- **Solution**: Check browser's download settings
- Allow downloads from localhost
- Try different browser

---

## 🚀 Future Enhancements

### Short-Term (v1.1)
- [ ] **Persistent Storage**: Migrate to SQLite or PostgreSQL
- [ ] **User Authentication**: Add login/signup functionality
- [ ] **Interview History**: View past sessions and feedback
- [ ] **Video Recording**: Optional video capture of practice sessions
- [ ] **More Roles**: Add 10+ additional interview roles

### Medium-Term (v1.5)
- [ ] **Custom Questions**: Allow users to add custom interview questions
- [ ] **Difficulty Levels**: Easy, Medium, Hard question sets
- [ ] **Multi-Language Support**: Support for 5+ languages
- [ ] **Resume Upload**: Tailor questions based on resume content
- [ ] **Team Accounts**: Organization accounts with team analytics

### Long-Term (v2.0)
- [ ] **AI Video Interviewer**: Avatar-based video interviews
- [ ] **Emotion Analysis**: Detect confidence levels and body language
- [ ] **Industry-Specific**: Domain-specific question banks (FinTech, Healthcare, etc.)
- [ ] **Interview Coaching**: Pre-interview tips and strategies
- [ ] **Job Matching**: Connect with real job opportunities
- [ ] **Mobile Apps**: Native iOS and Android applications

---

## 📝 Configuration Reference

### Environment Variables

| Variable | Required | Description | Example |
|----------|----------|-------------|---------|
| `GROQ_API_KEY` | Yes | Groq API authentication key | `gsk_xxxxx...` |

### Role Configuration Structure

```python
ROLE_CONFIGS = {
    "role_key": {
        "name": str,           # Display name
        "num_questions": int,  # Number of main questions
        "estimated_time": int, # Minutes
        "description": str     # Role description
    }
}
```

### System Prompt Guidelines

When modifying system prompts:
- Keep instructions clear and numbered
- Include edge case handling
- Specify desired tone explicitly
- Add examples for complex behaviors
- Test with various user inputs

---

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

**Code Style:**
- Follow PEP 8 for Python
- Use meaningful variable names
- Add comments for complex logic
- Update README for new features

*Last updated: November 24, 2024*

