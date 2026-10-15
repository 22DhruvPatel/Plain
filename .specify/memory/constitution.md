<!--
Sync Impact Report
Version change: 1.0.0 -> 1.1.0 (new technician trust and verification requirements)
Modified principles: None
Added sections: Technician Trust and Verification
Removed sections: None
Follow-up TODOs: Confirm original ratification date; resolve insurance requirements before
onboarding real clients.
-->
# Plain Constitution

## Core Principles

### I. Product Value and Scope
Software is the product; the marketplace is its distribution channel. Every feature MUST improve
the Customer's ticket history or tooling, or the Technician's workflow, so both sides gain a reason
to keep using Plain. The initial market is Canadian small businesses with 5 to 100 staff,
including clinics, law firms, tax clinics, and similar small offices without in-house IT. Enterprise
customers, other countries, and other industries are out of scope until a spec explicitly expands
the market.

### II. Automated, Fixed-Scope Marketplace
Customers buy fixed-price services and a monthly plan. Services MUST be productized, fixed-scope,
and fixed-price (for example, "New Employee Setup, $90 CAD"). Custom quotes and negotiation are
out of scope until explicitly specified. Platform software, not human staff, MUST run job matching,
payment execution, and marketplace recordkeeping. Prefer self-serve onboarding and billing,
automated vetting based on certifications, ratings, and probation tiers, matched jobs, automatic
technician payments, and rule-based disputes over human decisions. A feature that adds recurring
manual administration MUST include written justification in its plan.

### III. Trust, Privacy, and Financial Safety
Client data is confidential by default. Collect only what a feature needs; do not put sensitive client
data in tickets, logs, or analytics beyond that need. Design for PIPEDA, applicable provincial law
(including PHIPA where applicable), and the confidentiality duties of law firms and tax preparers.
Prefer Canadian database and storage residency where the hosting provider allows it. A spec MUST
record any decision to send client data to a third party, including an AI provider. Never ask users
to share raw credentials in free text. Support scoped access, session logging, and an audit trail
because Technicians access client systems.

Payments MUST use Stripe Connect in CAD with escrow-style holds, released on Client approval or
after a defined timeout. Plain MUST NOT store card data. AI features may assist with triage,
matching, or self-help but MUST NOT silently decide outcomes; each AI action MUST be logged,
explainable, and have a human-reviewable fallback.

### IV. Least-Privilege Roles
The only roles are Customer, Technician, and Administrator, and their names MUST exactly match the
existing `roles.role_name` values. A Customer may access only their organization's data; a
Technician may access only jobs assigned to them. Administrator actions MUST be limited and logged.
Authorization MUST be enforced by Supabase Row Level Security (RLS), never only by the UI.
ProtectedRoute and RoleRoute are navigation conveniences, not security controls. Every table MUST
have RLS enabled and its policies written and tested before any feature uses that table.

### V. Simple, Understandable Delivery
Prefer clear, simple designs over clever ones. Use consistent naming, short functions, and comments
that explain why. A new contributor MUST be able to understand each file in a few minutes. Build
small, complete user workflows rather than large, unused scaffolding.

## Technical Standards

The application stack is React 19 with Vite, React Router, Supabase (Auth, Postgres, and RLS),
lucide-react, and plain CSS in `src/styles`. A plan MUST state a reason for every added dependency.
Keep the project structure in `src/pages`, `src/components`, `src/context`, and `src/lib`. Business
logic belongs in `src/lib` or server functions, not page components.

Authentication MUST support email/password and Google OAuth through Supabase. Auth state MUST flow
through AuthContext only. Secrets MUST NOT enter the repository: only the Supabase anon key may
appear in client code; service-role keys, payment secrets, and webhook secrets MUST remain on the
server. `.env` files MUST be gitignored, and `.env.example` MUST document required variables without
values.

Database changes MUST use versioned migrations; manual Supabase dashboard changes require a
matching migration file. Code MUST pass `npm run lint` and `npm run build` before merge. New behavior
with security or money impact MUST ship with automated tests. Interfaces MUST use semantic HTML,
support keyboard navigation, show visible focus states, label form fields, and remain usable on
mobile.

## Technician Trust and Verification

Technicians MUST provide consent for identity verification and a Canadian criminal record check
through an independent third-party screening provider before receiving jobs. Plain MUST NOT conduct
manual background checks. Store only the verification outcome: status, check type, date, expiry, and
provider reference. NEVER store the screening report. Verification MUST expire and be renewed at
least annually. The provider MUST sit behind a replaceable interface; the plan MUST decide provider
choice, cost, and who pays.

Verification does not replace access control. Technicians are untrusted by default and MUST have no
standing credentials. Client access MUST use client-approved, time-limited, logged sessions with
least privilege. New Technicians MUST start with low-risk jobs. Payouts MUST require identity
verification through the payment provider. Insurance requirements are an open question and MUST be
resolved before onboarding real clients.

## Workflow and Clarity

Every feature MUST follow specify, plan, tasks, then implement. Specs MUST state user value,
acceptance criteria in plain language, and the role served; they MUST NOT contain implementation
details. Build thin vertical slices users can complete end to end, such as a Customer submitting a
service request that a Technician can see.

Use small, focused commits with conventional prefixes (`feat`, `fix`, `chore`, `docs`). Anything
unclear or affecting security or finances MUST be raised as an open question in the spec, not
assumed. Plans and reviews MUST verify compliance with this constitution.

## Governance

Amendments require a written reason and a version bump. When a spec conflicts with this
constitution, this constitution prevails until amended. Version numbers use MAJOR.MINOR.PATCH:
MAJOR for backward-incompatible governance or principle removals/redefinitions, MINOR for new
principles or materially expanded guidance, and PATCH for clarifications and non-semantic wording
changes. Feature plans and implementation reviews MUST check compliance; deviations require an
explicitly approved constitution amendment before the conflicting work proceeds.

**Version**: 1.1.0 | **Ratified**: TODO(RATIFICATION_DATE): confirm original adoption date | **Last Amended**: 2026-10-15
