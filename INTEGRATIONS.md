# Integration Implementation Guide

## Phase 1: Foundation (Recommended Start)

### 1. Active Directory Integration

**Why first**: Native connector available, foundational for user management.

**Integration Methods**:
- LDAP Authentication (SSO)
- Integration Connectors (Employee/Equipment sync)
- SSO (SAML v2, ADFS)

**Key Use Cases**:
- Centralized user authentication
- Automatic employee directory sync
- Equipment/workstation import
- Department/location hierarchy

**Data Mapping**:
| AD Attribute | EV Field |
|--------------|----------|
| sAMAccountName | Login |
| displayName | Full Name |
| mail | Email |
| telephoneNumber | Phone |
| manager | Manager: Login |
| Department | Entity |

**Documentation**:
- [LDAP Authentication](https://docs.easyvista.com/v1/docs/ldap)
- [Active Directory Integration](https://docs.easyvista.com/v1/docs/microsoft-active-directory)

---

### 2. Microsoft Teams Integration

**Why second**: Native app available, immediate user value.

**Integration Methods**:
- EasyVista App for Teams (native, billable add-on)
- Azure Bot Service (virtual agent)
- Microsoft Graph API (custom)

**Key Use Cases**:
- Virtual agent for IT support
- Create tickets from chat
- Ticket status monitoring
- Knowledge base search
- Real-time notifications
- In-Teams approvals

**Documentation**:
- [EasyVista App for Teams](https://docs.easyvista.com/v1/docs/easyvista-application-for-microsoft-teams)
- [Teams Integration Guide](https://docs.easyvista.com/v1/docs/microsoft-teams-integration)

---

### 3. Exchange/Outlook Integration

**Why third**: Email still primary intake channel.

**Integration Methods**:
- Technical Support Agent (TSA) - Email-to-ticket
- Microsoft Graph API (modern auth)
- Microsoft Power Automate

**Key Use Cases**:
- Email-to-ticket auto-conversion
- Approval workflows
- Shared mailbox processing
- Calendar integration

**Authentication**:
- Microsoft Graph (recommended)
- Office 365 OAuth2
- POP3/IMAP4 (deprecated)

**Documentation**:
- [Technical Support Agent](https://docs.easyvista.com/v1/docs/technical-support-agent)
- [Azure Integration](https://docs.easyvista.com/v1/docs/microsoft-azure-integration-create-an-app-with-api-permissions)

---

## Phase 2: Asset Management

### 4. Microsoft Intune Integration

**Status**: No native connector. Custom development required.

**Integration Method**:
- Microsoft Graph API (custom REST)
- Azure Logic Apps bridge
- Power Automate workflows

**Key Use Cases**:
- Device inventory sync
- Compliance status monitoring
- Application deployment tracking
- Policy management
- Remote device actions

**Required APIs**:
- `/deviceManagement/managedDevices`
- `/deviceManagement/deviceCompliancePolicySettingStates`
- `/deviceAppManagement/mobileApps`

**Prerequisites**:
- Entra ID application with Graph permissions
- REST service configuration in Easy Connect

**Difficulty**: Hard (requires custom development)

**Documentation**:
- [Microsoft Graph Integration](https://docs.easyvista.com/v1/docs/microsoft-graph-integration)
- [Azure App Setup](https://docs.easyvista.com/v1/docs/microsoft-azure-integration-create-an-app-with-api-permissions)

---

## Phase 3: Call Center

### 5. Genesys Integration

**Status**: No native connector. Custom development required.

**Integration Method**:
- Genesys Cloud REST API (custom)
- Webhook event subscription
- Power Automate bridge

**Key Use Cases**:
- Click-to-call from tickets
- Screen pop on incoming calls
- Call-to-ticket logging
- IVR self-service integration
- Call recording links
- Real-time queue analytics

**Required APIs**:
- `/api/v2/conversations` (call data)
- `/api/v2/users` (agent sync)
- `/api/v2/queues` (queue status)

**Difficulty**: Very Hard (significant API development)

**Note**: Consider engaging EasyVista professional services for this integration.

**Documentation**:
- [REST Configuration](https://docs.easyvista.com/v1/docs/rest-set-workflow-action-types)
- [Workflow Automation](https://docs.easyvista.com/v1/docs/workflow)

---

## Quick Reference

| System | Connector | Module | Effort | Native? |
|--------|-----------|--------|--------|---------|
| Active Directory | LDAP/SSO/Connectors | Service Manager | Medium | Yes |
| Microsoft Teams | Native App/Bot | Self Help | Easy | Yes |
| Exchange/Outlook | TSA/Graph API | Service Manager | Medium | Yes |
| Microsoft Intune | Graph API (custom) | Orchestrate | Hard | No |
| Genesys | REST API (custom) | Orchestrate | Very Hard | No |
| SharePoint | IFrame | Service Apps | Easy | Yes |
| **PowerBI** | **REST API** | **Analytics** | **Medium** | **No** |
| **Custom Frontend** | **REST API** | **External** | **High** | **No** |

## Frontend & Reporting

See [FRONTEND_REPORTING.md](./FRONTEND_REPORTING.md) for dashboard and visualization options.

### Quick Summary
- **Service Apps**: Codeless dashboards (recommended start)
- **PowerBI**: REST API integration for analytics
- **Custom JS**: Full REST API access for portals

## Implementation Notes

### Native Connectors (Easy)
- Configure via Easy Connect module
- Pre-built data mappings
- Admin-level configuration only

### Custom API Integrations (Hard)
- Require REST service setup in Easy Connect
- Need authentication configuration (OAuth2)
- Custom workflow development in Orchestrate

### Recommended Approach
1. Start with Phase 1 (AD, Teams, Exchange) for immediate value
2. Build Service Apps dashboards for ticket visibility
3. Add PowerBI for executive reporting
4. Document user requirements and workflows
5. Build Intune integration using Graph API patterns
6. Evaluate Genesys need vs. effort (may use Power Automate bridge)
