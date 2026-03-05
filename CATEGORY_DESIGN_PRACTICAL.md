# Practical Service Catalog Design

Based on actual operations and ticket types.

---

## Current Reality

### High-Volume Ticket Types
| Issue | Volume | Team | Notes |
|-------|--------|------|-------|
| Password Unlock | **Highest** | Helpdesk | Bulk of helpdesk work |
| Equipment Issue | High | Inventory | Requires signatures |
| Equipment Return | Medium | Inventory | Scheduled/unscheduled |
| Hardware Repair | Medium | Inventory | Dell warranty |
| Field Work | Medium | Field Techs | Physical site visits |
| Login Issues | Medium | Helpdesk | Various systems |
| Network | Low-Medium | Network | Connectivity |
| Cellphone | Low | Inventory/Field | Mobile devices |
| Signature Pads | Low | Field Techs | Specific hardware |
| MyAvatar | Low | Enterprise | Application support |
| Munis | Low | Enterprise | Application support |

### Teams
- **Helpdesk** - Passwords, basic issues, triage
- **Field Techs** - Physical work, site visits
- **Enterprise** - Munis, MyAvatar (business apps)
- **Systems & Security** - AD, M365, user provisioning
- **Network** - Connectivity, infrastructure
- **Inventory** - Equipment lifecycle (issue, return, repair)

---

## Proposed Catalog Structure

```
SERVICE CATALOG
│
├── 🔐 QUICK FIXES (No approval needed)
│   ├── Password Unlock
│   │   └── Auto-assign: Helpdesk
│   ├── Account Unlock
│   │   └── Auto-assign: Helpdesk
│   └── Login Issues (Generic)
│       └── Auto-assign: Helpdesk
│
├── 💻 EQUIPMENT LIFECYCLE
│   ├── Issue Equipment ⭐ HIGH VOLUME
│   │   ├── New Hire Setup
│   │   │   └── Auto-assign: Inventory → Systems
│   │   ├── Replacement/Upgrade
│   │   │   └── Auto-assign: Inventory
│   │   └── Temporary Loan
│   │       └── Auto-assign: Inventory
│   │
│   ├── Return Equipment
│   │   ├── Scheduled Return (Offboarding)
│   │   │   └── Auto-assign: Inventory + Notify Manager
│   │   └── Unscheduled Return (Broken/Leaving)
│   │       └── Auto-assign: Inventory
│   │
│   ├── Hardware Repair
│   │   ├── Dell Warranty Repair
│   │   │   └── Auto-assign: Inventory → Dell dispatch
│   │   ├── Non-Warranty Repair
│   │   │   └── Auto-assign: Inventory
│   │   └── Emergency Replacement
│   │       └── Auto-assign: Inventory (expedite)
│   │
│   └── Cellphone Issues
│       ├── New Cellphone
│       ├── Repair/Replace
│       └── Plan/Service Issues
│
├── 🏃 FIELD SERVICES
│   ├── Site Visit - Scheduled
│   │   ├── Monitor Swap
│   │   ├── Printer Install
│   │   ├── Workstation Setup
│   │   └── Signature Pad Issues
│   │
│   ├── Site Visit - Emergency
│   │   └── Auto-assign: Field Techs + Notify Supervisor
│   │
│   └── Equipment Delivery/Pickup
│       └── Auto-assign: Field Techs
│
├── 🌐 NETWORK
│   ├── Internet Connectivity
│   ├── WiFi Issues
│   ├── VPN Problems
│   └── Internal Network Access
│
├── 👤 USER ACCESS
│   ├── New User Creation
│   │   └── Auto-assign: Systems & Security
│   ├── Access Change
│   │   └── Auto-assign: Systems & Security
│   ├── Termination/Disable
│   │   └── Auto-assign: Systems & Security + Notify Manager
│   └── M365 License Issues
│
├── 🖥️ APPLICATION SUPPORT
│   ├── MyAvatar Issues
│   │   └── Auto-assign: Enterprise
│   ├── Munis Issues
│   │   └── Auto-assign: Enterprise
│   ├── Office 365
│   │   └── Auto-assign: Helpdesk → Escalate Enterprise
│   └── Other Software
│       └── Auto-assign: Helpdesk
│
└── 🚨 EMERGENCY
    ├── System Outage
    ├── Security Incident
    └── Critical Business Impact
```

---

## Detailed Category Specifications

### 1. Password Unlock (Highest Volume)

