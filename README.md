<div align="center">

# Fernando Ibarra

**Senior Backend Engineer**

Distributed systems · Payment engines · Multi-tenant platforms

<sub>Hermosillo, Sonora, México · Remote</sub>

<br/>

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/feribarra1/)
[![Email](https://img.shields.io/badge/fernandooibarra@gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:fernandooibarra@gmail.com)
[![Portfolio](https://img.shields.io/badge/feribarra.dev-006166?style=flat-square&logo=googlechrome&logoColor=white)](https://feribarra.dev)
[![Résumé](https://img.shields.io/badge/Résumé-PDF-14171B?style=flat-square&logo=adobeacrobatreader&logoColor=white)](https://feribarra.dev/cv/fernando-ibarra-cv-en.pdf)

</div>

---

I build backend systems where being wrong is expensive. Most of my work is payment flows, service-to-service messaging, and the unglamorous part of both: keeping data consistent when several services and several databases are involved in one transaction.

Four years in, mostly Node.js, TypeScript and NestJS. I care about architecture that survives contact with production — explicit domain boundaries, ports and adapters, transactions that actually roll back, and messages that don't get lost when a consumer dies mid-flight.

---

## Currently

**Senior Backend Developer** at [Rocket Code](https://therocketcode.com/en) — I work across the commercial area of Centro de Seguros, the insurance platform behind Liverpool and Suburbia.

Day to day that means a fleet of NestJS microservices, RabbitMQ between them, Redis and BullMQ for async work, and Prisma over SQL Server. The interesting problems are rarely the features; they're the race conditions, the reconciliation when a gateway charges but the local write fails, and the legacy business rules nobody documented.

---

## Featured work

### The commercial core of Centro de Seguros

I work across the commercial area of an insurance platform serving **Liverpool and Suburbia** — twelve NestJS microservices, a React SPA and three Python workers, covering quoting, policy issuance, renewals, cancellations, payments, reporting and document handling.

The payment engine is the piece I'd point at. It exposes **4 entry methods** (`pos`, `link`, `formulario`, `confirmar-informacion`) across **18 product domains** — autos, six motorcycle variants, six renewal flows, complementary coverages — implemented as **48 concrete strategies** registered in four factories, each one a `PaymentStrategyPort` carrying its own `method` and `domain`.

Beyond payments, the work spans:

**Policy administration** — issuance, cancellations, renewals, offline procedures and SKU handling, including document uploads to S3 with a review flow.

**Reporting** — report requests dispatched over RabbitMQ to Python workers that query Redash, generate CSV or XLSX, upload to S3 and publish a completion event back, which the service turns into a pre-signed download link delivered by email.

**Notifications** — an email consumer plus HubSpot integration and payment-audit records.

Three things I'd call out technically:

**Two doors, one pipeline.** Payments arrive either over REST or as an RPC message on `nova.payment.process.queue`. Both converge on the same service, so a payment behaves identically however it was triggered. The RPC path resolves a request-scoped service manually and replies on `replyTo` with the original `correlationId` intact.

**Database-per-tenant, resolved from the JWT.** Rather than migrating a shared legacy schema, the tenant is read from the token (`unidad` → `tenant` → `provider`, with a normalisation pass for legacy login-api claims) and used to select the Prisma client. Nine schemas, eleven clients, chosen per request.

**Consistency under failure.** Idempotency keys written *before* the upstream call so a charge can always be reconciled; a transactional context propagated through `AsyncLocalStorage` so writes join the ambient transaction without threading a client through every signature; a card antifraud layer (SHA3-512 hashes, consecutive-failure and distinct-card limits); and a RabbitMQ client with exponential backoff, three retries and a dead-letter exchange.

<details>
<summary><b>Supporting numbers</b> — counted from the source, not estimated</summary>

<br/>

| | |
|---|---|
| Microservices in the platform | 12 NestJS services, 1 React SPA, 3 Python workers |
| Payment strategies | 48 concrete classes across 4 factories |
| Payment methods × product domains | 4 × 18 |
| Prisma schemas (payment service) | 9 schemas, 11 clients |
| RabbitMQ topology | 6 exchanges, 10 queues, RPC with `correlationId` |
| Keycloak realms | 6, one per tenant |
| Frontend | React 19 SPA — ~470 routes |

</details>

<br/>

> **Scope note.** This is a team project. I work across the commercial area — payments, policy administration, reporting and document handling — but I didn't build the platform alone, and the numbers above are counts from the codebase rather than claims of authorship.

---

## Experience

| Role | Company | Period |
|---|---|---|
| **Senior Backend Developer** | Rocket Code | Nov 2025 – Present |
| **Full Stack Developer** | INOWU Development | Dec 2023 – Nov 2025 |
| **React Native Team Lead** | IGRTEC | Sep 2023 – Dec 2024 |

**Rocket Code** — Backend services in Node.js, TypeScript and NestJS inside a microservices architecture. Designed and evolved service boundaries for scalability and maintainability. Built inter-service communication over RabbitMQ with async processing and `correlationId` propagation. Developed and integrated payment-processing services, connecting new microservices to existing backends. Reverse-engineered legacy business logic to guarantee new implementations preserved established behaviour. Modelled persistence with Prisma over SQL Server, including transactional atomicity in payment flows. Containerised services with Docker across environments.

**INOWU Development** — Full-stack delivery with React, Next.js, NestJS and PostgreSQL for national and international clients. Built and maintained responsive web applications and REST APIs. Modernised existing systems, including integration between legacy platforms and new web frontends. Implemented synchronisation between legacy Firebird systems and cloud platforms via automated processes, triggers and backend services. Also handled infrastructure: backend and frontend servers, databases, Redis and mail services.

**IGRTEC** — Led a team building cross-platform mobile applications in React Native. Set development standards and code organisation practices, reviewed code, and supported the team through feature delivery. Drove improvements to performance, stability and maintainability. Coordinated workflows through Git.

---

## Selected projects

| Project | What it is | Stack |
|---|---|---|
| **[Votométrica](https://www.votometrica.com)** | Real-time vote counting. Digitises tally sheets, validates them with OCR, and maps results with periodic refresh. | Next.js · TypeScript · TypeORM · PostgreSQL · Leaflet |
| **[WFacturas](https://wfacturas.com)** | CFDI invoicing and stamping with a dashboard, self-invoicing and stamp plans, kept current with SAT requirements. | Next.js · TypeScript · Node.js · PostgreSQL |
| **[VR VideoRemixes](https://videoremixespacks.com)** | Subscription platform for browsing and downloading video-remix packs, with account migration and plan management. | Next.js · NestJS · Redis · Prisma · AWS S3 |
| **[RealDeal JC](https://www.realdealjc.com)** | Collectibles and toys e-commerce with a dynamic catalogue and a fast purchase path. | Next.js · TypeScript · Stripe · MongoDB |
| **[Entrify](https://www.entrify.mx)** | Physical access management with dynamic QR codes and identity verification, for residential and corporate sites. | Next.js · TypeScript · SASS |

---

## Stack

**Backend** — Node.js · NestJS · Express · REST APIs · Microservices · WebSockets

**Architecture** — Clean Architecture · Ports & Adapters · DDD · RabbitMQ · Redis · BullMQ · asynchronous messaging · RPC with `correlationId`

**Data** — SQL Server · PostgreSQL · MongoDB · Prisma · TypeORM

**Frontend** — React · Next.js · React Native · TypeScript · Tailwind CSS · shadcn/ui · TanStack Query · Zustand · React Router · React Hook Form · Zod

**DevOps & tooling** — Docker · Git · GitLab CI · Sentry · NPM · PNPM

**Integrations** — Payment gateways · legacy systems · third-party APIs · Keycloak · OpenAI API · Firebase

<p align="center">
  <img src="https://skillicons.dev/icons?i=ts,js,nodejs,nestjs,express,react,nextjs,tailwind,prisma,postgres,mongodb,redis,rabbitmq,docker,git,gitlab,linux&theme=light" alt="TypeScript, JavaScript, Node.js, NestJS, Express, React, Next.js, Tailwind, Prisma, PostgreSQL, MongoDB, Redis, RabbitMQ, Docker, Git, GitLab, Linux" />
</p>

---

## Certifications

19 certifications — 3 from Microsoft & LinkedIn, 16 from DevTalles.

<details>
<summary><b>Microsoft & LinkedIn</b> — 3</summary>
<br/>

| | | |
|:---:|:---:|:---:|
| [<img src="https://i.imgur.com/KZfZAxN.jpeg" width="240" alt="Generative AI Professional Essentials"/>](https://www.linkedin.com/learning/certificates/557f513e7ac29edaff10120c047fcaa20e14f49ad5cb22ada78fb992a133c298) | [<img src="https://i.imgur.com/j6bKiCz.jpeg" width="240" alt="Systems Administration Career Essentials"/>](https://www.linkedin.com/learning/certificates/6102dccfffdf2a7957f2b873e9b085e337a12fb2e79240241ce644d998838a5d) | [<img src="https://i.imgur.com/Ey2UJyU.jpeg" width="240" alt="Software Development Fundamentals"/>](https://www.linkedin.com/learning/certificates/099ea9806183134afcfa1ba686fc97525ac1e387ae7838aee28dd6db7fa5d48a) |
| Generative AI Professional Essentials | Systems Administration Career Essentials | Software Development Fundamentals |

</details>

<details>
<summary><b>DevTalles</b> — 16</summary>
<br/>

| | | |
|:---:|:---:|:---:|
| [<img src="https://i.imgur.com/ylYZO0S.jpeg" width="240" alt="NestJS"/>](https://cursos.devtalles.com/certificates/bcdvt6t2hd) | [<img src="https://i.imgur.com/94HoX2o.jpeg" width="240" alt="Node.js"/>](https://cursos.devtalles.com/certificates/epjl1mza9y) | [<img src="https://i.imgur.com/OadreRP.jpeg" width="240" alt="NestJS PDF reports"/>](https://cursos.devtalles.com/certificates/a6dki5q26m) |
| NestJS: Escalable con Node | Node.js: De cero a experto | NestJS + Reportes: PDFs desde Node |
| [<img src="https://i.imgur.com/DgrWi3k.jpeg" width="240" alt="TypeScript"/>](https://cursos.devtalles.com/certificates/hbll5frkg7) | [<img src="https://i.imgur.com/G0ct8M7.jpeg" width="240" alt="JavaScript Moderno"/>](https://cursos.devtalles.com/certificates/0ukjpjpu3m) | [<img src="https://i.imgur.com/uOyBwvP.jpeg" width="240" alt="React"/>](https://cursos.devtalles.com/certificates/1tufqctqtl) |
| TypeScript: Guía completa | JavaScript Moderno | React: De cero a experto |
| [<img src="https://i.imgur.com/WzpvI6C.jpeg" width="240" alt="React PRO"/>](https://cursos.devtalles.com/certificates/6pal3nwfr8) | [<img src="https://i.imgur.com/nssarnF.jpeg" width="240" alt="React edición actualizada"/>](https://cursos.devtalles.com/certificates/grfoac6egq) | [<img src="https://i.imgur.com/xKye8go.jpeg" width="240" alt="Next.js"/>](https://cursos.devtalles.com/certificates/f5vsw3jrvt) |
| React PRO | React: edición actualizada | Next.js para producción |
| [<img src="https://i.imgur.com/3OZvkWV.jpeg" width="240" alt="TanStack Query"/>](https://cursos.devtalles.com/certificates/irg3nsjnzj) | [<img src="https://i.imgur.com/dF9hMUJ.jpeg" width="240" alt="Zustand"/>](https://cursos.devtalles.com/certificates/igzbv9zjly) | [<img src="https://i.imgur.com/kHXedJz.jpeg" width="240" alt="React Router"/>](https://cursos.devtalles.com/certificates/etbadnszea) |
| TanStack Query | Zustand | React Router |
| [<img src="https://i.imgur.com/eYqQVQG.jpeg" width="240" alt="shadcn/ui"/>](https://cursos.devtalles.com/certificates/ymsslzknzy) | [<img src="https://i.imgur.com/vbdUQvc.jpeg" width="240" alt="OpenAI con React y NestJS"/>](https://cursos.devtalles.com/certificates/hmg7rnngij) | [<img src="https://i.imgur.com/1TU18ZR.jpeg" width="240" alt="Git y GitHub"/>](https://cursos.devtalles.com/certificates/60yhalceu6) |
| shadcn/ui | OpenAI + React + NestJS | Git + GitHub |
| [<img src="https://i.imgur.com/aiHXphb.jpeg" width="240" alt="Git y GitHub actualizado"/>](https://cursos.devtalles.com/certificates/czd3qegrqn) | | |
| Git + GitHub: edición actualizada | | |

</details>

---

## GitHub

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats.vercel.app/api?username=FerIbarra1&show_icons=true&hide_border=true&include_all_commits=true&count_private=true&bg_color=0D1012&title_color=2AA5AA&icon_color=2AA5AA&text_color=C9CDD2&border_radius=8" />
  <img src="https://github-readme-stats.vercel.app/api?username=FerIbarra1&show_icons=true&hide_border=true&include_all_commits=true&count_private=true&bg_color=FBFCFE&title_color=006166&icon_color=006166&text_color=53575C&border_radius=8" alt="Fernando Ibarra's GitHub stats" height="170" />
</picture>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats.vercel.app/api/top-langs/?username=FerIbarra1&layout=compact&hide_border=true&langs_count=8&bg_color=0D1012&title_color=2AA5AA&text_color=C9CDD2&border_radius=8" />
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=FerIbarra1&layout=compact&hide_border=true&langs_count=8&bg_color=FBFCFE&title_color=006166&text_color=53575C&border_radius=8" alt="Most used languages" height="170" />
</picture>

---

## Education

**Ingeniería en Sistemas Computacionales** — Tecnológico Nacional de México, Campus Hermosillo
<sub>Ago 2017 – Dic 2024 · Hermosillo, Sonora, México</sub>

**Languages** — Spanish (native) · English (professional working proficiency)

<div align="center">
<br/>
<sub>Open to backend and full-stack roles, remote or based in México.</sub>
</div>
