# SafeSellz AI

**Non-custodial transaction assurance for peer-to-peer commerce.**

SafeSellz AI is a transaction-assurance platform designed to make peer-to-peer transactions involving physical goods more transparent and verifiable. It coordinates agreed transaction terms, buyer and seller consent, payment-provider verification, fulfilment evidence, handover and inspection, and advisory AI review without taking custody of customer funds.

> **Public submission status (12 November 2026):** This is the public hackathon submission repository. When this README was created, the repository was empty. The sanitized source snapshot, approved open-source licence, verified judge package, public demo video and final Devpost submission still need to be added or completed. This README is a submission guide and status record. It is not a claim that every feature or integration is complete.

## 1. The problem

Peer-to-peer commerce can become difficult to trust after a buyer and seller agree to a deal. The parties may later disagree about the original terms, payment status, item condition, delivery evidence or whether a handover took place. Holding customer funds directly can also introduce unnecessary custody and operational risk.

SafeSellz AI is being built to make those steps more structured, traceable and reviewable while keeping financial authority outside the AI system.

## 2. How SafeSellz AI works

The intended workflow has five parts:

1. **Create an agreement.** A seller creates a structured deal. The application calculates amounts using integer minor units and creates a canonical representation of the agreed terms.
2. **Record mutual consent.** The terms are hashed, and the buyer and seller accept the same version. Invitations are designed to be single-use.
3. **Coordinate payment-provider checks.** The application includes PayPal Developer Platform Sandbox integration components for payment and capture verification, webhook verification and reconciliation. The live Sandbox verification status is disclosed below.
4. **Support physical fulfilment.** The application provides bounded evidence handling, buyer review, a one-time handover flow and an inspection period.
5. **Assist human review.** Advisory AI can organise structured evidence and highlight missing or contradictory information. It does not decide disputes or execute financial actions.

## 3. Implementation status

The statuses below describe the development implementation and the remaining verification gates. They must be checked against the exact source commit copied into this public repository before being treated as release evidence.

| Area | Current status | What this means |
| --- | --- | --- |
| Agreements and consent | Implemented in the development codebase | Canonical terms, SHA-256 terms hashes, exact minor-unit accounting, single-use invitation handling and mutual consent are part of the application design. |
| PayPal Sandbox integration | Code present; live verification pending | PayPal REST, webhook verification and reconciliation components exist. Live signed Sandbox capture and webhook delivery have not yet been confirmed. |
| Physical fulfilment | Implemented in the development codebase; formal acceptance pending | Evidence validation, buyer review, one-time handover and inspection controls are present in the development work. Formal M5 acceptance remains a separate gate. |
| Advisory AI | Optional provider path with deterministic fallback | The product contains an optional OpenAI Responses API path and a deterministic local evaluator. The exact model configured for a final runtime must be verified before it is named. |
| Financial operations | Draft domain work only | M6 financial-operation work must remain non-executable until its dependency and owner-acceptance gates are satisfied. Payout and refund execution controls remain disabled. |
| Hackathon release readiness | In progress | Public export review, licence approval, judge access, final video and submission checks remain to be completed. |

A feature appearing in a development document does not, by itself, prove that it is present or working in the public release. The public commit, automated tests and actual judge demonstration are the evidence.

## 4. Technology

The development implementation uses:

- **Frontend:** React 19, TypeScript and Vite.
- **Backend:** Node.js, Fastify, TypeScript and Zod request validation.
- **Database:** PostgreSQL 16.
- **Payment integration:** PayPal Developer Platform Sandbox REST and webhook components.
- **AI:** An optional OpenAI Responses API integration and a deterministic local evaluator.
- **Testing and release checks:** Vitest, TypeScript checks, ESLint, PostgreSQL integration tests and release/judge verification scripts.

The exact versions and commands must be confirmed from the package manifests and lockfile in the public release commit.

## 5. PayPal integration and financial disclosures

SafeSellz is designed as a non-custodial transaction-assurance application. It is not a bank, wallet or escrow custodian, and this submission must not be represented as holding customer funds.

The development codebase includes PayPal Sandbox integration components, including OAuth 2.0 client-credential access, order and capture lookups, webhook-signature verification and reconciliation logic.

The following limitations must remain visible in the submission:

