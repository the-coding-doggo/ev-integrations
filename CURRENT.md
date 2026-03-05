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

**Current**: Build Service Catalog NOW

### Status
- [x] Groups confirmed (Helpdesk, Field Techs, Enterprise, Inventory, Network, Systems)
- [x] Service Catalog structure designed
- [ ] **NEXT**: Create categories in EV
- [ ] Create 5 critical catalog items
- [ ] Set assignment rules (Group-based)
- [ ] Create team views
- [ ] Test end-to-end

See [CATEGORY_BUILD_GUIDE.md](./CATEGORY_BUILD_GUIDE.md) for step-by-step build.

### Build These 5 Items First (~2 hours total)

| Order | Catalog Item | Category | Routes To |
|-------|--------------|----------|-----------|
| 1 | Password Unlock | Quick Fixes | Helpdesk |
| 2 | New Hire Equipment | Equipment | Inventory |
| 3 | Schedule Field Visit | Field Services | Field Techs |
| 4 | MyAvatar Support | Applications | Enterprise |
| 5 | Munis Support | Applications | Enterprise |

### Immediate Result
- Tickets auto-route to correct team
- Each team sees only their queue
- No more hunting through mixed pool
- Specific forms per request type

### After Works
- Add Equipment Return
- Add Hardware Repair
- Add Emergency Field Visit
- Build supervisor dashboard
- Add email notifications

## Next Steps

1. Complete category implementation
2. Fix email notifications
3. Build supervisor dashboard
4. Resume Phase 1 integrations (AD sync, Teams app)
