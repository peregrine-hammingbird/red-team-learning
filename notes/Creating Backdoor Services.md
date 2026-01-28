# Creating Backdoor Services — Defensive Notes (Public-Safe)

## TL;DR
Windows Services are a common persistence mechanism because they can be configured to start automatically and run in the background. From a defensive/SOC perspective, the key is correlating **service creation/config changes** with **process execution** and **suspicious outbound activity**.

## Why it matters
Abusing services can provide:
- Persistence (auto-start on boot)
- Privilege (services often run under high-privilege accounts)
- Stealth (looks like legitimate administration)

## What changes (high level)
- A new service is created, pointing to a specific executable (ImagePath / binPath).
- The service is set to auto-start (persistence).
- The service is started (execution).

## Telemetry / What to monitor
### Host signals
- Service installation/creation events (Service Control Manager)
- Service start/stop events (and failures)
- Process creation where the parent chain includes `services.exe`
- New or unusual binaries placed in system/user-writable locations, then referenced by a service

### Network signals (if applicable)
- Outbound connections initiated by a service-launched process
- New destinations (rare domains/IPs), unusual ports, or beacon-like patterns

## SOC L1 triage checklist (5–10 min)
1. What service was created? (name, display name)
2. What binary/path does it point to? (ImagePath)
3. Who created it? When? Is there a change ticket?
4. Which account does it run as? (LocalSystem vs. low-priv user)
5. Did it start successfully? If yes, what child process/network activity followed?

## Detection ideas (practical)
- Alert on **new service creation** outside approved tooling/time windows
- Alert when a service points to:
  - User profile paths, temp directories, downloads
  - Recently created/unsigned binaries
- Correlate: service creation → service start → outbound connection within N minutes

## Hardening / Prevention
- Restrict who can create/modify services (least privilege)
- Enable detailed logging (service events + process creation + PowerShell logging if relevant)
- Application control (WDAC/AppLocker) to prevent unknown binaries from executing as services
- Monitor/alert on service binary paths and privilege context changes
