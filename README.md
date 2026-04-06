# FlowX

A real-time data pipeline orchestrator for small engineering teams.

## The Problem

Data Engineers at small startups spend most of their time debugging silent ETL failures with no visibility into what broke or why. Existing tools like Prefect and Airflow are too complex for a 3-person team. FlowX solves this with a visual pipeline builder, live execution monitoring, and an AI assistant that explains failures in plain English.

## Tech Stack

| Technology        | Purpose          | Why                                                     |
| ----------------- | ---------------- | ------------------------------------------------------- |
| Node.js + Express | API server       | JavaScript runtime, same language as frontend logic     |
| PostgreSQL        | Primary database | Relational data with transaction guarantees             |
| Redis             | In-memory store  | Low-latency job queue storage and pub/sub messaging     |
| BullMQ            | Job queue        | Manages background job execution, retries, and failures |
| WebSockets        | Real-time layer  | Server pushes live status updates without polling       |

## Architecture

_Diagram coming in Phase 1 completion_

## Status

Currently in Active Development

### Completed

- [x] Project structure and development setup
- [x] Database schema design
- [x] API Design

### In Progress

- [ ] Core Backend - Express server + PostgreSQL

## Running Locally

_Setup instructions coming once core backend is complete_