- Live signed PayPal Sandbox capture and webhook delivery are still pending verification.
- Do not describe a simulated or locally generated event as a genuine PayPal payment or webhook.
- The PayPal 28-day provider cutoff and automatic-release behaviour have not been verified in a live provider test.
- Payout and refund execution controls remain disabled pending the separate financial-operations acceptance and owner approval.
- No production-money transaction is part of this hackathon demonstration.

## 6. AI integration and disclosure

The product code contains an optional OpenAI Responses API path. Its use is subject to the application's configuration and external-data-egress approval controls. A deterministic local rules evaluator is available as a fallback.

**The exact runtime model must be confirmed from the final, owner-approved runtime configuration before a model name or version is stated in the Devpost entry or demo video.** Do not infer the model from the development tools used to build the project.

The product AI is advisory only. It has no authority to approve agreements, verify possession, accept fulfilment evidence, resolve exceptions, change deadlines, issue refunds, release funds or execute financial transactions. Human review remains necessary for elevated-risk and exception cases.

ChatGPT and Gemini were used as development and review assistants. This is separate from the product's runtime AI provider and model.

The AI input boundary is designed around permitted structured events, evidence metadata, fulfilment states and allowed evidence identifiers. User free text, case notes, exception reasons, buyer notes, image bytes and photo OCR are excluded from the described AI input path.

## 7. Important limitations and honest-demo rules

- **Sandbox only:** Do not present this submission as a production payment service.
- **No payout or refund execution:** These controls remain disabled.
- **Provider verification:** Live signed Sandbox capture and webhook delivery remain pending.
- **Provider timing:** The PayPal 28-day cutoff is not verified. An accelerated local timer must be labelled **SIMULATED INTERNAL TIME** and must never be presented as changing PayPal timestamps.
- **Image evidence:** File validation and evidence hashing do not prove what a photograph depicts or establish camera-sensor authenticity.
- **AI authority:** AI findings are advisory and do not replace human review or deterministic application controls.
- **Incomplete milestones:** Work in progress must be labelled as such. Do not describe planned, draft, simulated or unverified functionality as completed.

## 8. Local judge setup

The following instructions describe the intended reproducible local setup. They become usable from this public repository once the sanitized source snapshot and referenced files have been committed and independently checked.

### Prerequisites

- Node.js 22.16.x.
- npm 10.9.x.
- PostgreSQL 16.x, either locally or through Docker Compose.
- Docker with Docker Compose, if using the supplied PostgreSQL service.
- OpenSSL CLI available on PATH.

### Install and configure

    git clone https://github.com/jayblisstech-bot/SafeSellz-AI-Hackathon.git
    cd SafeSellz-AI-Hackathon
    npm ci

Create a local environment file from the example:

    cp .env.example .env

On Windows PowerShell:

    Copy-Item .env.example .env

Use local-only values. Keep database URLs bound to 127.0.0.1 or localhost. Never commit the resulting .env file or real provider credentials.

### Start the database and prepare the demo

    docker compose up -d postgres
    npm run db:migrate
    npx tsx scripts/seed_demo_accounts.ts
    npm run m7:judge-check -- --verify-db

### Start the application

Run the API and web application in separate terminals:

    npm run dev

    npm run dev:web

The documented local addresses are:

- Web application: http://127.0.0.1:5173
- API: http://127.0.0.1:3000
- API liveness check: http://127.0.0.1:3000/health/live
- API readiness check: http://127.0.0.1:3000/health/ready

For the full role-based walkthrough, account-seeding notes and judge access policy, see the judge guide in the release documentation once it is present in this repository.

Do not place judge passwords, hosted credentials, API keys or other secrets in this README or in public Git history. Any hosted judge credentials must be provided through the competition's confidential judge-instructions mechanism.

## 9. Verification commands

Run the checks supported by the public release snapshot:

    npm run typecheck
    npm run lint
    npm test
    npm run build
    npm run check
    npm run m7:judge-check -- --verify-db
    npm run m7:audit

The release agent must record the exact commit tested and the actual result of each command. A command listed here is not evidence that it has passed.

The public-release audit may report a licence blocker until the owner-approved licence has been added. Do not suppress or misrepresent that result.

## 10. Suggested judge walkthrough

The final walkthrough should use the actual application and a verified build:

