# Sample Webhook Payloads

Send these to your imported Lite (or Premium Core Engine) workflow to test scoring + routing without real data sources.

## alert_triage — RED tier (severity=critical)

```bash
curl -X POST https://your-n8n.com/webhook/alert-triage-lite \
  -H "Content-Type: application/json" \
  -d '{
    "alert_id": "TEST-ALERT-RED-001",
    "severity": "critical",
    "source": "datadog",
    "description": "Production DB connection pool exhausted on orders-api",
    "config_pack": "alert_triage",
    "affected_service": "orders-api",
    "error_count": 250,
    "customer_facing": "true",
    "environment": "production"
  }'
```

## alert_triage — AMBER tier (severity=high, staging)

```bash
curl -X POST https://your-n8n.com/webhook/alert-triage-lite \
  -H "Content-Type: application/json" \
  -d '{
    "alert_id": "TEST-ALERT-AMBER-001",
    "severity": "high",
    "source": "splunk",
    "description": "Latency spike on staging checkout flow",
    "config_pack": "alert_triage",
    "affected_service": "checkout",
    "error_count": 12,
    "customer_facing": "false",
    "environment": "staging"
  }'
```

## alert_triage — GREEN tier (severity=low, internal)

```bash
curl -X POST https://your-n8n.com/webhook/alert-triage-lite \
  -H "Content-Type: application/json" \
  -d '{
    "alert_id": "TEST-ALERT-GREEN-001",
    "severity": "low",
    "source": "manual",
    "description": "Cert renewal due in 30 days",
    "config_pack": "alert_triage",
    "environment": "internal"
  }'
```

## lead_qual — HOT tier (C-level, demo, referral)

```bash
curl -X POST https://your-n8n.com/webhook/lead-qual-lite \
  -H "Content-Type: application/json" \
  -d '{
    "alert_id": "TEST-LEAD-HOT-001",
    "severity": "n/a",
    "source": "hubspot",
    "description": "Demo request from C-level",
    "config_pack": "lead_qual",
    "company_size": 600,
    "industry": "saas",
    "country": "United States",
    "role_seniority": "C-level",
    "decision_authority": "true",
    "demo_requested": "true",
    "pricing_page_visited": "true",
    "content_downloaded": 4,
    "email_engagement_score": 85,
    "utm_source": "referral",
    "form_completeness": 90,
    "timeline_stated": "immediate need",
    "budget_match": "true"
  }'
```

## lead_qual — WARM tier (VP, demo, paid source)

```bash
curl -X POST https://your-n8n.com/webhook/lead-qual-lite \
  -H "Content-Type: application/json" \
  -d '{
    "alert_id": "TEST-LEAD-WARM-001",
    "severity": "n/a",
    "source": "hubspot",
    "description": "VP requested demo + viewed pricing",
    "config_pack": "lead_qual",
    "company_size": 120,
    "industry": "fintech",
    "country": "United States",
    "role_seniority": "VP",
    "demo_requested": "true",
    "pricing_page_visited": "true",
    "email_engagement_score": 75,
    "utm_source": "paid",
    "form_completeness": 60,
    "timeline_stated": "exploring options",
    "budget_match": "false"
  }'
```

## lead_qual — COLD tier (newsletter signup, no engagement)

```bash
curl -X POST https://your-n8n.com/webhook/lead-qual-lite \
  -H "Content-Type: application/json" \
  -d '{
    "alert_id": "TEST-LEAD-COLD-001",
    "severity": "n/a",
    "source": "hubspot",
    "description": "Newsletter signup, low signal",
    "config_pack": "lead_qual",
    "company_size": 25,
    "industry": "retail",
    "country": "United Kingdom",
    "role_seniority": "IC",
    "demo_requested": "false",
    "pricing_page_visited": "false",
    "content_downloaded": 1,
    "email_engagement_score": 40,
    "utm_source": "paid",
    "form_completeness": 40,
    "timeline_stated": "no timeline",
    "budget_match": "false"
  }'
```

## Negative test — missing required fields → HTTP 400

```bash
curl -X POST https://your-n8n.com/webhook/alert-triage-lite \
  -H "Content-Type: application/json" \
  -d '{ "severity": "critical" }'
```

Expected response:

```json
{
  "valid": false,
  "error": "VALIDATION_FAILED",
  "missing_fields": ["alert_id", "source", "description"],
  "received_fields": ["severity"]
}
```

## Premium — bulk testing via Test Data Generator

The Premium bundle ships with `NEXUS_Test_Data_Generator.json` — an n8n workflow that POSTs all 7 scenarios above (6 healthy + 1 malformed) in one click and returns a PASS/FAIL summary with pass-rate %. See README.md → Test Data Generator section.
