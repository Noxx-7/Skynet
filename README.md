# 🚀 SKYNET - LLM IDE Platform

![Version](https://img.shields.io/badge/version-2.0.0-blue)
![Status](https://img.shields.io/badge/status-production%20ready-brightgreen)
![No Login](https://img.shields.io/badge/access-no%20login%20required-orange)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

A comprehensive web-based IDE platform for testing, developing, and collaborating with Large Language Models. Test API-based models (OpenAI, Claude, Gemini) or upload custom models—all without requiring high-performance hardware or authentication.

**🎉 No login required—jump right in and start building!**
<img width="1450" height="775" alt="homepage" src="https://github.com/user-attachments/assets/c30e48a5-8b68-4c2a-ba93-79758385f525" />
<img width="500" height="286" alt="chat" src="https://github.com/user-attachments/assets/897e4038-8d2b-41eb-80ce-1076516f1b1a" />
<img width="500" height="285" alt="COde" src="https://github.com/user-attachments/assets/44265bb0-f8b1-4f23-bec7-4835b64149e7" />




---

## 📖 Overview

SKYNET is a comprehensive web-based IDE platform that enables users to interact with multiple LLM providers (OpenAI, Anthropic, Google Gemini), upload custom models, execute and analyze Python code, and collaborate in real-time. The system is designed with a no-authentication approach, making it immediately accessible while providing session-based API key management for LLM provider configuration.

---

## ✨ Key Features

### 🤖 Multi-Model LLM Support
- **OpenAI**: GPT-4o, GPT-4o-mini, GPT-4-turbo, GPT-3.5-turbo
- **Anthropic**: Claude 3.5 Sonnet, Claude 3 Opus/Sonnet/Haiku
- **Google**: Gemini 1.5 Pro/Flash/Flash-8B, Gemini 1.0 Pro
- Custom model upload capability
- Model Registry with centralized definitions
- Session-based API key management with Fernet encryption

### 💻 Advanced Python Code Editor
- **Monaco Editor**: Professional IDE experience
- **AI Suggestions**: Improve, Complete, Optimize, Debug, Refactor
- **Auto Unit Test Generation**: Comprehensive test suite creation
- **Performance Profiling**: Execution time, memory, CPU metrics
- **Code Analysis**: AST-based complexity and quality scoring
- **Session Management**: Save and load coding sessions

### 👥 Real-Time Collaboration
- WebSocket-powered live sessions
- Session sharing and synchronization
- Integrated chat communication
- Live code editing
- Zero authentication barriers

### ☁️ Cloud Architecture
- No GPU requirements
- FastAPI with async/await
- PostgreSQL + Supabase dual database
- Docker-ready deployment
- Scalable and production-ready

---

## 🏗️ System Architecture

```text
Frontend (Port 5000) - Next.js 15.5.6 + TypeScript
├── Homepage (SSR)
├── Playground (Chat UI)
└── Code Editor (Monaco + AI)
    ↓ REST API / WebSocket
Backend (Port 8000) - FastAPI + Python 3.11
├── Router Layer
│   ├── api_routes_no_auth.py
│   ├── api_routes_env.py
│   └── api_routes_keys.py
├── LLM Layer
│   ├── skynet_providers.py
│   ├── ModelRegistry
│   └── SkynetProviderFactory
├── Code Testing Layer
│   ├── CodeAnalyzer
│   ├── CodeProfiler
│   └── UnitTestGenerator
└── Collaboration Layer
    └── WebSocket Manager
    ↓
Data Layer
├── PostgreSQL (Primary DB)
└── Supabase (Chat History)
```

---

## 📊 Recent Updates (October 2025)

### Fixed Issues ✅
- **Chat History**: Fixed complete persistence and API routing
- **Database**: Resolved foreign key constraints and migration issues
- **Model Selection**: Shows only available models based on API keys
- **AI Suggestions**: Enhanced with multiple suggestion types
- **API Key Management**: Fixed race conditions and status display

### Enhancements 🚀
- No authentication system for instant access
- Session-based API key storage
- Enhanced WebSocket stability
- Improved code editor with AI integration

---

## 🚀 Installation & Setup

### Prerequisites
- Python 3.11+
- Node.js 18+
- PostgreSQL 15+
- Docker (optional)

### Step 1: Clone Repository
```bash
git clone <your-repo-url>
cd SKYNET
```

### Step 2: Environment Variables
Create `.env` file:
```bash
DATABASE_URL=postgresql://user:pass@localhost:5432/skynet
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_SUPABASE_ANON_KEY=your-key
ENCRYPTION_KEY=<generated-fernet-key>
JWT_SECRET_KEY=your-secret
BACKEND_URL=http://localhost:8000
```

Generate encryption key:
```bash
python -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"
```

### Step 3: Backend Setup
```bash
cd SKYNET/backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python main.py
```

**requirements.txt:**
```txt
fastapi==0.104.1
uvicorn[standard]==0.24.0
sqlalchemy==2.0.23
psycopg2-binary==2.9.9
supabase==2.3.4
pydantic==2.5.0
python-jose[cryptography]==3.3.0
passlib[bcrypt]==1.7.4
bcrypt==4.1.2
cryptography==41.0.7
openai==1.6.1
anthropic==0.8.1
google-generativeai==0.3.1
websockets==12.0
pytest==7.4.3
pytest-asyncio==0.21.1
httpx==0.25.2
python-multipart==0.0.6
```

### Step 4: Frontend Setup
```bash
cd SKYNET/frontend
npm install
BACKEND_URL=http://localhost:8000 npm run dev
```

**package.json:**
```json
{
  "name": "skynet-frontend",
  "version": "2.0.0",
  "dependencies": {
    "next": "15.5.6",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "typescript": "^5.3.3",
    "@monaco-editor/react": "^4.6.0",
    "zustand": "^4.4.7",
    "tailwindcss": "^3.3.6",
    "axios": "^1.6.2",
    "@supabase/supabase-js": "^2.39.0"
  }
}
```

### Access Points
- Frontend: http://localhost:5000
- Backend API: http://localhost:8000
- API Docs: http://localhost:8000/docs

---

## 🐳 Docker Deployment

**docker-compose.yml:**
```yaml
version: '3.8'
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_USER: ${PGUSER}
      POSTGRES_PASSWORD: ${PGPASSWORD}
      POSTGRES_DB: ${PGDATABASE}
    ports: ["5432:5432"]
    volumes: [postgres_data:/var/lib/postgresql/data]
    
  backend:
    build: ./backend
    container_name: llm_backend
    ports: ["8000:8000"]
    environment:
      DATABASE_URL: postgresql://${PGUSER}:${PGPASSWORD}@postgres:5432/${PGDATABASE}
    depends_on: [postgres]
    
  frontend:
    build: ./frontend
    ports: ["5000:5000"]
    environment:
      BACKEND_URL: http://llm_backend:8000
    depends_on: [backend]

volumes:
  postgres_data:
```

Run: `docker-compose up -d`

---

## 📡 API Reference

### LLM Operations
- GET `/llm/available-models` - List models
- POST `/llm/api-keys` - Add API keys
- POST `/llm/generate` - Generate responses
- POST `/llm/stream` - Stream responses

### Code Testing
- POST `/code/execute` - Execute code
- POST `/code/generate-tests` - Auto-generate tests
- POST `/code/analyze` - Code analysis
- POST `/code/profile` - Performance profiling
- POST `/code/suggest` - AI suggestions

### Collaboration
- POST `/collaboration/create` - Create session
- WS `/ws/{session_id}` - WebSocket connection

### Chat History
- GET `/api/chat-history` - Get history
- POST `/api/chat-history` - Save chat
- DELETE `/api/chat-history/{id}` - Delete chat

---

## 🧪 API Testing

List models:
```bash
curl http://localhost:8000/llm/available-models
```

Execute code:
```bash
curl -X POST http://localhost:8000/code/execute \
  -H "Content-Type: application/json" \
  -d '{"code": "print(\"Hello\")", "language": "python"}'
```

AI suggestions:
```bash
curl -X POST http://localhost:8000/code/suggest \
  -H "Content-Type: application/json" \
  -d '{"code": "def add(a,b): return a+b", "suggestion_type": "optimize"}'
```

---

## 🔒 Security Features
- Fernet encryption for API keys
- Session-based storage
- CORS protection
- Pydantic input validation
- SQL injection prevention
- WebSocket session validation

---

## 🛠️ Technology Stack

### Frontend
- Next.js 15.5.6 + React 18 + TypeScript
- Monaco Editor 0.45.0
- Zustand 4.4.7 + Tailwind CSS
- Supabase JS 2.39.0

### Backend
- FastAPI 0.104.1 + Python 3.11
- SQLAlchemy 2.0.23 + PostgreSQL
- OpenAI 1.6.1 + Anthropic 0.8.1 + Google AI 0.3.1
- WebSockets 12.0

---

## 🐛 Troubleshooting

**Port conflicts:**
```bash
lsof -ti:8000 | xargs kill -9
lsof -ti:5000 | xargs kill -9
```

**Database issues:**
```bash
psql -U user -d skynet
dropdb skynet && createdb skynet
```

**Dependencies:**
```bash
pip install -r requirements.txt --force-reinstall
rm -rf node_modules && npm install
```

**Docker:**
```bash
docker-compose down -v
docker-compose up --build
```

---

## 📝 Notes
- Platform runs in cloud environment
- PostgreSQL + Supabase dual database
- Frontend: Port 5000 | Backend: Port 8000
- No authentication required
- Session-based API key management

---

## 🤝 Contributing
1. Fork the repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Open Pull Request

---

## 📄 License
MIT License - See LICENSE file for details

---

**Built with ❤️ for the AI development community**

*Version 2.0.0 | Production Ready | No Login Required*
