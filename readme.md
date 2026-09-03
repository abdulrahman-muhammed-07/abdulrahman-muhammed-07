# Abdulrahman Muhammed

Backend software engineer with 4+ years building and running production systems: multi-tenant SaaS, payment integrations, message pipelines, and real-time services. I work in PHP, Laravel, MySQL and Redis, and own the Docker / Azure deploy path around them.

Currently focused on production system design: tenant isolation at the query layer, concurrency-safe write paths, idempotent queue jobs across a web and worker split, and multi-gateway payment reconciliation.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/abdulrahmanelnegery)

---

## Tech Stack

![PHP](https://img.shields.io/badge/PHP-777BB4?style=flat&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=flat&logo=laravel&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat&logo=springboot&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-5FA04E?style=flat&logo=nodedotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![Angular](https://img.shields.io/badge/Angular-DD0031?style=flat&logo=angular&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=flat&logo=nginx&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat&logo=microsoftazure&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=githubactions&logoColor=white)

**Also**

![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)
![Rust](https://img.shields.io/badge/Rust-000000?style=flat&logo=rust&logoColor=white)
![GitLab CI](https://img.shields.io/badge/GitLab_CI-FC6D26?style=flat&logo=gitlab&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat&logo=jenkins&logoColor=white)
![Pest](https://img.shields.io/badge/Pest_%2F_PHPUnit-3C5C8E?style=flat&logo=php&logoColor=white)
![PHPStan](https://img.shields.io/badge/PHPStan-2EB7A0?style=flat&logo=php&logoColor=white)

**Core Expertise**
- Multi-tenant architecture and tenant isolation
- Concurrency control and race-condition hardening
- Payment gateway integration and reconciliation
- Queues, background jobs, and fan-out pipelines
- Real-time delivery (Redis pub/sub, WebSockets)
- Database optimization and index design

<img height="145" alt="Top languages" src="https://github-readme-stats.vercel.app/api/top-langs/?username=abdulrahmanelnegery&layout=compact&langs_count=8&hide_border=true&count_private=true" />

---

> **Note:** My production work is proprietary. The systems below are described without source. The open-source repositories further down are separate, from-scratch implementations of the same patterns.

## Production Work

### Multi-Tenant Restaurant SaaS · Front Of House
`Laravel 9 · PHP 8.2 · MySQL · Redis · Angular · Node.js`

Backend across four product domains (reservations, walk-in queue, payments, event ticketing) for 50+ restaurants, roughly 2,000 reservations and 3,000 event tickets a day.
- Cut average API response time 90% with Redis cache-aside and index redesign; enforced company / venue / branch isolation on every query and closed cross-tenant IDOR paths
- Fixed a table double-booking race with row-level locking plus a unique-constraint backstop, locked down by concurrency tests running two live DB connections
- Maintain multi-gateway payments (Paymob, GetPayin, Geidea): callback signature verification, error-code mapping, payment-state reconciliation
- Built the messaging stack: a per-tenant template engine for SMS / WhatsApp / push, idempotent replayable queue jobs across a web and VM-worker split, and a Node.js WebSocket service for live reservation and ticket events

### Healthcare Consultation Platform · Guapa
`Laravel · Filament · MySQL · GitLab CI`

Backend for a platform serving hospitals and private practices across the Gulf.
- Shipped an online consultation booking flow end to end, with role-based access separating doctor, admin, and operational permissions
- Cut dashboard load from around 3s to under 1s via query optimization; stabilized the GitLab / Jenkins CI/CD pipelines

### E-commerce Integration Platform · Numinix
`Laravel · PHP · MySQL · Amazon SP-API · Google Merchant · eBay · Lightspeed POS`

Production marketplace integrations for 10+ sellers, one with a 1M+ SKU catalog.
- Retry logic, rate-limit handling, and incremental sync so integrations stayed within third-party quotas and survived vendor outages
- Reindexed 30+ core tables (worst-case queries went from 50 minutes to under 30 seconds); packaged shared patterns into a Laravel starter kit adopted by 15+ projects

### Noted · [itsnoted.net](https://itsnoted.net) (live side project)
`Laravel 11 · PHP 8.4 · MySQL · Redis · Angular`

Embeddable contact-sharing widget with a WhatsApp / SMS OTP auth flow. In production. Commercial, source private.

---

## Open Source

From-scratch reference implementations. Each has passing tests from a clean clone and green CI.

### [slotlock](https://github.com/abdulrahmanelnegery/slotlock)
`Laravel · PHP 8.4 · MySQL · Redis · Angular`

Multi-tenant booking API with a concurrency-safe reservation path.
- Global Eloquent scope for tenant isolation, plus an explicit ownership check that returns a deliberate 403 on a cross-tenant id instead of a 404
- `SELECT ... FOR UPDATE` plus a `unique(resource_id, slot_start)` backstop, proven by a test that races two real MySQL connections for the same slot

### [fanout](https://github.com/abdulrahmanelnegery/fanout)
`Laravel 11 · Redis pub/sub · Node 20 · TypeScript · WebSocket`

Real-time event delivery: API to queued job to Redis channel to a Node WebSocket service to the browser.
- Identical JSON payload from Redis through to the client; a short-lived HMAC token is checked on socket connect and the connection is closed with 4001 if it is bad

### [laravel-blueprint](https://github.com/abdulrahmanelnegery/laravel-blueprint)
`Laravel · Pest · PHPStan L8 · Pint`

How I layer a Laravel app: HTTP, application, domain, infrastructure, with one feature wired end to end through all four.
- Framework-free domain: value objects that enforce their own invariants, side effects hung off domain events, HTTP validation kept separate from business rules

### [hookrelay](https://github.com/abdulrahmanelnegery/hookrelay)
`Go · Gin · Postgres · Testcontainers`

Webhook delivery service. Built to learn Go properly.
- Transactional outbox written in the same transaction as the event; a worker pool claims batches with `FOR UPDATE SKIP LOCKED`, HMAC-signs each POST, retries on exponential backoff, dead-letters, and can replay

### [ledgerline](https://github.com/abdulrahmanelnegery/ledgerline)
`Java 21 · Spring Boot 3 · JPA · Flyway · Testcontainers`

Double-entry ledger API. Built to learn Spring Boot properly.
- Sum-to-zero journal invariant enforced in the domain factory, not only a database check; idempotent posting via an idempotency key; pessimistic account locking in ascending id order, under real concurrency tests

---

## Open to Work

Open to remote backend / software engineer roles.

---

## Connect

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/abdulrahmanelnegery)
[![Dev.to](https://img.shields.io/badge/Dev.to-0A0A0A?style=flat&logo=devdotto&logoColor=white)](https://dev.to/abdulrahmanmuhammad)
[![Stack Overflow](https://img.shields.io/badge/Stack_Overflow-FE7A16?style=flat&logo=stackoverflow&logoColor=white)](https://stackoverflow.com/users/18606007/abdelrahman-el-negery)
[![Email](https://img.shields.io/badge/Email-EA4335?style=flat&logo=gmail&logoColor=white)](mailto:abdulrahman.elnegery@gmail.com)
