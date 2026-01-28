# Modifying Existing Services — Defensive Notes (Public-Safe)

## TL;DR
Attackers can establish persistence by **modifying an existing service** (changing its binary path, start mode, or run-as account). Defensively, focus on **service configuration change auditing** and **unexpected privilege context shifts**.

## Why it matters
Modifying an existing service can be stealthier than creating a new one:
- The service name already exists (less suspicious at a glance)
- Changes can be buried among legitimate admin activity

## What changes (high level)
Key fields defenders should care about:
- `BINARY_PATH_NAME` (ImagePath / binPath): should not suddenly point to unusual locations
- `START_TYPE`: changing to auto-start increases persistence
- `SERVICE_START_NAME` (run-as account): switching to LocalSystem increases privilege risk

## Telemetry / What to monitor
- Service config change events (SCM / auditing)
- Process creation from `services.exe` after config changes
- File creation/modification of the new service binary path target
- Registry changes under Services keys (if you have that telemetry)

## SOC L1 triage checklist (5–10 min)
1. Identify the changed service and diff the config:
   - Before/after ImagePath (binary path)
   - Before/after start type
   - Before/after run-as account
2. Validate legitimacy:
   - Approved maintenance window? Change ticket?
   - Known admin account/tooling?
3. Follow-on activity:
   - Did the service start? Any unusual child processes?
   - Any suspicious outbound connections soon after?

## Detection ideas (practical)
- Alert on config changes where:
  - ImagePath changes to user-writable locations
  - Start type flips to auto-start unexpectedly
  - Run-as account changes to high-privilege context
- Correlate:
  - Service config change → service restart → new network destination

## Hardening / Prevention
- Limit service modification rights
- Use controlled admin workflows (PAM/JIT, approvals)
- Application control for service binaries
- Baseline critical services and alert on drift (config drift detection)
