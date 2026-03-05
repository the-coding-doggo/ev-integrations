# REST API Use Cases (Within EasyVista Platform)

REST APIs within EV are for **enhancing** the platform, not escaping it. Here's what you can build natively.

---

## 1. EV Service Apps: Interactive Widgets

### REST API Buttons (User-Facing Actions)
Add buttons in apps that call REST APIs without leaving EV.

**What You Can Build**:
| Use Case | API Target | User Action |
|----------|------------|-------------|
| Quick ticket creation | Service Manager | Click → Create incident with pre-filled data |
| Asset reservation | Service Manager | Click → Reserve laptop, update asset status |
| Meeting booking | Microsoft 365/Exchange | Click → Book room from ticket view |
| Profile updates | Service Manager | Click → Update phone/location |
| External notifications | Slack/Teams Webhook | Click → Notify team of critical issue |

**Example**: Hardware request form with "Reserve Asset" button that:
1. Calls Service Manager REST API to find available laptop
2. Updates asset status to "Reserved"
3. Links asset to ticket
4. All without leaving the app

**Implementation**: HTML Code widget with REST API button tags

---

## 2. EV Orchestrate: Backend Automation

### HTTP Request Block (Workflow Automation)
Automate operations by calling REST APIs within Orchestrate workflows.

**What You Can Build**:

| Use Case | API Target | Trigger |
|----------|------------|---------|
| Intune device compliance | Microsoft Graph | Ticket created → Check device status → Auto-approve if compliant |
| Azure AD user provisioning | Microsoft Graph | New employee ticket → Create AD account → Update ticket with credentials |
| M365 license assignment | Microsoft Graph | Role change → Assign/remove licenses |
| DevOps integration | Azure DevOps API | Change request approved → Trigger deployment pipeline |
| Cross-system orchestration | Multiple APIs | Workflow chains REST calls across systems |
| Service Manager automation | EV REST API | Scheduled job → Update all stale tickets → Send reports |

**Example Workflow**: New hire onboarding
```
Trigger: Ticket created (Category: Onboarding)
├─ Step 1: HTTP GET /users (Azure AD) - Check if user exists
├─ Step 2: HTTP POST /users (Azure AD) - Create account if not exists
├─ Step 3: HTTP POST /requests (Service Manager) - Create sub-task for IT setup
├─ Step 4: HTTP PATCH /requests/{id} (Service Manager) - Update main ticket
└─ Step 5: Send notification to manager
```

**Deployment Options**:
- API Gateway: Expose as REST endpoint
- Scheduled Jobs: Run nightly/weekly
- Event-Driven: Trigger from ticket changes

---

## 3. EV Self Help / Service Bots: Knowledge-Driven Actions

### HTTP Connector (Procedure-Based REST Calls)
Self Help procedures can call REST APIs to interact with Service Manager.

**What You Can Build**:

| Use Case | API Call | When |
|----------|----------|------|
| Auto-create incident | POST /requests | User completes troubleshooting → Still broken → Create ticket automatically |
| Asset lookup by user | GET /assets?employee_id={id} | User asks "What equipment do I have?" |
| Ticket status check | GET /requests/{id} | User asks "What's the status of my request?" |
| Knowledge base sync | POST /knownerrors | Self Help procedure resolves issue → Update known errors |
| CI updates | PUT /configuration-items/{id} | Hardware replacement procedure → Update CI details |
| Profile updates | PUT /employees/{id} | Self-service profile change → Push to Service Manager |

**Example Self Help Flow**:
```
User: "I can't access VPN"
├─ Self Help: Run diagnostic procedure
├─ Step 1: Check common issues (no API)
├─ Step 2: If unresolved, HTTP GET /employees?email=user@company.com
├─ Step 3: HTTP POST /requests (create incident with user data)
└─ User gets: "Ticket INC12345 created. IT will contact you."
```

**Service Bots (Teams Integration)**:
- Virtual agent in Microsoft Teams can call these procedures
- Natural language → Self Help procedure → REST API → Service Manager

---

## 4. EV-to-EV Internal API Usage

### Cross-Module Data Flow
Use REST APIs to connect EV modules internally.

