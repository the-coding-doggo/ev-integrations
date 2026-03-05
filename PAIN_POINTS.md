# Pain Points & Solutions

Common EV configuration issues and their fixes.

---

## Pain Point 1: No Supervisor Dashboard

### Problem
Supervisor cannot easily see:
- Who has most tickets closed
- Team performance statistics
- Ticket volume trends
- SLA compliance by technician

### Root Cause
EV ships with default views but no management dashboards.

### Solutions (Pick One)

#### Option A: Service Apps Dashboard (Recommended - 1 day)
Build codeless supervisor dashboard.

**Widgets to Add**:
| Widget | Data Source | Shows |
|--------|-------------|-------|
| KPI | Service Manager query | Tickets closed today/this week |
| Bar Chart | Service Manager query | Tickets by technician |
| Pie Chart | Service Manager query | Tickets by status/priority |
| Data Viewer | Service Manager query | Top performers table |
| Gauge | Service Manager query | SLA compliance % |

**Steps**:
1. Service Apps → Create App → Dashboard template
2. Add KPI widget → Select "Tickets closed this week"
3. Add Bar Chart → X-axis: Technician, Y-axis: Count
4. Add Data Viewer → Query: Top 10 technicians by closed tickets
5. Set permissions → Supervisor group only

**Pros**: Native, no code, real-time, mobile responsive
**Cons**: Limited to standard chart types

---

#### Option B: PowerBI Report (1-2 weeks)
Advanced analytics for executives.

**Data Sources**:
- `/requests` - Ticket data
- `/actions` - Activity/closure data
- `/employees` - Technician info

**Reports**:
- Technician leaderboard (closed tickets, resolution time)
- Trend analysis (volume over time)
- SLA breach analysis
- Category breakdown

**Steps**:
1. PowerBI Desktop → Get Data → Web → REST API
2. Authenticate with EV Access Token
3. Import `/requests` endpoint
4. Create visuals: Table, Charts, Cards
5. Schedule refresh (off-hours!)
6. Publish to PowerBI Service

**Pros**: Advanced analytics, shareable, scheduled refresh
**Cons**: API performance impact, requires PowerBI license

---

#### Option C: Native Report with Schedule (30 minutes)
Quick fix using built-in reporting.

**Steps**:
1. Service Manager → Reports → New
2. Select parent query: Tickets
3. Filter: Status = Closed, Date range = This week
4. Group by: Technician
5. Sort by: Count descending
6. Save → Schedule → Email to supervisor daily

**Pros**: Zero setup, automatic delivery
**Cons**: Static PDF/CSV, limited interactivity

---

### Recommended Approach

**Phase 1**: Native scheduled report (immediate fix)
**Phase 2**: Service Apps dashboard (interactive view)
**Phase 3**: PowerBI (executive analytics, if needed)

---

## Pain Point 2: No Ticket Categories

### Problem
All tickets go to same pool. No organization by:
- Type (incident vs request)
- Category (hardware, software, network)
- Department/team

### Root Cause
Missing or unused Service Catalog configuration.

### Solution: Configure Service Catalog (2-3 days)

#### Step 1: Define Categories

**Catalog Structure Example**:
```
Service Catalog
├── Incidents
│   ├── Hardware
│   │   ├── Laptop
│   │   ├── Desktop
│   │   ├── Printer
│   │   └── Mobile Device
│   ├── Software
│   │   ├── Office 365
│   │   ├── VPN
│   │   └── Business Applications
│   ├── Network
│   │   ├── Internet
│   │   ├── WiFi
│   │   └── Internal Access
│   └── Security
│       ├── Account Lockout
│       └── Password Reset
├── Service Requests
│   ├── Hardware
│   │   ├── New Equipment
│   │   └── Equipment Return
│   ├── Software
│   │   ├── License Request
│   │   └── Installation
│   └── Access
│       ├── New User
│       ├── Role Change
│       └── Termination
└── Changes
    ├── Standard
    └── Emergency
```

#### Step 2: Create Catalog Items

**Service Manager → Service Catalog → Catalog Items**

For each category:
1. Create catalog item
2. Set category/subcategory
3. Define workflow
4. Set assignment rules

#### Step 3: Configure Assignment Rules

**Service Manager → Administration → Assignment Rules**

| Category | Subcategory | Assigned Group |
|----------|-------------|----------------|
| Hardware | Laptop | Desktop Support |
| Software | Office 365 | M365 Team |
| Network | WiFi | Network Operations |
| Security | Password Reset | Service Desk |

#### Step 4: Update Ticket Views

