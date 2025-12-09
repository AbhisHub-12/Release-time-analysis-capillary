# Changelog - Release Duration Analysis Report

All notable changes to this report will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project uses version numbers: MAJOR.MINOR (e.g., 2.1, 3.0).

## [3.0] - 2025-12-09 - FINAL VALIDATED RECOMMENDATIONS

### Added
- **EXCLUSION_LIST_RECOMMENDATIONS_FINAL.md** - Complete validated recommendations
- Comprehensive Facets resource type analysis from state file
- Extracted all 33 Facets types with instance counts and TF resource mappings
- Cross-referenced Facets UI with state file to validate recommendations
- Final exclusion list additions: `iam` (confirmed) and `aurora` (safe)

### Fixed
- **Critical correction**: `application` is NOT a Facets resource type
  - `module.application` is a root-level legacy module, not a Facets resource
  - Cannot be excluded via `outputs_filtered` list (wrong module pattern)
  - Confirmed: No `module.level2.module.application_*` instances in state file
- **IAM type verification**: Confirmed `iam_policy` exists in Facets UI (not just `iam`)
- **Ingress exclusion**: Confirmed `ingress` must NOT be excluded (critical for routing)

### Analysis Findings
- 33 Facets resource types found in state file
- Top types: k8s (514 instances), mysql (247), service (222), grafana (107)
- Top 3 critical modules analysis:
  1. `module.application` (577K deps) - Root legacy module, NOT excludable
  2. `module.level2.module.ingress_intouch-a` (444K deps) - Must keep in outputs
  3. `module.level2.module.application_roles` (44K deps) - IAM module, add `iam` to exclusion

### Recommendations
- ✅ **Add to exclusion:** `iam` (10 instances, confirmed in UI as `iam_policy`)
- ✅ **Add to exclusion:** `aurora` (30 instances, safe to exclude)
- ❌ **Do NOT add:** `application` (not a Facets resource type)
- ❌ **Do NOT add:** `ingress` (critical infrastructure, must keep)
- Expected impact: 10-15% dependency reduction, 1-2 min release time improvement

---

## [2.1] - 2025-12-08

### Added
- Detailed dependency breakdowns for all Top 10 modules
- Shows where dependencies come from (module.level2.* paths)
- Explanation: module.level2 contains ~5,000 resources
- Summary box confirming 90-99.8% dependencies from module.level2
- Version number in header
- Changelog section in HTML report (later removed per user feedback)

### Changed
- Removed visible changelog from HTML report (keep only in CHANGELOG.md)

---

## [2.0] - 2025-12-08 - MAJOR CORRECTIONS

### Fixed
- **Terminology**: Changed "Facets Type/Name" → "Resource Type/Name"
- **Time estimates**: Corrected "Hours" → "16-27 min" based on actual graph data
- **Unrealistic timeline**: Removed "150+ minutes" → Changed to realistic "16-17 min for Nightly"
- Added clarification: module.level2 is MODULE PATH, not dependency source

### Changed
- Moved "Understanding Dependencies" section to beginning (after Executive Summary)
- Removed confusing "TF Resources" column from tables
- Updated terminology box with correct definitions

### Removed
- Removed all incorrect "Dependency Breakdown" sections showing module.level2 as source
  (These were confusing - module.level2 is the path, not the source)

---

## [1.0] - 2025-12-04 - INITIAL RELEASE

### Added
- Initial comprehensive dependency analysis
- Identified 5,452 resources with 1,483,106 dependency edges
- Found critical bottlenecks:
  - example_ingress: 441,210 dependencies
  - mysql_grants: 107,440 dependencies
  - mysql_user: 107,372 dependencies
- Created interactive visualizations with Chart.js:
  - Resource Type Distribution (doughnut chart)
  - Non-Production Environment Trends (line chart)
  - Production Environment Trends (line chart)
  - Resource Type Dependency Flow (bar chart)
- Deployed to GitHub Pages: https://abhishub-12.github.io/Release-time-analysis-capillary/

### Analysis Features
- Executive Summary with key metrics
- Understanding Dependency Analysis (Vertices, Edges, Fan-In, Fan-Out)
- Top 10 Modules by Dependency Count
- Resource Type Dependency Graph
- Dependencies Optimization Analysis
- Actionable Recommendations
- Next Steps for implementation

---

## Version Numbering System

- **MAJOR version** (e.g., 1.0 → 2.0): Major corrections, restructuring, or significant new sections
- **MINOR version** (e.g., 2.0 → 2.1): New features, additional analysis, incremental improvements
