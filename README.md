# 🛡️ CyberAudit — Cybersecurity Maturity Assessment Platform

> A web-based cybersecurity maturity assessment platform designed to help Moroccan SMEs evaluate their cybersecurity posture, identify security gaps, and prioritize improvement actions.

![Status](https://img.shields.io/badge/status-academic%20project-blue)
![Cybersecurity](https://img.shields.io/badge/domain-cybersecurity-red)
![GRC](https://img.shields.io/badge/focus-GRC-orange)
![React](https://img.shields.io/badge/React-19-61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-blue)
![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL-3ECF8E)

---

## 🇫🇷 Project Overview

**CyberAudit** is a cybersecurity maturity assessment platform developed for Moroccan SMEs.

The platform transforms the principles of the **CMRPI/AUSIM Cybersecurity Best Practices Guide for SMEs in Morocco** into an interactive assessment workflow.

It enables an SME to:

- Evaluate its cybersecurity maturity
- Identify security weaknesses
- Receive prioritized recommendations
- Visualize its maturity level
- Map assessment results to **ISO/IEC 27001** and **NIST CSF**
- Generate a cybersecurity diagnostic report in PDF
- Track improvement actions

An **auditor space** allows the CMRPI team to follow companies, validate assessments and publish official reports.

---

## 🎓 Academic Context

**End-of-year project (PFA2)**

- **Institution:** ENSA Agadir
- **Track:** IT Security & Digital Trust
- **Host organization:** CMRPI – Espace Maroc Cyberconfiance
- **Period:** 15 July – 31 August 2026
- **Project type:** Pair project

The project was developed as a practical application of cybersecurity maturity assessment, governance, risk management and security controls.

---

## 🎯 Problem Statement

Many Moroccan SMEs face difficulties when assessing their cybersecurity maturity.

Traditional cybersecurity audits can be:

- Time-consuming
- Expensive
- Difficult to perform without specialized expertise

At the same time, cybersecurity maturity guides are often provided as static documents.

**CyberAudit addresses this gap by transforming the assessment process into an interactive platform providing automated scoring, security recommendations, framework mapping and auditor follow-up.**

---

## ✨ Key Features

### 🏢 SME Space

- Account registration and authentication
- Password reset
- Company profile management
- Company information:
  - Sector
  - Organization size
  - Region
  - City

### 📝 Cybersecurity Assessment

The platform provides a **24-question questionnaire** covering five cybersecurity domains:

1. Risk context and exposure
2. Governance and organization
3. Access and network security
4. Security awareness
5. Backup and compliance

The assessment includes:

- 18 scored questions
- Score calculated out of 54
- Overall maturity score
- Per-domain scores
- Four maturity levels:
  - Initial
  - Basic
  - Intermediate
  - Advanced

### 💡 Security Recommendations

Recommendations are generated based on the weakest assessment responses.

This helps SMEs identify their main cybersecurity improvement priorities.

### 📚 ISO 27001 & NIST CSF Mapping

Each scored question is mapped to relevant cybersecurity controls.

The platform indicates whether a control is:

- **Covered**
- **To strengthen**

This provides a bridge between SME cybersecurity maturity assessment and recognized cybersecurity frameworks.

### 📄 PDF Diagnostic Reports

The platform generates PDF reports containing:

- Overall maturity score
- Score visualization
- Domain-level results
- Security recommendations
- ISO 27001 / NIST CSF mapping

### 📊 SME Dashboard

The SME dashboard provides:

- Assessment history
- Audit requests
- Action plan tracking
- Notifications
- Current cybersecurity maturity overview

---

## 👨‍💼 Auditor Space

A dedicated auditor interface allows cybersecurity auditors to monitor and validate assessments.

### Auditor Dashboard

Provides an overview of:

- Active missions
- Followed companies
- Audits awaiting validation
- Average maturity score

### Mission Management

Missions can be filtered by status:

- To plan
- In progress
- To validate
- Closed

### Mission Details

Auditors can access:

- Overall score
- Domain scores
- Recommendations
- ISO 27001 / NIST CSF mapping
- Follow-up notes

### Audit Validation

Auditors can validate an assessment and publish the resulting report.

Published reports can then be viewed and downloaded.

---

## 🏗️ Architecture

CyberAudit follows a three-tier architecture:

```text
┌──────────────────────────────┐
│          Web Client          │
│ React 19                     │
│ TanStack Router              │
│ Tailwind CSS                 │
│ shadcn/ui                    │
└──────────────┬───────────────┘
               │
               │ Fetch / Server Functions
               ▼
┌──────────────────────────────┐
│      Application Server      │
│ TanStack Start               │
│ Nitro                        │
└──────────────┬───────────────┘
               │
               │ Supabase Client
               ▼
┌──────────────────────────────┐
│           Supabase           │
│ PostgreSQL                   │
│ Authentication               │
│ Row Level Security (RLS)     │
└──────────────────────────────┘

---

🔐 Security Architecture

Security was considered at both the application and database levels.

Row Level Security

PostgreSQL Row Level Security (RLS) is enabled on business tables.

Each SME is restricted to its own data through ownership-based policies such as:

auth.uid() = owner_id
Role-Based Access Control

The application distinguishes between different roles:

SME
Auditor
Admin

Role checks are centralized through security functions such as:

has_role()
Auditor Restrictions

The auditor has broader read access for assessment follow-up but restricted write permissions.

Database-level controls prevent unauthorized modification of sensitive audit information.

Database Triggers

SQL triggers are used to protect sensitive fields and prevent unauthorized modification of:

Audit scores
Identification data
Other protected audit information
Credential Separation

The application separates public and privileged credentials.

The:

anon key

is used with permissions enforced by RLS.

The:

service role key

remains server-side and is never exposed to the browser.

Server-Side Privileged Operations

The dedicated auditor account is created through a server-side function using privileged credentials.

🗄️ Data Model

The application uses PostgreSQL with nine main tables.

Table	Purpose
profiles	Account profile and user type
companies	Company information associated with an SME
user_roles	Application roles
auditor_profiles	Auditor information
audits	Assessment records and scores
audit_questions	Question bank and scoring configuration
audit_answers	SME assessment responses
audit_notes	Auditor follow-up notes
audit_reports	Published assessment reports

The database schema is managed through versioned SQL migrations.

Questions are stored in the database, allowing the assessment questionnaire to evolve without requiring a complete application redeployment.

📊 Screenshots
SME Dashboard

Assessment Results

ISO 27001 / NIST CSF Mapping

Auditor Space

Replace the image paths above with the actual filenames used in the repository.

🧰 Technology Stack
Layer	Technologies
Frontend	React 19, TypeScript
Routing	TanStack Router
Application	TanStack Start, Nitro
UI	Tailwind CSS, Radix UI, shadcn/ui
Data Fetching	TanStack Query
Forms	React Hook Form
Validation	Zod
Backend	Supabase
Database	PostgreSQL
Authentication	Supabase Auth
Database Security	PostgreSQL RLS
Charts	Recharts
PDF Reports	jsPDF
Build	Vite
Version Control	Git / GitHub
Development	VS Code
Containerization	Docker (exploration)
🚀 Getting Started
Prerequisites

Make sure you have:

Node.js
npm
A Supabase project
1. Clone the repository
git clone https://github.com/asmamasrafi/cybersecurity-maturity-assessment.git

cd cybersecurity-maturity-assessment
2. Install dependencies
npm install
3. Configure environment variables

Create a local .env file based on the provided example:

cp .env.example .env

Configure the required Supabase environment variables.

⚠️ Never commit .env files or privileged Supabase credentials.

4. Configure the database

Apply the SQL migrations to the Supabase PostgreSQL database.

These migrations configure:

Database schema
Tables
Relationships
RLS policies
Security functions
Database triggers
5. Run the development server
npm run dev
6. Build the application
npm run build
🧪 Testing & Validation

The project was mainly validated through manual and end-to-end testing.

Testing included:

User registration
Authentication
Password reset
SME assessment workflow
Score calculation
Recommendation generation
PDF report generation
Auditor workflow
Audit validation
Report publication
RLS policy verification
Multi-user scenarios
Private browsing tests
TypeScript/build validation

The application was regularly validated using:

npm run build
🗺️ Project Timeline
Milestone 1 — 15–31 July 2026
Study of the CMRPI/AUSIM cybersecurity guide
Questionnaire design
Cybersecurity maturity model definition
Scoring rule design
Milestone 2 — 1–15 August 2026
Development of a Streamlit prototype
Python implementation
Testing with fictional SME profiles
Milestone 3 — 16–31 August 2026
Migration to React/Supabase architecture
Full web platform development
PDF report generation
Auditor space
ISO 27001 / NIST CSF mapping
Security controls and RLS implementation
📌 Key Results
Interactive cybersecurity maturity assessment
24-question assessment across five security domains
18 scored questions
Four maturity levels
Automated security recommendations
ISO 27001 / NIST CSF mapping
PDF cybersecurity diagnostic reports
SME and auditor workflows
PostgreSQL Row Level Security
Role-based access control
Database-level security controls
Server-side handling of privileged credentials
🔭 Future Work

Potential improvements include:

Maturity score evolution over time
Automated report delivery
Additional cybersecurity frameworks
Automated test coverage
Finalized Docker containerization
Production deployment
Multiple auditors
Auditor case assignment
Advanced cybersecurity analytics
📚 Reference

The assessment questionnaire is based on the:

Cybersecurity Best Practices Guide for SMEs in Morocco — CMRPI / AUSIM (2018).

The project also incorporates concepts from:

ISO/IEC 27001
NIST Cybersecurity Framework
Moroccan Law 09-08 regarding personal data protection
⚠️ Disclaimer

This project is an academic cybersecurity assessment platform and is intended for educational and demonstration purposes.

It does not replace a formal cybersecurity audit, penetration test, risk assessment, legal assessment or professional compliance audit.

All data presented in the demonstration environment is fictional or demo data.

👥 Team

Built as a pair project by:

Assma MASRAFI
Cybersecurity Engineering Student — ENSA Agadir

Wissal EZZAIRI

Supervision

Dr. Rachid Abouettahir

With the support of:

Pr. Zakia Errabih

Host Organization

CMRPI — Espace Maroc Cyberconfiance

📫 Contact

Assma MASRAFI

GitHub: https://github.com/asmamasrafi
LinkedIn: Assma MASRAFI

Interested in:

SOC • Blue Team • Cybersecurity GRC • Security Assessment • Security Engineering
