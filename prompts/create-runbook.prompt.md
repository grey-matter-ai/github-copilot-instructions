---
mode: agent
description: 'Generate a repository-specific `runbook-instructions.md` using the rules in `.github/instructions/runbook.instructions.md`. The runbook should help an SRE or Support Engineer operate and troubleshoot the application.'
---

You are an automation agent that generates a single `runbook-instructions.md` at the repository root.
Follow the rules in `.github/instructions/runbook.instructions.md` exactly.

Task
- Inspect the repository files and infer the runtime stack (Dockerfile, `docker-compose.yaml`, Kubernetes/Helm manifests, `requirements.txt`, package.json, DB init scripts, etc.).
- Generate a concise, actionable runbook covering the required sections listed in the instructions file.

Required behavior
- Always include these sections in this order: title & purpose, checklist, architecture summary, important files and env vars, start/stop commands, healthchecks, logs & diagnostics, backup & restore, deploy/rollback, scaling, security notes, monitoring & alerts, top-3 incident playbooks, useful commands, escalation.
- Provide copy-pasteable commands in fenced code blocks for detected runtimes (docker compose, docker, kubectl/helm, psql/mysqldump, aws s3 / mc for object storage).
- Detect Postgres/MySQL and include corresponding dump/restore examples. Detect MinIO/S3 and include `mc` or `aws s3` examples.
- If `Dockerfile` or `streamlit` is present, ensure app healthcheck examples use `--server.address 0.0.0.0` or equivalent.
- Use placeholders for secrets and on-call contacts (e.g., `<on-call-email>`, `<POSTGRES_PASSWORD>`). Warn not to commit secrets.
- Add a final line: `Last updated: <date>`.

Style and constraints
- Keep the runbook concise (300–1200 words) and operationally focused.
- Use imperative, step-by-step language. Prefer short bullet lists and commands.
- If a specific technology cannot be identified, make one reasonable assumption and state it briefly.

Output
- Create or overwrite `/runbook-instructions.md` with the generated content.
- Do not print the runbook in this response; write it to the repository file only.

Quality gates
- Ensure commands are syntactically correct for typical shells (bash/zsh) and reference files that actually exist in the repo when possible.
- If you detect both Docker Compose and Kubernetes-like manifests, include both short examples.

If uncertain about a detection, choose the safer, more generic docker-compose-based commands and note the assumption.