**Catalog Item**: "Password Unlock / Reset"

**Form Fields**:
- Username (required)
- System (dropdown: Windows, M365, VPN, Other)
- Contact Method (phone/email for verification)
- Manager approval required? (checkbox for executive accounts)

**Workflow**:
1. Auto-assign: Helpdesk
2. SLA: 1 hour (priority based on user level)
3. Auto-close if resolved via self-service first

**Assignment Rule**:
```
IF Category = "Quick Fixes" 
AND Subcategory = "Password Unlock"
THEN Assigned Group = Helpdesk
```

---

### 2. Equipment Issue (New Hire)

**Catalog Item**: "New Hire Equipment Setup"

**Form Fields**:
- New Hire Name
- Start Date
- Department
- Position
- Equipment Needed (checkboxes):
  - [ ] Laptop/Desktop
  - [ ] Monitor
  - [ ] Phone
  - [ ] Cellphone
  - [ ] Signature Pad
  - [ ] Printer
- Special Requirements
- Manager Approval (auto-routed)

**Workflow**:
1. Manager approval required
2. Auto-assign: Inventory
3. Check stock availability
4. If available: Schedule build
5. If not: Create procurement request
6. Parallel: Assign to Systems & Security for AD/M365 setup
7. Handoff to Field Techs for delivery/install
8. Digital signature capture on equipment form
9. Close when confirmed delivered

**Assignment Rules**:
```
Stage 1: Inventory (equipment prep)
Stage 2: Systems & Security (account setup)
Stage 3: Field Techs (delivery)
```

---

### 3. Equipment Return

**Catalog Item**: "Equipment Return"

**Two Subtypes**:

**A. Scheduled Return** (Offboarding)
- Termination Date
- Equipment List (pre-populated from asset db)
- Exit Interview Scheduled?
- Manager notified automatically

**B. Unscheduled Return** (Broken/Leaving)
- Reason (dropdown)
- Condition
- Urgency

**Workflow**:
1. Auto-assign: Inventory
2. Generate equipment checklist
3. Schedule pickup (Field Techs) OR
4. User drop-off at IT
5. Inspection checklist
6. Digital sign-off
7. Update asset database
8. Notify Systems & Security if termination

---

### 4. Dell Hardware Repair

**Catalog Item**: "Dell Warranty Repair"

**Form Fields**:
- Asset Tag (auto-lookup specs)
- Service Tag (Dell)
- Issue Description
- Warranty Status (auto-check if API available)
- Replacement Needed? (checkbox)

**Workflow**:
1. Auto-assign: Inventory
2. Inventory tech verifies warranty
3. If in warranty:
   - Create Dell dispatch ticket
   - Generate shipping label
   - Schedule Field Tech pickup
4. If out of warranty:
   - Quote for repair
   - Manager approval for cost
5. If replacement needed:
   - Check stock
   - Or create temporary loan
6. Track repair status
7. Return to user

**Assignment Rule**:
```
IF Equipment Type = Dell
AND Issue = Hardware Failure
THEN Assigned Group = Inventory
WITH Sub-status: Warranty Check
```

---

### 5. Field Service Tickets

**Catalog Item**: "On-Site Tech Support"

**Form Fields**:
- Location/Building
- Room Number
- Issue Type (dropdown):
  - Monitor swap
  - Printer install
  - Workstation setup
  - Signature pad
  - Other hardware
- Preferred Date/Time
- Urgency (Scheduled vs Emergency)
- Contact Number

**Workflow**:

**Scheduled**:
1. Auto-assign: Field Techs
2. Add to field tech calendar
3. Send confirmation to user
4. Pre-trip: Check parts needed
5. Post-trip: Digital sign-off
6. Close

**Emergency**:
1. Auto-assign: Field Techs
2. Notify Field Tech Supervisor
3. SLA: 4 hour response
4. Bypass scheduling queue

**Assignment Rule**:
```
IF Category = "Field Services"
AND Urgency = Emergency
THEN Assigned Group = Field Techs
AND Notify = Field Supervisor
AND SLA = 4 Hour
```

---

### 6. Signature Pad Issues

**Catalog Item**: "Signature Pad Support"

**Form Fields**:
- Location
- Pad Model
- Issue (dropdown):
  - Not connecting
  - Calibration
  - Replacement needed
  - Software/driver issue

