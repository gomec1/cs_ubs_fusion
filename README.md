# Projekt starten (wichtig)

> Ohne gültige `.env` lässt sich die Anwendung nicht starten. Die Datei ist absichtlich nicht im Repository. Der Dozent kann das Repo klonen; die benötigte `.env` sende ich separat. API-Keys, DB-URL, Auth-Secrets etc. sind zwingend.
## Quickstart (lokal)

Voraussetzungen:
- Node.js ≥ 18 (empfohlen) und npm (packageManager: npm@11.6.1)
- Git

Schritte:
1. Repository klonen  
    ```bash
2. Abhängigkeiten installieren  
    ```bash
    npm install
3. `.env` bereitstellen  
    - Die Datei kommt separat von mir (kein `.env.example` im Repo).
    - Enthält u. a. Datenbank-URL, Auth-Secrets, OAuth-Provider, Cloudinary.
4. Entwicklung starten  
    ```bash
    npm run dev   # Next.js mit Turbopack unter http://localhost:3000
5. (Optional) Build prüfen  
    ```bash
    npm run build
6. (Optional) Produktionsstart nach Build  
    ```bash
    npm start
7. (Optional) Linting  
    ```bash
    npm run lint
---

## Inhalt

- [Überblick](#überblick)
- [Projektstruktur](#projektstruktur)
- [Technologie-Stack](#technologie-stack)
- [Funktionen](#funktionen)
- [Konfiguration / Umgebungsvariablen](#konfiguration--umgebungsvariablen)
- [Scripts & Commands](#scripts--commands)
- [Deployment](#deployment)
- [Kontext](#kontext)
- [Lizenz & Disclaimer](#lizenz--disclaimer)

## Überblick
Webplattform zur Dokumentation und Diskussion der Credit Suisse–UBS-Fusion sowie der Fallstudien Archegos/Greensill. Enthält Blogs, Organigramm, rollenbasierten Zugang und Mehrsprachigkeit. Einzelprojekt im Studiengang Wirtschaftsinformatik (BFH); sicherer Login für Beitragende.

## Projektstruktur
- [app](app) — Next.js App Router (Layouts, Seiten, Routen, Server Actions); inkl. lokalisierter Routen ([app/[locale]](app/%5Blocale%5D)).
- [components](components) — UI-Bausteine (Navbar, Footer, Auth-Buttons, Blog-UI, shadcn/ui-basierte Inputs/Buttons).
- [lib](lib) — Hilfsfunktionen: Prisma-Client, Auth/Password-Helpers, Org-Chart-Logik, Caching/SEO, Sanitizer.
- [i18n](i18n) & [messages](messages) — next-intl Routing/Navigation, Lokalisierungsdateien (DE/EN/FR/IT/ES).
- [prisma](prisma) — [schema.prisma](prisma/schema.prisma) für MongoDB; Models für User, Blogs, Kommentare, Reaktionen, Org-Chart.
- [public](public) — statische Assets.
- [scripts](scripts) — z. B. [create-admin.ts](scripts/create-admin.ts) zum Anlegen eines Admin-Users.
- [middleware.ts](middleware.ts) — Locale-Handling, ggf. Auth-Routing.
- [eslint.config.mjs](eslint.config.mjs), [tailwind.config.ts](tailwind.config.ts), [tsconfig.json](tsconfig.json) — Tooling/Build-Konfiguration.

## Technologie-Stack
- Framework: Next.js 15 (App Router, SSR/ISR) mit React 19, TypeScript.
- Styling: Tailwind CSS 4, tailwindcss-animate; eigene Komponenten (shadcn/ui-Stil) unter [components/ui](components/ui).
- Auth: next-auth (Credentials + OAuth, Google u. a.), Adapter: @auth/prisma-adapter.
- DB/ORM: MongoDB mit Prisma Client; Password-Hashing via bcryptjs.
- Internationalisierung: next-intl, Inhalte gepflegt via i18nexus (CLI).
- Media: Cloudinary (next-cloudinary) für signierte Uploads.
- Rich Text: Quill (mit Image-Resize-Plugin) + sanitize-html für sichere Inhalte.
- Visualisierung: d3/d3-org-chart für Organigramm.
- Validation: zod.
- Utilities: jsonwebtoken, react-hot-toast.
- Tooling: ESLint 9 (eslint-config-next), TypeScript 5, Turbopack, concurrently (Dev mit i18nexus), Tailwind/PostCSS/Autoprefixer.

## Funktionen
- Authentifizierung & Rollen: Login/Registration (Credentials + OAuth), Rollen USER/ADMIN/EDITOR für geschützte Aktionen.
- Blog-Modul: Erstellen/Bearbeiten/Löschen (rollen- und besitzergesteuert), Reaktionen (Like/Dislike), Kommentare, Bild-Uploads via Cloudinary.
- Organigramm: Interaktives Org-Chart (d3-org-chart), Hinzufügen/Entfernen von Einträgen, Validierung von Hierarchien.
- Mehrsprachigkeit: Locale-Routing, Übersetzungen in DE/EN/FR/IT/ES.
- SEO & Auslieferung: SSR/ISR, Sitemaps/Robots, Metadaten-Helpers.
- UI/UX: Light/Dark-Mode, wiederverwendbare Layout-Sektionen, Toast-Feedback.

## Konfiguration / Umgebungsvariablen
- `.env` ist verpflichtend und wird separat bereitgestellt (nicht im Repo). Secrets niemals committen.
- Typische Variablen (aus dem Code abgeleitet):
    - Datenbank: `DATABASE_URL` (MongoDB)
    - Auth: `AUTH_SECRET`, OAuth-Provider (`GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, ggf. weitere)
    - Storage: `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`
    - App/Host: ggf. `NEXTAUTH_URL`, `NEXT_PUBLIC_*` für öffentliche Keys/Endpoints
- Falls eine `.env.example` ergänzt wird, daraus kopieren und lokal zu `.env.local` oder `.env` machen.

## Scripts & Commands
| Script | Zweck |
| --- | --- |
| `npm run dev` | Dev-Server mit Turbopack starten (http://localhost:3000). |
| `npm run devnexus` | Dev-Server + i18nexus-Live-Sync parallel (concurrently). |
| `npm run build` | Produktionsbuild (Turbopack). |
| `npm start` | Startet den gebauten Server. |
| `npm run lint` | ESLint-Check. |
| `npm run i18n:pull` | Übersetzungen via i18nexus ziehen. |
| `npm run create:admin` | Admin-User per Script anlegen (ts-node). |

## Deployment
- Standard Next.js Deployment möglich (z. B. Vercel oder eigener Node-Host).
- Erforderlich: Setze alle Umgebungsvariablen (DB, Auth, OAuth, Cloudinary) im Ziel-Environment.
- Build vorab mit `npm run build`; Start via `npm start` auf dem Zielsystem.

## Kontext
- Einzelprojekt im Studiengang Wirtschaftsinformatik (BFH).
- Zweck: Archivierung, Analyse und Diskussion zur CS–UBS-Fusion sowie den Fällen Archegos/Greensill.
- Zielgruppen: Dozent, Studierende, interessierte Leser; Rollen/Access für Beitragende.

## Lizenz & Disclaimer
- Lizenz: Siehe [LICENSE](LICENSE).
- Disclaimer: Plattform dient Dokumentation/Reflexion, keine Rechts- oder Finanzberatung; Inhalte können subjektiv oder unvollständig sein. Secrets gehören nicht ins Repository; Umgebungsvariablen stets sicher verwalten.
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

## 🔧 Setup – Step by Step
!For the following steps use CMD (Windows) / Terminal (Mac)!

1. 📁 Clone the Project

```bash
git clone https://github.com/gomec1/SDA2_2_Assingment_2025.git
cd SDA2_2_Assingment_2025/
```

2. 📦 Install Required Dependencies

```bash
npm install
```

3. ✅ Start application

```bash
npm run dev
```

4. 📂 Folder Structure

```text
app/
components/
lib/
prisma/
public/
scripts/
```

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
