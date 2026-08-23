# Resume Points

## Meta (never include in resume)

- Target roles: fullstack SWE intern, frontend SWE intern, backend SWE intern
- Strongest selling points: Aerospace Corp breadth (AI/RAG pipeline, full-stack, k8s/devops, design systems), LBNL breadth (UI, design systems, testing, backend, GCP), ZotBins full-stack ownership, UCI Research full-stack build
- Weak bullets to deprioritize: HPE "Learned about Git..." bullet, any generic "assisted in" phrasing
- Notes: HPE is old (2021) and should only appear if page space allows or role is explicitly systems/Python focused.

---

## Skills

### Programming Languages
TypeScript, JavaScript, Python, SQL, Go

### Frontend
React, HTML/CSS, Tailwind, Vue, Vite, Webpack, shadcn, MUI, Plotly.js, TanStack Query, D3.js

### Backend
FastAPI, Node/Express, Next.js, Remix.js, Postgres, pgvector, SQLite, Supabase, MongoDB, Elasticsearch, Redis

### Infrastructure / DevOps
GCP, AWS, Vercel, Docker, Kubernetes, Rancher, Helm, Jenkins, Podman, Nginx, GitHub Actions, Git

---

## Experience

### Software Engineering Intern, The Aerospace Corporation
**June 2026 - Present | El Segundo, CA**

> **Context:** Aerospace and defense R&D company. Working across several internal tools supporting launch operations and engineering visibility: STARS Jedi (a RAG-based document search/chatbot pipeline for day-of-launch analysts), Winds (a full-stack wind-monitoring dashboard used to decide launch go/no-go based on wind data), an internal React component library for data-viz dashboards, and an internal showcase app for division project demos. Stack: Go, Redis, Docling OCR, OpenAI API, Postgres/pgvector, Python, FastAPI, MongoDB, React, TypeScript, Vite, TanStack Query, Material UI, Plotly.js, GridStack, Node.js, Podman, Docker, Kubernetes, Rancher, Helm, Jenkins/Groovy, Gitea, Storybook. Good for: full-stack, backend, AI/RAG, DevOps/infra roles.

- ⭐ Modernized a launch day monitoring application for analysts to decide launch go/no-go decisions based on wind data, replacing a previously manual data-review process with a Python/FastAPI REST API over MongoDB and a React/TypeScript frontend that renders live wind telemetry with Plotly.js and TanStack Query. `[python, fastapi, mongodb, react, typescript, plotly, tanstack-query, fullstack, data-viz]`
- ⭐ Built the Winds REST API in Python/FastAPI from the company's internal service template with 100% test coverage, giving the React dashboard reliable access to the underlying data. `[python, fastapi, testing, backend, api]`
- ⭐ Engineered a Golang file-processing pipeline for a RAG-assisted AI chatbot that processes Redis queued documents by parsing and embedding hundreds of thousands of proprietary files into a Postgres/pgvector store, giving day-of-launch analysts fast semantic search used across rocket launches the company monitors. `[go, redis, postgres, pgvector, openai, rag, backend, ai]`
- ⭐ Assisted in building and maintaining an internal React dashboard/chart component library in Storybook, complete with integrated regression tests, adopted across 4 projects division-wide to let teams rapidly prototype responsive data-viz dashboards. `[react, storybook, component-library, testing, design-systems, data-viz]`
- Designed the MongoDB schema for Winds through interviews with launch analysts to identify required data to populate the UI, then populated it via a Python script consolidating web-scraped data and a legacy file-based system. `[mongodb, python, requirements, ux-research, backend, data-modeling]`
- Implemented a dynamic, user-configurable dashboard for Winds with GridStack, letting launch analysts create, resize, and reorganize chart cards to fit their workflow. `[react, gridstack, frontend, ui]`
- Kept the Winds dashboard continuously current by building its client data layer on Vite, TypeScript, and TanStack Query, caching API responses and polling for updates on a fixed interval to eliminate manual refreshes. `[react, typescript, vite, tanstack-query, frontend]`
- Utilized zero-downtime rolling deploys to achieve 100% uptime when deploying the Winds API and UI to an on-prem Rancher Kubernetes cluster, containerizing services with Docker and applying k8s manifests via kubectl. `[docker, kubernetes, rancher, gitea, devops]`
- Built a CI/CD pipeline for Winds with Jenkins (Groovy) and Helm charts, replacing manual kubectl deployments with automated, repeatable releases. `[jenkins, groovy, helm, kubernetes, ci-cd, devops]`
- Led requirements-gathering meetings and interviews with end-user launch analysts to iteratively refine the Winds dashboard's design and functionality. `[requirements, ux-research, leadership]`
- Developed a Node.js app that spins up ~12 project demo sites via Podman containers and renders them on an interactive touch-table kiosk, giving all ~120 engineers across the division visibility into each other's work. `[node, podman, containers, internal-tools]`

