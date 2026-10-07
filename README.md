🛡️ CyberAudit – Cybersecurity Maturity Assessment Platform for Moroccan SMEs

🇫🇷 Plateforme web d'audit de la maturité cybersécurité des PME marocaines : auto-évaluation de 24 questions, scoring automatique, correspondance ISO 27001 / NIST CSF, rapports PDF et espace auditeur sécurisé par RLS.

End-of-year project (PFA2) carried out at ENSA Agadir (IT Security & Digital Trust track) within the CMRPI – Espace Maroc Cyberconfiance, from 15 July to 31 August 2026.

🎯 Problem Statement

Moroccan SMEs lack simple tools to assess their cybersecurity maturity: a classic audit is long and expensive, and the CMRPI/AUSIM SME Cybersecurity Guide is a static document. This platform turns it into an interactive self-assessment with a score, prioritized recommendations, and auditor follow-up.

✨ Features
SME Space
Sign-up, login, password reset, company profile (sector, size, region, city)
24-question questionnaire across 5 domains: risk context and exposure, governance and organization, access and network, awareness, backup and compliance
Automated scoring: 18 scored questions, score out of 54, overall and per domain, mapped to 4 maturity levels (Initial, Basic, Intermediate, Advanced)
Prioritized recommendations generated from the weakest answers
ISO/IEC 27001 & NIST CSF mapping: each scored question is linked to a control, flagged "Covered" or "To strengthen"
PDF diagnostic report (score gauge, per-domain breakdown, recommendations, framework mapping)
Dashboard, audit history, audit request, action plan tracking, notifications
Auditor Space (single CMRPI account)
Overview: active missions, followed companies, audits to validate, average score
Missions filterable by status (to plan, in progress, to validate, closed)
Mission detail: score, domains, recommendations, ISO/NIST mapping, follow-up notes
Audit validation: status set to "Closed" and official report published
View and download published reports
🏗️ Architecture

Three-tier architecture: web client, lightweight application server, backend-as-a-service.

Web client (React 19, TanStack Router, Tailwind, shadcn/ui)
        │  fetch / server functions
Application server (TanStack Start, Nitro)
        │  SQL queries via the Supabase client
Supabase (PostgreSQL, Auth, Row Level Security)

Afficher l'image

🔐 Data Security

Security is enforced at the database level, not only in the frontend:

Row Level Security (RLS) enabled on all business tables: each SME can only read and modify its own data (auth.uid() = owner_id).
Broader read access for the auditor (SELECT on SME records) but restricted writes: only audit status and their own notes.
SQL trigger that prevents an auditor account from modifying an audit's score or identifying data, even through the API.
SECURITY DEFINER functions (e.g. has_role) centralizing role checks.
Single auditor account, created by a server function using the service key, never exposed to the browser.
Key separation: the public (anon) key only has the rights granted by RLS policies; the service role key stays server-side.
Targeted compliance with Moroccan Law 09-08 (personal data protection), built into the questionnaire.
🗄️ Data Model

Nine tables linked by foreign keys, managed as versioned SQL migrations:

Table	Purpose
profiles	Profile linked to each account (type: SME or auditor)
companies	Company attached to an SME account
user_roles	Application roles (pme, auditor, admin)
auditor_profiles	Auditor account information
audits	Audits performed (status, score, dates)
audit_questions	Question bank (axis, options, weighting)
audit_answers	An SME's answers with the associated score
audit_notes	Auditor follow-up notes
audit_reports	Official report published after validation

Questions are stored in the database, so the questionnaire can evolve without redeploying the app.

📊 Screenshots
SME Dashboard	Audit Results
Afficher l'image	Afficher l'image
ISO 27001 / NIST CSF Mapping	Auditor Space
Afficher l'image	Afficher l'image
🧰 Tech Stack
Layer	Technology
Framework	TanStack Start (React 19, TypeScript), TanStack Router, TanStack Query
UI	Tailwind CSS, Radix UI, lucide-react, Recharts
Forms & validation	react-hook-form, Zod
Backend	Supabase (PostgreSQL, Auth, auto-generated API)
PDF reports	jsPDF
Build	Vite, Nitro
Tooling	Git/GitHub, Docker (exploration), VS Code
🚀 Getting Started

⚠️ Check these commands and environment variable names against your code before publishing.

bash
# 1. Clone the repository
git clone https://github.com/asmamasrafi/cybersecurity-maturity-assessment.git
cd cybersecurity-maturity-assessment

# 2. Install dependencies
npm install

# 3. Configure the environment (never commit .env)
cp .env.example .env
# Fill in the Supabase project URL and the public (anon) key

# 4. Apply the SQL migrations (schema, RLS policies, triggers) to your Supabase project

# 5. Run in development
npm run dev

# Check compilation and TypeScript types
npm run build

⚠️ The Supabase service role key must never be committed or exposed client-side.

🧪 Testing & Validation

Mostly manual: end-to-end functional tests (sign-up, audit, validation, report), simultaneous multi-actor tests (normal and private browsing), RLS policy checks directly in the Supabase SQL editor, and npm run build before each integration.

🗺️ Project Timeline
Milestone 1 (15–31 Jul): studying the CMRPI/AUSIM guide, designing the questionnaire and scoring rule.
Milestone 2 (1–15 Aug): Streamlit (Python) prototype, tested on fictional SME profiles.
Milestone 3 (16–31 Aug): full React/Supabase platform, PDF reports, auditor space, ISO/NIST mapping.
🔭 Future Work
Score evolution tracking over time for a given SME
Automatic report delivery to company management by email
Additional comparison framework
Automated tests, finalized Docker containerization, production deployment
Multiple auditors with case assignment
⚠️ Notes
All data shown is demo data.
The questionnaire is based on the Cybersecurity Best Practices Guide for SMEs in Morocco (CMRPI/AUSIM, 2018).
👥 Team

Built as a pair project:

Assma MASRAFI – LinkedIn
Wissal EZZAIRI

Supervised by Dr. Rachid Abouettahir, with the support of Pr. Zakia Errabih. Host organization: CMRPI – Espace Maroc Cyberconfiance.

