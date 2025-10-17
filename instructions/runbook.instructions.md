---
description: 'Generate a `runbook-instructions.md` runbook for Site Reliability Engineers (SREs) or Support Engineers tailored to the target repository. The runbook must be concise, actionable, and focused on operational playbooks, diagnostics, healthchecks, backups, deploy/rollback, and escalation steps.'
applyTo: "**/runbook-instructions.md"
---

# Runbook generation instructions (for GitHub Copilot / automation)

Purpose
- Produce a single file named `runbook-instructions.md` at the repository root that enables an SRE or Support Engineer to operate, maintain, and troubleshoot the application.

High-level contract
- Input: repository files + package manifests, Dockerfiles, docker-compose, CI configs, and README.
- Output: a complete `runbook-instructions.md` containing start/stop, healthchecks, diagnostics, backups, deploy/rollback, scaling, monitoring, security, and escalation guidance.
- Success criteria: the runbook contains explicit, copy-pasteable commands for local/infra operations and a short incident playbook for the top 3 likely failures.

Required sections (must appear, in this order)
1. Title & one-line purpose
2. Checklist (quick actions for on-call)
3. High-level architecture (1-paragraph)
4. Environment & important files (where to find config, manifests)
5. Required environment variables / secrets (list and note storage recommendations)
6. Start / stop / restart commands (compose, docker, k8s variants when detected)
7. Healthchecks & expected signals (URLs, ports, readiness/liveness probes)
8. Logs & diagnostics (docker logs, kubectl logs, common greps, how to inspect stack traces)
9. Backup & restore (DB dump/restore, object store backup steps)
10. Deploy / rollback (build, push, rollout, and rollback commands)
11. Scaling guidance (stateful vs stateless notes)
12. Security notes (secrets handling, non-root user, exposed ports)
13. Monitoring & alerting recommendations (metrics to monitor, suggested exporters)
14. Common incidents and remediation playbooks (top 3 incidents, step-by-step)
15. Useful commands reference (copyable commands)
16. Escalation contacts and where to find runbook history

Formatting & content rules
- Keep it concise: aim for 300–1200 words, but cover required sections fully.
- Provide copyable commands in fenced code blocks where appropriate.
- When the repo contains Docker or docker-compose, include docker compose commands (build, up, logs, exec, down).
- When the repo contains Kubernetes manifests or Helm charts, add kubectl/helm examples for rollout/rollback and logs.
- If a database is detected (Postgres/MySQL), include pg_dump/pg_restore or mysqldump examples and `psql`/`mysql` connectivity checks.
- If object storage (S3/MinIO) is detected, include `aws s3` or `mc` examples to list and copy buckets.
- When an orchestrator is not present, prefer docker-compose commands for local reproduction.
- Include healthcheck endpoints or example `curl` commands. If none are obvious, instruct to check the main web port or process.
- Add a short troubleshooting playbook for the three most likely failures inferred from repo (e.g., dependency install error, DB connection error, model download issues).

Security and secret handling
- Never print secrets or include real credentials. Use placeholders and recommend `.env` or secret stores (Docker secrets, Vault, AWS Secrets Manager).
- Recommend running images as non-root; check Dockerfile for `USER` instructions and note if missing.

Quality gates (what to verify before finishing)
- Commands run locally in the repository context (if possible) or are syntactically correct.
- No platform-specific claims unless the repo contains evidence (e.g., existence of `k8s/` or `docker-compose.yaml`).
- Ensure `runbook-instructions.md` references files that exist in the repo (Dockerfile, docker-compose.yaml, db/init.sql, etc.) when present.

Placeholders and editable fields
- Use placeholders for on-call contacts and alerting channels (e.g., `<on-call-email>`). Add a short note telling the maintainer to replace placeholders.

Optional enhancements
- Suggest adding `.env.example` if env vars are inferred.
- Suggest monitoring exporters and a minimal Prometheus/Grafana setup if metrics are not present.
- Recommend image scanning (Trivy) and Dockerfile linting (hadolint) as CI checks.

Tone and style
- Friendly, concise, and imperative (step-by-step). Use action verbs.
- Prioritize operational clarity over exhaustive engineering detail.

Example snippet to include where relevant (do not hardcode unless repo matches):

```markdown
## Start / stop (local)
Build and start the stack:

```bash
docker compose up --build -d
```

Tail app logs:

```bash
docker compose logs -f alfred-app
```
```

Delivery
- Generate `runbook-instructions.md` in the repository root following the rules above. If multiple application components exist (web, db, worker), include short subsections for each.
- Add a short line at the bottom: "Last updated: <date>" and a short changelog if the repo has a CONTRIBUTORS or CHANGELOG.

If uncertain about details in the repository, make one reasonable assumption and note it in the runbook (e.g., assume Postgres if `psycopg2` or `requirements.txt` contains `psycopg2-binary`).

End of instruction file.
