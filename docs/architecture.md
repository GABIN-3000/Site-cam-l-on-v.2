# 🏗️ Architecture de Site Caméléon V2

## Vue d'ensemble

```
┌─────────────────────────────────────────────────────┐
│              🌐 User Browser                        │
│  Public Site (Mode-aware) + Admin Panel             │
└─────────────────┬───────────────────────────────────┘
                  │
          ┌───────▼────────┐
          │   Next.js App  │
          │ (React + SSR)  │
          └───────┬────────┘
                  │
        ┌─────────┴──────────┐
        │                    │
   ┌────▼────────┐    ┌─────▼──────┐
   │ API Routes  │    │ Static/SSG │
   │(Clerk Auth) │    │  Pages     │
   └────┬────────┘    └────────────┘
        │
   ┌────▼───────────────┐
   │  PostgreSQL DB     │
   │  ├─ users/roles    │
   │  ├─ modes config   │
   │  ├─ applications   │
   │  ├─ staff logs     │
   │  └─ settings       │
   └────────────────────┘
```

## Data Flow

### 1. Mode Selection (Admin)
```
Admin Panel → Select Mode → API /api/modes/switch
  ↓
Database updates: current_mode, theme, content
  ↓
Frontend: Real-time update via SWR/React Query
  ↓
All users see new theme instantly
```

### 2. Application Submission
```
Public Form → /api/applications/submit
  ↓
Compatibility scoring (NLP keyword matching)
  ↓
Staff notification + log entry
  ↓
Moderator can accept/reject
  ↓
Applicant gets access or feedback
```

### 3. Role-Based Access
```
User Login (Clerk) → Token verification
  ↓
API checks user role + permissions
  ↓
Return appropriate dashboard UI
  ↓
Action logging for all staff changes
```

## Database Schema (Simplified)

```
users
  id: UUID
  email: string
  roles: JSONB (Founder, Co-Owner, Admin, etc.)
  permissions: JSONB (granular flags)
  created_at: timestamp

mode_config
  id: UUID
  name: enum (RP, IA, CANDIDATURE, LEARN, MAINTENANCE)
  is_active: boolean
  theme_colors: JSONB
  content_overrides: JSONB
  icon_state: enum (red, blue, yellow)
  created_at: timestamp

applications
  id: UUID
  user_email: string
  content: text
  compatibility_score: float (0-100)
  status: enum (pending, accepted, rejected)
  staff_notes: text
  reviewed_by: UUID
  reviewed_at: timestamp

staff_logs
  id: UUID
  user_id: UUID
  action: string ("mode_switch", "app_review", etc.)
  details: JSONB
  timestamp: timestamp
```

## Technology Stack

| Layer | Tech |
|-------|------|
| **Frontend** | React 18 + TypeScript + Tailwind CSS + Shadcn/ui |
| **Framework** | Next.js 14 (App Router) |
| **Auth** | Clerk (cloud) or simple JWT |
| **Database** | PostgreSQL + Prisma ORM |
| **Validation** | Zod (TypeScript-first) |
| **Styling** | Tailwind + CSS Modules for themes |
| **State** | React Query + Zustand |
| **Testing** | Vitest + React Testing Library |

## Deployment

```
Frontend: Vercel (Next.js)
Backend: Vercel Serverless (API Routes) or Railway/Fly.io
Database: Railway, Supabase, or AWS RDS
Auth: Clerk (hosted)
```
