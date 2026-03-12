# MAJC — Hospitality AI Platform Demo

A working prototype mirroring the core architecture of the MAJC hospitality workforce platform — built to demonstrate full-stack TypeScript engineering across mobile, edge API, AI matching, and Postgres schema layers.

**[Live Demo →](https://your-demo-url.netlify.app)**

---

## What This Demonstrates

This demo shows a simplified but production-structured implementation of the three core MAJC platform pillars:

**AI-powered job matching marketplace** — hospitality operators post roles, professionals get AI-ranked matches based on skills via OpenRouter / Gemini Flash.

**Hospitality professional network** — user profiles with skills, experience, and application tracking backed by a typed Drizzle / Neon Postgres schema.

**Interactive learning academy** — training modules with SCORM integration, progress tracking, and job-requirement linkage.

---

## Stack

Mirrors the MAJC production architecture end-to-end:

| Layer | Technology |
|---|---|
| Mobile | Expo SDK 55, React Native 0.83, React 19, Tamagui |
| State | Redux Toolkit + RTK Query |
| API | Cloudflare Workers, Hono framework, Zod validation |
| Database | Neon Postgres, Drizzle ORM |
| AI | OpenRouter — Gemini Flash 1.5 |
| Storage | Cloudflare R2 |
| CI/CD | GitHub Actions, EAS build pipelines |
| Testing | Playwright (E2E), Jest (unit) |
| Language | TypeScript — end-to-end, monorepo |

---

## Monorepo Structure

```
majc-hospitality-ai-platform-demo/
│
├── apps/
│   ├── mobile/              # Expo React Native app
│   │   ├── screens/         # JobFeed, AIConcierge, JobDetails, Training, Profile
│   │   ├── store/           # Redux slices + RTK Query endpoints
│   │   └── components/      # Tamagui UI components
│   │
│   └── api/                 # Cloudflare Worker — Hono
│       └── src/
│           ├── routes/      # jobs, match, apply, training, users
│           ├── middleware/  # auth, cors, validation
│           └── index.ts     # Worker entrypoint
│
├── packages/
│   ├── db/                  # Drizzle ORM schema — Neon Postgres
│   │   ├── schema.ts        # users, jobs, operators, applications, ai_matches...
│   │   └── migrations/
│   ├── ai/                  # OpenRouter matching engine
│   │   └── matcher.ts       # Gemini Flash skill-to-job ranking
│   └── types/               # Shared TypeScript types across apps
│
├── infra/
│   ├── wrangler.toml        # Cloudflare Worker config
│   └── .github/workflows/   # GitHub Actions CI/CD + EAS builds
│
└── demo/
    └── majc-hospitality-demo.html   # Single-file interactive UI demo
```

---

## Database Schema

Core tables implemented with Drizzle ORM:

`users` — professionals and operators, role enum, skills array, experience

`operators` — venue profiles, type enum (restaurant / hotel / bar), tier

`jobs` — listings with skills_required array, salary range, status enum

`applications` — user-to-job with status lifecycle (applied → review → offer)

`ai_matches` — persisted match results with score and reasoning from LLM

`training_modules` — SCORM-linked courses with required_for job type array

`certifications` — earned certs linked to users and training completions

---

## Key API Endpoints

```
GET    /api/jobs                    List active jobs with filters and pagination
POST   /api/jobs                    Operator creates a job listing (Zod validated)
POST   /api/match                   AI concierge matches user skills to active jobs
POST   /api/apply                   Professional submits an application
GET    /api/training-modules        Returns modules with completion status
GET    /api/users/:id/profile       Full profile with skills, matches, certifications
PUT    /api/applications/:id        Operator updates application status
```

---

## AI Matching Flow

```typescript
// packages/ai/src/matcher.ts
export async function aiMatch(skills: string[], jobs: Job[]): Promise<Match[]> {
  const prompt = `
    Match this hospitality professional.
    Skills: ${skills.join(', ')}
    Jobs: ${JSON.stringify(jobs.map(j => j.title))}
    Return ranked JSON: [{jobId, score, reason}]
  `
  const res = await openrouter({
    model: 'google/gemini-flash-1.5',
    messages: [{ role: 'user', content: prompt }]
  })
  return JSON.parse(res.choices[0].message.content)
}
```

---

## Running the Demo

Open the single-file demo directly in any browser — no build step required:

```bash
git clone https://github.com/devkashifilyas-ui/majc-hospitality-ai-platform-demo
cd majc-hospitality-ai-platform-demo
open demo/majc-hospitality-demo.html
```

---

## Deploying the Demo

**Netlify Drop:** drag `majc-hospitality-demo.html` onto [app.netlify.com/drop](https://app.netlify.com/drop) for an instant live URL.

**GitHub Pages:** Settings → Pages → Source: `main` / `root` → live at `https://your-username.github.io/majc-hospitality-ai-platform-demo`

---

## About

Built as a technical demonstration for the MAJC Full Stack Engineer engagement. The architecture mirrors the production MAJC platform and demonstrates end-to-end TypeScript engineering across mobile, API, AI, and database layers — exactly the scope of the role.

**[Your Name]** — [Upwork Profile](https://upwork.com) · [GitHub](https://github.com/devkashifilyas-ui) · [Email](mailto:you@email.com)
