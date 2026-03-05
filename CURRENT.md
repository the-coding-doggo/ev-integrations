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

**Current**: Ticket Categories (Practical Design)

### Status
- [x] Catalog structure designed based on actual ticket types
- [x] Team assignments mapped
- [ ] Create top-level categories in EV
- [ ] Configure assignment rules
- [ ] Build high-volume workflows (Password, Equipment)
- [ ] Create team-specific views
- [ ] Test with real tickets

See [CATEGORY_DESIGN_PRACTICAL.md](./CATEGORY_DESIGN_PRACTICAL.md) for detailed catalog structure.

### Quick Start - Do These First
1. Password Unlock → Helpdesk
2. Equipment Issue → Inventory  
3. Field Services → Field Techs
4. MyAvatar/Munis → Enterprise

These 4 handle 80% of tickets.

## Next Steps

1. Complete category implementation
2. Fix email notifications
3. Build supervisor dashboard
4. Resume Phase 1 integrations (AD sync, Teams app)
