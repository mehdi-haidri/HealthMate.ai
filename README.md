# HealthMate.ai

HealthMate.ai is an AI-assisted health companion built with Next.js. It provides a conversational interface for health questions, lets users keep authenticated chat sessions, and can summarize abnormalities found in uploaded clinical reports.

> **Medical disclaimer:** HealthMate.ai is an educational assistant, not a doctor or a diagnostic service. Its responses and report summaries must not replace professional medical advice. Always consult a qualified healthcare professional for diagnosis, treatment, or urgent symptoms.

## Features

- AI health conversations powered by Google Gemini.
- Streaming chat responses with Markdown rendering.
- User registration, login, Google authentication, logout, and password reset flows.
- Persistent users, chats, and messages stored in MongoDB.
- Retrieval-augmented responses using embeddings and Astra DB vector search.
- Clinical-report image upload and AI-generated summaries of potentially abnormal biomarkers.
- Light and dark themes.
- Responsive chat UI with saved sessions and a collapsible navigation drawer.

## Technology

- [Next.js 15](https://nextjs.org/) App Router and React 19
- [NextAuth.js](https://next-auth.js.org/) for authentication
- [Google Gemini](https://ai.google.dev/) for chat, embeddings, and report analysis
- [MongoDB](https://www.mongodb.com/) with Mongoose for application data
- [DataStax Astra DB](https://www.datastax.com/products/datastax-astra) for vector search
- Tailwind CSS, Radix UI, Lucide, and React Spring for the interface

## Requirements

- Node.js 18.18 or newer
- npm
- A MongoDB database
- A Google Gemini API key
- An Astra DB database and application token for retrieval-augmented chat
- OAuth credentials if Google sign-in is enabled
- SMTP credentials if password-reset email delivery is enabled

## Getting started

1. Clone the repository and install dependencies:

   ```bash
   git clone https://github.com/mehdi-haidri/HealthMate.ai.git
   cd HealthMate.ai
   npm install
   ```

2. Create a local environment file:

   ```bash
   copy example.env .env.local
   ```

   Replace the placeholder values in `.env.local`. The application currently reads the following variables:

   | Variable | Purpose |
   | --- | --- |
   | `MONGODB_URI` | MongoDB connection string |
   | `GEMINI_API_KEY` | Gemini chat and report-analysis access |
   | `ASTRA_DB_NAMESPACE` | Astra DB namespace |
   | `ASTRA_DB_COLLECTION` | Astra DB vector collection |
   | `ASTRA_DB_URL` | Astra DB API URL |
   | `ASTRA_DB_APPLICATION_TOKEN` | Astra DB application token |
   | `NEXTAUTH_SECRET` | NextAuth session encryption secret |
   | `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET` | Google OAuth credentials |
   | `NEXT_PUBLIC_APP_URL` | Public application URL |
   | `EMAIL_USER` / `EMAIL_PASS` | SMTP account for password reset |

   Keep `.env.local` out of version control and rotate any credential that has been exposed.

3. Start the development server:

   ```bash
   npm run dev
   ```

4. Open [http://localhost:3000](http://localhost:3000).

## Available scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Start the local Next.js development server |
| `npm run build` | Create a production build |
| `npm run start` | Serve the production build |
| `npm run seed` | Load PDF chunks and embeddings into Astra DB |
| `npm run image` | Run the image-extraction utility |
| `npm test` | Run the repository's Node-based test script |

Before running `npm run seed`, configure Astra DB and update the PDF path in `scripts/script2.mjs` for the document you want to index.

## Project structure

```text
app/
  api/                    API routes for auth, chat, messages, users, themes, and reports
  chatbot/                Chat experience and session loading
  components/             Chat, report scanner, navigation, and visual components
  login/                  Login screen
  signup/                 Registration screen
  reset-password/         Password reset screen
  page.jsx                Landing page
db/                       MongoDB connection helper
models/                   Mongoose models for users, chats, and messages
scripts/                  Data-ingestion and utility scripts
public/                   Static assets
```

## API overview

The App Router API handlers are organized under `app/api`:

- `auth/[...nextauth]` - NextAuth authentication
- `auth/forgot-password` and `auth/reset-password` - password recovery
- `chat` - streaming AI chat and contextual retrieval
- `chats` and `messages` - chat-session persistence
- `ImageExtractor` - clinical-report image analysis
- `theme` - user theme preferences
- `users` - user-related operations

## Screenshots

<img alt="HealthMate.ai light theme" width="700" src="v2_light.png">
<img alt="HealthMate.ai dark theme" width="700" src="v2_dark.png">

## Deployment

The application can be deployed to any Next.js-compatible host, including [Vercel](https://vercel.com/). Configure all production environment variables in the hosting provider, set `NEXT_PUBLIC_APP_URL` to the deployed URL, and verify MongoDB, Astra DB, OAuth callback URLs, and SMTP access before launch.
