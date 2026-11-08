# Unit 7 Memo: Asset Criticality (CR / IR / AR)

**Course:** IA 202L4, Risk-Vulnerability Analysis
**Scope:** Five fictional assets for a mid-size fintech/healthcare-services company
**Scale:** Low (L), Medium (M), High (H), per the CVSS Environmental Security Requirements

## 1. Summary of Ratings

| # | Asset | CR | IR | AR |
|---|-------|----|----|----|
| 1 | Payment database (PostgreSQL cluster) | H | H | M |
| 2 | Public marketing website | L | M | L |
| 3 | Internal wiki | M | L | L |
| 4 | Infusion pump gateway (OT/medical) | M | H | H |
| 5 | CI/CD build server | M | H | M |

CR = Confidentiality Requirement, IR = Integrity Requirement, AR = Availability Requirement.

## 2. Justifications

### 1. Payment database
- **CR: High.** It stores cardholder and bank account data. A breach triggers PCI DSS obligations, notification costs, and loss of customer trust.
- **IR: High.** Altered balances or transaction records cause direct financial loss and make reconciliation and audits unreliable.
- **AR: Medium.** Downtime stops payments, but the cluster has replicas and failover, and a short outage is survivable and recoverable.

### 2. Public marketing website
- **CR: Low.** Content is already public, so there is nothing confidential to disclose.
- **IR: Medium.** Defacement or injected malicious scripts damage reputation and can harm visitors, even though no sensitive data lives here.
- **AR: Low.** Brief outages cost some leads but do not disrupt operations, and a CDN and static backups allow quick restoration.

### 3. Internal wiki
- **CR: Medium.** It holds architecture notes, runbooks, and some internal procedures. Exposure helps attackers but contains no regulated data by policy.
- **IR: Low.** Pages are versioned and low-stakes; incorrect edits are easy to detect and revert.
- **AR: Low.** Staff can work without it for a day or more, and nothing time-critical depends on it.

### 4. Infusion pump gateway (OT/medical)
- **CR: Medium.** Messages may include patient identifiers and dosing records, which is regulated, but the gateway holds only data in transit and limited records.
- **IR: High.** Tampered dosing commands or settings could cause patient harm. Integrity failure here is a safety issue.
- **AR: High.** Loss of the gateway disrupts monitoring and drug delivery for patients on life-critical therapy, so uptime is essential.

### 5. CI/CD build server
- **CR: Medium.** It holds signing keys, deployment tokens, and source code, but secrets are held in a vault and rotated, which limits what the server itself exposes.
- **IR: High.** A compromised pipeline can ship malicious code to every downstream environment (a supply-chain risk), so trusting build output is critical.
- **AR: Medium.** An outage delays releases but does not stop production, and emergency fixes can follow a manual path.

## 3. Mapping to Asset-Management Fields

An asset-management system needs more than three ratings. Each rating must be traceable, owned, and reviewable.

| Field | Purpose | Example (Payment database) |
|-------|---------|----------------------------|
| Asset ID | Unique key for joining to vulnerability scan results | AST-0001 |
| Asset name / type | Human-readable identity and category | Payment DB / database |
| Business owner | Person accountable for the ratings | Head of Payments |
| Technical owner / custodian | Person who patches and operates it | Platform DBA team |
| Business function | What the asset supports | Transaction processing |
| Data classification | Drives the CR rating | Restricted (PCI) |
| Regulatory scope | Explains why ratings are set | PCI DSS, SOX |
| CR / IR / AR | Inputs to Environmental scoring | H / H / M |
| Rating justification | Audit trail for each rating | "Cardholder data; fraud exposure" |
| RTO / RPO | Evidence behind the AR rating | RTO 1 h, RPO 5 min |
| Environment | Production, staging, or dev | Production |
| Network exposure | Internet-facing, internal, or isolated | Internal, behind app tier |
| Dependencies | Upstream and downstream systems | Payment API, ledger service |
| Safety flag | Supports the Safety supplemental metric | N (Y for infusion pump gateway) |
| Last reviewed / next review | Keeps ratings from going stale | 2026-11-08 / 2027-05-08 |

## 4. Notes

- Ratings are not permanent. They should be reviewed on a schedule and whenever an asset's role, data, or exposure changes.
- CR, IR, and AR are set by the asset's business context, not by any specific vulnerability. The same flaw can score differently on assets 1 and 3 because of these requirements.
- All five assets are fictional and the ratings are illustrative.
