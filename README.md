# n8n High-Availability Data Pipeline: Webhook to PostgreSQL

This repository contains a production-ready n8n workflow designed for mission-critical data ingestion. Unlike standard automations, this system is engineered with a **Security-First** and **Self-Healing** architecture.

##  System Architecture
The workflow follows a 4-stage processing lifecycle:
1. **Ingestion**: Scalable Webhook entry point with custom response headers.
2. **Normalization**: A custom JavaScript Code Node flattens nested JSON, sanitizes null/undefined values, and enforces ISO-standard timestamps for database consistency.
3. **Persistence**: Transactional sync to PostgreSQL with dual-layer logging (Primary Data + Execution Metadata).
4. **Resilience**: A global "Circuit Breaker" pattern implementing **Exponential Backoff** (1s, 2s, 4s...) and Slack alerting to handle API or Database downtime.

##  Key Features
- **Data Integrity**: Automated flattening of complex objects to ensure no data loss during PostgreSQL upserts.
- **Error Handling**: 3-stage retry logic that prevents workflow termination during transient network failures.
- **Observability**: Real-time Slack notifications for database errors and final failure states.
- **Audit Trail**: Metadata logging for every execution (Execution ID, Workflow ID, and Item Count).

##  Deployment Requirements
- **n8n Instance**: Self-hosted (Docker) or Cloud.
- **Database**: PostgreSQL instance (compatible with Supabase).
- **Messaging**: Slack Webhook URL for alerting.

##  Setup
1. Import the `.json` file into your n8n instance.
2. Update the **PostgreSQL** credentials to point to your database.
3. Replace the **Slack** placeholders with your error-log channel ID.
4. (Optional) Set `N8N_EXECUTIONS_MODE=queue` in your environment to handle high-concurrency traffic.