| From | To | Use Case |
|------|-----|----------|
| **Orchestrate** | Service Manager | Workflow creates/updates tickets, assets, employees |
| **Self Help** | Service Manager | Procedures create tickets, search assets |
| **Observe** | Service Manager | Monitoring event → Auto-create incident ticket |
| **Discovery** | Service Manager | Discovered assets → Import to CMDB |
| **Service Apps** | Service Manager | Dashboard widgets create/update records |
| **Service Manager** | Orchestrate | Ticket triggers workflow via REST |

**Example - Observe → Service Manager**:
```
Monitor detects server down (Observe)
├─ REST API: POST /requests (Service Manager)
│  ├─ Category: Incident
│  ├─ Priority: Critical
│  ├─ CI: Server-01
│  └─ Description: "Auto-created from Observe alert"
└─ Ticket created, team notified
```

---

## 5. Integration Models: Scheduled Data Sync

### REST Data Source (2024.2+)
Import external data into Service Manager on schedule.

**What You Can Sync**:

| Source | Data | Frequency |
|--------|------|-----------|
| Microsoft Graph (Intune) | Device inventory, compliance status | Daily |
| Microsoft Graph (M365) | User licenses, groups, calendar | Hourly |
| Azure DevOps | Work items, build status | Real-time webhook or scheduled |
| External CMDB | Configuration items | Daily |
| Custom REST API | Any JSON data | Configurable |

**Example - Intune Device Sync**:
```
Schedule: Daily at 2 AM
Source: Microsoft Graph API /deviceManagement/managedDevices
├─ GET devices from Intune
├─ Transform JSON to Service Manager format
├─ Import to Equipment table
└─ Update existing records, create new ones
```

**Features**:
- Selector tool to extract specific JSON fields
- Pagination support (2025.3+) for large datasets
- Data type enforcement (force int, float, etc.)

---

## 6. Power Automate Integration (Bridge to Microsoft Ecosystem)

### Data Feeds Between EV and M365

**Available Connectors**:
| Source | Target | Use Case |
|--------|--------|----------|
| Service Manager | Azure DevOps | Bug work items sync |
| Service Manager | GitHub | Issue synchronization |
| Service Manager | Jenkins | CI/CD job status |
| Service Manager | JIRA | Ticket bidirectional sync |
| Azure DevOps | Service Manager | Deployment status → Ticket update |

**Implementation**: Power Automate flow calls EV REST API and Microsoft APIs.

---

## Quick Reference: What REST API Enables

### Without Leaving EV Platform

| Capability | Where Used | Example |
|------------|------------|---------|
| **Create tickets** | Service Apps buttons, Self Help, Orchestrate | User clicks "Report Issue" → Ticket created |
| **Update records** | Orchestrate workflows, REST buttons | Auto-assign ticket based on CI location |
| **Search data** | Service Apps widgets, Self Help | "Find all laptops in Building A" |
| **Cross-module sync** | Orchestrate, Observe | Monitoring alert → Auto-ticket |
| **External system sync** | Orchestrate, Integration Models | Intune devices → CMDB daily |
| **User self-service** | Service Apps, Self Help | Update profile, check ticket status |
| **Automation** | Orchestrate | New user → Create AD → Create ticket → Notify |

---

## What You CANNOT Do with REST API (Internal)

- ❌ Modify EV core UI/UX (use Service Apps for that)
- ❌ Create new EV modules
- ❌ Access EV database directly (must use API)
- ❌ Bypass EV authentication/permissions
- ❌ Real-time bidirectional sync (polling or webhooks only)

---

## Implementation Path

### Phase 1: Service Apps REST Buttons
- Quick wins in existing apps
- No workflow complexity
- User-facing actions

### Phase 2: Self Help Integration
- Knowledge-driven automation
- Virtual agent capabilities
- Ticket creation from procedures

### Phase 3: Orchestrate Workflows
- Complex automation chains
- Scheduled operations
- Cross-system orchestration

### Phase 4: Integration Models
- Scheduled data imports
- CMDB synchronization
- External system feeds

---

## Documentation Links

- [REST API Reference](https://docs.easyvista.com/v1/docs/webservice-rest/)
- [Orchestrate HTTP Request](https://docs.easyvista.com/v1/docs/workflow-http-request/)
- [REST API Buttons](https://docs.easyvista.com/v1/docs/apps-rest-api-buttons/)
- [Self Help Connectors](https://docs.easyvista.com/v1/docs/self-help-create-connector/)
- [Integration Models](https://docs.easyvista.com/v1/docs/integration-model/)
