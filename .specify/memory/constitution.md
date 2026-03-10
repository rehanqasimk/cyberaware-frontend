<!--
  Sync Impact Report
  ===================
  Version change: N/A → 1.0.0 (initial ratification)
  Modified principles: N/A (initial creation)
  Added sections:
    - Core Principles (7 principles across 3 categories)
    - Technical Stack
    - Application Architecture
    - Design System
    - AI Assistant Interface
    - State Management
    - Accessibility Standards
    - Performance Targets
    - Project Structure
    - Testing Strategy
    - Error Handling
    - Security Considerations
    - Development Workflow
    - Governance
  Removed sections: N/A
  Templates requiring updates:
    - .specify/templates/plan-template.md: ✅ compatible (Constitution Check
      section references constitution dynamically)
    - .specify/templates/spec-template.md: ✅ compatible (user stories and
      requirements align with principle-driven workflow)
    - .specify/templates/tasks-template.md: ✅ compatible (phase structure
      supports component-driven and test-first workflow)
  Follow-up TODOs: None
-->

# CyberAwareness Frontend Constitution

> Foundational principles and guidelines for AI-assisted development
> of the CyberAwareness frontend application.

## Core Principles

### I. User-Centered Security

- **Clarity Over Complexity**: Security information MUST be
  understandable by non-experts. Jargon MUST be accompanied by
  plain-language explanation or tooltip.
- **Progressive Disclosure**: Show essential information first;
  details on demand via expand/collapse or drill-down navigation.
- **Actionable Insights**: Every alert and notification MUST include
  clear next steps the user can take.
- **Anxiety-Free Design**: Inform without alarming; empower without
  overwhelming. Tone MUST be constructive, never fear-based.

### II. AI Integration Philosophy

- **Conversational, Not Commanding**: AI assistants MUST guide
  users through dialogue rather than issuing directives.
- **Transparent Intelligence**: The UI MUST indicate when AI is
  processing and SHOULD surface reasoning or confidence levels.
- **Graceful Boundaries**: When AI cannot help, it MUST offer a
  clear handoff to human support or alternative resources.
- **Trust Through Consistency**: AI behavior MUST be predictable
  and reliable across sessions and contexts.

### III. Component-Driven Development

- Every UI element MUST be a reusable, composable building block.
- Components MUST be independently testable via Storybook stories
  and unit tests.
- shadcn/ui (Radix primitives) is the canonical component library;
  custom components MUST follow the same API patterns.

### IV. Type Safety

- TypeScript strict mode MUST be enabled project-wide.
- All component props MUST be fully typed; `any` is prohibited
  except in generated code or explicit escape hatches with
  inline justification.
- Zod schemas MUST validate all external data boundaries (API
  responses, form inputs, URL params).

### V. Accessible by Default

- WCAG 2.1 AA compliance is the minimum standard for all pages.
- Keyboard navigation MUST reach every interactive element.
- Screen reader support via semantic HTML; ARIA attributes used
  only when native semantics are insufficient.
- Color contrast MUST meet 4.5:1 for normal text, 3:1 for large.
- Color MUST NOT be the sole indicator of meaning.

### VI. Performance Budget

- Core Web Vitals targets: LCP < 2.5s, FID < 100ms, CLS < 0.1,
  TTFB < 200ms.
- Bundle budgets: initial JS < 200KB gzipped, per-route JS < 50KB
  gzipped, total CSS < 50KB gzipped.
- Heavy components MUST use dynamic imports with loading skeletons.
- Large datasets MUST use virtualized rendering.

### VII. Simplicity & YAGNI

- Start simple; avoid premature abstraction.
- Only make changes that are directly requested or clearly
  necessary; do not add features, configurability, or error
  handling for hypothetical scenarios.
- Three similar lines of code are preferable to a premature
  abstraction.

## Technical Stack

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 14+ (App Router) |
| Language | TypeScript 5+ (strict mode) |
| Runtime | React 18+ |
| Package Manager | pnpm |
| Styling | Tailwind CSS 3+ |
| Component Library | shadcn/ui (Radix primitives) |
| Icons | Lucide React |
| Animation | Framer Motion |
| Charts | Recharts / Tremor |
| Server State | TanStack Query (React Query) |
| Client State | Zustand (minimal global state) |
| Forms | React Hook Form + Zod |
| API Client | Generated from OpenAPI spec (orval/openapi-typescript) |
| AI Streaming | Vercel AI SDK |
| Markdown Rendering | react-markdown + remark-gfm |
| Syntax Highlighting | Shiki |
| Unit/Integration Tests | Vitest + React Testing Library |
| E2E Tests | Playwright |
| Visual Regression | Chromatic (Storybook) |
| Linting | ESLint + Prettier |
| Type Checking | tsc --noEmit |

