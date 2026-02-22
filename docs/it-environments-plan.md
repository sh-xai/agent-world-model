# Plan: Design & Synthesize 20 IT Support Environments for AWM

## Context

We're building a specialist IT support agent using the AWM (Agent World Model) framework. The pre-built dataset has ~22 IT-adjacent environments covering helpdesk/ticketing (10), ITSM (6), IAM (4), and DevOps (2). But there are major gaps in: infrastructure management, network operations, security, observability, deployment, and IT lifecycle management.

We need 20 new environments to fill those gaps, giving us ~30 total IT environments × 10 tasks each = 300 IT-specific training tasks. Research (web search + Context7 queries against ServiceNow, Ansible, Kubernetes, Grafana, Intune docs) confirmed these 20 domains as the right coverage.

**Synthesis cost**: ~$11.40 total (20 × $0.57/env using GPT-5 via the AWM pipeline)

---

## The 20 New IT Environments

### Group A: ITIL Service Management (4 environments)

**1. service_catalog_request_fulfillment_1**
Employees browse an IT service catalog and submit requests with approval workflows.
- *Domain*: Service request lifecycle distinct from incident management
- *Key tables*: ServiceCategory, CatalogItem, ServiceRequest, ApprovalStep, Approver, FulfillmentTask, RequestItem, SlaTarget
- *Sample tasks*: Submit laptop request, approve/deny requests, track fulfillment SLA, update catalog item pricing, reassign fulfillment task, create bundle offer, check request backlog by department

**2. cmdb_configuration_management_1**
Track configuration items (CIs) and their relationships — which servers run which apps, network dependencies, impact analysis.
- *Domain*: CMDB / configuration management (ITIL core)
- *Key tables*: ConfigurationItem, CiType, CiRelationship, RelationshipType, CiAttribute, ChangeHistory, ImpactAssessment, BusinessService
- *Sample tasks*: Register a new server CI, map app-to-server dependency, run impact analysis for a CI, update CI lifecycle status, query all CIs affected by a network segment, merge duplicate CIs, generate dependency graph for a business service

**3. change_management_cab_1**
Change requests go through CAB (Change Advisory Board) approval, risk assessment, scheduling, and rollback planning.
- *Domain*: ITIL change management
- *Key tables*: ChangeRequest, ChangeType, RiskLevel, CabMember, CabVote, MaintenanceWindow, RollbackPlan, ChangeTask, ChangeCollision
- *Sample tasks*: Submit an emergency change, schedule CAB review, record approval votes, detect scheduling conflicts, attach rollback plan, link change to affected CIs, close change post-implementation, escalate rejected change

**4. release_management_1**
Plan, schedule, and coordinate software releases across environments (dev → staging → prod).
- *Domain*: Release lifecycle and deployment coordination
- *Key tables*: Release, ReleasePhase, Environment, DeploymentStep, Artifact, ReleaseNote, RollbackRecord, ApprovalGate, TestResult
- *Sample tasks*: Create release plan, promote release from staging to prod, record gate approval, attach release notes, trigger rollback, link release to change request, query deployment history for a service, mark release as complete

### Group B: Infrastructure & Cloud (4 environments)

**5. server_vm_management_1**
Provision VMs, manage snapshots, restart services, check resource utilization, scale instances.
- *Domain*: Server/VM lifecycle management
- *Key tables*: Server, VirtualMachine, Hypervisor, Snapshot, ResourceMetric, Service, MaintenanceSchedule, ProvisioningRequest
- *Sample tasks*: Provision a new VM, take a snapshot before patching, restart a hung service, check CPU/memory utilization, schedule maintenance downtime, decommission an old server, resize a VM, list all VMs by host

**6. cloud_infrastructure_1**
AWS/Azure-style cloud resource management — instances, security groups, storage, networking.
- *Domain*: Cloud infrastructure (IaaS)
- *Key tables*: Instance, SecurityGroup, SecurityRule, StorageBucket, VirtualNetwork, Subnet, LoadBalancer, IamPolicy, IamRole, CostRecord
- *Sample tasks*: Launch an instance in a subnet, create a security group rule, attach a load balancer, create a storage bucket with retention policy, assign an IAM role, query monthly cost by service, stop all non-production instances, resize an instance type

**7. network_device_configuration_1**
Switch/router configuration management — VLANs, ACLs, interfaces, wireless APs.
- *Domain*: Network operations (Cisco Meraki/DNAC-style)
- *Key tables*: NetworkDevice, DeviceType, Interface, Vlan, VlanAssignment, AccessControlList, AclRule, WirelessAp, FirmwareVersion, ConfigBackup
- *Sample tasks*: Create a VLAN and assign ports, add an ACL rule to block a subnet, update firmware on a switch, backup device config, enable a disabled interface, query all devices with outdated firmware, assign an AP to a site, view interface error counters

