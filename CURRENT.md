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
Service Catalog is EMPTY. No categories exist. All tickets unclassified.

### Status
- [x] Catalog structure designed (4 critical + 2 secondary)
- [x] Team assignments mapped
- [ ] **DAY 1**: Create category structure in EV
- [ ] **DAY 2**: Build 5 critical catalog items
- [ ] **DAY 3**: Configure assignment rules
- [ ] **DAY 4**: Create team views
- [ ] **DAY 5**: Test & train

See [SERVICE_CATALOG_BUILD.md](./SERVICE_CATALOG_BUILD.md) for step-by-step build instructions.

### Critical 4 (Build These First)
| Category | Volume | Assigned To |
|----------|--------|-------------|
| Quick Fixes (Password) | **Highest** | Helpdesk |
| Equipment | High | Inventory |
| Field Services | Medium | Field Techs |
| Applications (MyAvatar/Munis) | Low-Medium | Enterprise |

### Result After Week 1
- 80% of tickets auto-categorized
- 80% of tickets auto-assigned
- Teams see only their work
- Supervisor can view by category

## Next Steps

1. Complete category implementation
2. Fix email notifications
3. Build supervisor dashboard
4. Resume Phase 1 integrations (AD sync, Teams app)
