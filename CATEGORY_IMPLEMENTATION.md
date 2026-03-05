# Category Implementation Plan

Phase 1 of EV optimization: Service Catalog structure.

---

## Goals

1. Auto-categorize incoming tickets
2. Auto-assign to correct teams
3. Enable filtered views by category
4. Create supervisor reporting by category

---

## Step 1: Catalog Design (30 minutes)

### Recommended Structure for IT Department

```
SERVICE CATALOG
│
├── INCIDENTS (Things are broken)
│   ├── Hardware
│   │   ├── Laptop/Desktop Issues
│   │   ├── Printer Problems
│   │   └── Mobile Device Issues
│   ├── Software
│   │   ├── Office 365
│   │   ├── VPN Access
│   │   └── Application Errors
│   ├── Network
│   │   ├── Internet Connectivity
│   │   ├── WiFi Problems
│   │   └── Internal Network
│   └── Security
│       ├── Account Lockout
│       └── Password Reset
│
├── SERVICE REQUESTS (I need something)
│   ├── Hardware
│   │   ├── New Equipment Request
│   │   └── Equipment Return
│   ├── Software
│   │   ├── Software Installation
│   │   └── License Request
│   ├── Access
│   │   ├── New User Setup
│   │   ├── Access Change
│   │   └── Termination
│   └── Moves
│       ├── Office Move
│       └── Department Transfer
│
└── CHANGES (Planned modifications)
    ├── Standard Changes
    │   ├── Software Update
    │   └── Hardware Upgrade
    └── Emergency Changes
```

---

## Step 2: Create Categories in EV (2 hours)

### Path: Service Manager → Service Catalog → Catalog Structure

1. **Create Top-Level Categories**
   ```
   - Incidents
   - Service Requests  
   - Changes
   ```

2. **Create Subcategories**
   ```
   Incidents
   ├── Hardware
   ├── Software
   ├── Network
   └── Security
   ```

3. **Create Catalog Items**
   
   For each leaf node, create a catalog item:
   ```
   Name: "Laptop/Desktop Issues"
   Category: Incidents
   Subcategory: Hardware
   Workflow: Incident Workflow
   Assignment: ? (we'll set this in Step 3)
   ```

### Catalog Item Template

| Field | Value Example |
|-------|---------------|
| Catalog Name | Laptop/Desktop Issues |
| Category | Incidents |
| Subcategory | Hardware |
| Workflow | Standard Incident |
| Priority | Medium (default) |
| Impact | User |
| Urgency | Medium |

---

## Step 3: Assignment Rules (1 hour)

### Path: Service Manager → Administration → Assignment Rules

Create rules to auto-assign by category:

| Rule Name | Condition | Assigned Group |
|-----------|-----------|----------------|
| HW Incidents | Category=Incidents AND Subcategory=Hardware | Desktop Support |
| SW Incidents | Category=Incidents AND Subcategory=Software | Application Support |
| Net Incidents | Category=Incidents AND Subcategory=Network | Network Operations |
| Sec Incidents | Category=Incidents AND Subcategory=Security | Security Team |
| HW Requests | Category=Service Requests AND Subcategory=Hardware | Procurement |
| Access Req | Category=Service Requests AND Subcategory=Access | Identity Management |
| Std Changes | Category=Changes AND Subcategory=Standard | Change Management |

### Pro Tip: Use Cascading Rules

Order matters. Most specific first:

```
1. Emergency Changes → CAB Emergency
2. Security Incidents → Security Team  
3. Hardware Incidents → Desktop Support
4. Software Incidents → App Support
5. Network Incidents → Network Ops
6. Default → Service Desk (catch-all)
```

---

## Step 4: Update Ticket Views (30 minutes)

### Path: Service Manager → Views → New View

Create filtered views for teams:

**View: Hardware Team Queue**
```
Filter:
- Status = Open OR In Progress
- Category = Incidents
- Subcategory = Hardware
- Assigned Group = Desktop Support

Columns:
- Ticket #, Brief Description, Priority, Requestor, Created Date, SLA Deadline

Sort: Priority (High→Low), Created Date (Oldest first)
```

**View: My Team's Tickets**
```
Filter:
- Status ≠ Closed
- Assigned Group = [Current User's Group]

Columns:
- Ticket #, Category, Brief Description, Status, Requestor
```

**View: Supervisor Dashboard**  
```
Filter:
- Status = Open OR In Progress OR Resolved
- Created Date = This Week

Group By: Assigned To
Count: Tickets per technician
```

---

## Step 5: Update Portal (1 hour)

### Path: Service Apps → Edit App

**Option A: Category Menu**

Replace flat ticket list with category-based navigation:

```
[Create Ticket]
  ├─ [Report an Issue]
  │   ├─ Hardware Problem
  │   ├─ Software Problem  
  │   ├─ Network Issue
  │   └─ Security Concern
  └─ [Request Something]
      ├─ New Equipment
      ├─ Software Install
      └─ Access Request
```

**Option B: Category Icons**

Use visual category selector:
```
[🖥️] [💻] [🌐] [🔒] [📱]
 Hardware  Software  Network  Security  Mobile
```

---

## Step 6: Test End-to-End (30 minutes)

### Test Cases

| Test | Action | Expected Result |
|------|--------|-----------------|
| 1 | Create ticket "Laptop won't start" → Select Hardware | Ticket auto-assigned to Desktop Support |
| 2 | Create ticket "Need Office 365" → Select Software Install | Ticket auto-assigned to App Support |
| 3 | Create ticket "Can't connect to WiFi" → Select Network | Ticket auto-assigned to Network Ops |
| 4 | Check Hardware Team view | Only hardware tickets visible |
| 5 | Check Supervisor view | All tickets visible, grouped by assignee |

---

## Step 7: Train Users (1 hour)

### Quick Reference Card

**For Requestors:**
```
Creating a Ticket:
1. Click "Create Ticket"
2. Select type:
   • Something broken → Report an Issue
   • Need something → Request Something
3. Pick the closest category
4. Fill details
5. Submit

Your ticket will automatically go to the right team!
```

**For Technicians:**
```
Finding Your Tickets:
1. Go to "My Team's Tickets" view
2. Only tickets assigned to your group show
3. No need to hunt through the pool
```

---

## Success Metrics

Measure these after 1 week:

| Metric | Before | Target After |
|--------|--------|--------------|
| Tickets in correct category | 0% | 90%+ |
| Auto-assigned tickets | 0% | 85%+ |
| Avg assignment time | 2-4 hours | <10 minutes |
| Wrong team escalations | High | Low |
| Supervisor visibility | None | Full |

---

## Quick Reference: EV Navigation Paths

| Task | Path |
|------|------|
| Create category | Service Manager → Service Catalog → Structure |
| Create catalog item | Service Manager → Service Catalog → Items |
| Set assignment rules | Administration → Assignment Rules |
| Create views | Service Manager → Views |
| Edit portal | Service Apps → Select App → Edit |

---

## Next Steps After Categories

Once categories work:
1. Build supervisor dashboard (by category)
2. Fix email notifications (include category in templates)
3. Set up SLA rules per category (hardware = 4hr, software = 8hr)
4. Create reports by category for management

---

## Troubleshooting

| Issue | Check | Fix |
|-------|-------|-----|
| Tickets not auto-assigned | Assignment rules enabled? | Enable rules |
| Wrong team gets ticket | Rule order? | Reorder: specific → general |
| Category not in list | Catalog item published? | Publish catalog item |
| Users can't see categories | Permissions? | Check role rights |
| Views showing wrong tickets | Filter criteria? | Verify category/subcategory filters |