**8. dns_dhcp_ipam_1**
DNS record management, DHCP scopes, and IP address allocation/tracking.
- *Domain*: DNS/DHCP/IPAM
- *Key tables*: DnsZone, DnsRecord, DhcpScope, DhcpLease, IpSubnet, IpAllocation, DnsFailoverGroup, ReverseLookup
- *Sample tasks*: Create an A record, allocate a static IP from a subnet, create a DHCP scope, query active leases, set up a CNAME alias, delete an expired reservation, check IP utilization percentage for a subnet, create a reverse DNS entry

### Group C: Security & Compliance (3 environments)

**9. firewall_security_policy_1**
Firewall rule management, security zone policies, network access control.
- *Domain*: Network security policy management
- *Key tables*: Firewall, SecurityZone, FirewallRule, RuleGroup, NatRule, TrafficLog, PolicyChange, ComplianceCheck
- *Sample tasks*: Add an allow rule for HTTPS traffic, block an IP range, create a NAT rule, audit all rules permitting any-to-any, move a rule within priority order, check compliance against baseline, review recent policy changes, disable a deprecated rule

**10. certificate_secret_management_1**
SSL/TLS certificate lifecycle and Vault-style secret storage with rotation policies.
- *Domain*: PKI and secrets management
- *Key tables*: Certificate, CertificateAuthority, SecretStore, Secret, SecretVersion, RotationPolicy, AccessGrant, ExpiryAlert
- *Sample tasks*: Issue a new SSL certificate, set up auto-rotation for a database password, revoke a compromised certificate, query all certificates expiring within 30 days, grant secret access to a service account, rotate an API key, create a new secret store, view audit log for a secret

**11. patch_vulnerability_management_1**
OS/application patch inventory, compliance scanning, CVE tracking, rollout scheduling.
- *Domain*: Vulnerability and patch management
- *Key tables*: PatchBulletin, PatchStatus, Vulnerability, CveEntry, ComplianceScan, ScanResult, RolloutSchedule, RolloutGroup, ExceptionRequest
- *Sample tasks*: Import a patch bulletin, run a compliance scan, schedule a patch rollout to a server group, create an exception for a critical system, query all servers missing a critical patch, link a CVE to affected assets, approve a rollout, review scan results by severity

### Group D: Observability & Operations (3 environments)

**12. observability_platform_1**
Grafana-style dashboard management, alert rules, notification channels, SLO tracking.
- *Domain*: Monitoring and observability
- *Key tables*: Dashboard, Panel, DataSource, AlertRule, AlertInstance, NotificationChannel, Silence, Annotation, SloDefinition, SloRecord
- *Sample tasks*: Create a dashboard with CPU/memory panels, configure an alert rule for high error rate, add a Slack notification channel, silence alerts during maintenance, create an SLO target, query active firing alerts, annotate a dashboard for an incident, update a data source connection

**13. log_management_siem_1**
Log search, alert correlation, incident timeline reconstruction, retention policies.
- *Domain*: SIEM / log management
- *Key tables*: LogSource, LogEntry, SavedQuery, AlertCorrelation, CorrelationRule, RetentionPolicy, IncidentTimeline, TimelineEvent, IndexConfig
- *Sample tasks*: Create a saved search query, set up a correlation rule for brute-force detection, reconstruct an incident timeline, update log retention to 90 days, query logs by source and severity, create a new log source integration, export timeline to incident report, check index storage utilization

**14. backup_disaster_recovery_1**
Backup schedules, restore jobs, retention policies, DR plan execution.
- *Domain*: Business continuity / DR
- *Key tables*: BackupPolicy, BackupJob, BackupTarget, RestoreJob, RetentionRule, DrPlan, DrTest, DrTestResult, StorageQuota
- *Sample tasks*: Create a daily backup policy for a database, trigger an on-demand backup, restore from a specific backup point, run a DR test, update retention from 30 to 90 days, check storage quota utilization, verify last backup status for all critical systems, schedule a DR drill

### Group E: Endpoint & Device Management (2 environments)

**15. endpoint_management_mdm_1**
Intune-style device compliance, configuration policies, remote actions, software deployment.
- *Domain*: MDM / endpoint management
- *Key tables*: Device, DeviceGroup, CompliancePolicy, ComplianceStatus, ConfigurationProfile, RemoteAction, AppPackage, AppAssignment, SoftwareUpdateRing
- *Sample tasks*: Enroll a new device, create a compliance policy requiring encryption, push a configuration profile, trigger a remote wipe, deploy an app to a device group, check compliance status across all devices, create an update ring for Windows patches, quarantine a non-compliant device

**16. license_saas_management_1**
Software license inventory, usage tracking, cost optimization, renewal management.
- *Domain*: IT asset management (software)
- *Key tables*: SoftwareProduct, License, LicenseType, SeatAssignment, UsageRecord, RenewalAlert, CostCenter, ComplianceReport, Vendor
- *Sample tasks*: Add a new software license, assign seats to users, check license utilization rate, create a renewal alert, reassign seats from departed employees, generate a compliance report, query cost by department, flag unused licenses for cost optimization

