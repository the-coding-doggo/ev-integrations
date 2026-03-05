# Service Catalog - Build From Scratch

Current state: Empty. All tickets unclassified.
Target: Auto-route 80% of tickets to correct team.

---

## Why Empty Catalog = Chaos

Without Service Catalog:
- ❌ No categories/subcategories
- ❌ No auto-assignment
- ❌ All tickets mixed together
- ❌ Manual routing for every ticket
- ❌ No reporting by type
- ❌ No specialized workflows

With Service Catalog:
- ✅ User selects category on creation
- ✅ Auto-assignment to teams
- ✅ Specialized forms per category
- ✅ Workflows (approvals, signatures)
- ✅ Views filtered by category
- ✅ Reports by department

---

## Phase 1: Critical 4 (Week 1)

Build these 4 categories first. Handle 80% of volume.

### Category 1: Password & Account (→ Helpdesk)

**EV Path**: Service Manager → Service Catalog → Structure → New

**Create**:
```
Category: Quick Fixes
Subcategory: Password & Account
```

**Create Catalog Items**:
1. "Password Unlock / Reset"
   - Form: Username, System (Windows/M365/VPN), Phone
   - Workflow: Standard
   - Assignment: Helpdesk
   
2. "Account Unlock"
   - Form: Username, Reason
   - Workflow: Standard
   - Assignment: Helpdesk

3. "Login Issues"
   - Form: System, Error message
   - Workflow: Standard
   - Assignment: Helpdesk

**Assignment Rule**:
```
Path: Administration → Assignment Rules → New

Rule Name: Quick Fixes → Helpdesk
Condition: Category = "Quick Fixes"
Action: Assigned Group = Helpdesk
Priority: 100
```

**View for Helpdesk**:
```
Path: Service Manager → Views → New View

View Name: Helpdesk Queue
Filter:
  - Status = Open OR In Progress
  - Category = Quick Fixes

Columns: Priority, Ticket #, Brief Description, User, Created
Sort: Priority DESC, Created ASC
```

---

### Category 2: Equipment (→ Inventory)

**Create**:
```
Category: Equipment
Subcategories:
  - New Hire Setup
  - Replacement
  - Return
  - Repair
```

**Create Catalog Items**:

1. "New Hire Equipment"
   - Form: New hire name, Start date, Equipment checklist (laptop, monitor, phone, etc.), Department
   - Workflow: Approval required (manager)
   - Assignment: Inventory
   
2. "Equipment Replacement"
   - Form: Current asset tag, Reason, New equipment needed
   - Workflow: Standard
   - Assignment: Inventory
   
3. "Equipment Return"
   - Form: Asset tags, Return reason (scheduled/unscheduled), Condition
   - Workflow: Standard
   - Assignment: Inventory
   
4. "Hardware Repair"
   - Form: Asset tag, Issue description, Warranty status
   - Workflow: Standard
   - Assignment: Inventory

**Assignment Rule**:
```
Rule Name: Equipment → Inventory
Condition: Category = "Equipment"
Action: Assigned Group = Inventory
Priority: 100
```

**View for Inventory**:
```
View Name: Equipment Queue
Filter:
  - Status = Open OR In Progress
  - Category = Equipment

Group By: Subcategory
```

---

### Category 3: Field Services (→ Field Techs)

**Create**:
```
Category: Field Services
Subcategories:
  - Scheduled Visit
  - Emergency Visit
  - Equipment Delivery
```

**Create Catalog Items**:

1. "Schedule On-Site Visit"
   - Form: Location, Room, Issue type (monitor, printer, setup, signature pad), Preferred date, Contact phone
   - Workflow: Standard
   - Assignment: Field Techs
   
2. "Emergency Field Support"
   - Form: Location, Issue, Impact
   - Workflow: Urgent
   - Assignment: Field Techs + Notify supervisor

**Assignment Rule**:
```
Rule Name: Field Services → Field Techs
Condition: Category = "Field Services"
Action: Assigned Group = Field Techs
Priority: 100
```

**View for Field Techs**:
```
View Name: Field Schedule
Filter:
  - Status = Open OR Scheduled
  - Category = Field Services

Columns: Scheduled Date, Location, Issue, Contact
Sort: Scheduled Date ASC
```

---

### Category 4: Applications (→ Enterprise)

**Create**:
```
Category: Applications
Subcategories:
  - MyAvatar
  - Munis
  - Office 365
  - Other Software
```

**Create Catalog Items**:

1. "MyAvatar Support"
   - Form: Issue type (login, function, data, report), Error details
   - Workflow: Standard
   - Assignment: Enterprise
   
