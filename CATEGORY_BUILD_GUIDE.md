# Service Catalog Build - Groups-Based

Groups confirmed. Build categories now, route by Group.

---

## Step 1: Create Categories (15 minutes)

**Path**: Service Manager → Service Catalog → Structure

### Create These 4 Categories

```
1. QUICK FIXES
   └── Password & Account

2. EQUIPMENT
   ├── Issue Equipment
   ├── Return Equipment
   └── Hardware Repair

3. FIELD SERVICES
   ├── Scheduled Visit
   └── Emergency Visit

4. APPLICATIONS
   ├── MyAvatar
   ├── Munis
   └── Other Software
```

**How**:
1. Click "New Category"
2. Name: "Quick Fixes"
3. Save
4. Repeat for all 4

---

## Step 2: Create Catalog Items (45 minutes)

**Path**: Service Manager → Service Catalog → Items → New

### Item 1: Password Unlock

| Field | Value |
|-------|-------|
| Catalog Name | Password Unlock |
| Category | Quick Fixes |
| Subcategory | Password & Account |
| Workflow | Standard Incident |
| Priority | Medium |

**Form Fields**:
```
[Username*]        - Text field
[System*]          - Dropdown: Windows / M365 / VPN / Other
[Phone Number*]    - Text field (for verification)
[Details]          - Text area
```

**Save**

---

### Item 2: New Hire Equipment

| Field | Value |
|-------|-------|
| Catalog Name | New Hire Equipment Setup |
| Category | Equipment |
| Subcategory | Issue Equipment |
| Workflow | Standard Request |
| Priority | Medium |

**Form Fields**:
```
[New Hire Name*]      - Text field
[Start Date*]         - Date picker
[Department*]         - Dropdown (your departments)
[Position*]           - Text field

Equipment Needed:
[ ] Laptop/Desktop
[ ] Monitor
[ ] Phone
[ ] Cellphone
[ ] Signature Pad
[ ] Printer

[Special Requirements] - Text area
```

**Save**

---

### Item 3: Schedule Field Visit

| Field | Value |
|-------|-------|
| Catalog Name | Schedule On-Site Visit |
| Category | Field Services |
| Subcategory | Scheduled Visit |
| Workflow | Standard Request |
| Priority | Medium |

**Form Fields**:
```
[Location/Building*]   - Text field
[Room Number*]         - Text field
[Issue Type*]          - Dropdown:
                          - Monitor swap
                          - Printer install
                          - Workstation setup
                          - Signature pad issue
                          - Other hardware
[Preferred Date*]      - Date picker
[Preferred Time]       - Time picker
[Contact Phone*]       - Text field
[Description]          - Text area
```

**Save**

---

### Item 4: MyAvatar Support

| Field | Value |
|-------|-------|
| Catalog Name | MyAvatar Support |
| Category | Applications |
| Subcategory | MyAvatar |
| Workflow | Standard Incident |
| Priority | Medium |

**Form Fields**:
```
[Issue Type*]         - Dropdown:
                         - Cannot log in
                         - Function not working
                         - Data/Report issue
                         - Performance slow
                         - Other
[Module*]             - Dropdown: HR / Finance / Payroll / Other
[Error Message]       - Text area
[Urgency*]            - Dropdown: Normal / Urgent
```

**Save**

---

### Item 5: Munis Support

Copy MyAvatar, change:
- Catalog Name: "Munis Support"
- Subcategory: Munis

**Save**

---

## Step 3: Create Assignment Rules (15 minutes)

**Path**: Administration → Assignment Rules → New

### Rule 1: Quick Fixes → Helpdesk

```
Rule Name: Password Issues to Helpdesk
Active: Yes

CONDITIONS:
  Category = Quick Fixes

ACTIONS:
  Assigned Group = Helpdesk
  Priority = Medium

Save
```

---

### Rule 2: Equipment → Inventory

```
Rule Name: Equipment to Inventory
Active: Yes

CONDITIONS:
  Category = Equipment

ACTIONS:
  Assigned Group = Inventory
  Priority = Medium

Save
```

---

### Rule 3: Field Services → Field Techs

```
Rule Name: Field Work to Field Techs
Active: Yes

CONDITIONS:
  Category = Field Services

ACTIONS:
  Assigned Group = Field Techs
  Priority = Medium

Save
```

---

### Rule 4: Applications → Enterprise

