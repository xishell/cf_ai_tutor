# CF AI Tutor

> **Work in Progress** - AI-powered study tutor built on Cloudflare's platform

An intelligent study assistant that converts course materials and lecture notes into interactive flashcards, helping students learn through AI-powered spaced repetition.

Built for the Cloudflare AI application assignment.

## Features

### Implemented

- AI-Powered Chat Interface - Natural conversation with study tutor
- Workers AI Integration - Llama 3.3 70B for intelligent flashcard generation
- Real-time Communication - WebSocket-based chat using Cloudflare Agents
- Persistent State - Durable Objects with SQLite storage
- Dark/Light Theme - User preference saved to localStorage

### In Progress

- Flashcard Generation - Converting study materials into Q&A pairs
- Spaced Repetition Algorithm - Smart review scheduling based on mastery
- Study Sessions - Interactive flashcard review interface
- File Upload - Support for PDF and text file processing
- Progress Tracking - Statistics dashboard for learning analytics

## Architecture

### Tech Stack

- **Frontend**: React 19 + TypeScript + Vite + TailwindCSS
- **Backend**: Cloudflare Workers + Durable Objects
- **AI/LLM**: Cloudflare Workers AI (Llama 3.3 70B Instruct)
- **Framework**: Cloudflare Agents SDK
- **Database**: SQLite (Durable Objects Storage)
- **State Management**: Built-in agent state synchronization

### Cloudflare Services Used

- **Workers AI** - LLM inference with Llama 3.3
- **Durable Objects** - Stateful agent instances
- **SQLite Storage** - Flashcard persistence
- **Pages** - Static asset serving
- **R2** (Planned) - File storage for uploads

## Setup & Development

### Prerequisites

- Node.js 18+
- npm or pnpm
- Cloudflare account (for deployment)

### Local Development

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd cf-ai-tutor
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Start development server**

   ```bash
   npm run start
   ```

4. **Access the app**
   - Open http://localhost:5173
   - The Vite dev server will proxy requests to Wrangler

### Project Structure

```
cf-ai-tutor/
├── src/
│   ├── server.ts           # StudyTutorAgent (Durable Object)
│   ├── tools.ts            # AI tool definitions (flashcard operations)
│   ├── app.tsx             # Main React application
│   ├── client.tsx          # Client entry point
│   └── components/         # React UI components
├── public/                 # Static assets
├── wrangler.jsonc          # Cloudflare Workers configuration
└── README.md              # This file
```

## How It Works

### 1. Flashcard Generation

```typescript
// User provides study content
"Create flashcards about photosynthesis..."

// AI generates structured flashcards
{
  topic: "Biology - Photosynthesis",
  question: "What is the primary pigment in photosynthesis?",
  answer: "Chlorophyll"
}

// Saved to SQLite via Durable Objects
```

### 2. Spaced Repetition

```
Mastery Levels (0-5):
├── 0 (New): Review in 5 minutes
├── 1: Review in 1 day
├── 2: Review in 3 days
├── 3: Review in 1 week
├── 4: Review in 2 weeks
└── 5 (Mastered): Review in 1 month

Correct answer → Level +1
Incorrect answer → Level -1
```

### 3. Agent Architecture

```
User ←→ WebSocket ←→ StudyTutorAgent (Durable Object)
                           ├── Workers AI (Llama 3.3)
                           ├── SQLite Storage
                           └── Tool Calling (flashcard ops)
```

## Deployment

### Deploy to Cloudflare

```bash
# Build and deploy
npm run deploy

# The app will be deployed to:
# - Workers: Your agent backend
# - Pages: Your frontend assets
```

### Environment Setup

No API keys required! This project uses:

- Cloudflare Workers AI (no external API key needed)
- Built-in Durable Objects storage

## Database Schema

```sql
CREATE TABLE flashcards (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  topic TEXT NOT NULL,
  question TEXT NOT NULL,
  answer TEXT NOT NULL,
  mastery_level INTEGER DEFAULT 0,      -- 0-5 scale
  last_reviewed_at INTEGER,              -- Unix timestamp
  next_review_at INTEGER,                -- Unix timestamp
  created_at INTEGER DEFAULT (strftime('%s', 'now'))
);
```

## Testing

### Run the sanity check

```bash
npm run check
```

This runs:

- Prettier (formatting)
- Biome (linting)
- TypeScript (type checking)

### Manual Testing

1. Open the chat interface
2. Paste study content or lecture notes
3. Ask the AI to create flashcards
4. Review generated flashcards
5. Test spaced repetition by marking cards as correct/incorrect

## Future Enhancements

- PDF text extraction
- Image OCR for slide content
- Multi-topic study sessions
- Export flashcards to Anki/Quizlet
- Collaborative study groups
- Voice input/output
- Mobile app (React Native)

---

**Status**: Active Development | **Last Updated**: 2025-01-28