2. "Munis Support"
   - Form: Issue type, Module, Error details
   - Workflow: Standard
   - Assignment: Enterprise
   
3. "Software Support"
   - Form: Application name, Issue
   - Workflow: Standard
   - Assignment: Helpdesk (can escalate to Enterprise)

**Assignment Rule**:
```
Rule Name: MyAvatar/Munis → Enterprise
Condition: Category = "Applications" AND (Subcategory = "MyAvatar" OR Subcategory = "Munis")
Action: Assigned Group = Enterprise
Priority: 200 (higher = evaluated first)

Rule Name: Other Software → Helpdesk
Condition: Category = "Applications" AND Subcategory = "Other Software"
Action: Assigned Group = Helpdesk
Priority: 100
```

**View for Enterprise**:
```
View Name: Enterprise Queue
Filter:
  - Status = Open OR In Progress
  - Category = Applications
  - Subcategory IN (MyAvatar, Munis)
```

---

## Phase 2: Fill Gaps (Week 2)

### Category 5: User Access (→ Systems & Security)

**Create**:
```
Category: User Access
Subcategories:
  - New User
  - Access Change
  - Termination
```

**Catalog Items**:
- "New User Request" → Systems & Security
- "Access Change" → Systems & Security
- "Disable Account" → Systems & Security + Notify manager

---

### Category 6: Network (→ Network Team)

**Create**:
```
Category: Network
Subcategories:
  - Internet
  - WiFi
  - VPN
```

**Catalog Items**:
- "Internet Connectivity" → Network
- "WiFi Issues" → Network
- "VPN Problems" → Network

---

## Implementation Checklist

### Day 1: Create Structure
- [ ] Log into Service Manager
- [ ] Navigate to Service Catalog → Structure
- [ ] Create Category: Quick Fixes
- [ ] Create Category: Equipment
- [ ] Create Category: Field Services
- [ ] Create Category: Applications
- [ ] Create Subcategories under each

### Day 2: Build Catalog Items
- [ ] Create catalog item: Password Unlock
- [ ] Create catalog item: New Hire Equipment
- [ ] Create catalog item: Schedule Field Visit
- [ ] Create catalog item: MyAvatar Support
- [ ] Create catalog item: Munis Support

### Day 3: Assignment Rules
- [ ] Create rule: Quick Fixes → Helpdesk
- [ ] Create rule: Equipment → Inventory
- [ ] Create rule: Field Services → Field Techs
- [ ] Create rule: MyAvatar/Munis → Enterprise
- [ ] Test each rule with sample ticket

### Day 4: Views
- [ ] Create Helpdesk Queue view
- [ ] Create Equipment Queue view
- [ ] Create Field Schedule view
- [ ] Create Enterprise Queue view
- [ ] Create Supervisor Dashboard view

### Day 5: Portal & Test
- [ ] Update Service Apps portal with category menu
- [ ] Create test tickets for each category
- [ ] Verify auto-assignment works
- [ ] Verify views show correct tickets
- [ ] Train team leads on new structure

---

## Verification Steps

Test each category:

| Test | Action | Expected |
|------|--------|----------|
| 1 | Create ticket, select "Password Unlock" | Auto-assigned to Helpdesk |
| 2 | Create ticket, select "New Hire Equipment" | Auto-assigned to Inventory |
| 3 | Create ticket, select "MyAvatar Support" | Auto-assigned to Enterprise |
| 4 | Create ticket, select "Schedule Field Visit" | Auto-assigned to Field Techs |
| 5 | Check Helpdesk view | Only Quick Fixes visible |
| 6 | Check Inventory view | Only Equipment visible |

---

## Common Issues

| Problem | Cause | Fix |
|---------|-------|-----|
| Tickets not auto-assigning | Assignment rules not enabled | Check Administration → Assignment Rules → Active |
| Category not showing | Catalog item not published | Publish the catalog item |
| Wrong team gets ticket | Rule order wrong | Most specific rules first (higher priority number) |
| Views empty | Filter criteria wrong | Check category/subcategory names match exactly |

---

## Before vs After

| Metric | Before (Empty) | After (Configured) |
|--------|----------------|-------------------|
| Ticket routing | Manual (2-4 hours) | Automatic (< 1 minute) |
| Team visibility | Mixed pool | Filtered queues |
| Supervisor reporting | Impossible | By category/team |
| User experience | Generic form | Specific forms per need |
| First assignment accuracy | Low | 80%+ |

---

## Next After Categories

Once categories work:
1. Email notifications (include category in templates)
2. Supervisor dashboard (views by category)
3. SLA rules (different SLAs per category)
4. Advanced workflows (approvals, signatures)
