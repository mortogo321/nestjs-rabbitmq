# NestJS Microservices with RabbitMQ + MongoDB

A NestJS monorepo demonstrating an event-driven microservices architecture: independent services communicate over RabbitMQ, backed by a MongoDB replica set for multi-document transactions.

## What's inside

- Three independently deployable services in a Nest CLI monorepo: `auth`, `orders`, `billing`
- `orders` creates an order inside a MongoDB transaction and emits an `order_created` event over RabbitMQ
- `billing` defines an `order_created` event handler with manual message acknowledgment
- `auth` issues JWT session cookies via local + JWT Passport strategies, hashes passwords with bcrypt, and exposes a `validate_user` RabbitMQ message pattern for other services to verify a session
- Shared `libs/common` library: `RmqModule` / `RmqService` (RabbitMQ client + manual ack), a Mongoose `AbstractRepository` / `AbstractSchema` base, and shared auth wiring (cookie parsing)
- Request validation with `class-validator` DTOs and environment validation with `Joi`
- Docker Compose stack: the three services, a RabbitMQ broker (management UI), a 3-node MongoDB replica set (primary/secondary/arbiter, required for transactions), and MailHog (included as an SMTP sandbox, not currently used by any service)

## Current state

Microservice bootstrapping is inconsistent across apps: `auth` and `orders` connect a RabbitMQ microservice listener in `main.ts`, but `billing` does not, so its `order_created` handler is not currently wired up to consume from the queue.

## Tech stack

- NestJS (`@nestjs/microservices`, `@nestjs/mongoose`, `@nestjs/jwt`, `@nestjs/passport`, `@nestjs/config`)
- MongoDB via Mongoose
- RabbitMQ via `amqplib` and `amqp-connection-manager`
- Passport (local + JWT strategies), bcrypt, class-validator, Joi
- TypeScript, Jest for unit tests

## Quickstart

```bash
yarn install

# copy env templates for each service
cp apps/auth/.env.example apps/auth/.env
cp apps/orders/.env.example apps/orders/.env
cp apps/billing/.env.example apps/billing/.env

# start all services, RabbitMQ, the MongoDB replica set, and MailHog
docker compose up -d
```

- Orders: http://localhost:3000
- Auth: http://localhost:3001
- RabbitMQ management UI: http://localhost:15672 (`admin` / `password@01`)
- MailHog UI: http://localhost:8025

## Key endpoints

- `POST /auth/users` - register a user
- `POST /auth/login` - log in, sets an httpOnly JWT cookie
- `POST /orders` - create an order (validates the request, persists it, emits `order_created`)
- `GET /orders` - list orders

## Tests

```bash
yarn test
```