## Application Architecture

### Layout Structure

The application shell consists of a collapsible sidebar, main
content area, and a collapsible AI assistant panel accessible
from any page.

### Feature Domains

| Domain | Key Views |
|--------|----------|
| Dashboard | Risk Overview, Activity Feed, Quick Actions, Metrics |
| Threats | Threat Feed, Alert Center, Analysis View, IOC Search |
| Training | Course Catalog, My Learning, Assessments, Simulations |
| Reports | Compliance, Training Progress, Threat Intel, Export |
| Settings | Profile, Organization, Integrations, Notifications |
| AI Assistant | Chat Interface, Context Panel, Suggestions, History |

### Project Structure

```
src/
├── app/                    # Next.js App Router
│   ├── (auth)/             # Auth route group (login, register)
│   ├── (dashboard)/        # Protected routes
│   │   ├── layout.tsx      # Dashboard shell
│   │   ├── page.tsx        # Dashboard home
│   │   ├── threats/
│   │   ├── training/
│   │   ├── reports/
│   │   └── settings/
│   ├── api/                # API routes (if needed)
│   ├── layout.tsx          # Root layout
│   └── globals.css
├── components/
│   ├── ui/                 # shadcn/ui primitives
│   ├── features/           # Feature-specific components
│   │   ├── threats/
│   │   ├── training/
│   │   └── ai-assistant/
│   └── layouts/            # Shell, Sidebar, Header
├── lib/
│   ├── api/                # API client (generated)
│   ├── hooks/              # Custom hooks
│   ├── stores/             # Zustand stores
│   └── utils/              # Utilities (cn, date, validation)
├── types/                  # API and domain type definitions
└── styles/                 # Global styles
```

## Design System

### Semantic Colors

- **Brand**: primary (blue-600), secondary (slate-600)
- **Severity**: critical (red-600), high (orange-500),
  medium (yellow-500), low (green-500), info (blue-400)
- **Status**: success (green-600), warning (amber-500),
  error (red-600)
- **Surfaces**: background (slate-50), surface (white),
  muted (slate-100)

### Typography Scale

| Token | Classes |
|-------|---------|
| h1 | text-3xl font-bold tracking-tight |
| h2 | text-2xl font-semibold |
| h3 | text-xl font-semibold |
| h4 | text-lg font-medium |
| body | text-base |
| small | text-sm |
| caption | text-xs text-muted-foreground |

### Component Patterns

- **Card Pattern**: Card > CardHeader > CardContent > CardFooter
- **Data Display**: DataTable with pagination, sorting, filtering
- **AI Response**: AIMessage with role, streaming content, sources,
  and suggested actions

### Icon Conventions

Shield (security), AlertTriangle (warnings), CheckCircle (success),
XCircle (error), Eye (monitoring), Lock/Unlock (auth), Bot (AI),
GraduationCap (training), TrendingUp (metrics).

## AI Assistant Interface

### Chat Architecture

- Messages typed as `ChatMessage` with id, role, content,
  timestamp, and optional metadata (agentType, sources,
  suggestedActions, isStreaming).
- Suggested actions are clickable chips: navigate, action, or query.
- Context awareness: the assistant receives current page, selected
  entity, user role, organization ID, and recent actions.

### Agent Personalities

| Agent | Name | Tone |
|-------|------|------|
| Threat Analyst | Sentinel | Professional, precise, technical |
| Training Coach | Sage | Encouraging, patient, educational |
| Security Advisor | Guardian | Friendly, clear, reassuring |

### Streaming UI

- Responses render in real-time via Vercel AI SDK streaming.
- Markdown rendered with react-markdown + remark-gfm.
- AI-generated HTML MUST be sanitized with DOMPurify before render.
- AI-suggested actions MUST be validated against a whitelist
  before execution.

## State Management

- **Server state**: TanStack Query with 1-minute stale time default.
  Mutations SHOULD use optimistic updates with rollback.
