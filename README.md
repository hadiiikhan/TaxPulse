# TaxPulse
# TaxPulse – Tax & Financial Regulation Intelligence Engine

**TaxPulse** is a backend platform that tracks, versions, and analyzes Canadian tax and financial regulations in real time.  
Unlike traditional tax calculators, TaxPulse continuously monitors changing CRA rules, recalculates user outcomes, and reports the financial impact of each regulatory update with full auditability.

---

## 🚀 Key Features

### 🔹 1. Versioned Tax Rules Engine
TaxPulse stores every tax regulation—past and present—as a versioned rule set:
- Federal & provincial tax brackets  
- Basic personal amounts  
- Deductions & credits  
- Surtaxes and provincial variations  

Each rule includes:
- `effective_from` and `effective_to`  
- Full parameters in JSON (thresholds, rates, caps)  
- Source metadata  

This allows precise calculations for **any year**, past or future.

---

### 🔹 2. Regulation Update Pipeline
TaxPulse automatically ingests new rules from regulatory sources or admin inputs.

The pipeline:
1. Detects changes  
2. Closes outdated rules  
3. Inserts new versions  
4. Logs 100% of differences  
5. Triggers user recalculations  

All changes are stored in a **regulation_change_log** for compliance and audit.

---

### 🔹 3. Real-Time Impact Analyzer
Whenever rules change, TaxPulse:
- Re-runs tax calculations for all affected users  
- Computes `old_liability`, `new_liability`, and `delta`  
- Generates per-user explanations like:

> “Your 2025 tax refund increased by \$320 due to updated CRA tax brackets.”

This makes TaxPulse a **regulation-aware financial advisor**.

---

### 🔹 4. RRSP, TFSA, CPP, EI, FHSA, and More
TaxPulse supports multiple financial rule sets beyond basic taxation:
- **RRSP** yearly limits, carry-forward, deduction impact  
- **TFSA** annual contribution room + over-contribution detection  
- **CPP** contribution caps  
- **EI** inflation-adjusted premiums  
- **FHSA** contribution limits  
- **GST/HST credit** thresholds  
- **CCB** eligibility models  

Each follows the same versioned, update-friendly structure.

---

### 🔹 5. RESTful API for Developers
TaxPulse exposes flexible REST endpoints:
- `GET /tax/calculate` – compute federal+provincial tax  
- `GET /regulations/current` – fetch active rules  
- `POST /regulations/update` – ingest new rules  
- `GET /users/{id}/impact` – show rule-change impact summary  
- `GET /finance/rrsp` – compute RRSP room and tax benefits  
- `GET /finance/tfsa` – track TFSA room  

APIs respond in **<150ms** and are ready for integration with:
- Mobile apps  
- Web dashboards  
- Internal tools  
- Financial analysis platforms  

---

### 🔹 6. Architecture Highlights
- **Java 17 + Spring Boot** (Web, Data, Scheduler)  
- **PostgreSQL** with JSONB rule definitions  
- **Liquibase/Flyway** for schema versioning  
- **Asynchronous recalculation jobs**  
- **Rule comparison + diffing engine**  
- **Modular domain-driven design**  

---

## 📊 Data Model Overview

### Example: Tax Rule
```json
{
  "jurisdiction": "CA-ON",
  "rule_type": "TAX_BRACKET",
  "year": 2025,
  "parameters": {
    "brackets": [
      { "threshold": 50000, "rate": 0.15 },
      { "threshold": 100000, "rate": 0.205 }
    ]
  },
  "effective_from": "2025-01-01",
  "effective_to": null
}
{
  "rule_type": "RRSP_LIMIT",
  "year": 2025,
  "parameters": {
    "limit": 31400,
    "carry_forward": true,
    "penalty_threshold": 2000
  }
}
{
  "user_id": 42,
  "old_liability": 6240,
  "new_liability": 5920,
  "difference": -320,
  "description": "Your 2025 tax refund increased by $320 due to updated CRA tax brackets."
}
