# Polish Law MCP

[![npm](https://img.shields.io/npm/v/@ansvar/polish-law-mcp)](https://www.npmjs.com/package/@ansvar/polish-law-mcp)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)
[![CI](https://github.com/Ansvar-Systems/polish-law-mcp/actions/workflows/ci.yml/badge.svg)](https://github.com/Ansvar-Systems/polish-law-mcp/actions/workflows/ci.yml)
[![MCP Registry](https://img.shields.io/badge/MCP-Registry-green)](https://registry.modelcontextprotocol.io/)
[![OpenSSF Scorecard](https://img.shields.io/ossf-scorecard/github.com/Ansvar-Systems/polish-law-mcp)](https://securityscorecards.dev/viewer/?uri=github.com/Ansvar-Systems/polish-law-mcp)

A Model Context Protocol (MCP) server providing comprehensive access to Polish legislation, including data protection (RODO/GDPR implementing provisions), national cybersecurity system (KSC), cybercrime (Kodeks karny), commercial companies (KSH), and e-services with Polish full-text search.

**MCP Registry:** `eu.ansvar/polish-law-mcp`
**npm:** `@ansvar/polish-law-mcp`
**License:** Apache-2.0

---

## Deployment Tier

**MEDIUM** -- dual tier, free database bundled in npm package.

| Tier | Platform | Database | Content |
|------|----------|----------|---------|
| **Free** | Vercel (Hobby) / npm (stdio) | Core legislation (~100-180 MB) | Key laws (DPA 2018, KSC, Kodeks karny cybercrime, KSH, e-Services Act, Telecommunications Law), FTS search, EU cross-references |
| **Professional** | Azure Container Apps / Docker / Local | Full database (~500 MB - 900 MB) | + All ustawy and rozporzadzenia, UODO decisions and guidance, Supreme Court/Constitutional Tribunal summaries, NIS2 transposition materials |

The full database is larger due to the comprehensive scope of Polish legislation and supplementary regulatory materials. The free tier contains all key data protection, cybersecurity, cybercrime, commercial, and e-services legislation from ISAP.

---

## Data Sources

| Source | Authority | Method | Update Frequency | License | Coverage |
|--------|-----------|--------|-----------------|---------|----------|
| [ISAP](https://isap.sejm.gov.pl) | Kancelaria Sejmu RP (Chancellery of the Sejm) | HTML Scrape / API | Weekly | Government Open Data (public domain per Art. 4 Copyright Act) | All Polish legislation published in Dziennik Ustaw and Monitor Polski |

> Full provenance metadata: [`sources.yml`](./sources.yml)

---

## Quick Start

### Claude Desktop / Cursor (stdio)

```json
{
  "mcpServers": {
    "polish-law": {
      "command": "npx",
      "args": ["-y", "@ansvar/polish-law-mcp"]
    }
  }
}
```

### Vercel Streamable HTTP (ChatGPT / Claude.ai)

Once deployed, the public endpoint will be available at:

```
https://polish-law-mcp.vercel.app/api/mcp
```

---

## Tools

| Tool | Description | Free Tier | Professional |
|------|-------------|-----------|-------------|
| `get_provision` | Retrieve a specific article from a Polish law or code | Yes | Yes |
| `search_legislation` | Full-text search across all Polish legislation (Polish) | Yes | Yes |
| `list_laws` | List all available laws with metadata | Yes | Yes |
| `get_law_structure` | Get table of contents / structure of a law or code | Yes | Yes |
| `get_provision_eu_basis` | Cross-reference Polish law to EU directives/regulations | Yes | Yes |
| `search_rozporzadzenia` | Search rozporzadzenia (regulations) and executive instruments | No (upgrade) | Yes |
| `get_uodo_guidance` | Retrieve UODO decisions and guidance | No (upgrade) | Yes |

---

## Key Legislation Covered

| Law | Identifier | Domain | Key Topics |
|-----|-----------|--------|------------|
| **DPA 2018** | Ustawa o ochronie danych osobowych (Dz.U. 2018 poz. 1000) | Data Protection | GDPR/RODO implementing provisions, UODO structure and powers, administrative procedures, certification |
| **KSC** | Ustawa o krajowym systemie cyberbezpieczenstwa (Dz.U. 2018 poz. 1560) | Cybersecurity | National cybersecurity system, CSIRT teams, incident reporting, essential services operators, NIS implementation |
| **Kodeks karny (cybercrime)** | Art. 267-269b | Cybercrime | Unauthorized access (267), data destruction (268), system sabotage (268a), critical system sabotage (269), hacking tools (269b) |
| **KSH** | Kodeks spolek handlowych (Dz.U. 2000 nr 94 poz. 1037) | Company Law | Sp. z o.o. and S.A. companies, partnerships, corporate governance, shareholders' rights |
| **e-Services Act** | Ustawa o swiadczeniu uslug droga elektroniczna (Dz.U. 2002 nr 144 poz. 1204) | e-Commerce | Electronic service providers, hosting liability, commercial communications, consumer information |
| **Telecommunications Law** | Prawo telekomunikacyjne (Dz.U. 2004 nr 171 poz. 1800) | Telecommunications | Telecom regulation, UKE authority, licensing, data retention, lawful interception |

---

## Database Estimates

| Component | Free Tier | Full (Professional) |
|-----------|-----------|---------------------|
| Core codes and key laws | ~70-130 MB | ~70-130 MB |
| All ustawy and rozporzadzenia | -- | ~300-500 MB |
| UODO decisions and guidance | -- | ~50-100 MB |
| Case law summaries | -- | ~70-150 MB |
| Cross-references and metadata | ~5 MB | ~15 MB |
| **Total** | **~100-180 MB** | **~500 MB - 900 MB** |

**Delivery strategy:** Free-tier DB bundled in npm package (Strategy A -- fits within Vercel 250 MB function limit). If final size exceeds 250 MB after ingestion, switch to Strategy B (runtime download from GitHub Releases).

---

## Regulatory Context

- **Supervisory Authority:** UODO (Urzad Ochrony Danych Osobowych - Personal Data Protection Office) with growing enforcement activity
- **RODO** is the Polish name for GDPR (Rozporzadzenie o Ochronie Danych Osobowych); the DPA 2018 provides national implementing provisions
- **KSC** established Poland's national cybersecurity system with three CSIRT teams: CSIRT NASK (private sector), CSIRT GOV (government), CSIRT MON (military)
- **NASK** (Naukowa i Akademicka Siec Komputerowa) operates the national cybersecurity research center
- Polish legal texts are **explicitly excluded from copyright** under Art. 4 of the Copyright Act, ensuring free reuse
- **Dziennik Ustaw** (Journal of Laws) is the sole authoritative publication vehicle for Polish legislation
- Poland is the **EU's sixth largest economy** and the largest in Central and Eastern Europe
- NIS2 transposition is expected to expand KSC significantly with new entity categories and reporting obligations

---

## Development

```bash
# Clone the repository
git clone https://github.com/Ansvar-Systems/polish-law-mcp.git
cd polish-law-mcp

# Install dependencies
npm install

# Build
npm run build

# Run tests
npm test

# Run contract tests
npm run test:contract

# Build database (requires raw data in data/ directory)
npm run build:db

# Build free-tier database
npm run build:db:free

# Run drift detection
npm run drift:detect

# Full validation
npm run validate
```

---

## Architecture

```
polish-law-mcp/
├── .github/
│   ├── workflows/
│   │   ├── ci.yml                    # Test + lint + security scan
│   │   ├── publish.yml               # npm publish on version tags
│   │   ├── check-source-updates.yml  # Data freshness monitoring
│   │   └── drift-detect.yml          # Upstream drift detection
│   ├── SECURITY.md
│   ├── SECURITY-SETUP.md
│   └── ISSUE_TEMPLATE/
│       └── data-error.md
├── data/
│   └── .gitkeep
├── fixtures/
│   ├── golden-tests.json             # 12 contract tests
│   ├── golden-hashes.json            # 6 drift detection anchors
│   └── README.md
├── scripts/
│   ├── build-db.ts
│   ├── build-db-free.ts
│   ├── download-free-db.sh
│   ├── ingest.ts
│   ├── drift-detect.ts
│   └── check-source-updates.ts
├── src/
│   ├── server.ts
│   ├── db.ts
│   └── tools/
│       ├── get-provision.ts
│       ├── search-legislation.ts
│       ├── list-laws.ts
│       ├── get-law-structure.ts
│       ├── get-provision-eu-basis.ts
│       ├── search-rozporzadzenia.ts
│       └── get-uodo-guidance.ts
├── __tests__/
│   ├── unit/
│   ├── contract/
│   │   └── golden.test.ts
│   └── integration/
├── sources.yml
├── server.json
├── package.json
├── tsconfig.json
├── vercel.json
├── CHANGELOG.md
├── LICENSE
└── README.md
```

---

## Notes on Polish Data Protection Landscape

**RODO (GDPR)** and the **DPA 2018** have distinctive Polish implementation features:

- Poland's DPA 2018 focuses on **UODO structure and procedures** rather than substantive data protection rules (which come directly from GDPR/RODO)
- **UODO** has been growing in enforcement activity, with increasing fine amounts
- Poland maintains **sector-specific data protection provisions** in telecommunications and health legislation
- **Age of digital consent** set at 16 (GDPR default)

**KSC** (National Cybersecurity System) is comprehensive:
- Three-CSIRT model: **CSIRT NASK** (private sector), **CSIRT GOV** (government), **CSIRT MON** (military)
- **Essential services operators** must implement security measures and report incidents
- **Digital service providers** have separate obligations
- NIS2 transposition will significantly expand the scope and requirements

**Kodeks karny** cybercrime provisions (Arts. 267-269b) cover:
- Art. 267: Unauthorized access to information
- Art. 268: Destruction or alteration of data
- Art. 268a: Disruption of automated data processing
- Art. 269: Sabotage of computer systems of particular importance
- Art. 269b: Manufacturing or possession of hacking tools

Poland is the **largest economy in Central and Eastern Europe** and a key market for EU-wide compliance programs.

---

## Related Documents

- [MCP Quality Standard](../../mcp-quality-standard.md) -- quality requirements for all Ansvar MCPs
- [MCP Infrastructure Blueprint](../../mcp-infrastructure-blueprint.md) -- infrastructure implementation templates
- [MCP Deployment Tiers](../../mcp-deployment-tiers.md) -- free vs. professional tier strategy
- [MCP Server Registry](../../mcp-server-registry.md) -- operational registry of all MCPs
- [MCP Remote Access](../../mcp-remote-access.md) -- public Vercel endpoint URLs

---

## Security

Report vulnerabilities to **security@ansvar.eu** (48-hour acknowledgment SLA).

See [SECURITY.md](.github/SECURITY.md) for full disclosure policy.

---

**Maintained by:** Ansvar Systems Engineering
**Contact:** hello@ansvar.eu
