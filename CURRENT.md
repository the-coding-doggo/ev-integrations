# Active Work

Branch: main
Status: Initialized + Roadmap defined

## Current Task

Integration roadmap documented based on existing tools.

## Environment
- Active Directory (auth/directory)
- Microsoft Intune (device mgmt)
- Genesys (call center)
- M365 Suite (Teams, Exchange, SharePoint)

## Integration Roadmap

### Phase 1 (Start Here)
1. Active Directory - SSO + user sync
2. Microsoft Teams - Native app
3. Exchange/Outlook - Email-to-ticket

### Phase 2
4. Microsoft Intune - Device sync (custom API)

### Phase 3
5. Genesys - Call integration (custom API)

## Active Implementation

**Current**: Build Service Catalog FROM SCRATCH

### Discovery
- Service Catalog is EMPTY (no categories)
- Departments EXIST (can leverage for routing)

### Status
- [x] Catalog structure designed (4 critical + 2 secondary)
- [x] Team assignments mapped
- [x] Department usage strategy defined
- [ ] **DAY 1**: Create category structure in EV
- [ ] **DAY 2**: Build 5 critical catalog items (with dept fields)
- [ ] **DAY 3**: Configure assignment rules (use departments)
- [ ] **DAY 4**: Create team views (dept-specific)
- [ ] **DAY 5**: Test & train

See [SERVICE_CATALOG_BUILD.md](./SERVICE_CATALOG_BUILD.md) for build instructions.
See [DEPARTMENTS_USAGE.md](./DEPARTMENTS_USAGE.md) for department integration.

### Leverage Existing Departments
- Route tickets by requestor's department
- Set priority by department (Executive = High)
- Create department-specific views
- Report metrics by department

### Critical 4 (Build These First)
| Category | Volume | Assigned To | Dept Logic |
|----------|--------|-------------|------------|
| Quick Fixes (Password) | **Highest** | Helpdesk | Executive=Critical |
| Equipment | High | Inventory | By location |
| Field Services | Medium | Field Techs | By department location |
| Applications | Low-Medium | Enterprise | HR=MyAvatar, Finance=Munis |

### Result After Week 1
- 80% of tickets auto-categorized
- 80% of tickets auto-assigned
- Department-based priority
- Teams see only their work
- Supervisor can view by category AND department

## Next Steps

1. Complete category implementation
2. Fix email notifications
3. Build supervisor dashboard
4. Resume Phase 1 integrations (AD sync, Teams app)
