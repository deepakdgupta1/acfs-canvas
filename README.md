# ACFS Canvas

A self-guided, GUI-based software building experience for technical non-developers. Describe what you want to build, watch AI agents build it transparently, and deploy with one click.

Built as an extension of [ACFS (Agentic Coding Flywheel Setup)](https://github.com/deepakdgupta1/agentic-coding).

---

## What It Does

ACFS Canvas guides users through building complete software applications through a 5-step flow:

1. **Describe** — Natural language project description with expandable detail fields
2. **Clarify** — AI asks targeted questions to sharpen the spec
3. **Type** — Select project type (web app, API, full-stack, etc.) with AI recommendation
4. **Review** — Inline-editable specification with feature management
5. **Build** — Interactive SVG canvas showing components, dependencies, and live agent progress

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 16 App Router |
| Runtime | Bun |
| State | TanStack Query + localStorage |
| Animations | Framer Motion (LazyMotion) |
| Styling | Tailwind CSS 4, oklch design tokens |
| Icons | lucide-react |
| Language | TypeScript (strict) |

---

## Getting Started

```bash
# Install dependencies
bun install

# Start development server
bun dev

# Type check
bun run type-check

# Lint
bun run lint
```

Navigate to `/canvas` to start a new project.

---

## Project Structure

```
app/canvas/
├── page.tsx                  # Creates project UUID → redirects to [projectId]
└── [projectId]/page.tsx      # Renders CanvasWizard

components/canvas/
├── canvas-wizard.tsx         # Main orchestrator (step nav, AI calls, transitions)
├── canvas-new-project.tsx    # UUID generation + redirect
├── project-canvas.tsx        # SVG canvas (Epic 2: E2-S1, E2-S3)
├── canvas-detail-panel.tsx   # Slide-in node detail panel (Epic 2: E2-S2)
└── steps/
    ├── description-step.tsx  # E1-S1
    ├── questions-step.tsx    # E1-S2
    ├── type-step.tsx         # E1-S3
    └── review-step.tsx       # E1-S4

lib/canvas/
├── types.ts                  # All Canvas types (project, nodes, agents, build)
├── storage.ts                # localStorage persistence (schema v1)
├── use-canvas-project.ts     # TanStack Query hook
├── mock-ai.ts                # Deterministic mock AI responses (swap-ready)
├── canvas-graph.ts           # Derives SVG graph from CanvasSpecification
├── mock-build.ts             # Build event simulation engine
└── index.ts                  # Barrel re-exports

docs/requirements/            # Full MVP requirements specification
├── README.md                 # Overview + implementation progress
├── epics/                    # 8 epics, 40 user stories
├── user-journeys/            # 5 journey documents
├── acceptance-criteria/      # 170 AC traceability matrix
├── architecture/             # ACFS integration spec
└── metrics/                  # Success metrics framework
```

---

## Implementation Status

| Epic | P0 Stories | Status |
|------|-----------|--------|
| 1. Project Vision & Scoping | 4/4 | ✅ Complete |
| 2. Interactive Canvas | 3/3 | ✅ Complete |
| 3. Agent Orchestration | 2 | ⬜ Not started |
| 4. Task Management | 2 | ⬜ Not started |
| 5. Code Visibility | 1 | ⬜ Not started |
| 6. Testing & Validation | 2 | ⬜ Not started |
| 7. Deployment | 2 | ⬜ Not started |
| 8. Project History | 1 | ⬜ Not started |

See [`docs/requirements/README.md`](docs/requirements/README.md) for full specification and implementation notes.

---

## Architecture Decisions

- **Routing:** `/canvas` creates a UUID and redirects to `/canvas/[projectId]`. Revisiting the URL auto-resumes the project from localStorage.
- **State:** TanStack Query (`staleTime: Infinity`) as in-memory state, backed by localStorage. No extra state libraries.
- **Mock AI:** Deterministic keyword-based mocks with artificial delays. Same return types as real API — swap-in is a one-file change.
- **Animations:** `m` components (not `motion`) from framer-motion's LazyMotion strict mode. All variants from `@/components/motion`.
- **Canvas rendering:** Pure SVG — no canvas element, no third-party graph library. Layout derived from `canvas-graph.ts`.

---

## Key Conventions

- `"use client"` on all interactive components; server components for pages only
- `m.div` not `motion.div` (LazyMotion strict mode)
- Import paths: `@/components/...`, `@/lib/...`
- Icons: lucide-react only
- Button variants: `default`, `ghost`, `secondary`, `gradient` (for primary CTAs)
- Always run `bun run type-check && bun run lint` before committing

---

## License

MIT
