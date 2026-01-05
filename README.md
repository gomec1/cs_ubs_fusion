# CS · UBS · Fusion — Platform Overview

## Run Locally (Quick Start)
1) Install dependencies: `npm install`
2) Environment: copy `.env.example` to `.env.local` (if present) and set at least:
    - `DATABASE_URL` (MongoDB connection string)
    - `AUTH_SECRET` (next-auth secret)
    - OAuth client IDs/secrets (e.g., Google/GitHub) if you want SSO
    - `CLOUDINARY_*` (cloud name, key, secret) for uploads
3) Dev server: `npm run dev` and open http://localhost:3000
4) Build check: `npm run build` (optional) for production readiness.

## What This Is
A Next.js 15 (App Router) app with React 19, TypeScript, Tailwind CSS. It documents and enables discussion of the Credit Suisse–UBS merger and the Archegos/Greensill cases. Features: blogs with reactions/comments, interactive organigram for self-registration, authentication (credentials + OAuth), role-based access (USER/ADMIN/EDITOR), multilingual UI via next-intl, SEO-friendly SSR/ISR, media uploads via Cloudinary, and data persistence with Prisma on MongoDB.

## Architecture at a Glance
```
                        +--------------------------+
                        | Next.js 15 (App Router)  |
                        | React 19, TS, Tailwind   |
                        +------------+-------------+
                                         |
                      +-------------+-------------+
                      | Server (SSR/ISR, actions) |
                      | Auth, Prisma, i18n        |
                      +-------------+-------------+
                                         |
    +---------------------------+---------------------------+
    | next-auth (cred+SSO)      | Prisma + MongoDB          |
    | sessions with roles       | users, blogs, comments,   |
    |                          | reactions, organigram     |
    +---------------------------+---------------------------+
                                         |
                            Cloudinary (signed uploads)
                                         |
                            Tailwind UI (blogs, org chart)
```
Key properties: SSR/ISR for SEO and freshness, role-based checks, signed uploads, locale-aware routing, server-side validation.

## Key Features
- Blogs: create/edit, likes/dislikes, threaded comments, image uploads via Cloudinary; auth-gated mutations; owner or ADMIN/EDITOR moderation.
- Organigram: interactive chart; users add themselves, choose parent divisions, upload photos; admin delete; cycle checks for parents.
- Auth & Roles: next-auth with credentials and OAuth; roles USER/ADMIN/EDITOR guard edit/delete/moderation paths.
- Internationalization: next-intl with localized routing and message catalogs (DE, EN, FR, IT, ES).
- SEO & Performance: SSR/ISR for content, metadata helpers, sitemap/robots, optimized images.
- Theming & UX: Tailwind, light/dark mode, responsive layout, reusable layout components.

## Development Scripts
- `npm run dev` — start dev server
- `npm run build` — production build
- `npm run lint` — linting

## Data & Persistence
- Prisma ORM targeting MongoDB. Set `DATABASE_URL` to your instance. Consider separate databases per environment.

## Media
- Images upload to Cloudinary using signed parameters from a backend route. Set `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`.

## Auth
- next-auth with credentials plus optional OAuth (e.g., Google/GitHub). Set provider IDs/secrets and `AUTH_SECRET`. Sessions carry role for RBAC.

## Internationalization
- next-intl powers locale routing and translations. Keep locale message files in sync to avoid missing keys.

## Security Notes
- Role-based checks on server actions (blogs, users, organigram) for edit/delete.
- Signed upload parameters to prevent client-side tampering.
- Input validation in server actions before writes.

## Testing & Verification
- `npm run build` to ensure type/lint/build pass.
- Manual checks: auth flows (login/register/SSO), blog CRUD with reactions/comments, organigram add/delete, locale switching.

## Deployment
- Standard Next.js deployment (e.g., Vercel or custom Node runtime). Set environment variables server-side; ensure MongoDB and Cloudinary are reachable.
