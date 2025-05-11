# AI SDR + Meeting AI + Knowledge Base(code is private)

A comprehensive AI-powered system that integrates multiple AI capabilities for sales development, meeting analysis, and knowledge management. The platform helps sales teams identify prospects, generate personalized outreach, automate follow-ups, and capture meeting insights.

![System Architecture](https://app.eraser.io/workspace/NjlgvzEzm0bygGKFcO9f?origin=share)

![image](https://github.com/user-attachments/assets/04c0259c-a959-4024-8784-dff9e215e2b1)


## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Module Details](#module-details)
  - [AI SDR Module](#ai-sdr-module)
  - [Meeting AI Module](#meeting-ai-module)
  - [Knowledge Base Module](#knowledge-base-module)
  - [Frontend Module](#frontend-module)
- [Usage](#usage)
- [Development](#development)
- [Deployment](#deployment)
- [Technologies Used](#technologies-used)
- [Contributing](#contributing)
- [License](#license)

## Overview

This system is designed to transform the traditional sales development process by leveraging AI at every stage:

1. **AI Lead Generator & Analyzer**: Scrapes LinkedIn and other platforms to find and analyze potential leads.
2. **Personalized AI Email Generator**: Creates highly personalized emails based on prospect data and pain points.
3. **AI Follow-up & Meeting Scheduler**: Monitors email responses and automates follow-ups.
4. **AI Meeting Notes & Knowledge Base**: Captures, summarizes, and makes searchable all meeting knowledge.

## Architecture

The system uses a modern microservices architecture:

```
├── Frontend (Next.js)
│   ├── Dashboard
│   ├── Prospect Management
│   ├── Email Editor
│   ├── Meeting Management
│   └── Knowledge Search
│
├── Backend (FastAPI)
│   ├── AI SDR Services
│   ├── Meeting AI Services
│   ├── Knowledge Base Services
│   └── Authentication/Authorization
│
├── Storage
│   ├── Supabase (Persistent Storage)
│   ├── Redis (Caching)
│   └── Pinecone (Vector Database)
│
└── External Services
    ├── Groq (LLM Provider)
    ├── MeetingBaaS (Meeting Bot)
    └── Jina AI (Vector Embeddings)
```

## Module Details

### AI SDR Module

The AI SDR module is responsible for finding, analyzing, and engaging with prospects.

#### LinkedIn Service (`agents/services/linkedin_service.py`)

- **Functionality**: Scrapes LinkedIn for potential leads, analyzes posts for pain points, and identifies promising prospects.
- **Key Components**:
  - `LinkedInPublicScraper`: Scrapes LinkedIn content
  - `analyze_posts()`: Extracts insights from posts using LLM
  - `get_prospects()`: Filters and scores leads based on alignment
- **Technologies**: Selenium, BeautifulSoup, Groq LLM

#### Email Service (`agents/services/email_service.py`)

- **Functionality**: Generates personalized email drafts using a multi-stage LangGraph workflow.
- **Key Components**:
  - `build_workflow()`: Creates the email generation state graph
  - `subject_agent()`: Generates compelling subject lines
  - `content_builder_agent()`: Creates initial email content
  - `content_refiner_agent()`: Refines and improves the draft
  - `final_draft_agent()`: Formats the final email
- **Technologies**: LangGraph, Groq LLM

#### Reply Tracker (`agents/services/reply_tracker.py`)

- **Functionality**: Analyzes email responses for sentiment and intent, generates follow-ups.
- **Key Components**:
  - `analyze_sentiment()`: Determines email sentiment and intent
  - `generate_followup_email()`: Creates contextual follow-up emails
  - `GmailService`: Handles Gmail API integration
- **Technologies**: Google Gmail API, Groq LLM

### Meeting AI Module

The Meeting AI module handles meeting participation, transcription, and analysis.

#### Meeting Analyzer (`agents/services/meeting_analyzer.py`)

- **Functionality**: Analyzes meeting transcripts to extract summaries, action items, and insights.
- **Key Components**:
  - `analyze_meeting()`: Processes transcripts to generate structured insights
  - Few-shot examples for consistent output formatting
- **Technologies**: Groq LLM, MeetingBaaS API

#### Meeting Bot Integration (`agents/main.py`)

- **Functionality**: Adds bots to meetings, receives webhooks with transcripts.
- **Key Components**:
  - `/add-bot`: Endpoint to add a bot to a meeting
  - `/webhook`: Receives meeting completion events
  - Meeting status management
- **Technologies**: MeetingBaaS API, FastAPI

### Knowledge Base Module

The Knowledge Base module stores and retrieves meeting knowledge.

#### Vector Service (`agents/services/vector_service.py`)

- **Functionality**: Creates embeddings, stores meeting data, and enables semantic search.
- **Key Components**:
  - `create_embedding()`: Generates vector embeddings using Jina AI
  - `store_meeting_data()`: Chunks and stores meeting content
  - `search_meetings()`: Performs vector similarity search
  - `generate_rag_response()`: Creates RAG-based answers from meetings
- **Technologies**: Pinecone, Jina AI Embeddings, RAG

#### LLM Service (`agents/services/llm_service.py`)

- **Functionality**: Provides a unified interface for LLM interactions.
- **Key Components**:
  - `get_json_response()`: Gets structured JSON responses
  - `get_streaming_response()`: Streams responses for UI
  - `get_completion()`: Gets regular text completions
- **Technologies**: Groq, LangChain

### Frontend Module

The frontend provides a user-friendly interface for the system.

#### Dashboard (`components/dashboard/`)

- **Functionality**: Displays key metrics, recent activities, and insights.
- **Key Components**:
  - `DashboardOverview`: Shows statistics and activity
  - `FollowUps`: Manages email responses
  - `MeetingNotes`: Displays meeting summaries
  - `MeetingSearch`: Searches knowledge base
- **Technologies**: Next.js, Recharts, shadcn/ui

#### Prospect Management (`components/ProspectList.tsx`)

- **Functionality**: Displays and manages prospects found by the AI.
- **Key Components**:
  - `ProspectList`: Lists all prospects with filtering
  - `ProspectCard`: Displays prospect details
  - `ProspectModal`: Shows detailed prospect information
  - `EmailDrafts`: Manages email generation and sending
- **Technologies**: Next.js, shadcn/ui

#### Authentication (`app/(auth)/`)

- **Functionality**: Handles user authentication and authorization.
- **Key Components**:
  - `login/page.tsx`: Login/signup interface
  - `login/actions.ts`: Server actions for auth
  - Supabase integration
- **Technologies**: Next.js, Supabase Auth

## Setup and Installation

### Prerequisites

- Node.js 19+
- Python 3.9+
- Docker (optional but recommended)
- Supabase account
- Pinecone account
- Groq API key
- MeetingBaaS account

## Usage

### Access the Application

Login with these creds:
- Email:jamikhann@gmail.com
- Pass: abcd1234
- Or directly login with github

### Key Workflows

1. **Prospect Generation**:
   - Navigate to the Prospects page
   - Set minimum alignment score and click "Generate New Prospects"
   - View and filter prospects based on criteria

2. **Email Generation**:
   - Click on a prospect to view details
   - Select "Generate Email Draft" 
   - Edit the draft and send

3. **Reply Tracking**:
   - Go to the Dashboard and select "Follow-ups" tab
   - Click "Analyze Replies" to process new emails
   - View sentiment analysis and suggested follow-ups

4. **Meeting Management**:
   - Navigate to the Dashboard and select "Meeting Notes" tab
   - Add a meeting bot by providing a meeting URL
   - Once a meeting completes, view the transcript, summary, and insights

5. **Knowledge Search**:
   - Use the search bar in the Meeting Notes tab
   - Ask questions about past meetings
   - View answers with source information

### Adding New Features

1. **New AI Agents**:
   - Create a new service in `agents/services/`
   - Add endpoints in `agents/main.py`
   - Integrate with existing services as needed

2. **New Frontend Components**:
   - Add components in `components/`
   - Create new pages in `app/`
   - Update navigation as needed

3. **Database Changes**:
   - Update Supabase schema as needed
   - Modify the relevant services that interact with the database

## Deployment

### Frontend (Vercel)

1. Push your code to GitHub
2. Connect your repository to Vercel
3. Configure environment variables
4. Deploy

### Backend (Railway)

1. Push your code to GitHub
2. Connect your repository to Railway
3. Configure environment variables
4. Set the start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Deploy

## Technologies Used

### Backend

- **FastAPI**: Web framework for building APIs
- **LangChain**: Framework for LLM applications
- **LangGraph**: State machines for LLM workflows
- **Groq**: LLM provider for text generation
- **Selenium & BeautifulSoup**: Web scraping
- **Pinecone**: Vector database for embeddings
- **Jina AI**: Embedding model provider
- **Redis**: Caching and temporary storage
- **Supabase**: PostgreSQL database and authentication

### Frontend

- **Next.js 15**: React framework with App Router
- **TypeScript**: Typed JavaScript
- **Tailwind CSS**: Utility-first CSS framework
- **shadcn/ui**: Component library
- **Recharts**: Charting library
- **Sonner**: Toast notifications
- **React Hook Form**: Form handling

### DevOps

- **Docker**: Containerization
- **Railway**: Backend deployment
- **Vercel**: Frontend deployment

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

For questions or support, please contact [mohdjamikhann@example.com](mailto:mohdjamikhann@example.com).
