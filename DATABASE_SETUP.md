# Database & Authentication Setup

This project uses **Neon** (PostgreSQL) with **Prisma ORM** and **BetterAuth** for authentication.

## Prerequisites

Ensure you have the following environment variables in your `.env` file:

```env
DATABASE_URL=postgres://...
BETTER_AUTH_SECRET=your_secret_here
BETTER_AUTH_URL=http://localhost:3000
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

## Setup Instructions

### 1. Install Dependencies

If you haven't already, install the project dependencies:

```bash
npm install
```

### 2. Prisma Configuration

The project uses Prisma 7 which separates the datasource URL from the schema for better driver adapter support. The connection is managed in `prisma.config.ts`.

To generate the Prisma Client:

```bash
npx prisma generate
```

### 3. Database Migration / Sync

If you are setting up the database for the first time or the schema has changed, sync your database with:

```bash
npx prisma db push
```

*Note: `db push` is recommended for rapid prototyping with Neon. For production, consider using migrations.*

### 4. BetterAuth CLI

If you need to update the auth schema (e.g., adding new plugins), you can use the BetterAuth CLI:

```bash
npx @better-auth/cli generate --config lib/auth.ts
```

## Usage

### Server-side Auth

Import the `auth` instance from `@/lib/auth`:

```ts
import { auth } from "@/lib/auth";
import { headers } from "next/headers";

const session = await auth.api.getSession({
    headers: await headers()
});
```

### Client-side Auth

Use the `authClient` from `@/lib/auth-client`:

```ts
"use client"
import { authClient } from "@/lib/auth-client";

const { data: session } = authClient.useSession();
```
