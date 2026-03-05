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
- Organizational structure location UNCLEAR (may be Entities, not Departments)
- Groups EXIST (Helpdesk, Field Techs, etc. - used for assignment)

### Verification Needed
Check which org structure exists:
- [ ] Administration → Entities
- [ ] Reference Data → Departments  
- [ ] Employee fields (Department, Location, Entity)
- [ ] Only Groups (fallback)

See [FIND_ORG_STRUCTURE.md](./FIND_ORG_STRUCTURE.md) for discovery steps.

### Status
- [x] Catalog structure designed (4 critical)
- [x] Team → Group mapping confirmed
- [ ] **Verify**: Where is org structure managed?
- [ ] **DAY 1**: Create category structure in EV
- [ ] **DAY 2**: Build 5 critical catalog items
- [ ] **DAY 3**: Configure assignment rules (based on verified structure)
- [ ] **DAY 4**: Create team views
- [ ] **DAY 5**: Test & train

See [SERVICE_CATALOG_BUILD.md](./SERVICE_CATALOG_BUILD.md) for build instructions.

### Critical 4 (Build These First)
| Category | Volume | Assigned To |
|----------|--------|-------------|
| Quick Fixes (Password) | **Highest** | Helpdesk |
| Equipment | High | Inventory |
| Field Services | Medium | Field Techs |
| Applications | Low-Medium | Enterprise |

### Routing Strategy (Pending Verification)
**Option A**: If Entities exist → Route by requestor's entity + category
**Option B**: If only Groups → Route by category selection only
**Option C**: If Location populated → Route field techs by location

### Result After Week 1
- 80% of tickets auto-categorized
- 80% of tickets auto-assigned to Groups
- Teams see only their work
- Supervisor can view by category

## Next Steps

1. Complete category implementation
2. Fix email notifications
3. Build supervisor dashboard
4. Resume Phase 1 integrations (AD sync, Teams app)