**Workflow**:
1. Auto-assign: Field Techs (usually requires visit)
2. Check if remote fix possible (Helpdesk can try first)
3. If hardware issue: Replace from stock
4. If software: Helpdesk handles remotely

**Special**: High priority for departments using signature pads (HR, Finance)

---

### 7. Application Issues (MyAvatar / Munis)

**Catalog Item**: "Enterprise Application Support"

**Form Fields**:
- Application (dropdown: MyAvatar, Munis, Other)
- Issue Type:
  - Cannot log in
  - Function not working
  - Data issue
  - Report problem
  - Training request
- Error Message (text)
- Urgency

**Workflow**:
1. Auto-assign: Enterprise team
2. If login issue: CC Systems & Security
3. If training: Route to training queue
4. If data issue: Check permissions

**Assignment Rules**:
```
IF Application = MyAvatar OR Munis
THEN Assigned Group = Enterprise
```

---

## Assignment Rules Summary

| Category | Subcategory | Assigned To | Notes |
|----------|-------------|-------------|-------|
| Quick Fixes | Password Unlock | **Helpdesk** | SLA: 1hr |
| Quick Fixes | Login Issues | **Helpdesk** | SLA: 2hr |
| Equipment | Issue | **Inventory** | Manager approval |
| Equipment | Return | **Inventory** | Notify manager |
| Equipment | Repair | **Inventory** | Dell dispatch |
| Field Services | Any | **Field Techs** | Calendar integration |
| Field Services | Emergency | **Field Techs** + Notify Supervisor | 4hr SLA |
| Network | Any | **Network** | 4hr SLA |
| User Access | New/Change | **Systems & Security** | Approval workflow |
| Applications | MyAvatar/Munis | **Enterprise** | CC Helpdesk if login |
| Emergency | Any | **On-call Manager** | Page/SMS |

---

## Form Design Best Practices

### Capture Everything Upfront

**Bad**: Generic "Description" field
**Good**: Specific fields that route correctly

Example - Equipment Issue Form:
```
[Asset Tag]      [Auto-lookup: Equipment Type, Warranty Status]
[Issue Type]     [Dropdown: Screen/Keyboard/Battery/Software/Other]
[Warranty?]      [Read-only: Yes/No/Unknown]
[Loaner Needed?] [Checkbox]
[Urgency]        [Scheduled/Emergency]
```

### Use Conditional Fields

Show/hide fields based on selections:
```
IF Issue Type = "Password"
THEN Show: System dropdown (Windows/M365/VPN)

IF Category = "Equipment Return"
AND Reason = "Termination"
THEN Show: Termination Date
AND Require: Manager Approval
```

### Auto-Populate When Possible

- Username from login
- Department from user profile
- Equipment list from asset database
- Manager from org chart

---

## Views by Team

### Helpdesk View
```
Filter:
- Assigned Group = Helpdesk
- Status = Open OR In Progress
- Category = Quick Fixes OR (category = Applications AND subcategory = Office 365)

Columns:
- Priority, Ticket #, Brief Description, User, Time Open, SLA Remaining

Sort: Priority DESC, Created Date ASC
```

### Field Techs View
```
Filter:
- Assigned Group = Field Techs
- Status = Open OR Scheduled

Columns:
- Scheduled Date, Location, Issue Type, Contact, Status

Group By: Date (today, tomorrow, this week)
```

### Inventory View
```
Filter:
- Assigned Group = Inventory

Group By: Subcategory (Issue, Return, Repair)
```

### Supervisor Dashboard
```
View 1: Open by Team (pie chart)
View 2: SLA Breach Risk (list)
View 3: Today's Field Visits (map or list)
View 4: Password Resets Today (counter)
```

---

## Implementation Priority

### Week 1: Foundation
1. Create top-level categories
2. Set up assignment rules
3. Configure basic views

### Week 2: High Volume
4. Password unlock workflow (forms, assignment)
5. Equipment issue workflow (forms, approvals)
6. Equipment return workflow

### Week 3: Specialized
7. Dell repair workflow
8. Field service scheduling
9. Application support routing

### Week 4: Polish
10. Supervisor dashboard
11. Reports by category
12. User training

---

## Quick Wins (Do These First)

1. **Password Unlock category** → Helpdesk (immediate routing)
2. **Equipment Issue** → Inventory (stop hunting)
3. **Field Services** → Field Techs with calendar
4. **MyAvatar/Munis** → Enterprise (stop misrouting)

These 4 categories will handle 80% of tickets correctly.
