# Using Existing Departments

Departments exist → Leverage them for auto-assignment and reporting.

---

## What Departments Give You

Since departments are configured:
- ✅ User profiles have department populated
- ✅ Can auto-assign by requestor's department
- ✅ Can route tickets to department-specific teams
- ✅ Can report by department
- ✅ Can create department-specific views
- ✅ Can set department-specific SLAs

---

## Department-to-Team Mapping

Map requestor's department to IT team:

| Requestor Department | Likely Need | Route To |
|---------------------|-------------|----------|
| HR | MyAvatar (HR module), Signature pads | Enterprise → Field Techs |
| Finance | Munis, Signature pads | Enterprise → Field Techs |
| Any Department | Password issues | Helpdesk |
| Any Department | Equipment | Inventory |
| Any Department | Network/VPN | Network |
| Field Locations | On-site support | Field Techs |
| Executive | Priority handling | Helpdesk (escalate) |

---

## Enhanced Assignment Rules

Use department in assignment logic.

### Example: Multi-Criteria Rules

```
Rule 1: HR MyAvatar Issues
IF Category = "Applications" 
AND Subcategory = "MyAvatar"
AND Requestor.Department = "HR"
THEN Assigned Group = Enterprise
AND Priority = High

Rule 2: General MyAvatar Issues
IF Category = "Applications"
AND Subcategory = "MyAvatar"
THEN Assigned Group = Enterprise
AND Priority = Medium

Rule 3: Executive Password Reset
IF Category = "Quick Fixes"
AND Subcategory = "Password"
AND Requestor.Department = "Executive"
THEN Assigned Group = Helpdesk
AND Priority = Critical
AND Notify = IT Manager

Rule 4: Standard Password Reset
IF Category = "Quick Fixes"
AND Subcategory = "Password"
THEN Assigned Group = Helpdesk
AND Priority = Medium
```

---

## Department-Specific Catalog Items

Create items that know the user's department.

### For HR Department
```
Catalog Item: "HR System Support"
Pre-selected: Subcategory = MyAvatar (HR module)
Auto-assign: Enterprise (HR specialist)
Form: HR-specific fields (employee records, payroll, etc.)
```

### For Finance Department
```
Catalog Item: "Finance System Support"
Pre-selected: Subcategory = Munis
Auto-assign: Enterprise (Finance module specialist)
Form: Munis module selection
```

### For Field Offices
```
Catalog Item: "Field Office Support"
Pre-populated: Location from user profile
Auto-assign: Field Techs (by location)
Form: Site-specific fields
```

---

## Smart Views Using Departments

### Department-Specific IT Views

**Helpdesk - Executive Priority**:
```
Filter:
- Assigned Group = Helpdesk
- Requestor.Department = "Executive"
- Status = Open

Sort: Created Date ASC
Highlight: SLA risk
```

**Enterprise - By Module**:
```
Filter:
- Assigned Group = Enterprise

Group By: Requestor.Department
- HR → MyAvatar HR issues
- Finance → Munis issues
- Other → General support
```

**Field Techs - By Location**:
```
Filter:
- Assigned Group = Field Techs

Group By: Requestor.Department (location)
Sort: Scheduled Date
```

---

## Supervisor Dashboard with Departments

### Metrics by Department

Create dashboard widgets:

| Widget | Data | Use |
|--------|------|-----|
| Tickets by Department | Bar chart | Which dept creates most tickets |
| Avg Resolution by Dept | KPI | Dept-specific performance |
| SLA Compliance by Dept | Gauge | Which depts need attention |
| Equipment Requests by Dept | Table | Track new hire volume |

### Example: Department Leaderboard

```
Table: Top Ticket-Creating Departments

Department        | This Week | This Month | Avg Resolution
------------------|-----------|------------|---------------
HR                | 12        | 45         | 4.2 hours
Finance           | 8         | 32         | 3.1 hours
Operations        | 24        | 89         | 6.5 hours
Field Office A    | 5         | 18         | 8.2 hours
```

---

## Implementation Steps

### Step 1: List Current Departments

**Path**: Service Manager → Administration → Departments

Document:
- Department names (exact spelling)
- Department codes
- Associated locations
- Department managers

### Step 2: Map Departments to Services

Create matrix:

| Department | Primary App | Secondary Needs | IT Team |
|------------|-------------|-----------------|---------|
| HR | MyAvatar | Signature pads, equipment | Enterprise/Field |
| Finance | Munis | Signature pads, reports | Enterprise |
| Operations | Various | Equipment, field support | Inventory/Field |
| Field Sites | Local apps | On-site hardware | Field Techs |
| Executive | All | Priority support | Helpdesk+ |

### Step 3: Update Assignment Rules

Add department criteria:

```
Path: Administration → Assignment Rules

For each rule, add condition:
- IF Department = X
- THEN Additional actions (priority, notification)
```

### Step 4: Create Department Views

**Example Views**:
- "HR Tickets" → Filter: Requestor.Dept = HR
- "Finance Tickets" → Filter: Requestor.Dept = Finance
- "Field Locations" → Filter: Location = Field Office
- "Executives" → Filter: Requestor.Dept = Executive

### Step 5: Department Reports

**Scheduled Reports**:
```
Report: Weekly Department Summary
Frequency: Every Monday 8am
Recipients: IT Manager, Department Heads

Contents:
- Tickets by department (count)
- Average resolution time by department
- Open tickets by department
- SLA breaches by department
```

---

## Quick Wins with Departments

### 1. Executive Priority
```
Rule: IF Department = "Executive" THEN Priority = High
Result: VIPs get faster service
```

### 2. Department-Specific Routing
```
Rule: IF Dept = "HR" AND App = "MyAvatar" THEN Assign to HR Specialist
Result: Domain experts handle domain issues
```

### 3. Location-Based Field Tech Dispatch
```
Rule: IF Location = "Field Office A" THEN Assign to Tech A
Result: Familiarity with site
```

### 4. Department Budget Tracking
```
Custom Field: Department Cost Center
Result: Charge IT time back to departments
```

---

## Service Catalog + Departments

### Pre-Populate Department

In catalog item forms:
```
Field: Department
Type: Read-only
Value: {Requestor.Department}
```

Users can't change it, but IT sees it for routing.

### Department-Specific Menus

In Service Apps portal:
```
IF User.Department = "HR"
THEN Show: "HR Systems" menu item

IF User.Department = "Finance"
THEN Show: "Munis Support" menu item
```

---

## Before vs After (Using Departments)

| Aspect | Before | After |
|--------|--------|-------|
| Ticket routing | Random/manual | By requestor dept |
| Priority | All same | Dept-based (exec = high) |
| Reporting | Total only | By department |
| Field dispatch | Any tech | Tech familiar with site |
| App support | General queue | Domain specialist |

---

## Verification

Check department usage:

1. **Path**: Service Manager → Views → New
2. **Filter**: Group by Requestor.Department
3. **Result**: See ticket distribution by department

If departments not populated:
- Check AD sync (should populate department field)
- Or manually update key users

---

## Next Steps

1. ✅ Document current departments
2. ✅ Map departments to IT teams
3. ✅ Update assignment rules with dept criteria
4. ✅ Create department-specific views
5. ✅ Build department dashboard
6. ✅ Set department-specific SLAs (executive = 1hr)