- **Client state**: Zustand for minimal UI state only (sidebar
  collapsed, assistant open, theme). MUST NOT duplicate server state.
- **Form state**: React Hook Form + Zod. All form schemas MUST be
  defined with Zod and validated via zodResolver.

## Accessibility Standards

- WCAG 2.1 AA compliance (minimum) on all pages.
- Skip links in root layout.
- All images MUST have alt text.
- All forms MUST have associated labels.
- Error messages MUST be announced to screen readers.
- Modals MUST trap focus correctly.
- Tab order MUST be logical.
- Axe DevTools MUST pass on all pages before merge.

## Performance Targets

| Metric | Target | Warning Threshold |
|--------|--------|-------------------|
| LCP | < 2.5s | < 4s |
| FID | < 100ms | < 300ms |
| CLS | < 0.1 | < 0.25 |
| TTFB | < 200ms | < 600ms |
| Initial JS (gzip) | < 200KB | — |
| Per-route JS (gzip) | < 50KB | — |
| Total CSS (gzip) | < 50KB | — |

### Optimization Strategies

- Dynamic imports for heavy components (charts, editors) with
  skeleton loading states and `ssr: false` where appropriate.
- Next.js `<Image>` for all images with priority for above-fold.
- Virtualized lists for datasets exceeding ~50 rows.

## Testing Strategy

- **Component unit tests**: Vitest + React Testing Library.
  Test behavior, not implementation.
- **Hook tests**: renderHook with waitFor for async hooks.
- **Integration tests**: Render full pages, simulate user
  interactions with userEvent.
- **E2E tests**: Playwright for critical user journeys.
- **Visual regression**: Storybook stories + Chromatic.
- Tests MUST be co-located or in a parallel `__tests__` directory.

## Error Handling

- Global error boundary at app root with user-friendly fallback.
- Feature-specific error boundaries for isolated failure recovery.
- Consistent skeleton/loading components via Suspense boundaries.
- API errors MUST map to user-friendly messages; raw error codes
  MUST NOT be shown to end users.
- Error boundaries MUST report to error tracking (Sentry).

## Security Considerations

### Frontend Security

- XSS: rely on React's default escaping; `dangerouslySetInnerHTML`
  MUST NOT be used without DOMPurify sanitization.
- CSRF: SameSite cookies + CSRF tokens for mutations.
- Auth tokens: httpOnly cookies only; MUST NOT use localStorage.
- CSP: strict Content-Security-Policy headers via Next.js config.

### AI-Specific Security

- All AI-generated content MUST be sanitized before rendering.
- AI-suggested navigation/actions MUST be validated against a
  whitelist of allowed action types and routes.

## Development Workflow

### Spec-Driven Process

1. `/speckit.constitution` — establish principles (this document)
2. `/speckit.specify` — define feature specification
3. `/speckit.clarify` — resolve ambiguities
4. `/speckit.plan` — generate implementation plan
5. `/speckit.tasks` — break into atomic tasks
6. `/speckit.implement` — execute implementation
7. Visual review via Storybook
8. Test: unit, integration, E2E
9. Iterate: update spec as source of truth

### Component Development Flow

1. Define props interface with TypeScript
2. Create Storybook story
3. Implement component with Tailwind + shadcn/ui
4. Add unit tests
5. Review in Storybook (visual)

### Git Workflow

```
main            <- Production-ready code
  └── develop   <- Integration branch
        └── feature/XXX-description
        └── fix/XXX-description
```

### Commit Convention

```
<type>(<scope>): <description>

Types: feat, fix, docs, style, refactor, test, chore
Scope: threats, training, ai, dashboard, ui
```

## Governance

- This constitution is the authoritative source of project
  principles and technical standards.
- All code reviews and PRs MUST verify compliance with these
  principles.
- Amendments require:
  1. A written proposal describing the change and rationale.
  2. Update to this document with version bump.
  3. Migration plan if the change affects existing code.
- Versioning follows SemVer:
  - MAJOR: principle removals or incompatible redefinitions.
  - MINOR: new principles/sections or material expansions.
  - PATCH: clarifications, wording, typo fixes.
- Compliance review SHOULD occur at the start of each feature
  via the Constitution Check gate in plan-template.md.
- Complexity beyond these standards MUST be justified in the
  plan's Complexity Tracking table.

**Version**: 1.0.0 | **Ratified**: 2026-03-11 | **Last Amended**: 2026-03-11