Create filtered views:
- "Hardware Incidents" → Category = Hardware
- "My Team Tickets" → Assigned Group = [Current User's Group]
- "Open by Category" → Group by Category

#### Step 5: Update Portals

**Service Apps**:
- Create category-based menu
- Group catalog items by category
- Show category icons

**Self Help**:
- Route users to correct category
- Decision tree → Category selection

---

### Impact

| Before | After |
|--------|-------|
| All tickets mixed | Tickets categorized automatically |
| Manual assignment | Auto-assigned by category |
| No visibility | Views filtered by category |
| Generic reporting | Reports by category |

---

## Pain Point 3: No Email Notifications to Users

### Problem
Creator/affected user not notified when:
- Ticket is resolved
- Status changes
- Comments added
- Assigned to technician

### Root Cause
Missing or misconfigured notification rules.

### Solution: Configure Notifications (1 day)

#### Step 1: Check SMTP Configuration

**Service Manager → Administration → Email → SMTP Server**

Verify:
- SMTP server address
- Port (25, 587, 465)
- Authentication credentials
- Test email succeeds

#### Step 2: Enable Notification Rules

**Service Manager → Administration → Notification Rules**

Create/enable rules for:

| Event | Recipient | Template |
|-------|-----------|----------|
| Ticket Created | Affected User | New Ticket Confirmation |
| Status = Resolved | Affected User | Ticket Resolved |
| Status = Closed | Affected User | Ticket Closed |
| Comment Added | Affected User | Update Notification |
| Assigned | Assigned To | Assignment Notification |
| SLA Breach | Manager | SLA Alert |

#### Step 3: Configure Email Templates

**Service Manager → Administration → Email Templates**

**Ticket Created Template**:
```
Subject: Ticket #{RfcNumber} Created - {BriefDescription}

Hello {AffectedUser.FullName},

Your ticket has been created:
Ticket #: {RfcNumber}
Description: {BriefDescription}
Category: {CatalogName}
Priority: {PriorityName}

You will receive updates via email.

View ticket: {TicketLink}
```

**Ticket Resolved Template**:
```
Subject: Ticket #{RfcNumber} Resolved

Hello {AffectedUser.FullName},

Your ticket has been resolved:
Ticket #: {RfcNumber}
Resolution: {ResolutionDescription}
Resolved by: {AssignedTo.FullName}
Date: {ResolutionDate}

If you still experience issues, please reply to reopen.

View ticket: {TicketLink}
```

#### Step 4: Test Notifications

1. Create test ticket with your email as affected user
2. Check inbox for creation notification
3. Resolve ticket
4. Check inbox for resolution notification
5. Verify sender address (not marked spam)

---

### Common Issues & Fixes

| Issue | Cause | Fix |
|-------|-------|-----|
| No emails sent | SMTP not configured | Add SMTP server settings |
| Emails in spam | SPF/DKIM missing | Configure email authentication |
| Wrong recipient | Notification rule | Check recipient field mapping |
| Blank fields | Missing template variables | Use correct field names |
| Delayed emails | Queue backed up | Check email queue status |

---

## Additional Quick Wins

### Pain Point 4: Duplicate Tickets
**Solution**: Enable Self Help knowledge base + virtual agent for self-resolution before ticket creation.

### Pain Point 5: No Mobile Access
**Solution**: Service Apps are mobile responsive. Enable EV mobile app or browser access.

### Pain Point 6: Manual Asset Linking
**Solution**: Intune integration (if not working) or Discovery for automatic CI linking.

### Pain Point 7: No SLA Visibility
**Solution**: Add SLA fields to ticket views + dashboard widgets showing breach risk.

---

## Implementation Priority

| Priority | Pain Point | Effort | Impact |
|----------|------------|--------|--------|
| 1 | Email Notifications | 1 day | High |
| 2 | Ticket Categories | 2-3 days | High |
| 3 | Supervisor Dashboard | 1 day | Medium |
| 4 | Self Help Knowledge | 1 week | Medium |
| 5 | Intune/Discovery Fix | 3-5 days | Medium |

---

## Documentation Links

- [Service Catalog Configuration](https://docs.easyvista.com/v1/docs/service-catalog/)
- [Assignment Rules](https://docs.easyvista.com/v1/docs/assignment-rules/)
- [Notification Rules](https://docs.easyvista.com/v1/docs/notification-rules/)
- [Email Templates](https://docs.easyvista.com/v1/docs/email-templates/)
- [Service Apps Dashboards](https://docs.easyvista.com/v1/docs/apps-widgets-library/)
