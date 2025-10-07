# High-Level System Design for Advogram

As the system architect for Advogram, I've updated the design based on your input. All tasks are now assigned to hypothetical collaborators (e.g., based on GitHub usernames or roles—devs can self-assign via issues). Repository: Single monorepo (e.g., via Turborepo/Lerna for managing packages/services/extension). No CONTRIBUTING.md for Azure access guides (handle via private channels for contributors). PoC skips LinkedIn API integration tests—focus on mocks for core flows.

This remains an open-source project—core PoC for essentials (blacklist, proxy email, basic automation, AI advisor). Active contributors get Azure resource access (compute, Azure OpenAI, SendGrid, etc.) paid for them.

Key principles:
- **Scalability:** Serverless via Azure Functions for variable traffic.
- **Security:** End-to-end encryption for emails/calls; anonymized analytics; Azure Key Vault for secrets.
- **Tech Stack Baseline:** Flexible—devs choose (e.g., Node.js/Express or Python/FastAPI backend; React for dashboard/extension). Database: Azure Database for PostgreSQL + Azure Cache for Redis. Real-time: Azure SignalR Service.
- **Deployment:** Azure Container Apps; Azure DevOps for CI/CD. Monorepo setup.

## 1. Overall Architecture
- **Client Layer:** Browser extension (Chrome/Edge-first, cross-browser via WebExtensions API) + Web Dashboard (React app on Azure Static Web Apps).
- **API Gateway:** Centralized via Azure API Management for auth (JWT/OAuth), rate-limiting, routing to microservices.
- **Core Microservices (PoC Focus):**
  - **Blacklist Service:** Real-time user-reported blacklists.
  - **Proxy Email Service:** Secure inbound/outbound routing.
  - **Automation Service:** Extension features (auto-apply, call recording).
  - **AI Service:** Resume advisor and background check.
  - **Analytics Service:** Basic dashboard metrics and notifications.
- **Data Flow:** Extension actions → API calls → Services update DB → Push via Azure SignalR or Notification Hubs.
- **External Integrations:** OAuth mocks for job APIs; Twilio (via Azure) for reminders; Azure OpenAI for AI.

High-level diagram (conceptual):
[Browser Extension] <-> [Azure API Management] <-> [Microservices: Blacklist | Proxy Email | Automation | AI | Analytics]
                          |
                     [Azure PostgreSQL + Redis Cache] + [Azure Blob Storage for recordings/resumes]
                          |
                     [Notifications: Email/SMS/Push via Azure Notification Hubs]

## 2. Task Breakdown & Ownership
All features/tasks split across collaborators for ownership. Each owns PoC delivery (1-2 weeks), PRs, and Azure deploys. Assign via GitHub issues (e.g., @dev1 owns Blacklist). Core PoC: Parallel tracks, merge in monorepo.

**Browser Extension (Auto-Apply & Call Recording):**
- Ownership: @extension-dev (fork/integrate options, build basic auto-apply + recording mocks).
- Tasks: Setup extension scaffold; integrate 1-2 GitHub options; test hiding/listing filters; Azure deploy for static assets.
- Options:
  - [davidwarshawsky/Easy-Apply-Automater](https://github.com/davidwarshawsky/Easy-Apply-Automater) - LinkedIn Easy Apply automation; add blacklist hiding.
  - [GodsScion/Auto_job_applier_linkedIn](https://github.com/GodsScion/Auto_job_applier_linkedIn) - Web scraping bot for auto-applies; extend for one-click.
  - [ruslanaleks/linkedin-easy-applier](https://github.com/ruslanaleks/linkedin-easy-applier) - Chrome extension for fast Easy Apply; integrate recording.
  - [sushen123/ai-auto-apply](https://github.com/sushen123/ai-auto-apply) - AI-filtered LinkedIn applies; for smart automation.
  - Audio: [teamplanes/audio-capture-extension](https://github.com/teamplanes/audio-capture-extension) - Tab audio capture; for call logging/reminders.

**Proxy Email Service:**
- Ownership: @email-dev (build secure proxy with anti-spam; integrate SendGrid on Azure).
- Tasks: Proxy inbound/outbound flows; encryption setup; test data isolation; Azure Function deploy.
- Options:
  - [simonrob/email-oauth2-proxy](https://github.com/simonrob/email-oauth2-proxy) - IMAP/SMTP proxy with OAuth; add anti-spam.
  - [forwardemail/forwardemail](https://github.com/forwardemail) - Privacy-focused forwarding; customize routing.
  - [kz26/mailproxy](https://github.com/kz26/mailproxy) - Simple SMTP proxy; for secure retransmit.

**Real-Time Blacklist:**
- Ownership: @blacklist-dev (community DB with real-time updates; Redis pub/sub on Azure).
- Tasks: User submission/reporting API; query/filter endpoints; moderation mocks; Azure Cache integration.
- Options:
  - [scamsniffer/scam-database](https://github.com/scamsniffer/scam-database) - Community-maintained blacklist; adapt for recruiters with Azure Redis pub/sub.
  - [activecm/rita-bl](https://github.com/activecm/rita-bl) - API for blacklists; for real-time storage/retrieval.
  - [maravento/blackweb](https://github.com/maravento/blackweb) - Domain blacklist unification; extend to companies.

**Dashboard & Analytics:**
- Ownership: @dashboard-dev (metrics viz + notifications; deploy on Azure Static Web Apps).
- Tasks: Basic tracking (applies/responses); invite push setup; React dashboard scaffold.
- Options:
  - [TheCaptainFalcon/j_serp_extractor](https://github.com/TheCaptainFalcon/j_serp_extractor) - Job board analytics dashboard; add invite tracking.
  - [metabase/metabase](https://github.com/metabase/metabase) - Open-source BI dashboard; for metrics viz, deploy on Azure.
  - [PostHog/posthog](https://github.com/PostHog/posthog) - Product analytics; self-host on Azure for user hunts.

**AI Personal Branding & Background Check:**
- Ownership: @ai-dev (resume advisor + HR nudge logic; Azure OpenAI integration).
- Tasks: Prompt engineering for branding; scan/mock HR channels; feedback generation.
- Options:
  - [olyaiy/resume-lm](https://github.com/olyaiy/resume-lm) - AI resume builder; ATS-optimized for advisor, integrate with Azure OpenAI.
  - [sahidrajaansari/Ai-Resume-Builder](https://github.com/sahidrajaansari/Ai-Resume-Builder) - AI suggestions/templates; extend for feedback nudges.
  - [Mahmud0808/ResumeAI](https://github.com/Mahmud0808/ResumeAI) - Effortless AI resume gen; for branding insights.
  - [AmruthPillai/Reactive-Resume](https://github.com/AmruthPillai/Reactive-Resume) - Free resume tool; integrate GPT via Azure.

**Cross-Cutting (Monorepo Setup, API Gateway, Auth):**
- Ownership: @ARogovsky
- Tasks: Azure Credits; CI/CD pipelines; security audits.

**Dev Feedback Loop:** Weekly syncs via GitHub Discussions. Each owner: Deliver PoC branch. Merge to main after reviews.

This PoC design is OSS-first, ownership-driven, core-only. Let's iterate!