### Group F: IT Lifecycle & Knowledge (3 environments)

**17. user_provisioning_lifecycle_1**
Employee onboarding/offboarding — account creation, group assignment, device provisioning, access revocation.
- *Domain*: Identity lifecycle management
- *Key tables*: Employee, OnboardingWorkflow, WorkflowStep, Account, GroupMembership, DeviceAssignment, OffboardingChecklist, AccessReview, DataTransfer
- *Sample tasks*: Initiate onboarding for a new hire (create accounts, assign groups, provision laptop), trigger offboarding (revoke access, wipe device, transfer data), run quarterly access review, reassign assets from a departing employee, check onboarding completion status, update group membership for a role change, audit stale accounts

**18. knowledge_base_runbook_1**
IT knowledge articles, runbooks, review workflows, incident linking, usage analytics.
- *Domain*: Knowledge management for IT ops
- *Key tables*: Article, ArticleCategory, ArticleVersion, ReviewRequest, Reviewer, Runbook, RunbookStep, ArticleIncidentLink, UsageMetric
- *Sample tasks*: Create a knowledge article for password reset, submit article for peer review, link a runbook to an incident type, update a runbook step, query most-viewed articles, retire an outdated article, create a troubleshooting guide with decision tree, check review backlog

**19. database_administration_1**
Database user/role management, backup/restore, slow query analysis, replication monitoring.
- *Domain*: DBA operations
- *Key tables*: DatabaseInstance, DatabaseUser, Role, RoleGrant, BackupRecord, RestoreRecord, SlowQueryLog, ReplicationStatus, ConnectionPool, MaintenanceTask
- *Sample tasks*: Create a database user with read-only role, run a backup, identify top 10 slow queries, check replication lag, resize connection pool, schedule a maintenance task, restore a specific table from backup, revoke access for a departing DBA

**20. container_orchestration_1**
Kubernetes-style namespace management, RBAC, deployments, pod troubleshooting, resource quotas.
- *Domain*: Container platform operations
- *Key tables*: Cluster, Namespace, Deployment, Pod, Service, Ingress, ResourceQuota, RbacRole, RbacBinding, ConfigMap, PersistentVolume, HorizontalPodAutoscaler
- *Sample tasks*: Create a namespace with resource quotas, deploy an application, scale a deployment, troubleshoot a CrashLoopBackOff pod, create an RBAC role for a team, expose a service via ingress, update a ConfigMap, check cluster resource utilization

---

## Synthesis Approach

Use the AWM pipeline to generate these environments. Two options:

### Option A: Full Pipeline (Recommended)
Run `awm gen all` for each environment using the scenario descriptions above as seed inputs. This generates all 7 stages (scenario → tasks → DB → sample data → spec → env code → verifiers) automatically via GPT-5.

**Steps:**
1. Create a seed file with the 20 scenario descriptions in the format matching `gen_scenario.jsonl`
2. Run pipeline stage by stage: `awm gen scenario`, `awm gen task`, `awm gen db`, `awm gen sample`, `awm gen spec`, `awm gen env`, `awm gen verifier`
3. After each stage, spot-check outputs for quality
4. Reset databases: `awm env reset_db`
5. Start each server and run `awm env check` to verify endpoints work

### Option B: Manual JSONL Creation
Write the JSONL entries directly (scenario, tasks, DB schema, sample data, API spec, FastAPI code, verifiers) by hand. Much more work but gives full control.

**Recommendation**: Option A. The pipeline is designed for exactly this. We provide rich scenario descriptions as seeds and let GPT-5 generate the details. We review and iterate.

---

## Files to Modify/Create

- `C:\AI Projects\agent-world-model\outputs\it_scenarios_seed.jsonl` — NEW: 20 seed scenario descriptions
- `C:\AI Projects\agent-world-model\outputs\` — Pipeline will generate new JSONL files for IT environments
- `C:\AI Projects\agent-world-model\awm\tools.py` — Already fixed (encoding bug)
- `C:\AI Projects\agent-world-model\awm\core\server.py` — Already fixed (path quoting bug)

---

## Verification

1. **Per-environment smoke test**: Start each server (`awm env start`), hit all endpoints, verify 200 responses
2. **Database integrity**: Reset DB, verify all tables created, sample data inserted without errors
3. **Task execution**: Manually walk through 1-2 tasks per environment (like we did with DeskQueue)
4. **Verifier check**: Run verifier against each task to confirm pass/fail detection works
5. **MCP connectivity**: `awm env check` confirms MCP tools are accessible

---

## Prerequisites Before Starting

1. **OpenAI API key**: Needed for GPT-5 calls in the synthesis pipeline (~$11.40 total)
2. **Git commit**: The two bug fixes (tools.py encoding, server.py path quoting) need to be committed first
3. **Git identity**: Need Saj's name/email configured for commits
