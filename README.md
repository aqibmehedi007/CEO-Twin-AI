# CEO Twin AI 🤖

> An AI-powered digital assistant that acts as a "twin" of the CEO, serving as a secure knowledge repository and proxy communicator.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Node.js](https://img.shields.io/badge/node.js-16+-green.svg)](https://nodejs.org/)

## 📋 Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Getting Started](#getting-started)
- [Usage Examples](#usage-examples)
- [API Documentation](#api-documentation)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

CEO Twin AI is an intelligent digital assistant designed to help CEOs manage personal and team information securely. It serves as a memory aid, team proxy, and intelligent escalation system, ensuring sensitive data remains protected while enabling efficient team collaboration.

### Core Benefits

- **🧠 Memory Aid**: Stores and recalls sensitive data privately for the CEO
- **👥 Team Proxy**: Allows team access to shared info without exposing private details
- **🚨 Intelligent Escalation**: Flags low-confidence responses and notifies the CEO for review
- **🔒 Security Focus**: Uses layered access controls to protect data

## ✨ Key Features

### Data Management
- **📁 File Uploads**: Support for documents, PDFs, images, and credential files
- **🔐 Access Layers**:
  - **Public Layer**: Shareable with specific team members or the whole team (e.g., project timelines)
  - **Private Layer**: CEO-only access (e.g., passwords, sensitive credentials)
- **🔍 Indexing**: AI automatically indexes content for semantic search and retrieval

### Query and Response Handling
- **💬 Natural Language Interaction**: Users ask questions via chatbot (e.g., "What's the KPCards admin password?")
- **📊 Confidence Scoring**: AI evaluates response certainty (0-100%)
  - ≥60%: Direct response
  - <60%: Tentative answer (if possible), logs query, and escalates to CEO when online
- **🧠 Context Awareness**: Retains conversation history for better responses

### Collaboration and Availability
- **👥 Account Sharing**: CEO invites team members to the chatbot
- **🔄 Proxy Mode**: Handles team queries when CEO is unavailable, using only public data
- **📧 Notifications**: Alerts CEO via email/app for escalations or reviews

### Security and Auditing
- 🔐 Encrypted storage and role-based permissions
- 📝 Query logs for CEO review
- 🚫 No team access to private data

## 🏗️ System Architecture

The architecture is modular, cloud-based, and scalable, separating user-facing components from backend processing for security and efficiency.

```
+-------------------+       +-------------------+
|   User Interface  |       |  Notifications    |
| (Chatbot/Web/App) |       | (Email/App Alerts)|
+-------------------+       +-------------------+
           | ^                            ^
           | | Queries/Responses          | Escalations
           v |                            |
+-------------------+       +-------------------+
|     API Server    | <---> |   AI Engine     |
| (Handles Requests)|       | (LLM for NLP,    |
+-------------------+       |  Confidence Score)|
           ^ |              +-------------------+
           | | Data Access           ^
           | v                       |
+-------------------+       +-------------------+
|     Database      |       |   File Storage  |
| (Users, Logs,     |       | (Encrypted Files,|
|  Metadata)        |       |  Public/Private  |
+-------------------+       |  Layers)         |
                            +-------------------+
```

### Technical Components

- **🎨 UI Layer**: Cross-platform chatbot (React Native)
- **⚙️ Backend**: API server (Node.js), integrated with LLM (Grok API)
- **💾 Storage**: Relational DB (PostgreSQL) for metadata; object storage (AWS S3) for files with ACLs
- **🤖 AI Integration**: Vector database (Pinecone) for semantic search; message queue (RabbitMQ) for escalations
- **📊 Monitoring**: Tools for logging confidence metrics and errors

## 🚀 Getting Started

### Prerequisites

- Node.js 16+ 
- Python 3.8+
- PostgreSQL
- AWS S3 account (for file storage)
- Grok API access

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/CEO-Twin-AI.git
   cd CEO-Twin-AI
   ```

2. **Install dependencies**
   ```bash
   # Backend dependencies
   npm install
   
   # AI/ML dependencies
   pip install -r requirements.txt
   ```

3. **Environment setup**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

4. **Database setup**
   ```bash
   npm run db:migrate
   npm run db:seed
   ```

5. **Start the application**
   ```bash
   npm run dev
   ```

## 💡 Usage Examples

### CEO Flow (Setup and Usage)

1. **Account Creation**
   ```bash
   # Sign up and upload files
   curl -X POST /api/auth/register \
     -H "Content-Type: application/json" \
     -d '{"email": "ceo@company.com", "password": "secure123"}'
   ```

2. **Upload Private File**
   ```bash
   curl -X POST /api/files/upload \
     -H "Authorization: Bearer YOUR_TOKEN" \
     -F "file=@passwords.txt" \
     -F "access=private"
   ```

3. **Query Private Data**
   ```bash
   curl -X POST /api/chat/query \
     -H "Authorization: Bearer YOUR_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"message": "What is the KPCards admin password?"}'
   ```

### Team Flow (Query During CEO Absence)

1. **Team Member Query**
   ```bash
   curl -X POST /api/chat/query \
     -H "Authorization: Bearer TEAM_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"message": "What is the status of Project Y?"}'
   ```

## 📚 API Documentation

### Authentication Endpoints

- `POST /api/auth/register` - Register new CEO account
- `POST /api/auth/login` - Login to system
- `POST /api/auth/invite` - Invite team member

### File Management

- `POST /api/files/upload` - Upload file (public/private)
- `GET /api/files/list` - List accessible files
- `DELETE /api/files/:id` - Delete file

### Chat Interface

- `POST /api/chat/query` - Send message to AI
- `GET /api/chat/history` - Get conversation history
- `POST /api/chat/escalate` - Manually escalate query

### Team Management

- `GET /api/team/members` - List team members
- `PUT /api/team/permissions` - Update member permissions
- `DELETE /api/team/members/:id` - Remove team member

## 🔧 Configuration

### Environment Variables

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/ceo_twin_ai

# AI Services
GROK_API_KEY=your_grok_api_key
PINECONE_API_KEY=your_pinecone_key
PINECONE_ENVIRONMENT=us-west1-gcp

# Storage
AWS_ACCESS_KEY_ID=your_aws_key
AWS_SECRET_ACCESS_KEY=your_aws_secret
AWS_S3_BUCKET=ceo-twin-ai-files

# Messaging
RABBITMQ_URL=amqp://localhost:5672

# Security
JWT_SECRET=your_jwt_secret
ENCRYPTION_KEY=your_encryption_key
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

- 📧 Email: support@ceotwin.ai
- 📖 Documentation: [docs.ceotwin.ai](https://docs.ceotwin.ai)
- 🐛 Issues: [GitHub Issues](https://github.com/yourusername/CEO-Twin-AI/issues)

## 🙏 Acknowledgments

- Built with ❤️ for CEOs who need a reliable digital twin
- Powered by advanced AI/ML technologies
- Secure by design with enterprise-grade encryption

---

**Made with ❤️ by the CEO Twin AI Team**
