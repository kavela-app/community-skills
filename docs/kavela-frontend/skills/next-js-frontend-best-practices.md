# Next.js Frontend Best Practices

> Best practices for the Kavela frontend Next.js application covering server components, API patterns, authentication, and environment configuration

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Kavela)

**Keywords:** `next.js` `server components` `api` `fetch` `authentication` `env` `environment variables` `.env` `NEXT_PUBLIC` `best practices` `frontend` `react server components` `RSC`

[View on Kavela Marketplace](https://kavela.ai/marketplace/kavela-frontend)

---
# Next.js Frontend Best Practices

## API / MCP Data Fetching

### Server Components (Preferred)
All API calls and MCP server interactions should be done in **React Server Components** whenever possible. Server Components run only on the server, which means:
- API keys and secrets are never exposed to the client bundle
- Requests are made server-to-server (faster, no CORS issues)
- Responses can be streamed and cached at the edge

```tsx
// app/dashboard/page.tsx — Server Component (default in Next.js App Router)
import { fetchSkills } from "@/lib/api";

export default async function DashboardPage() {
  const skills = await fetchSkills(); // Runs on the server only
  return <SkillsList skills={skills} />;
}
```

### When Client Components Are Necessary
Use `"use client"` only when you need:
- Browser APIs (localStorage, window, document)
- Event handlers (onClick, onChange)
- React hooks (useState, useEffect, useRef)
- Third-party client-only libraries (Framer Motion, Three.js)

For data that needs client-side interactivity, fetch in a Server Component and pass as props:

```tsx
// Server Component: fetches data
export default async function Page() {
  const data = await fetchFromAPI();
  return <InteractiveChart data={data} />; // Client component receives pre-fetched data
}
```

### Current Exception
The dashboard currently uses client-side fetching (`useEffect` + `fetch`) because the entire dashboard is wrapped in client-side providers (ThemeProvider, Framer Motion). This is acceptable for the MVP but should be migrated to server components with a streaming/suspense pattern as the product matures.

## Authentication

### Server-Side Auth (Required)
Authentication checks and token handling **must** happen in Server Components or middleware — never in client-side code:

```tsx
// middleware.ts — Protects routes server-side
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  const token = request.cookies.get("session_token");
  if (!token && request.nextUrl.pathname.startsWith("/dashboard")) {
    return NextResponse.redirect(new URL("/authui", request.url));
  }
}
```

### Auth Token Storage
- **HttpOnly cookies** for session tokens (set by the backend, not accessible via JS)
- **Never** store auth tokens in localStorage or sessionStorage
- Use Next.js `cookies()` API in Server Components to read auth state

### Current State
Auth is not yet implemented. When it is added:
- Session management should use HttpOnly cookies
- Auth checks should be in middleware.ts or Server Component data fetchers
- Client components should receive auth state via props or a server-side context provider

## Environment Variables

### Configuration
All static configuration values (API URLs, feature flags, third-party keys) must be declared in `.env.local` (or `.env` for defaults):

```env
# .env.local
NEXT_PUBLIC_KAVELA_API_URL=https://kavela-mcp.tanyonghong-prv.workers.dev
KAVELA_API_SECRET=sk_secret_key_here
```

### Naming Convention
- `NEXT_PUBLIC_` prefix: Exposed to the browser (client components). Use for non-sensitive values only.
- No prefix: Server-only. Use for API secrets, database URLs, auth tokens.

### Usage
```tsx
// Client-safe (bundled into client JS)
const apiUrl = process.env.NEXT_PUBLIC_KAVELA_API_URL;

// Server-only (throws error if accessed in client component)
const secret = process.env.KAVELA_API_SECRET;
```

### Rules
- **Never** hardcode URLs or secrets in source code
- **Never** prefix sensitive values with `NEXT_PUBLIC_`
- Always provide a fallback in `lib/api.ts` for local development
- Add `.env.local` to `.gitignore` (it already is by default in Next.js)

## Component Organization

### File Structure
```
app/
  dashboard/
    page.tsx          # Server Component — data fetching entry point
    layout.tsx        # Shared layout with providers
components/
  dashboard/
    Sidebar.tsx       # Client component (interactivity)
    DashboardHome.tsx # Client component (animations)
    SettingsPanel.tsx  # Client component (localStorage)
lib/
  api.ts             # API service layer (can be used server or client)
```

### Theme / User Preferences
- Theme preference is stored in `localStorage` via the `SettingsPanel` component
- Uses the `kavela_user_preferences` key in localStorage
- TODO: Migrate to user configuration API endpoint once auth is implemented
- The ThemeProvider applies the `dark` class to `<html>` for Tailwind dark mode