1. Open the Sandbox and timing disclosures.
2. Create a seller deal and show the actual configured amount and fee.
3. Show the canonical terms hash and seller consent.
4. Accept the same terms as the buyer.
5. Demonstrate a genuine unauthorised-access rejection, if available in the verified build.
6. Walk through the physical fulfilment and handover controls.
7. Show the advisory AI findings and its limitations.
8. Finish with the judge preflight and release-audit output.

Do not fabricate API responses, payment confirmations, webhook events, screenshots or application footage. Do not use mockups or an older demonstration page as proof of the current build.

## 11. Hackathon submission checklist

The PayPal AI Hackathon submission package must cover the following:

### Core competition requirements

- [ ] A clear English-language project description explaining the problem, product and functionality.
- [ ] A meaningful PayPal Developer Platform integration, with accurate disclosure of what is implemented and what has been verified.
- [ ] A meaningful AI integration, with the actual runtime provider and model identified accurately.
- [ ] A functional application that judges can run or interact with.
- [ ] A public GitHub repository containing the sanitized source code, required assets and setup instructions.
- [ ] A visible OSI-approved open-source licence.
- [ ] A publicly accessible YouTube demonstration shorter than three minutes, showing the actual application functioning.
- [ ] Free judge access for the judging period through 15 December 2026.
- [ ] English-language README, video narration/captions and Devpost submission materials.

### Repository and technical-readiness checks

- [ ] Sanitized source snapshot copied into this repository using fresh public Git history.
- [ ] No private development history, coordination handoffs, agent-control files or internal operating instructions published.
- [ ] No .env files, real credentials, private keys, personal data, merchant secrets, raw payment evidence or private infrastructure details published.
- [ ] Package manifests, lockfile, source, migrations, assets, scripts and required configuration examples included.
- [ ] Database setup and migrations tested on a clean local environment.
- [ ] Automated tests and build checks run against the exact public commit.
- [ ] Public-release audit completed and findings reviewed.
- [ ] Judge setup guide and safe local demo-account seeding included.
- [ ] Known limitations and unfinished work disclosed clearly.

### Demo and final submission checks

- [ ] Actual application footage recorded from the verified release build.
- [ ] Video duration checked to be strictly under 3:00.
- [ ] Video checked for visible secrets, private information and unlicensed media.
- [ ] Simulated internal time, pending PayPal verification, disabled payout/refund controls and advisory-only AI disclosed.
- [ ] Judge access tested and kept free through 15 December 2026.
- [ ] Devpost project description, technology list, AI disclosure, PayPal explanation, repository URL, video URL and judge instructions completed.
- [ ] Final public release and submission approved by the project owner.

Screenshots, a technical evidence index and a compliance matrix are useful supporting materials. They should be included where available, but they do not replace the working application, public source repository, licence, short public video or judge access.

## 12. Submission links

- **Public source repository:** https://github.com/jayblisstech-bot/SafeSellz-AI-Hackathon
- **Competition page:** https://paypalaihackathon.devpost.com/
- **Competition rules:** https://paypalaihackathon.devpost.com/rules
- **Public demo video:** To be added after the actual video is recorded, checked and approved.
- **Devpost project entry:** To be added after the final entry is reviewed.
- **Judge access:** Setup instructions belong in this repository; any confidential hosted credentials belong only in the private judge-instructions field.

## 13. Licence

**An open-source licence has not yet been added to this repository.**

A repository being publicly visible does not, by itself, make the code open source or grant permission to reuse it. The project owner must select an OSI-approved licence and add its complete canonical licence text as the top-level LICENSE file before the repository is described as an open-source release.

Do not choose or add a licence on the owner's behalf.

## 14. Security and responsible use

- Use PayPal Sandbox credentials only for the hackathon demonstration.
- Keep secrets in local environment variables or an approved secret manager, never in Git.
- Do not connect local tests or seeded demo accounts to production databases or production payment systems.
- Do not represent simulated events or internal clocks as provider-confirmed activity.
- Do not allow AI output to override deterministic controls or human approval.
- Report suspected security issues privately rather than publishing exploit details or credentials.

---

**SafeSellz AI | PayPal AI Hackathon 2026**

This README will be updated as the verified public source snapshot, licence, judge materials and final submission assets are added.
