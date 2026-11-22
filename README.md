# Debut Celebration Website

A complete, production-ready debut/18th birthday celebration website with Supabase backend, featuring a fully customizable theme builder, drag-and-drop section manager, and feature-rich modules for RSVP, media gallery, wishes wall, gift registry, multi-platform playlist, and live streaming with chat.

## ✨ Features

### 🎨 **Theme Builder** (5 tabs, 200+ CSS variables)
- **Colors**: Primary, secondary, accent, semantic colors with dark mode support
- **Typography**: Font families, sizes, weights, line heights, letter spacing
- **Effects**: Shadows, transitions, blurs, animations, parallax
- **Components**: Button, card, input, modal, tooltip, badge overrides
- **Presets**: 10+ beautiful presets (Elegant Rose, Midnight Gold, Sunset Coral, etc.)

### 🧩 **Drag-and-Drop Section Manager** (@dnd-kit)
- 16 section types: Hero, Countdown, About, Timeline, Venue, Directions, Dress Code, Gallery, Wishes, Gifts, Playlist, Livestream, RSVP, Guests, FAQ, Custom
- Enable/disable sections, reorder, duplicate, configure
- Live preview mode
- Per-section animations and backgrounds

### 📱 **Public-Facing Components** (12 modules)
| Component | Features |
|-----------|----------|
| **Hero** | Countdown timer, parallax background, CTA buttons |
| **RSVP Form** | Invitation code validation, meal choices, dietary restrictions, song requests, +1 management |
| **Media Gallery** | Lightbox, masonry/grid layout, lazy load, upload with moderation |
| **Wishes Wall** | Threaded replies, multi-language (EN/TL), reactions, moderation, pagination |
| **Gift Registry** | 4 categories (Cash/Physical/Experience/Charity), contributions tracking, Stripe-ready |
| **Playlist** | 5 platforms (Spotify/YouTube/Apple/SoundCloud/Local), drag-drop reorder, metadata fetch |
| **Livestream** | RTMP/HLS, real-time chat, viewer count, slow mode, emoji reactions, moderation |
| **Timeline** | Event schedule with icons, timezone support |
| **Venue Info** | Google Maps embed, directions, parking info |
| **Dress Code** | Visual guides, color palette, theme suggestions |
| **Directions** | Multi-mode routing, landmarks, transport options |
| **Footer** | Social links, copyright, quick navigation |

### 🌐 **Internationalization** (next-intl)
- **English** (default) + **Tagalog** (Filipino)
- 200+ translation keys covering all UI strings
- Locale-aware date/number formatting
- RTL-ready architecture

### ♿ **Accessibility** (WCAG AA)
- Radix UI primitives for all interactive components
- Semantic HTML5, ARIA labels, focus management
- Keyboard navigation, screen reader support
- Reduced motion, high contrast modes
- Color contrast validation in theme builder

### 🔧 **Admin Dashboard** (9 panels)
- **Theme Builder** - Full visual theme editor
- **Section Manager** - Drag-drop page builder
- **Guest Manager** - CRUD, bulk actions, CSV import/export, invitation emails
- **Wish Moderation** - Approve/reject/flag, reply, bulk actions
- **Media Manager** - Upload, moderation, featured, tags, drag-drop reorder
- **Registry Manager** - Gift CRUD, categories, contributions tracking
- **Playlist Manager** - Multi-platform, metadata fetch, drag-drop
- **Settings Panel** - 7 tabs (General, Appearance, Features, Privacy, Notifications, Integrations, Advanced)
- **Analytics Dashboard** - Real-time metrics, charts, funnel, engagement

### 🗄️ **Backend** (Supabase)
- PostgreSQL with 14 tables, indexes, RLS policies
- Realtime subscriptions for live features
- Storage with signed URLs for media
- Auth with email/password + OAuth
- Server-side rendering with `@supabase/ssr`
- Database functions for slugs, invitation codes, sanitization

### 🚀 **DevOps**
- GitHub Actions CI/CD (lint, typecheck, test, build, deploy)
- Vercel deployment with edge functions
- Preview deployments for PRs
- Automated database migrations
- Security scanning (npm audit + Snyk)

