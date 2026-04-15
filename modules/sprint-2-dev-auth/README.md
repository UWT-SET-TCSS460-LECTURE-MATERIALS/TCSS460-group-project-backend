# Sprint 2 — Dev Auth Module

A temporary authentication module for the TCSS 460 group project. Students curl these files into their own backend repo during Sprint 2 to get:

- `requireAuth` middleware — verifies `Authorization: Bearer <token>` against `JWT_SECRET`, attaches `request.user`
- `requireRole('admin')` middleware — role gate; use after `requireAuth`
- `POST /auth/dev-login` — accepts a `username`, find-or-creates a user, returns a signed JWT

The `dev-login` endpoint is **local-development only**. It does not validate a password. Sprint 3 replaces it with the real Auth-Squared integration; the middleware stays.

## Expected student project setup

To use these files, the student's group-project repo must have:

1. **Dependencies**
   ```bash
   npm install jsonwebtoken
   npm install -D @types/jsonwebtoken
   ```

2. **`JWT_SECRET` in `.env`** — any long random string for local dev

3. **A Prisma `User` model** with at minimum:
   ```prisma
   model User {
     id       Int    @id @default(autoincrement())
     username String @unique
     email    String @unique
     role     String @default("user")
   }
   ```
   Plus whatever other fields the team designs.

4. **A shared Prisma client at `src/lib/prisma.ts`:**
   ```typescript
   import { PrismaClient } from '@prisma/client';
   export const prisma = new PrismaClient();
   ```

5. **Mount the router** in `src/app.ts` or `src/server.ts`:
   ```typescript
   import devAuthRouter from './routes/devAuth';
   app.use('/auth', devAuthRouter);
   ```

## Destination paths

These files are curl'd into the student's project at:

- `modules/sprint-2-dev-auth/requireAuth.ts` → `src/middleware/requireAuth.ts`
- `modules/sprint-2-dev-auth/devAuth.ts` → `src/routes/devAuth.ts`

The curl commands are documented in the Sprint 2 assignment.

## Why this is a standalone module folder

Kept outside `src/` so this repo's own `tsc` and `eslint` pipelines don't try to compile the files against dependencies (Prisma) that this repo may not install. These files are download-only reference material for students.
