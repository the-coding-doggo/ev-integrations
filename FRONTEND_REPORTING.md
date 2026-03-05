# Frontend & Reporting Options

## Overview

EasyVista provides multiple paths for ticket visibility and reporting:

| Approach | Effort | Customization | Best For |
|----------|--------|---------------|----------|
| EV Service Apps | Low | Medium | Codeless dashboards, portals |
| PowerBI Integration | Medium | High | Executive reporting, analytics |
| Custom JS Frontend | High | Full | External portals, custom UX |
| Native Reports | Low | Low | Standard operational reports |

---

## 1. EV Service Apps (Recommended First)

### What It Is
Codeless platform for building micro apps, portals, and interactive dashboards.

### Widget Library
- **Layout**: 12/6/4/3/2 column grids
- **Charts**: Area, Bar, Column, Line, Spline, Multi, Benchmark, Bullet, Funnel, Gauge, KPI, Pie/Donut, Pyramid, Treemap
- **Maps**: Google Maps
- **EasyVista**: Knowledge Base, Search, Virtual Agent, Discussion
- **Custom**: HTML Script (JavaScript support)

### JavaScript Support
HTML Script widget supports:
```javascript
// Access widget data
const widget = EVServicesApps.getWidgetById('widget_id');
const data = widget.getData();
const columns = widget.getColumns();
```

### Data Sources
- Service Manager (native)
- REST APIs (real-time third-party)
- CSV files (local/online)
- HTML Scripts

### When to Use
- Manager dashboards
- Team-specific portals
- Self-service interfaces
- Quick wins without coding

---

## 2. PowerBI Integration

### Integration Method
**No native connector** - uses REST API.

### Authentication
- Basic auth (login/password)
- Access Token (recommended)

### API Endpoint
```
https://{server}/api/v1/{account}/{resource}
```

### Key Resources for PowerBI
| Resource | Use Case |
|----------|----------|
| `/requests` | Ticket data (incidents, changes, problems) |
| `/assets` | Asset inventory |
| `/employees` | User/technician data |
| `/actions` | Ticket activities |
| `/slas` | SLA metrics |

### Performance Considerations
⚠️ **Critical**: Large data extraction impacts platform performance

**Recommendations**:
- Schedule refreshes during off-hours
- Use dedicated report server for international orgs
- Filter data at API level, not in PowerBI
- Implement incremental refresh

### Pre-built Templates Available
- Incident Dashboard
- XLA Management
- SLA Compliance

### When to Use
- Executive dashboards
- Cross-system analytics
- Advanced visualizations
- Trend analysis

---

## 3. Custom JavaScript Frontend

### REST API Capabilities
Full CRUD access to Service Manager data.

### Authentication Options
1. **Access Token** (secure, preferred)
2. **Basic Auth** (simple, legacy)

### Key Endpoints for Custom Frontends
```
GET    /api/v1/{account}/requests          # List tickets
GET    /api/v1/{account}/requests/{id}     # Ticket details
POST   /api/v1/{account}/requests          # Create ticket
PUT    /api/v1/{account}/requests/{id}     # Update ticket
GET    /api/v1/{account}/actions           # List actions
POST   /api/v1/{account}/requests/{id}/actions  # Add action
```

### Building a Custom Portal

#### Option A: React/Vue/Angular App
```typescript
// Example: Fetch open incidents
const response = await fetch(
  'https://ev.company.com/api/v1/ITSM/requests?catalog_id=1&status_id=1',
  {
    headers: {
      'Authorization': 'Basic ' + btoa('api_user:token'),
      'Content-Type': 'application/json'
    }
  }
);
const tickets = await response.json();
```

#### Option B: Embed in SharePoint/Teams
Use Service Apps with IFrame embed or build standalone SPA.

### When to Use
- Public-facing portals
- Highly customized UX
- Integration with other systems
- Mobile-first experiences

---

## 4. Native Reporting

### Report Builder
- Built into Service Manager
- Parent queries + filters + views
- Export: PDF, CSV
- Scheduling + alerts

### Quick Dashboard
- Notification bar counters
- Late actions, approvals
- Auto-refresh

### Canvas Pages (2025.2+)
- Personal workspaces
- Widgets: Clock, Charts, News, Text
- Set as home page

### When to Use
- Operational reports
- Standard ticket lists
- Scheduled distribution
- No external tools needed

---

## Decision Matrix

| Need | Solution | Effort |
|------|----------|--------|
| Manager dashboard | Service Apps | 1-2 days |
| Team portal with custom UX | Service Apps + HTML widgets | 3-5 days |
| Executive analytics | PowerBI | 1-2 weeks |
| Cross-system reporting | PowerBI + API | 2-3 weeks |
| Public ticket portal | Custom JS frontend | 2-4 weeks |
| Mobile app | Custom JS + API | 3-6 weeks |

---

## Implementation Recommendations

### Phase 1: Immediate Value
**EV Service Apps**
- Build manager dashboard
- Create team-specific portals
- Use existing widget library

### Phase 2: Analytics
**PowerBI**
- Connect to REST API
- Import pre-built templates
- Schedule off-hours refresh

### Phase 3: Custom Experience
**Custom JS Frontend**
- Build public portal
- Integrate with M365/Teams
- Mobile optimization

---

## Documentation Links

- [Service Apps Widgets](https://docs.easyvista.com/v1/docs/apps-widgets-library/)
- [REST API Reference](https://docs.easyvista.com/v1/docs/webservice-rest/)
- [PowerBI Integration](https://docs.easyvista.com/v1/docs/Microsoft-Power-BI-Integration/)
- [HTML Script Widget](https://docs.easyvista.com/v1/docs/apps-widget-html-script/)
- [Canvas Pages](https://docs.easyvista.com/v1/docs/service-manager-canvas-pages/)