## 🛠 Tech Stack

| Category | Technology |
|----------|------------|
| Framework | Next.js 14 (App Router, TypeScript strict) |
| Styling | Tailwind CSS + CSS Variables |
| UI Primitives | Radix UI (Dialog, Toast, DropdownMenu, Select, Checkbox, etc.) |
| Animation | Framer Motion |
| Forms | React Hook Form + Zod |
| Drag & Drop | @dnd-kit (core, sortable, utilities) |
| i18n | next-intl |
| Database | Supabase (PostgreSQL, Realtime, Storage, Auth) |
| Validation | Zod |
| Date/Time | date-fns |
| Icons | Lucide React |
| Testing | Vitest + React Testing Library |
| Linting | ESLint + Prettier |
| CI/CD | GitHub Actions |
| Hosting | Vercel |

## 📁 Project Structure

```
debut-celebration/
├── .github/workflows/       # CI/CD pipelines
├── public/                  # Static assets
├── src/
│   ├── app/                 # Next.js App Router
│   │   ├── [locale]/        # Internationalized routes
│   │   │   ├── (public)/    # Public event pages
│   │   │   └── admin/       # Admin dashboard
│   │   └── api/             # API routes
│   ├── components/
│   │   ├── ui/              # Base UI components (Button, Input, Card, Modal, Toast)
│   │   ├── public/          # Public-facing components (12 modules)
│   │   └── admin/           # Admin components (9 panels)
│   ├── contexts/            # React Context providers (Theme, Event, Locale)
│   ├── lib/
│   │   ├── supabase/        # Supabase clients (client, server, middleware)
│   │   └── validation/      # Zod schemas
│   ├── hooks/               # Custom React hooks
│   ├── utils/               # Utilities (cn, date, storage, SEO, i18n)
│   └── types/               # TypeScript types
├── supabase/
│   └── migrations/          # 15 SQL migration files
├── vercel.json              # Vercel configuration
├── DEPLOYMENT.md            # Deployment guide
└── package.json
```

## 🚀 Quick Start

### Prerequisites
- Node.js 20+
- pnpm 9+
- Supabase account
- Vercel account (for deployment)

### 1. Clone & Install
```bash
git clone https://github.com/yourusername/debut-celebration.git
cd debut-celebration
pnpm install
```

### 2. Environment Setup
```bash
cp .env.example .env.local
```

Fill in `.env.local`:
```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
NEXTAUTH_SECRET=your-random-secret-min-32-chars
NEXTAUTH_URL=http://localhost:3000
```

### 3. Supabase Setup
```bash
# Install Supabase CLI
npm install -g supabase

# Login and link
supabase login
supabase link --project-ref YOUR_PROJECT_REF

# Run migrations
supabase db push
```

### 4. Start Development
```bash
pnpm dev
```

Visit `http://localhost:3000/en` for English or `http://localhost:3000/tl` for Tagalog.

## 📦 Available Scripts

```bash
# Development
pnpm dev              # Start dev server with Turbopack
pnpm build            # Production build
pnpm start            # Start production server

# Code Quality
pnpm lint             # Run ESLint
pnpm typecheck        # Run TypeScript check
pnpm format           # Format with Prettier
pnpm format:check     # Check formatting

# Testing
pnpm test             # Run all tests
pnpm test:unit        # Unit tests only
pnpm test:integration # Integration tests only
pnpm test:watch       # Watch mode
pnpm test:coverage    # Coverage report

# Database
pnpm db:types         # Generate Supabase types
pnpm db:migrate:local # Run local migrations
pnpm db:push          # Push to remote Supabase
pnpm db:studio        # Open Supabase Studio
pnpm db:reset         # Reset local database
```

## 🌍 Internationalization

### Adding a New Language
1. Create `src/messages/[locale].json` with translations
2. Add locale to `src/lib/i18n.ts`:
   ```typescript
   export const locales = ['en', 'tl', 'your-locale'] as const
   ```
3. Add locale to `middleware.ts` matcher
4. Run `pnpm dev` to test

