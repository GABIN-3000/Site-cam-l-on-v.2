# 🎭 Site Caméléon V2

**Votre tableau de bord « tout-en-un » sans code — changez l'apparence entière de votre site en un clic.**

## 🚀 Concept

Site Caméléon V2 est une plateforme dynamique qui change de visage selon son mode :

- **Mode RP** : Univers sombre et immersif (yeux rouges du caméléon)
- **Mode IA** : Futuriste et néon (yeux bleu néon)
- **Mode Candidature** : Formulaires et recrutement
- **Mode Apprendre** : Tutoriels et quiz (vert épuré)
- **Mode Maintenance** : Panneau de travaux

## 👥 Rôles & Permissions

### Hiérarchie des Rôles

```
Founder (Droit absolu)
├─ Co-Owner (Gestion globale)
├─ Admin (Gestion des modes + staff)
├─ Manager Staff (Manage les rôles)
├─ Manager Tout (Tout sauf les modes)
├─ Manager Designer (Supervise les designers)
├─ Designer (Refonte visuelle)
├─ Coder (Développement technique)
├─ Tester (QA & tests)
└─ Modérateur (Modération basique)
```

## 📚 Documentation

- **API**: `docs/api.md`
- **Database**: `docs/database.md`
- **Architecture**: `docs/architecture.md`

## 🛠️ Installation

```bash
# 1. Clone
git clone https://github.com/GABIN-3000/Site-cam-l-on-v.2.git
cd Site-cam-l-on-v.2

# 2. Install dependencies
npm install

# 3. Setup environment
cp .env.example .env.local
# Edit .env.local with your database and Clerk keys

# 4. Initialize database
npm run db:migrate
npm run db:seed

# 5. Start development
npm run dev
```

## 📦 Project Structure

```
Site-cam-l-on-v.2/
├── apps/
│   ├── web/              # Next.js frontend + API routes
│   └── api/              # Express backend (optional)
├── packages/
│   ├── db/               # Database schema & migrations
│   ├── types/            # Shared TypeScript types
│   └── utils/            # Shared utilities
├── docs/                 # Documentation
└── scripts/              # Development scripts
```

## 🎨 Features (Roadmap)

### Phase 1: Foundation ✅
- [x] Project structure
- [x] Role system design
- [ ] Admin authentication
- [ ] Mode switching UI

### Phase 2: Core 🔄
- [ ] Application management
- [ ] Staff logging
- [ ] Panic button
- [ ] Staff ratings

### Phase 3: Advanced 📅
- [ ] Virtual jail system
- [ ] Community blog/gazette
- [ ] Advanced dashboards
- [ ] Custom role creation

## 📝 License

Private project
