# Note2Self Patterns & Quick Reference

> **note2self**: Quick reference guide for effective note2self documentation patterns. Use this as a template when leaving context for future Copilot sessions.

## 🎯 Quick Start

### What is Note2Self?

A documentation pattern that creates persistent context for GitHub Copilot and developers by embedding strategic comments directly in code and documentation.

**Format**: 
```markdown
> **note2self**: [Your contextual message here]
```

Or in code comments:
```python
# note2self: [Your contextual message here]
```

## 📋 Common Patterns

### 1. Dependency Reference
**When**: Explaining why a specific library/service is used

```python
# note2self: Using Cogpilot's auth-lib v2.1+ for enterprise SSO
# integration. Don't implement custom auth - it violates security
# policy SEC-001 and won't pass compliance audits.
```

### 2. Data Flow Explanation
**When**: Complex data transformations or pipelines

```python
# note2self: City data flows through three stages:
# 1. Ingestion (raw data from sensors) → Cogpilot data-pipeline
# 2. Normalization (standardized format) → Cogpilot transform-service  
# 3. Analytics (insights and metrics) → This service
# Each stage is independently scalable and monitored.
```

### 3. Performance Rationale
**When**: Non-obvious optimization decisions

```python
# note2self: This query is denormalized intentionally. We duplicate
# city names in events table to avoid joins on the hot path. 
# The dashboard loads 10x faster (1.5s → 150ms) with acceptable
# data redundancy. City names change rarely (< 0.01% annually).
```

### 4. Historical Context
**When**: Explaining why code exists in a certain way

```python
# note2self: Legacy support for v1 API format maintained until Q2 2026.
# Three cities (NYC, LA, Chicago) still on old mobile apps that can't
# upgrade immediately due to app store review cycles. Removal tracked
# in issue #456.
```

### 5. Integration Points
**When**: Connecting to external services or systems

```python
# note2self: This webhook receives city boundary updates from the
# admin panel. It invalidates our geometry cache and triggers
# re-calculation of service areas. The admin panel is maintained
# by Cogpilot platform team - see cogpilot/admin-panel repo.
```

### 6. Security Context
**When**: Handling sensitive data or security requirements

```python
# note2self: Citizen PII is encrypted using Cogpilot encryption-lib
# with AES-256-GCM. Keys are rotated quarterly by the key-management
# service. Never log or cache unencrypted PII - violates GDPR/CCPA.
```

### 7. Business Logic
**When**: Domain-specific rules that aren't obvious

```python
# note2self: Cities are considered "active" if they've had any API
# activity in the last 90 days. Inactive cities get archived but not
# deleted (retention policy RET-003). This affects billing and
# resource allocation.
```

### 8. Configuration Rationale
**When**: Explaining specific settings

```yaml
# note2self: Timeout set to 30s because city boundary calculations
# can take 15-25s for large metropolitan areas. Lower timeouts
# cause false failures during peak hours.
timeout: 30
```

### 9. Testing Strategy
**When**: Explaining test approach or coverage

```python
# note2self: Integration tests use Cogpilot test fixtures (test-utils)
# which provide realistic city data. Don't mock the data pipeline -
# we need to test actual data transformations. Mocks hide integration
# bugs that only appear with real city data structures.
```

### 10. Migration Context
**When**: Transitioning between systems or versions

```python
# note2self: Migrating from v1 to v2 city data schema. Both formats
# supported during transition (Nov 2025 - Mar 2026). v1 detector:
# checks for 'cityData' vs 'city_data' field naming. Remove v1 support
# after migration deadline in issue #789.
```

## 📐 Pattern Templates

### Architectural Decision Template
```markdown
> **note2self**: [DECISION] We chose [OPTION A] over [OPTION B] because
> [REASON]. This impacts [COMPONENTS]. Alternative [OPTION B] would have
> [TRADEOFFS]. See [REFERENCE] for full discussion.
```

### Integration Template
```markdown
> **note2self**: [INTEGRATION] This connects to [SERVICE] via [METHOD].
> The [SERVICE] is maintained by [TEAM] in [REPOSITORY]. Key dependency:
> [REQUIREMENT]. Failure mode: [BEHAVIOR].
```

### Performance Template
```markdown
> **note2self**: [OPTIMIZATION] Optimized for [METRIC] by [METHOD].
> Measured improvement: [BEFORE] → [AFTER]. Trade-off: [COST].
> Acceptable because [JUSTIFICATION].
```

### Security Template
```markdown
> **note2self**: [SECURITY] Handles [DATA TYPE] per policy [POLICY ID].
> Encryption: [METHOD]. Key management: [SYSTEM]. Compliance: [STANDARDS].
> Never [PROHIBITED ACTION].
```

## ✅ Quality Checklist

A good note2self comment should:

- [ ] Provide context not available from code alone
- [ ] Explain "why" not just "what"
- [ ] Reference relevant documentation/issues/repos
- [ ] Include measurable details when relevant
- [ ] Be concise but complete
- [ ] Help future developers/Copilot understand decisions
- [ ] Age well (still relevant months later)

