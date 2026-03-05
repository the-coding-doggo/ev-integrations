# Finding Your Organizational Structure

EV uses different terminology. Let's locate where your org structure lives.

---

## Possible Locations

EV may use these terms for organizational hierarchy:

| Term | Likely Location | What It Is |
|------|-----------------|------------|
| **Entities** | Administration → Entities | Most likely - org structure |
| **Organizations** | Administration → Organizations | Company/division structure |
| **Departments** | Reference Data → Departments | Functional groups |
| **Locations** | Reference Data → Locations | Physical sites |
| **Groups** | Administration → Groups | Assignment groups (your IT teams) |

---

## Check These Paths

### Path 1: Administration → Entities (Most Likely)
```
Service Manager → Administration → Entities
```

What to look for:
- Tree structure (parent/child)
- Your company name at top
- Divisions underneath
- Departments as leaf nodes
- Codes for each entity

**If found here**: This is your org structure. Use Entity field for routing.

---

### Path 2: Administration → Organizations
```
Service Manager → Administration → Organizations
```

Similar to Entities but may be company-level only.

---

### Path 3: Reference Data → Departments
```
Service Manager → Reference Data → Departments
```

Flat list of departments without hierarchy.

---

### Path 4: Employee Record
```
Service Manager → Employees → [Select Employee] → Department field
```

Check if Department field is populated on user profiles.

**If blank**: AD sync not populating department data.

---

## What You Actually Have

Based on your environment, check for:

### Assignment Groups (Definitely Exist)
```
Service Manager → Administration → Groups
```

You mentioned these teams:
- Helpdesk
- Field Techs
- Enterprise
- Systems & Security
- Network
- Inventory

**These are Groups** - used for ticket assignment.

---

### Employee Data
```
Service Manager → Employees
```

Check fields on employee records:
- Entity
- Department
- Location
- Cost Center
- Manager

**Which fields are populated?**

---

## Simplified Approach

If org structure is unclear, use what definitely exists:

### Option 1: Use Groups (Guaranteed to Work)

Route by catalog selection, not by user org data:

```
User selects: "Password Issue" → Assign to Helpdesk (Group)
User selects: "Equipment" → Assign to Inventory (Group)
```

**Pros**: Simple, works immediately
**Cons**: No automatic priority by user type

---

### Option 2: Use Location

If employees have Location populated:

```
Service Manager → Reference Data → Locations
```

Route field techs by location:
```
IF Category = "Field Services"
AND Requestor.Location = "Field Office A"
THEN Assign to Field Tech A
```

---

### Option 3: Use AD Department (If Synced)

Check if Department field syncs from AD:

```
Path: Service Manager → Employees → [User]
Field: Department
```

If populated → Can use in assignment rules.
If empty → AD sync not configured for department.

---

## Quick Discovery Checklist

- [ ] **Groups**: Service Manager → Administration → Groups
  - List your 6 IT teams
  
- [ ] **Entities**: Service Manager → Administration → Entities
  - Org tree exists? Y/N
  
- [ ] **Employees**: Service Manager → Employees
  - Select your user record
  - Which fields populated: Entity? Department? Location?
  
- [ ] **Reference Data**: Service Manager → Reference Data
  - Departments listed? Y/N
  - Locations listed? Y/N

---

## Recommended Action

### If Entities Exist (Most Likely)
```
Use: Administration → Entities
In Assignment Rules: Requestor.Entity
```

### If Only Groups Exist
```
Use: Assignment by Catalog Category only
Simpler but less sophisticated
```

### If Employee Data Populated
```
Use: Requestor.Department or Requestor.Location
Check Employee record to confirm
```

---

## Correcting Assignment Rules

Based on what actually exists:

### If Using Entities:
```
Rule: IF Requestor.Entity = "HR" THEN Priority = High
```

### If Using Groups Only:
```
Rule: IF Category = "Applications" THEN Assign = Enterprise
(No dept-based priority)
```

### If Using Locations:
```
Rule: IF Requestor.Location = "Field Office A" THEN Assign = Tech A
```

---

## Documentation Updates Needed

Previous docs assumed "Departments" menu exists. Need to verify actual structure.

**Next step**: Check which of these exist in your EV:
1. Administration → Entities
2. Reference Data → Departments
3. Employee fields (Department, Location, Entity)
4. Only Groups

Then we'll build rules based on what you actually have.