```
Rule Name: MyAvatar/Munis to Enterprise
Active: Yes

CONDITIONS:
  Category = Applications
  AND (Subcategory = MyAvatar OR Subcategory = Munis)

ACTIONS:
  Assigned Group = Enterprise
  Priority = Medium

Save
```

**Note**: Create a second rule for "Other Software → Helpdesk"

---

## Step 4: Create Views (15 minutes)

**Path**: Service Manager → Views → New

### View 1: Helpdesk Queue

```
View Name: Helpdesk Queue

FILTERS:
  Status = Open OR In Progress
  Assigned Group = Helpdesk

COLUMNS:
  Priority, Ticket #, Brief Description, Requestor, Created Date

SORT:
  Priority (High to Low), Created Date (Oldest first)

Save
```

---

### View 2: Inventory Queue

```
View Name: Equipment Queue

FILTERS:
  Status = Open OR In Progress OR Scheduled
  Assigned Group = Inventory

COLUMNS:
  Ticket #, Subcategory, Brief Description, Requestor, Created Date

GROUP BY: Subcategory

Save
```

---

### View 3: Field Tech Schedule

```
View Name: Field Schedule

FILTERS:
  Status = Open OR Scheduled
  Assigned Group = Field Techs

COLUMNS:
  Scheduled Date, Location, Issue Type, Contact, Status

SORT: Scheduled Date (Ascending)

Save
```

---

### View 4: Enterprise Queue

```
View Name: Enterprise Queue

FILTERS:
  Status = Open OR In Progress
  Assigned Group = Enterprise

COLUMNS:
  Priority, Ticket #, Application, Brief Description, Requestor

Save
```

---

## Step 5: Test (15 minutes)

Create 4 test tickets:

### Test 1: Password
```
Create Ticket:
  Category: Quick Fixes
  Subcategory: Password & Account
  Catalog Item: Password Unlock
  
Expected: Auto-assigned to Helpdesk
Check: Helpdesk Queue view shows ticket
```

### Test 2: Equipment
```
Create Ticket:
  Category: Equipment
  Subcategory: Issue Equipment
  Catalog Item: New Hire Equipment Setup

Expected: Auto-assigned to Inventory
Check: Equipment Queue view shows ticket
```

### Test 3: Field
```
Create Ticket:
  Category: Field Services
  Subcategory: Scheduled Visit
  Catalog Item: Schedule On-Site Visit

Expected: Auto-assigned to Field Techs
Check: Field Schedule view shows ticket
```

### Test 4: Application
```
Create Ticket:
  Category: Applications
  Subcategory: MyAvatar
  Catalog Item: MyAvatar Support

Expected: Auto-assigned to Enterprise
Check: Enterprise Queue view shows ticket
```

---

## Step 6: Set as Default Views (5 minutes)

**For Helpdesk Users**:
```
User Profile → Default View: Helpdesk Queue
```

**For Inventory**:
```
User Profile → Default View: Equipment Queue
```

**For Field Techs**:
```
User Profile → Default View: Field Schedule
```

**For Enterprise**:
```
User Profile → Default View: Enterprise Queue
```

---

## Verification Checklist

- [ ] 4 categories created in Service Catalog
- [ ] 5 catalog items created and published
- [ ] 4 assignment rules created and active
- [ ] 4 team views created
- [ ] Test ticket 1 auto-assigned to Helpdesk
- [ ] Test ticket 2 auto-assigned to Inventory
- [ ] Test ticket 3 auto-assigned to Field Techs
- [ ] Test ticket 4 auto-assigned to Enterprise
- [ ] Each team's view shows only their tickets

---

## Result

| Before | After |
|--------|-------|
| All tickets mixed | Routed to correct team automatically |
| Manual assignment | < 1 minute auto-assignment |
| No visibility | Each team sees only their queue |
| Generic forms | Specific forms per need |

---

## Time Estimate

| Step | Time |
|------|------|
| Create Categories | 15 min |
| Create Catalog Items | 45 min |
| Create Assignment Rules | 15 min |
| Create Views | 15 min |
| Test | 15 min |
| Set Defaults | 5 min |
| **Total** | **~2 hours** |

---

## Next After This Works

1. Add more catalog items (Equipment Return, Hardware Repair, Munis)
2. Build supervisor dashboard (combines all views)
3. Add email notifications
4. Add SLA rules per category
5. Add approval workflows (manager sign-off for equipment)
