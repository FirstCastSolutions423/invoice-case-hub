# Invoice Case Hub

> AI-assisted invoice resolution system with Microsoft 365, Linear, and GitHub integrations

## 🚀 Overview

This application provides an intelligent invoice case management system that:
- Searches across Microsoft 365 (Outlook, OneDrive, SharePoint) for invoice-related documents
- Creates and manages cases for invoice resolution
- Synchronizes with Linear for project management
- Provides GitHub integration for external AI assistant access
- Implements intelligent retry logic with exponential backoff
- Offers real-time webhook support for document updates

## 🏗 Architecture

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│   Frontend  │────▶│   Backend    │────▶│  Database   │
│    (React)  │     │   (Express)  │     │ (PostgreSQL)│
└─────────────┘     └──────────────┘     └─────────────┘
                            │
                            ▼
        ┌───────────────────────────────────┐
        │         External Services         │
        ├───────────────┬───────────────────┤
        │  Microsoft 365 │  Linear  │ GitHub │
        └───────────────┴───────────────────┘
```

## 📁 Project Structure

```
invoice-case-hub/
├── client/              # Frontend React application
│   ├── src/
│   │   ├── components/  # React components
│   │   ├── pages/       # Page components
│   │   ├── hooks/       # Custom React hooks
│   │   └── lib/         # Utilities and helpers
├── server/              # Backend Express application
│   ├── routes.ts        # API endpoints
│   ├── storage.ts       # Data storage interface
│   ├── linear.ts        # Linear integration
│   ├── github.ts        # GitHub integration
│   ├── microsoft-graph-clients.ts  # M365 integration
│   └── supabase.ts      # Case management service
├── shared/              # Shared types and schemas
│   └── schema.ts        # Database schema and types
└── migrations/          # Database migrations
```

## 🔌 API Endpoints

### Core Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/search` | Search for invoice references across M365 |
| GET | `/api/search-history` | Get search history |
| GET | `/api/stats` | Get system statistics |
| POST | `/api/cases` | Create a new case |
| GET | `/api/cases/:id` | Get case details |
| GET | `/api/cases/:id/artifacts` | Get case artifacts |

### GitHub Integration

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/github/init-repo` | Initialize GitHub repository |
| POST | `/api/cases/:id/sync-github` | Sync case to GitHub issue |
| POST | `/api/github/commit` | Commit project state |
| GET | `/api/github/repo-status` | Get repository status |

### Linear Integration

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/linear/init-project` | Initialize Linear project |
| POST | `/api/cases/:id/sync-linear` | Sync case to Linear issue |

### Webhook Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/hooks/m365` | Microsoft 365 webhook |
| POST | `/hooks/linear` | Linear webhook |

## 🗄 Database Schema

### Cases Table
```sql
CREATE TABLE cases (
  id UUID PRIMARY KEY,
  ref_norm TEXT NOT NULL,
  tenant_id TEXT NOT NULL,
  customer TEXT,
  service_date TIMESTAMP,
  status TEXT DEFAULT 'open',
  linear_issue_id VARCHAR,
  github_issue_number INTEGER,
  github_repo_url VARCHAR,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### Artifacts Table
```sql
CREATE TABLE artifacts (
  id UUID PRIMARY KEY,
  case_id UUID REFERENCES cases(id),
  source TEXT NOT NULL,
  artifact_type TEXT NOT NULL,
  title TEXT,
  m365_link TEXT,
  metadata JSONB DEFAULT '{}'
);
```

## 🔧 Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| DATABASE_URL | PostgreSQL connection string | Yes |
| TENANT_ID | Microsoft 365 tenant ID | Yes |
| CLIENT_ID | Microsoft app client ID | Yes |
| CLIENT_SECRET | Microsoft app client secret | Yes |
| REDIS_URL | Redis connection URL | Optional |
| BETTER_STACK_SOURCE_TOKEN | Logging token | Optional |

## 🚦 Getting Started

### Prerequisites
- Node.js 18+
- PostgreSQL database
- Microsoft 365 organization with app registration
- Linear account (optional)
- GitHub account (optional)

### Installation
```bash
# Install dependencies
npm install

# Set up environment variables
cp .env.example .env

# Run database migrations
npm run db:push

# Start development server
npm run dev
```

## 🤖 AI Assistant Integration

This repository is optimized for AI assistant consumption with:

- **Comprehensive documentation**: All endpoints, schemas, and flows are documented
- **Clear architecture**: Modular design with separation of concerns
- **Type safety**: Full TypeScript coverage with Zod validation
- **Webhook support**: Real-time updates via webhooks
- **Error handling**: Robust retry logic with exponential backoff
- **Monitoring**: Built-in metrics and logging

### Key Integration Points

1. **Case Management**: Create and track invoice resolution cases
2. **Document Search**: Search across Microsoft 365 services
3. **Issue Tracking**: Sync with Linear and GitHub for project management
4. **Webhook Processing**: Real-time updates from external services
5. **Artifact Storage**: Store and link related documents

## 📊 Recent Changes

- **2025-10-07**: Added GitHub integration for AI assistant access
- **2025-10-07**: Implemented automatic README generation
- **2025-10-07**: Added GitHub sync triggers for high-priority cases
- **2025-10-07**: Created comprehensive project documentation

## 📝 License

This project is proprietary and confidential.

---

*Generated on 2025-10-07T16:22:24.866Z for AI assistant consumption*
