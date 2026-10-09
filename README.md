# CredNest

CredNest is an alternative-credit platform for thin-file borrowers. It combines consent-based financial data, psychometric signals, and machine-learning risk models to produce explainable credit assessments and connect eligible users with lending offers.

## What is included

- **Flutter app** for onboarding, consent flows, credit insights, quizzes, and loan journeys.
- **Node.js API gateway** for authentication, consent orchestration, Account Aggregator/GST/DigiLocker integrations, scoring, ledgers, and OCEN lending workflows.
- **FastAPI ML service** with alternative-credit scoring, narration classification, seasonal-income, psychometric, ensemble-risk, and NPA early-warning models.
- **Local infrastructure** for PostgreSQL, MongoDB, Redis, Kafka, and the application services through Docker Compose.

## Repository layout

```text
backend/
  api-gateway/       Express API and integration layer
  ml-service/        FastAPI inference service and model training code
docs/                Architecture and API documentation
infra/               Docker Compose and Kubernetes manifests
mobile/
  creditdna_app/     Flutter client application
```

## Prerequisites

- Docker and Docker Compose (recommended for the complete backend stack)
- Node.js 18+ and npm (for local API development)
- Python 3.11+ (for local ML-service development)
- Flutter with Dart 3.13+ (for the mobile app)

## Quick start with Docker

1. Create the API gateway environment file:

   ```bash
   cp backend/api-gateway/.env.example backend/api-gateway/.env
   ```

2. Replace the placeholder JWT and encryption values in `.env`. External provider credentials can remain as placeholders while mock integrations are enabled.

3. Start the stack:

   ```bash
   docker compose -f infra/docker-compose.yml up --build
   ```

The API gateway is available at `http://localhost:3000`, and the ML service is available at `http://localhost:8001`. Check them with:

```bash
curl http://localhost:3000/health
curl http://localhost:8001/health
```

## Run services locally

### API gateway

```bash
cd backend/api-gateway
cp .env.example .env
npm install
npm run migrate
npm run dev
```

The example environment uses port `4000` for local development. PostgreSQL, MongoDB, Redis, and Kafka must be reachable at the addresses configured in `.env`.

### ML service

```bash
cd backend/ml-service
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8001 --reload
```

### Flutter app

```bash
cd mobile/creditdna_app
flutter pub get
flutter run
```

Configure the app's API base URL and Firebase project for the target platform before using authenticated flows.

## Tests

Run the API gateway tests:

```bash
cd backend/api-gateway
npm test
```

Run the ML-service tests:

```bash
cd backend/ml-service
pytest
```

Run the Flutter tests:

```bash
cd mobile/creditdna_app
flutter test
```

## API overview

The gateway exposes endpoints under `/v1` for authentication, consent, onboarding, Account Aggregator data, GST data, scoring, quizzes, loans, webhooks, and ledger operations. ML endpoints are exposed under `/ml`.

See [docs/api-spec.md](docs/api-spec.md) and [backend/api-gateway/docs/api-spec.md](backend/api-gateway/docs/api-spec.md) for the available routes and payloads.

## Security notes

- Never commit `.env` files, private keys, access tokens, or production financial data.
- Use strong, distinct JWT secrets and a 256-bit encryption key outside development.
- Mock integrations are intended for local development only.
- Review consent, data-retention, encryption, and audit requirements before production use.

## Project status

CredNest is under active development and should be treated as a prototype until its security, compliance, model validation, and production deployment controls have been independently reviewed.
