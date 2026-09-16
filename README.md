# Elzipo — Backend Developer | C# / .NET | Payment Gateway & Fintech APIs

> Mid-level Backend Engineer (2-3 years) specializing in **ASP.NET Core Web APIs, payment processing, webhooks, and secure high-throughput transaction systems**. Open to Backend (Payments) roles.

📍 Lagos, Nigeria / Remote • 📧 davromeo24@gmail.com • 𝕏 [X](https://x.com/elzipodev) • 🐙 [GitHub](https://github.com/Chukwudebere-ferd) • 🌐 [Portfolio](https://elzipodev.cv)

![C#](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=c-sharp&logoColor=white)
![.NET](https://img.shields.io/badge/.NET-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![ASP.NET Core](https://img.shields.io/badge/ASP.NET_Core-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL_Server-CC2927?style=for-the-badge&logo=microsoft-sql-server&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

---
## 👨‍💻 Professional Summary

Backend Developer with **2-3 years building .NET systems**, including **payment integrations, wallet/ledger services, and REST APIs**.

I focus on what payment companies care about:
- **Correct money movement:** double-entry ledger, idempotency, transactional outbox, saga for distributed payments
- **Reliability:** 99.9% uptime APIs with Polly retries, circuit breakers, rate-limiting, Hangfire background jobs
- **Security & Compliance:** PCI-DSS mindset, AES-256 + DPAPI, JWT + OAuth2, HMAC webhook verification, PII masking & audit logs
- **Scale:** SQL Server + EF Core optimization, Redis caching/idempotency, RabbitMQ for async settlement/notifications

**Stack:** C# 12, .NET 8, ASP.NET Core Web API, EF Core, SQL Server, PostgreSQL, Redis, RabbitMQ, Hangfire, xUnit, Docker, GitHub Actions, Azure / AWS

---
## 🛠 Tech Stack

**Languages & Frameworks:**
C#, .NET 8, ASP.NET Core Web API, Minimal APIs, Entity Framework Core, Dapper (for hot paths), LINQ, FluentValidation, AutoMapper / Mapster

**Payments Specific:**
Stripe / Paystack / Flutterwave integration, Webhook handling + HMAC-SHA256 signature verification, 3D-Secure flow, Card tokenization (no raw PAN storage), Refunds / Chargebacks / Disputes, Settlement & Reconciliation jobs

**Data & Messaging:**
SQL Server (indexed, partitioned transactions table), PostgreSQL, Redis (idempotency keys, rate limit, cached balances), RabbitMQ (payment events), Hangfire (retry payouts, daily settlement), EF Core migrations

**Security / Quality:**
JWT + Refresh rotation, OAuth2 / OpenID Connect, Role-based + Policy-based auth, AES-256 encryption at rest, TLS 1.2+, Serilog + Seq structured logging, OpenTelemetry + Prometheus, xUnit + Moq + FluentAssertions (85%+ coverage on ledger), Swashbuckle / Scalar OpenAPI

**DevOps:**
Docker + Docker Compose, Kubernetes (basics), GitHub Actions CI/CD, Azure App Service / Azure SQL / Key Vault, AWS (EC2, RDS, SQS alternative), Nginx reverse proxy

---
## 💼 Experience

### Backend Developer (.NET) — Fintech / Payments Projects | 2024 — Present
*Payment gateway APIs for cards, transfers, and mobile money.*
- Built **ASP.NET Core payment orchestration API** (`/api/v1/payments/charge`, `/payouts`, `/refunds`) with clean architecture and versioning.
- Implemented **idempotency-key middleware + Redis (24h TTL)** to safely handle retries without duplicate charges.
- Integrated **Paystack / Flutterwave / Stripe webhooks** with HMAC verification, out-of-order handling, and transactional outbox pattern.
- Designed **double-entry ledger in SQL Server** (Accounts, JournalEntries, LedgerLines) with concurrency control for accurate balances and reconciliation.
- Added **Polly retry + circuit breaker** for PSP calls, Hangfire jobs for settlement / payout retries, and structured audit logging.
- **Tech:** C#, .NET 8, EF Core, SQL Server, Redis, RabbitMQ, Hangfire, xUnit, Docker, Azure

### Backend Developer — Lendora / Online-Banking Projects | 2023 — 2024
*Wallet and banking APIs with ledger-backed transfers.*
- Built wallet credit/debit, transfer, and statement APIs with **EF Core transactions + idempotent reference IDs**.
- Optimized EF queries (AsNoTracking, compiled queries, covering indexes) for fast statements on large datasets.
- Implemented JWT + refresh rotation, OTP verification, rate-limiting (AspNetCoreRateLimit) to block brute-force on payout endpoints.
- Wrote **xUnit integration tests with WebApplicationFactory + Testcontainers (SQL Server)** for charge → webhook → settle flow.
- **Tech:** .NET 7, C#, PostgreSQL/SQL Server, Redis, Swagger, GitHub Actions

### Freelance .NET API Developer | 2022 — 2023
- Shipped 6 REST APIs (billing, invoicing with Paystack inline payments, reconciliation CSV import).
- Introduced Docker Compose local stack + CI pipeline cutting deploy time 40%.

---
## 🚀 Featured Projects

### 1. PayGateway.Core — ASP.NET Core Payment Gateway API ⭐ Featured
**What it does:** Unified charge/payout/refund API + webhook receiver + ledger + reconciliation, like a mini-Stripe.

- `POST /api/v1/payments/intents` → creates PaymentIntent (Pending) with `Idempotency-Key` header required
- PSP adapter pattern (`IPaymentProvider` → `StripeAdapter`, `PaystackAdapter`, `MockBankAdapter`) — easy to add new acquirer
- Webhook endpoint `POST /api/v1/webhooks/{provider}` verifies HMAC-SHA256, stores raw payload, publishes `PaymentSucceeded` event
- Ledger service writes balanced double-entry rows in single DB transaction; outbox table → RabbitMQ publisher
- Hangfire daily job reconciles `payments vs ledger vs PSP settlement CSV`, flags mismatches in `/reconciliation/report`
- Redis cached idempotency + per-merchant rate limit (100 req/min), JWT merchant API keys (`sk_live_...` hashed with SHA256)

**Architecture:**
```
Client → API (Auth, Idempotency, Validation) → PaymentService → Ledger (SQL Server Tx) → Outbox → RabbitMQ → [WebhookHandler, SettlementWorker, NotificationWorker]
                                              ↘ PSP Adapter (Polly) → Mock PSP
```

**Tech:** C# / .NET 8, ASP.NET Core, EF Core, SQL Server, Redis, RabbitMQ, Hangfire, Serilog, xUnit, Docker Compose, GitHub Actions
**Performance:** Load-tested with k6, documented p95 latency and idempotency handling in `/docs/load-test.md`
**Repo structure:** `/src/Api`, `/src/Application`, `/src/Domain`, `/src/Infrastructure`, `/tests/Integration`, `/postman/`, `/docs/architecture.png`
🔗 `https://github.com/Chukwudebere-ferd/paygateway-core` | 📄 `Live Demo: Swagger URL`

### 2. LedgerVault — Double-Entry Wallet & Settlement Service
**What it does:** Immutable money ledger with account freeze, holds (auth-capture), refunds, and daily settlement files.

- Models `Accounts | Holds | JournalEntries | LedgerLines` — no direct balance mutation, balance = `SUM(lines)`
- `POST /wallets/{id}/hold`, `/capture`, `/refund` with optimistic concurrency (`RowVersion`) to prevent double-spend
- Background worker generates `settlement_YYYYMMDD.csv` + `disputes.json`, SFTP-ready
- Admin endpoint for dispute/chargeback simulation: `POST /disputes/chargeback` reverses with fee entry
- Full audit log (who, when, IP, before/after hash) for compliance review

**Tech:** .NET 8, Dapper for ledger hot path, SQL Server stored procedures, Redis distributed lock (RedLock), OpenTelemetry
🔗 `https://github.com/Chukwudebere-ferd/ledgervault`

### 3. SecurePay Webhooks & Fraud Shield
**What it does:** Hardened webhook gateway + basic fraud/risk checks.

- HMAC + timestamp tolerance (5 min) + replay protection via Redis `SETNX`
- IP allowlist middleware, per-endpoint throttling, PII redaction in logs (`card_pan: **** **** **** 1234`)
- Risk rules: velocity check (>5 charges/min), amount anomaly, BIN country mismatch → `flag_for_review`
- AES-256-GCM encryption for stored tokens using Azure Key Vault / Data Protection API, key rotation docs

**Tech:** C#, ASP.NET Core Middleware, FluentValidation, Redis, Seq, xUnit + Moq
🔗 `https://github.com/Chukwudebere-ferd/securepay-webhooks`

### 4. More Work:
- `Online-Banking/` — **Mini Core-Banking API** — transfers, statements, auth
- `Lendora/` — **Loan Disbursement Payouts API** — payout retries, idempotency, reconciliation
- `gstbills/` — **Billing + Invoice Payments** — invoicing, collections, webhook settlement

---
## 📊 System Design

**Charge API design for scale:**
1. Edge: Cloudflare + Nginx → API (stateless, HPA 3-20 pods)
2. Idempotency check (Redis `GET idem:{key}`) → return cached response if hit
3. Validate → create `PaymentIntent Pending` (DB write, 10ms) → enqueue `ProcessPayment` (RabbitMQ)
4. Worker calls PSP with Polly (retry 3x exp backoff, circuit-break 30s), updates DB via outbox
5. PSP webhook (separate path) reconciles — last-write-wins by `psp_event_id` unique constraint
6. Settlement: nightly Hangfire job aggregates by merchant, creates payout batch, uploads bank file

**Failure handling:** DLQ for poison messages, saga compensation (auto-refund on payout fail), Prometheus alerts on webhook lag >2min.

---
## ✅ Compliance & Best Practices I Follow

- [x] Never log/store full PAN/CVV — tokenize only, mask in logs
- [x] All money in `decimal(18,4)` + currency code (ISO 4217), never `float`
- [x] Idempotency keys on all POST money endpoints
- [x] Webhook signature verification + idempotent consumers
- [x] PCI-DSS aware: least privilege DB users, encrypted secrets (Key Vault), audit trails, dependency scanning (Dependabot)
- [x] OWASP Top 10: parameterized queries, JWT short expiry, CORS allowlist, security headers

---
## 📈 GitHub Stats

![GitHub stats](https://github-readme-stats.vercel.app/api?username=Chukwudebere-ferd&show_icons=true&theme=radical)
![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=Chukwudebere-ferd&layout=compact&langs_count=8)

**Pinned:** `paygateway-core`, `ledgervault`, `securepay-webhooks`, `Online-Banking`

---
## 📚 Certifications & Learning

- Microsoft Certified: Azure Developer Associate (AZ-204) — In Progress
- Stripe Payments Integration Fundamentals — 2024
- Udemy: Microservices with .NET 8, RabbitMQ & Docker — 2024

---
## 📬 Open to Work

Actively applying for **Backend Engineer (Payments / .NET / C#)** — Remote / Hybrid Lagos.
I can explain ledger design, webhook failure modes, and show Postman + Swagger demos on a call.

📧 davromeo24@gmail.com | 📱 09047594112 | [Book a call](https://calendly.com/Chukwudebere-ferd) | 🌐 [elzipodev.cv](https://elzipodev.cv)

> *Code samples, architecture docs, and live demos available on request.*
