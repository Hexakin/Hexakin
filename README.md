# Hexakin Monorepo

## Overview

This repository hosts a Turborepo workspace with two Next.js applications (`apps/web`, `apps/docs`) and a shared UI package. The `web` app uses Prisma/PostgreSQL via tRPC; the `docs` app consumes the shared UI components. Tooling is standardised with Turborepo, pnpm, ESLint, Prettier, and TypeScript configs that live in the `packages` directory.

## Installation

1. Install Node.js 18.18 or newer and pnpm 9.x (`corepack enable pnpm`).
2. Clone the repository and install dependencies:
   ```sh
   pnpm install
   ```

## Setup

- Copy the example environment file for the web app and adjust credentials for your PostgreSQL instance:
  ```sh
  cp apps/web/.env.example apps/web/.env
  ```
- Ensure the database in `DATABASE_URL` exists, then initialise Prisma:
  ```sh
  pnpm --filter web run db:push
  ```
  (Use `db:migrate` once you add migrations.)

## Run

- Start all apps in development:
  ```sh
  pnpm dev
  ```
- Start a single app:
  ```sh
  pnpm --filter web run dev   # Next.js app at http://localhost:3002
  pnpm --filter docs run dev  # Docs app at http://localhost:3001
  ```
- Build and type-check everything:
  ```sh
  pnpm run build
  pnpm run check-types
  ```
- Run Prisma Studio for inspecting data:
  ```sh
  pnpm --filter web run db:studio
  ```

## Known Limitations

- A running PostgreSQL instance is required for Prisma commands and any feature that touches the database. The default connection string points to `postgresql://postgres:password@localhost:5432/web`; update it if your credentials differ.
- No automated test suite is included. Add unit/integration tests before relying on the project for production workloads.
- Tailwind CSS v4 is enabled for the `web` app; third-party plugins that assume v3 syntax may need adjustments.