## ❌ Anti-Patterns

### Too Obvious
```python
# note2self: This function calculates the sum
def calculate_sum(a, b):
    return a + b
```
**Problem**: Comment adds no value; function name is self-explanatory.

### Too Vague
```python
# note2self: This is important for performance
cache = {}
```
**Problem**: Doesn't explain why or how it affects performance.

### Duplicate Information
```python
# note2self: Returns city population from database
def get_city_population(city_id):
    """Returns the population for the given city."""
    return db.query(f"SELECT population FROM cities WHERE id={city_id}")
```
**Problem**: Repeats what the docstring and function name already convey.

### Outdated Context
```python
# note2self: Temporary workaround until API v1 is fixed (June 2023)
# (Still present in November 2025...)
```
**Problem**: Not maintained; creates confusion about current state.

## 🎨 Markdown Note2Self Variations

### In Documentation Files
```markdown
> **note2self**: This section documents the enterprise-wide authentication
> flow. All Cogcities applications must follow this pattern for SSO.
```

### Callout Boxes
```markdown
> **note2self - Performance**: 
> - Cache hit rate: ~85%
> - Avg response time: 120ms
> - Peak load: 10k req/min
```

### Nested Context
```markdown
> **note2self**: Database schema versioning:
> - v1: Legacy format (deprecated)
> - v2: Current standard (use this)
> - v3: In development (see cogpilot/schema-evolution)
```

## 🔧 Language-Specific Examples

### Python
```python
# note2self: Async implementation required because city sensor data
# comes from distributed edge nodes with varying latency (50-500ms).
# Sequential fetching would timeout on 30+ sensors per city.
async def fetch_sensor_data(city_id: str) -> List[SensorReading]:
    ...
```

### JavaScript/TypeScript
```typescript
// note2self: React Context used instead of Redux because state is
// simple and localized to city dashboard. Redux would add unnecessary
// complexity. Revisit if state management needs grow beyond 5 components.
const CityContext = React.createContext<CityState>(defaultState);
```

### YAML
```yaml
# note2self: These environment variables are injected by Cogpilot's
# deployment system. Don't hardcode values - they differ per environment
# (dev/staging/prod) and per city deployment.
database:
  host: ${DB_HOST}
  port: ${DB_PORT}
```

### SQL
```sql
-- note2self: Composite index on (city_id, timestamp) because queries
-- always filter by city and sort by time. Single index on city_id alone
-- caused slow queries (2s+) on large datasets. This reduced to <100ms.
CREATE INDEX idx_events_city_time ON events(city_id, timestamp DESC);
```

### Dockerfile
```dockerfile
# note2self: Using Cogpilot base image (cogpilot/node:18-alpine) which
# includes enterprise security patches and monitoring agents. Don't use
# public node images directly - they lack required security compliance.
FROM cogpilot/node:18-alpine
```

## 📚 Real-World Examples

### Example: API Rate Limiting
```python
# note2self: Rate limiting implemented per city, not per user, because
# cities have varying populations and API usage patterns. NYC needs 10x
# the rate limit of smaller cities. Limits defined in city_config table
# and can be adjusted by city admins. Default: 1000 req/min per city.
@rate_limit(key=lambda req: req.path_params['city_id'])
async def city_endpoint(city_id: str):
    ...
```

### Example: Data Retention
```python
# note2self: Sensor data retention: 90 days hot storage (fast queries),
# 2 years cold storage (S3/Glacier), then deleted. Legal requirement
# varies by jurisdiction - some cities need longer retention. Check
# city.data_retention_days before cleanup. EU cities typically need
# 2+ years for GDPR compliance.
def cleanup_old_data(city_id: str):
    retention = get_city_config(city_id).data_retention_days
    ...
```

### Example: Backward Compatibility
```python
# note2self: Supporting both 'cityId' (v1) and 'city_id' (v2) field
# names during API migration. v2 is preferred. v1 support ends Q2 2026.
# Client detection: v1 clients send 'X-API-Version: 1' header.
# See migration guide: docs/api-migration-v1-to-v2.md
def normalize_request(data: dict) -> dict:
    if 'cityId' in data:
        data['city_id'] = data.pop('cityId')  # Convert v1 to v2
    return data
```

## 🚀 Getting Started Checklist

When starting work on a repository:

- [ ] Read all note2self comments in relevant files
- [ ] Check ENTERPRISE_OVERVIEW.md for organizational context
- [ ] Review COPILOT_GUIDE.md for integration patterns
- [ ] Understand existing note2self patterns in the codebase
- [ ] Plan where to add new note2self comments
- [ ] Leave breadcrumbs for future Copilot sessions

## 🔄 Maintenance

Note2self comments require maintenance:

- **Review Quarterly**: Check if context is still accurate
- **Update During Changes**: Keep comments synchronized with code
- **Remove When Obsolete**: Delete outdated temporary context
- **Refine Unclear Comments**: Improve vague or confusing notes

---

> **note2self**: This pattern reference should be updated with new patterns
> as they emerge from real development work. Add examples from actual
> Cogcities repositories to make this guide increasingly useful over time.
