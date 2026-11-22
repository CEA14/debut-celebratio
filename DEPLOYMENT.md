# Deployment Guide

This guide walks you through deploying the Debut Celebration website to production using Supabase, Vercel, and GitHub.

## Prerequisites

- GitHub account
- Vercel account
- Supabase account
- Node.js 20+ installed locally

## 1. Supabase Setup

### Create a New Project

1. Go to [Supabase Dashboard](https://supabase.com/dashboard)
2. Click "New Project"
3. Choose your organization
4. Enter project name (e.g., `debut-celebration`)
5. Set a strong database password
6. Choose region closest to your users (e.g., Singapore for Philippines)
7. Click "Create new project"

### Run Migrations

```bash
# Install Supabase CLI
npm install -g supabase

# Login to Supabase
supabase login

# Link your project
supabase link --project-ref YOUR_PROJECT_REF

# Run migrations
supabase db push
```

Or use the SQL editor in Supabase Dashboard to run each migration file in order from `supabase/migrations/`.

### Configure Storage Buckets

1. Go to Storage in Supabase Dashboard
2. Create bucket named `event-media`
3. Set bucket to public: **No** (we use signed URLs)
4. Add the following policies:

```sql
-- Allow authenticated users to upload
create policy "Authenticated users can upload" on storage.objects
  for insert with check (bucket_id = 'event-media' and auth.role() = 'authenticated');

-- Allow public to download via signed URLs (handled by API)
create policy "Public can download" on storage.objects
  for select using (bucket_id = 'event-media');

-- Allow users to update their own uploads
create policy "Users can update own uploads" on storage.objects
  for update using (bucket_id = 'event-media' and auth.uid() = owner);

-- Allow users to delete their own uploads
create policy "Users can delete own uploads" on storage.objects
  for delete using (bucket_id = 'event-media' and auth.uid() = owner);
```

### Enable Realtime

Go to Database > Replication and enable realtime for these tables:
- `events`
- `themes`
- `sections`
- `guests`
- `rsvps`
- `wishes`
- `media`
- `gifts`
- `gift_contributions`
- `playlist_tracks`
- `livestreams`
- `livestream_chat`
- `livestream_viewers`

### Get API Keys

From Supabase Dashboard > Settings > API:
- `NEXT_PUBLIC_SUPABASE_URL` - Project URL
- `NEXT_PUBLIC_SUPABASE_ANON_KEY` - anon public key
- `SUPABASE_SERVICE_ROLE_KEY` - service_role key (keep secret!)

## 2. Vercel Setup

### Create Vercel Project

1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click "Add New..." > "Project"
3. Import your GitHub repository
3. Configure:
   - Framework Preset: Next.js
   - Build Command: `pnpm build`
   - Output Directory: `.next`
   - Install Command: `pnpm install`
   - Root Directory: `./` (or your project root)

### Environment Variables

Add these in Vercel Project Settings > Environment Variables:

**Required:**
```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
NEXTAUTH_SECRET=your-random-secret-min-32-chars
NEXTAUTH_URL=https://your-domain.vercel.app
```

**Optional (for integrations):**
```env
# Email (Resend)
RESEND_API_KEY=re_xxxxx
EMAIL_FROM=noreply@yourdomain.com

# Analytics
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX
NEXT_PUBLIC_CLARITY_ID=xxxxxx

# Payment (for gifts)
STRIPE_SECRET_KEY=sk_xxxxx
STRIPE_WEBHOOK_SECRET=whsec_xxxxx
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_xxxxx

# Livestream
LIVESTREAM_INGEST_URL=rtmp://your-server/live
LIVESTREAM_PLAYBACK_URL=https://your-cdn.com

# Social APIs
SPOTIFY_CLIENT_ID=xxxxx
SPOTIFY_CLIENT_SECRET=xxxxx
YOUTUBE_API_KEY=xxxxx
APPLE_MUSIC_TEAM_ID=xxxxx
APPLE_MUSIC_KEY_ID=xxxxx
APPLE_MUSIC_PRIVATE_KEY=xxxxx
SOUNDCLOUD_CLIENT_ID=xxxxx
```

### Custom Domain (Optional)

1. Go to Project Settings > Domains
2. Add your custom domain
3. Configure DNS records as instructed
4. Update `NEXTAUTH_URL` to your custom domain

## 3. GitHub Setup

### Repository Secrets

Go to your GitHub repo > Settings > Secrets and variables > Actions and add:

**Required:**
| Secret | Description |
|--------|-------------|
| `VERCEL_TOKEN` | From Vercel Account Settings > Tokens |
| `VERCEL_ORG_ID` | From Vercel Project Settings |
| `VERCEL_PROJECT_ID` | From Vercel Project Settings |
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase Project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase anon key |
| `SUPABASE_SERVICE_ROLE_KEY` | Supabase service role key |
| `NEXTAUTH_SECRET` | Random 32+ char string |
| `NEXTAUTH_URL` | Your production URL |
| `SUPABASE_ACCESS_TOKEN` | From Supabase Account Settings > Access Tokens |
| `SUPABASE_PROJECT_REF` | From Supabase Project Settings > General |

**Optional:**
| Secret | Description |
|--------|-------------|
| `SNYK_TOKEN` | For security scanning |
| `RESEND_API_KEY` | For email sending |
| `STRIPE_SECRET_KEY` | For payments |
| `STRIPE_WEBHOOK_SECRET` | For Stripe webhooks |

### Branch Protection

1. Go to Settings > Branches
2. Add rule for `main`:
   - Require pull request reviews
   - Require status checks to pass (lint, typecheck, test, build)
   - Require branches to be up to date
   - Include administrators

## 4. Local Development

### Clone and Setup

```bash
git clone https://github.com/yourusername/debut-celebration.git
cd debut-celebration

# Install dependencies
pnpm install

# Copy environment template
cp .env.example .env.local

# Fill in your .env.local with Supabase credentials
```

### Run Development Server

```bash
pnpm dev
```

Visit `http://localhost:3000`

### Database Commands

```bash
# Generate types from Supabase
pnpm db:types

# Run migrations locally
pnpm db:migrate:local

# Reset local database
pnpm db:reset

# Open Supabase Studio
pnpm db:studio
```

## 5. First Deployment

### Automatic (Recommended)

1. Push to `main` branch
2. GitHub Actions will:
   - Run lint, typecheck, tests
   - Build the application
   - Deploy to Vercel Preview (for PRs)
   - Deploy to Vercel Production (for main)
   - Run database migrations

### Manual Deployment

```bash
# Build locally
pnpm build

# Deploy to Vercel
vercel --prod
```

## 6. Post-Deployment

### Create First Event

1. Visit your deployed site
2. Sign up / Sign in with email
3. Create your first event
4. Configure theme, sections, and content
5. Add guests and send invitations
6. Test RSVP, wishes, gallery, gifts, playlist, livestream

### Configure Integrations

**Email (Resend):**
1. Add `RESEND_API_KEY` to Vercel
2. Verify domain in Resend
3. Test invitation emails

**Analytics (GA4 + Clarity):**
1. Add `NEXT_PUBLIC_GA_ID` and `NEXT_PUBLIC_CLARITY_ID`
2. Verify tracking in browser dev tools

**Payments (Stripe):**
1. Add Stripe keys to Vercel
2. Create webhook endpoint: `https://your-domain.com/api/webhooks/stripe`
3. Subscribe to events: `payment_intent.succeeded`, `payment_intent.failed`
4. Add webhook secret to Vercel

**Livestream:**
1. Set up RTMP server (nginx-rtmp, SRS, or cloud provider)
2. Add ingest/playback URLs to Vercel
3. Test stream with OBS

## 7. Monitoring & Maintenance

### Health Checks

- Vercel Analytics: Project > Analytics
- Supabase Dashboard: Database > Logs, API > Logs
- GitHub Actions: Actions tab for CI/CD status

### Database Maintenance

```bash
# Backup (run weekly)
supabase db dump --file backup-$(date +%Y%m%d).sql

# Monitor performance
# Check Supabase Dashboard > Database > Advisors
```

### Updates

```bash
# Update dependencies
pnpm update

# Check for security vulnerabilities
pnpm audit

# Update Supabase CLI
npm install -g supabase@latest
```

## 8. Troubleshooting

### Common Issues

**Build fails with "Module not found":**
- Check all imports use `@/` alias correctly
- Run `pnpm install` to ensure dependencies

**Supabase connection errors:**
- Verify environment variables are set correctly
- Check Supabase project is not paused
- Verify RLS policies allow the operation

**Realtime not working:**
- Ensure tables are added to supabase_realtime publication
- Check WebSocket connection in browser dev tools

**Vercel deployment timeout:**
- Increase function maxDuration in vercel.json
- Optimize heavy API routes

**CORS errors:**
- Verify `NEXTAUTH_URL` matches deployed domain
- Check Supabase auth settings allow your domain

### Getting Help

- Check GitHub Actions logs for CI/CD errors
- Check Vercel Function Logs for runtime errors
- Check Supabase Logs for database errors
- Open GitHub Issue for bugs

## 9. Scaling Considerations

### Database

- Enable connection pooling (Supabase provides PgBouncer)
- Add indexes for high-volume queries
- Consider read replicas for analytics

### Storage

- Use CDN for media delivery (Supabase has built-in CDN)
- Implement image optimization (Next.js Image component)
- Set up lifecycle policies for old uploads

### Functions

- Use Edge Functions for low-latency endpoints
- Implement caching for frequent queries
- Monitor function duration and optimize

### Realtime

- Limit subscriptions per client
- Use presence for viewer counts
- Implement message throttling

## 10. Security Checklist

- [ ] All environment variables set in Vercel (not in code)
- [ ] Service role key only in server environment
- [ ] RLS policies enabled on all tables
- [ ] CSP headers configured in vercel.json
- [ ] Rate limiting on API routes
- [ ] Input validation with Zod on all endpoints
- [ ] HTTPS enforced (Vercel default)
- [ ] Secure cookies configured
- [ ] Regular security audits (GitHub Actions + Snyk)
- [ ] Backup strategy in place

---

## Quick Reference Commands

```bash
# Development
pnpm dev                    # Start dev server
pnpm build                  # Production build
pnpm start                  # Start production server
pnpm lint                   # Run ESLint
pnpm typecheck              # Run TypeScript check
pnpm test                   # Run tests
pnpm format                 # Format with Prettier

# Database
pnpm db:types               # Generate Supabase types
pnpm db:migrate:local       # Run local migrations
pnpm db:push                # Push to remote Supabase
pnpm db:studio              # Open Supabase Studio

# Deployment
vercel                      # Deploy preview
vercel --prod               # Deploy production
```