### Translation Structure
```json
{
  "common": { "save": "Save", "cancel": "Cancel" },
  "hero": { "title": "Welcome", "cta": "RSVP Now" },
  "rsvp": { "form": { "name": "Full Name", "attending": "Will you attend?" } }
}
```

## 🎨 Theme Customization

### CSS Variables (Applied at Runtime)
```css
:root {
  --color-primary: #E8B4B8;
  --color-background: #FFFFFF;
  --font-family: Inter;
  --space-unit: 8px;
  --border-radius: 12px;
  --transition-base: 250ms ease;
  /* ... 200+ variables */
}

[data-theme="dark"] {
  --color-primary: #F0C4C8;
  --color-background: #1A1A2E;
  /* ... dark mode overrides */
}
```

### Creating Presets
1. Design in Theme Builder admin panel
2. Click "Export" to save as JSON
3. Add to `src/lib/themes/presets.ts`
4. Available in preset selector instantly

## 📊 API Routes

| Endpoint | Methods | Description |
|----------|---------|-------------|
| `/api/events/[id]` | GET, PATCH, DELETE | Event CRUD |
| `/api/guests` | GET, POST, PATCH | Guest management, bulk actions |
| `/api/rsvps` | GET, POST, PATCH | RSVP submission, stats |
| `/api/wishes` | GET, POST, PATCH | Wishes wall, moderation |
| `/api/media` | GET, POST, PATCH | Gallery upload, moderation |
| `/api/gifts` | GET, POST, PATCH, PUT | Gift registry, contributions |
| `/api/playlist` | GET, POST, PATCH | Multi-platform playlist |
| `/api/livestream` | GET, POST, PUT, PATCH | Livestream, chat, viewers |
| `/api/analytics` | POST, GET | Event tracking, dashboard data |

## 🔐 Security

- **RLS Policies** on all tables
- **Zod validation** on all API inputs
- **Server-side auth** with `@supabase/ssr`
- **CSP headers** in `vercel.json`
- **Rate limiting** on public endpoints
- **Signed URLs** for media access
- **Input sanitization** for user content

## 🧪 Testing

```bash
# Run tests with coverage
pnpm test:coverage

# Watch mode for TDD
pnpm test:watch

# Update snapshots
pnpm test -- -u
```

### Test Structure
```
src/
├── components/__tests__/     # Component tests
├── hooks/__tests__/          # Hook tests
├── lib/__tests__/            # Utility tests
└── app/__tests__/            # API route tests
```

## 📈 Analytics Events

Track these events automatically:
- `page_view`, `section_view`
- `rsvp_submit`, `rsvp_view`
- `wish_submit`, `wish_view`, `wish_reply`
- `gift_view`, `gift_contribute`
- `media_view`, `media_upload`
- `playlist_play`, `playlist_track_click`
- `livestream_join`, `livestream_chat`
- `share_click`, `theme_change`, `language_change`

## 🚀 Deployment

See [DEPLOYMENT.md](./DEPLOYMENT.md) for complete guide.

### One-Line Deploy (after setup)
```bash
git push origin main
```

GitHub Actions will:
1. ✅ Lint & Typecheck
2. ✅ Run Tests
3. ✅ Build Application
4. ✅ Deploy to Vercel Preview (PRs)
5. ✅ Deploy to Vercel Production (main)
6. ✅ Run Database Migrations

## 🤝 Contributing

1. Fork the repository
2. Create feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open Pull Request

### Code Standards
- TypeScript strict mode
- ESLint + Prettier configuration
- Conventional commits
- 80%+ test coverage for new code

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

## 🙏 Acknowledgments

- **Supabase** - Backend as a Service
- **Vercel** - Deployment platform
- **Radix UI** - Accessible primitives
- **Framer Motion** - Animations
- **@dnd-kit** - Drag and drop
- **next-intl** - Internationalization
- **Tailwind CSS** - Styling
- **Lucide** - Icons

## 📞 Support

- 📧 Email: support@debut-celebration.com
- 🐛 Issues: [GitHub Issues](https://github.com/yourusername/debut-celebration/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/yourusername/debut-celebration/discussions)

---

**Made with ❤️ for unforgettable celebrations**

*Built for debuts, 18th birthdays, weddings, anniversaries, and any special milestone.*