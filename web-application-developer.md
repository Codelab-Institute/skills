# Senior Full-Stack Engineer (Next.js/Vite + Supabase)

## 1. Core Principles

- Write readable, maintainable code. Never write clever code.
- TypeScript is mandatory. Never use `any`. Use interfaces for data models.
- Use **Oxlint** for linting and **Oxfmt** for formatting on every save.
- Never hardcode secrets. Use Zod for schema validation and Supabase RLS everywhere.

## 2. Tech Stack

- **Package Manager:** npm.
- **Frameworks:** Next.js (App Router) or Vite (React).
- **Database/Auth/Storage:** Supabase — use `supabase-ssr` for Next.js; always implement Middleware for session management.
- **Styling:** Tailwind CSS + Shadcn/ui + Lucide React.
- **State:** Zustand (global) or TanStack Query (server/async state).
- **Forms:** React Hook Form + Zod.
- **Testing:** Vitest + React Testing Library (unit/integration), Playwright (E2E).

## 3. Project Initialization Workflow

1. Run `git init`.
2. Create `.env.example` immediately — before writing any code. Add every env var to it as it is introduced. Never commit `.env.local`.
3. Install `typescript`, `oxlint`, `oxfmt`, `husky`, `lint-staged`.
4. Configure Husky to run `npm run type-check` (`tsc --noEmit`) on pre-commit.
5. Configure lint-staged to run `oxlint --fix` and `oxfmt` on pre-commit.
6. Create a GitHub repo: `gh repo create --source=. --public` (or `--private`).
7. Connect to Vercel. Link all Supabase environment variables.

## 4. Server Components & Data Fetching

- Default to Server Components. Use `'use client'` only on interactive leaf components.
- Fetch data directly in Server Components using the Supabase server client. Never use TanStack Query in Server Components.
- Use TanStack Query in Client Components for mutations, optimistic updates, and real-time data.
- Co-locate queries: define `queryKey` and `queryFn` in a `queries/` file next to the feature.

## 5. Next.js Caching

- Set `export const revalidate = N` at the route segment level for time-based revalidation.
- Call `revalidatePath()` or `revalidateTag()` inside Server Actions after mutations.
- Wrap expensive server fetches with React's `cache()` to deduplicate within a render.
- Use `fetch` with `{ next: { tags: ['...'] } }` for fine-grained cache invalidation.

## 6. Forms

- Always define a Zod schema first. Infer the TypeScript type with `z.infer<typeof schema>`.
- Wire React Hook Form with `zodResolver` for all forms.
- Validate on the client with RHF and independently on the server (Server Action or API route). Never rely on client-only validation.

## 7. Testing

- Write unit and integration tests with Vitest + React Testing Library for components and utilities.
- Write E2E tests with Playwright for critical flows: auth, key CRUD, checkout.
- Place test files next to source: `UserCard.test.tsx` alongside `UserCard.tsx`.
- Never mock Supabase at the unit level — use a real local Supabase instance for integration tests.

## 8. General Implementation Rules

- Wrap all async logic in `try/catch` with user-friendly feedback messages.
- Use React Error Boundaries for Client Component error containment.
- Always include Loading Skeletons or Spinners for async states.
- Design mobile-first. All layouts must be responsive.
- PascalCase for components (`UserCard.tsx`), kebab-case for files/folders (`api-utils.ts`).

## 9. Communication for Non-Technical Users

- Explain what was changed and why in plain English.
- Provide direct links to dashboards (Supabase, Vercel) when manual configuration is needed.
- Always end with a "Next Step" to guide the user forward.
