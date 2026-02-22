# Agent World Model (AWM) — IT Support Agent Project

## Project Location
- Repo: `C:\AI Projects\agent-world-model`
- GitHub: sh-xai/agent-world-model (fork of Snowflake-Labs/agent-world-model)
- Branch: main
- Python 3.12, uses `uv` for dependency management
- CLI tool: `awm` (needs `PYTHONIOENCODING=utf-8` on Windows)

## What AWM Is
A fully synthetic environment generation pipeline for training tool-use agents via RL.
Each environment = FastAPI + MCP server backed by SQLite.
Paper: arxiv.org/abs/2602.10090

## Setup Status (completed)
- Cloned and `uv sync` done — 98 packages installed, .venv in project dir
- Downloaded all 1,000 pre-built environments from HuggingFace (~495 MB) to `outputs/`
- Files: gen_scenario.jsonl, gen_tasks.jsonl, gen_db.jsonl, gen_sample.jsonl, gen_spec.jsonl, gen_envs.jsonl, gen_verifier.jsonl, gen_verifier.pure_code.jsonl
- HuggingFace download command: `huggingface-cli download Snowflake/Agent-World-Model --local-dir outputs/`

## Bugs Fixed (committed & pushed to origin/main)
- `awm/tools.py:337` — Added `encoding='utf-8'` to `tools_jsonl_load` (Windows cp1252 crash)
- `awm/core/server.py:80` — Quoted paths in `os.system()` call (spaces in Windows paths)

## Paper Key Details

### Synthesis Pipeline (7 stages)
1. Scenarios — Self-instruct expansion from 100 seeds → 1,000 unique domains
2. Tasks — 10 tasks per scenario = 10,000 total
3. Database — SQLite schema + sample data to satisfy task preconditions
4. Interface — ~2,000 lines Python per env (SQLAlchemy + Pydantic + FastAPI + MCP), avg 35 tools/env
5. Verification — Code-augmented LLM-as-a-Judge (compares DB state before/after)

### Models Used
- **GPT-5 for ALL synthesis stages** (scenario, task, db, sample, spec, env code, verification)
- **GPT-5 also used as LLM-as-a-Judge** during RL training (~$1.80 per training step for 1,024 samples)
- Claude 4.5 Sonnet + GPT-5.1 used only as independent judges for quality evaluation (Table 5)
- Agent models: Qwen3 thinking models (4B, 8B, 14B)

### CRITICAL: Costs (per 100 environments, NOT per environment!)
| Stage | Cost/100 envs |
|-------|--------------|
| Scenario | $0.43 |
| Task | $0.56 |
| Database | $3.59 |
| Sample Data | $13.75 |
| Toolset Schema | $23.74 |
| Env Code | $12.81 |
| Verification | $2.21 |
| **Total** | **$57.09/100 envs = $0.57/env with GPT-5** |

### Training Details (GRPO)
- Group Relative Policy Optimization on Qwen3 models
- Reward: 1.0 completed, 0.1 partial, 0.0 fail, -1.0 format violation
- 1,024 parallel isolated MCP instances per training step
- History-aligned truncation (window=3) critical for generalization
- Trained on 526/1000 envs, 3,315/10,000 tasks
- LR: 7e-7, batch: 64, rollouts: 16, max steps: 96, KL: 0.001, clip: 0.28
- 10,010 pure-code verifiers exist (97% need NO LLM judge) — verification is essentially free

### Resource Per Environment
- RAM: ~108 MB per running environment
- SQLite DB: ~304 KB
- Server code: ~83 KB (generated temp file)

## Hardware
- Current machine: 64 GB RAM, 12 logical cores, 2x GTX 1080 Ti (11 GB VRAM each) — NOT enough for training
- **GMKtec EVO-X2**: Ryzen AI Max+ 395, 128 GB LPDDR5X unified memory (up to 96 GB as VRAM), Radeon 8060S (gfx1151, RDNA 3.5), ~50 tps for Qwen3-30B
  - ROCm support early (gfx1151), rocBLAS 2.5-6x slower than gfx1100, needs nightly builds
  - Can do full local training in ~3-8 hours with optimizations (reduced rollouts, shorter max turns)
  - Alternative: A100 spot cloud ~$0.75-1.00/step, full run $110-250

## IT Support Agent — Goal
Saj wants to train an agent on full-spectrum IT support:
- Helpdesk / ticketing (password resets, account provisioning, ticket routing, SLA, escalation)
- Infrastructure / ops (server management, monitoring, incident response, network config, deployment)
- Internal tooling (SaaS admin panels, user permissions, license management, onboarding/offboarding)

### Existing IT Environments in Pre-built Dataset (~22 total)
**ITSM (6):** it_service_management_1 (DeskQueue), it_service_management_2 (ServiceNow ITSM), it_service_management_3 (IssueTrackr), it_service_management_itsm_1 (FieldFix), it_asset_management_1 (DeviceNest), incident_management_1 (PagerDuty)
**IAM (4):** authentication_and_identity_management_1 (Okta), authentication_identity_management_1, identity_and_access_management_1, user_accounts_and_identity_1
**DevOps (2):** code_hosting_1 (GitLab), code_hosting_2
**Helpdesk/Ticketing (10):** customer_support_helpdesk_1, ticketing_1, ticketing_and_access_management_1, ticketing_helpdesk_1, ticketing_helpdesk_system_1, ticketing_system_1-4

### 20 New Environments Designed (plan approved, ready to synthesize)
**A: ITIL Service Management (4):** service_catalog, cmdb, change_management_cab, release_management
**B: Infrastructure & Cloud (4):** server_vm_management, cloud_infrastructure, network_device_configuration, dns_dhcp_ipam
**C: Security & Compliance (3):** firewall_security_policy, certificate_secret_management, patch_vulnerability_management
**D: Observability & Operations (3):** observability_platform, log_management_siem, backup_disaster_recovery
**E: Endpoint & Device (2):** endpoint_management_mdm, license_saas_management
**F: IT Lifecycle & Knowledge (3):** user_provisioning_lifecycle, knowledge_base_runbook, database_administration
**+1:** container_orchestration

Full design details in plan file: `C:\Users\sajaw\.claude\plans\imperative-giggling-bonbon.md`

## Walkthrough Completed
- Successfully ran DeskQueue (it_service_management_1) end-to-end:
  - Reset DB (18 tables, 80 sample records) → started server on port 8001 → queried 23 endpoints
  - Manually executed Task 3 (update ticket, change state) via PATCH (not PUT)
  - Examined verification system (pure Python, compares DB state before/after)
  - Read full agent module (awm/core/agent.py): 2 meta-tools, 1 tool/turn, XML tool calling, max 30 iterations

## Next Steps
1. **Clone repo on EVO-X2** and re-download HuggingFace outputs
2. **Approve plan** for 20 new IT environments
3. **Synthesize environments** using AWM pipeline (~$11.40 via GPT-5)
4. **Set up EVO-X2** for inference/training (ROCm, vLLM/llama.cpp, PyTorch)
5. **Train IT support agent** using GRPO on Qwen3-8B
