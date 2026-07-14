# 🗄️ Schéma Base de Données - Site Caméléon V2

## Vue d'ensemble

La base de données utilise **PostgreSQL** avec **Prisma** comme ORM.

## Tables Principales

### `users`
Stocke les informations utilisateur et leurs rôles.

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  clerk_id VARCHAR(255) UNIQUE,
  name VARCHAR(255),
  avatar_url VARCHAR(255),
  roles JSONB NOT NULL DEFAULT '{}',
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### `mode_config`
Gère la configuration des 5 modes du site.

```sql
CREATE TABLE mode_config (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  mode_name VARCHAR(50) UNIQUE NOT NULL,
  is_active BOOLEAN DEFAULT false,
  theme_colors JSONB NOT NULL,
  content_overrides JSONB,
  cameleon_eye_color VARCHAR(20),
  banner_state VARCHAR(20),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Exemple de mode: RP
-- {
--   "mode_name": "RP",
--   "is_active": true,
--   "theme_colors": {
--     "primary": "#8B0000",
--     "secondary": "#DC143C",
--     "background": "#1a1a1a",
--     "text": "#ffffff"
--   },
--   "cameleon_eye_color": "red",
--   "banner_state": "immersive"
-- }
```

### `applications`
Gère les candidatures des utilisateurs.

```sql
CREATE TABLE applications (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_email VARCHAR(255) NOT NULL,
  user_name VARCHAR(255),
  content TEXT NOT NULL,
  compatibility_score FLOAT CHECK (compatibility_score >= 0 AND compatibility_score <= 100),
  keywords_matched JSONB,
  status VARCHAR(20) DEFAULT 'pending',
  reviewer_id UUID REFERENCES users(id),
  reviewer_notes TEXT,
  reviewed_at TIMESTAMP,
  rejected_reason TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Status: pending, accepted, rejected
```

### `staff_logs`
Audit trail de toutes les actions du staff.

```sql
CREATE TABLE staff_logs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id),
  action VARCHAR(100) NOT NULL,
  action_type VARCHAR(50),
  details JSONB,
  affected_user_id UUID REFERENCES users(id),
  affected_resource_id UUID,
  status VARCHAR(20),
  timestamp TIMESTAMP DEFAULT NOW()
);

-- Action types: MODE_SWITCH, APP_REVIEW, STAFF_ADD, STAFF_REMOVE, THEME_UPDATE, etc.
```

### `site_content`
Contenu personnalisable pour chaque mode.

```sql
CREATE TABLE site_content (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  mode_name VARCHAR(50) NOT NULL,
  section_key VARCHAR(100) NOT NULL,
  content TEXT,
  is_custom BOOLEAN DEFAULT false,
  updated_by UUID REFERENCES users(id),
  updated_at TIMESTAMP DEFAULT NOW(),
  UNIQUE(mode_name, section_key)
);

-- Examples:
-- section_key: "homepage_title", "homepage_description", "learn_quiz_content", etc.
```

### `applications_ratings`
Notations du staff sur les candidatures.

```sql
CREATE TABLE applications_ratings (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  application_id UUID NOT NULL REFERENCES applications(id),
  rater_id UUID NOT NULL REFERENCES users(id),
  rating INT CHECK (rating >= 0 AND rating <= 5),
  comment TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### `virtual_jail`
Système de délits fictifs pour le mode RP.

```sql
CREATE TABLE virtual_jail (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  player_id UUID NOT NULL REFERENCES users(id),
  offense_type VARCHAR(100),
  offense_description TEXT,
  created_by UUID NOT NULL REFERENCES users(id),
  created_at TIMESTAMP DEFAULT NOW(),
  is_active BOOLEAN DEFAULT true,
  cleared_at TIMESTAMP
);
```

### `community_posts`
Blog/Gazette communautaire.

```sql
CREATE TABLE community_posts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  author_id UUID NOT NULL REFERENCES users(id),
  title VARCHAR(255) NOT NULL,
  content TEXT NOT NULL,
  category VARCHAR(50),
  is_published BOOLEAN DEFAULT false,
  published_at TIMESTAMP,
  views INT DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

## Indexes Critiques

```sql
-- Recherche rapide par email
CREATE INDEX idx_users_email ON users(email);

-- Filtrer les applications en attente
CREATE INDEX idx_applications_status ON applications(status);

-- Logs par utilisateur
CREATE INDEX idx_staff_logs_user_id ON staff_logs(user_id);

-- Logs par timestamp
CREATE INDEX idx_staff_logs_timestamp ON staff_logs(timestamp);

-- Mode actif
CREATE INDEX idx_mode_config_active ON mode_config(is_active);
```

## Migrations (avec Prisma)

```bash
# Créer une migration
npm run db:migrate:dev --name "add_virtual_jail"

# Appliquer les migrations
npm run db:migrate:deploy

# Réinitialiser la DB (dev only)
npm run db:reset
```