### Software Engineering Intern, Lawrence Berkeley National Labs
**June 2025 - June 2026 | Berkeley, CA**

> **Context:** Worked as a SWE intern on several projects in a team of ~9 people with 2 mentors. One project, TerraNova, is an open-source network visualization tool used by DOE researchers to map ESnet (the Department of Energy's high-speed research network) traffic and topology. Primarily frontend-heavy with backend and infra work. Stack: React, TypeScript, FastAPI, SQLite, GCP, GitHub Actions, Playwright, Jest, Storybook, SCSS. Good for: frontend, fullstack, data visualization, design systems, testing roles. Another project, called Packets, is ESnet's organization-wide design system and component library. Stack: React, Web Components (HTMX), SCSS, GitHub Actions, Playwright, Docker

- ⭐ Built a network visualization tool allowing DOE researchers to construct, visualize, and export network topology graphs, with a Python/FastAPI/SQLite backend and TypeScript/React/Tailwind user interface. `[react, typescript, tailwind, python, fastapi, sqlite, ui, data-viz]`
- ⭐ Accelerated frontend development for other engineers by implementing a custom 30+ component library with React and SCSS, documented fully in Storybook. `[react, scss, storybook, component-library, design-systems]`
- Enabled teams to manage network topology data in Google Sheets by integrating a GCP service account for ingestion via Google Drive API. `[gcp, python, backend, integrations]`
- Served low-latency graph data to network map builders from a cached SQLite store, refreshed automatically every hour via GitHub Actions. `[sqlite, github-actions, ci-cd, backend]`
- Eliminate feature regression for all user journeys in network map creation and publishing by creating E2E tests in Pytest and Playwright. `[playwright, pytest, testing, e2e]`
- Simplify deployment pipeline by migrating database from Elasticsearch to SQLite. `[sqlite, elasticsearch, backend, devops]`
- Reduced Python REST API response sizes up to 75% through pagination, cutting page load times for viewing large datasets of map information. `[python, fastapi, backend, performance]`
- Assisted in designing an organization-wide token-based design system in Figma for consistent UI/UX across applications. `[figma, design-systems, ui]`
- Prevented future regressions from shipping to downstream applications by implementing Playwright screenshot regression and interaction tests across all 30+ design system components. `[playwright, jest, testing, screenshot, regression]`
- Shortened component library release cycles by authoring GitHub Actions testing workflows with test sharding and Docker containers, cutting CI runtime by 80% and tightening contributor feedback loops. `[github-actions, ci-cd, docker, devops, testing]`

### Undergraduate Research Assistant, University of California, Irvine
**Nov 2024 - Dec 2025 | Irvine, CA**

> **Context:** Built internal tooling for a CS research lab to manage HPC (high-performance computing) jobs on university Linux servers. Solo full-stack build. Stack: React, Material UI, Node.js, Express, GitHub Actions, Vercel. Good for: fullstack, backend, research software, systems-adjacent roles.

- ⭐ Enable researchers to submit hundreds of concurrent HPC jobs on school Linux servers that were previously manually managed by implementing a React MUI interface with Node.js and Express backend. `[react, node, express, fullstack, ui]`
- Configured CI/CD pipeline with GitHub Actions to ensure stable deployments on locally hosted machines, maintaining 99%+ uptime. `[github-actions, ci-cd, vercel, devops]`

### Engineering Intern, Hewlett Packard Enterprise
**May 2021 - Aug 2021 | Remote**

> **Context:** Early internship (high school). Wrote Python tooling for a networking/server team. Low relevance for most roles — include only if page space allows or role is Python/systems focused.

- Developed a Python data-retrieval script to remotely query live production servers for traffic data, giving insight on failures and outages. `[python, scripting, networking]`
- Automated unit tests with Pytest to ensure cross-platform functionality across Windows and Linux. `[pytest, testing, python]`
- Learned about Git, version control systems, and GitHub for collaboration and code history management. `[git]`

---

## Projects

### UCI ZotBins Project (Smart Trash Bin)
**September 2025 - Present | Irvine, CA**

> **Context:** IoT smart trash bin project at UCI — sensors in bins send waste sorting data to a backend, which surfaces analytics to bin maintainers via a dashboard. Team project under UCI research. Stack: React, TypeScript, D3.js, Next.js, FastAPI, Supabase, AWS Lambda, API Gateway, Nginx. Good for: fullstack, data viz, IoT/embedded-adjacent, backend API roles.

- ⭐ Led the design and development of a TypeScript and React dashboard with D3.js, consolidating complex datasets into insights into trash data trends. `[react, typescript, d3, data-viz, frontend]`
- ⭐ Implemented Python RESTful API with FastAPI, complete with OpenAPI specs, to post thousands of sensor recordings from dozens of smart trash bins to Supabase. `[python, fastapi, api, supabase, backend]`
- Deployed a serverless REST API using AWS Lambda and API Gateway to ingest and retrieve trash bin IoT sensor data. `[aws, lambda, serverless, backend]`
- Leveraged Next.js SSR to deliver ~200ms load times for the dashboard bin maintainers use to verify waste-sorting accuracy. `[next.js, ssr, frontend, performance]`
- Architected Nginx reverse proxy to selectively expose public-facing API endpoints for external consumers. `[nginx, devops, backend, api]`

### Sense (Personal Finance Application)

> **Context:** Personal project. Full-stack budgeting app. Stack: Remix.js, React, TypeScript, Node.js, Supabase. Good for: fullstack, frontend, consumer app roles.

- ⭐ Utilized Remix.js SSR and optimistic UI for instantaneous loading and seamless UX when managing accounts and tracking dozens of budget items. `[remix, react, ssr, frontend, ux]`
- Implemented Node.js REST API and Supabase backend to manage hundreds of users and budgets. `[node, supabase, backend, api]`

### Facets (Online Stickyboard and Forum)

> **Context:** Personal project. Social forum with daily engagement prompts. Stack: AWS Lambda, Prisma, PostgreSQL. Good for: backend, cloud, infrastructure roles.

- Architected daily engagement posts by implementing cron-triggered AWS Lambda functions. `[aws, lambda, serverless, cron]`
- Leveraged Prisma ORM to create data models and store them in a PostgreSQL database. `[prisma, postgres, backend, orm]`

### Unimarket (Student Reseller Market)

> **Context:** Personal project. Marketplace for UCI students. Stack: Vue.js, Quasar, Python, FastAPI, Supabase. Good for: fullstack, frontend, marketplace/platform roles.

- Designed and implemented an online marketplace for UCI students using Vue.js and the Quasar frontend framework. `[vue, quasar, frontend, ui]`
- Utilized Python FastAPI with Supabase to handle marketplace post creation and updates. `[python, fastapi, supabase, backend]`
