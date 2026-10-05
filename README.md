# ClubSphere — Student Club and Event Governance Platform

ClubSphere is a web-based Student Club and Event Governance Platform designed for ITM Skills University. It replaces fragmented manual workflows—such as physical paper approval slips, uncoordinated venue registers, and personal club budget notebooks—with a unified, auditable system governing 46 student clubs and 320 annual campus events.

---

## Problem Statement and Operational Baseline

Prior to ClubSphere, student club event operations suffered from four primary structural bottlenecks:

1. **Approval Latency (11 Days to <= 3 Days SLA):** Paper slips required sequential physical signatures across Faculty Coordinators, the Dean of Student Affairs, and Campus Security, averaging 11 days (264 hours) per event. ClubSphere enforces a 72-hour end-to-end SLA (72.73% turnaround reduction) with automated escalations and a fast-track rule for zero-budget classroom sessions.
2. **Venue Double-Bookings (17 Annual Clashes to 0):** Decoupled paper approvals allowed multiple clubs to book the same venue simultaneously (5.31% historical clash rate across 320 events). ClubSphere introduces an atomic `SOFT_HOLD` locking mechanism with mandatory +/- 30-minute setup and teardown buffers backed by PostgreSQL GiST exclusion constraints.
3. **Opaque Club Budgets and Unreconciled Advances:** Club finances tracked in personal notebooks led to lost receipts and carryover disputes. ClubSphere maintains a persistent multi-year institutional ledger with automated fund earmarking upon Tier-2 sanction and a strict 7-working-day post-event receipt reconciliation gate.
4. **Annual Leadership Handover Friction:** Annual transitions of final-year student leadership caused loss of historical event records and credentials. ClubSphere provides a formal digital handover protocol with co-signed audit trails and automated SHA-256 QR-verified PDF participation certificates.

---

## Key Features and Functional Requirements

- **Interactive 4-Step Governance Workflow (`index.html`):**
  - **Step 1 (Proposal Intake and Validation):** Enforces Boundary Value Analysis (BVA) constraints on expected attendees (10 to 2,000), venue fire-safety capacities, and requested budgets (INR 0 to INR 1,50,000).
  - **Step 2 (Decoupled Venue Soft-Hold Engine):** Acquires an opaque `HoldToken` via `IVenueBookingPort` (Stamp Coupling) with +/- 30-minute buffer enforcement and live clash simulation.
  - **Step 3 (3-Tier Sequential Approval Pipeline):** Routes proposals through Tier-1 (Faculty Coordinator, 24h SLA), Tier-2 (Dean of Student Affairs, 48h SLA), and Tier-3 (Campus Security Desk, 72h SLA). Supports auto-fast-tracking (`FR-05`) when `Budget == 0`, `Attendees <= 50`, and `VenueType == CLASSROOM`.
  - **Step 4 (Governance Seal and QR Certificates):** Generates printable gate passes, exports signed JSON audit payloads, and issues bulk QR-verified participation certificates.
- **Campus Overview Dashboard:** Real-time KPIs and ledger summaries across all 46 student clubs and 320 annual events.
- **Venue Clash and Buffer Engine:** Visualizes certified fire-safety capacities, active/maintenance venue statuses, and time-slot reservation locks.
- **Club Budget Ledger and 7-Day Receipt Gate:** Tracks allocated, earmarked, and spent funds per club, automatically blocking new paid proposals if past event receipts remain unsettled beyond 7 working days.
- **Annual Leadership Handover:** Co-signed role transfer state machine (`INITIATED` -> `PENDING_SIGN` -> `COMPLETED`) preserving multi-year institutional continuity.
- **Tamper-Evident SHA-256 Audit Verifier:** Cryptographic hash chain (`prev_hash` + canonical payload) with interactive tamper simulation to verify `NFR-03` immutability.
- **SEPM Case Study Evidence Module:** Built-in interactive calculators and verification tables for agile velocity forecasting, Boundary Value Analysis (BVA), decision tables, Defect Removal Efficiency (DRE), and RMMM risk matrices.

---

## Repository Structure

```text
.
├── index.html                                      # Self-contained interactive ClubSphere web prototype
├── itm_logo.jpg                                    # Institutional branding asset
├── SoftwareEnginnering.docx                        # Case study problem specification document
├── Deliverable_01_SRS.pdf                          # Deliverable 1: IEEE-830 Software Requirements Specification
├── Deliverable_02_UML_Package.pdf                  # Deliverable 2: UML Architecture & Loose Coupling Design
├── Deliverable_03_Project_Plan.pdf                 # Deliverable 3: WBS, Critical Path Method (CPM) & Gantt Plan
├── Deliverable_04_Release_Forecast.pdf             # Deliverable 4: Agile Velocity & 8-Sprint Release Forecast
├── Deliverable_05_Test_Plan_and_Evidence.pdf       # Deliverable 5: BVA, Decision Tables, Traceability & 95% DRE
├── Deliverable_06_Risk_Register_and_Closure_Note.pdf # Deliverable 6: RMMM Risk Register & 9-Month Handover Plan
└── diagrams/                                       # High-resolution vector architecture & SEPM diagrams (SVG)
    ├── activity_diagram.svg                        # Swimlane workflow across Club, Faculty, Dean & Security
    ├── class_diagram.svg                           # Domain model & port-adapter interfaces (IVenueBookingPort)
    ├── gantt_chart.svg                             # 16-week project schedule across 8 two-week sprints
    ├── network_critical_path.svg                   # AON network diagram & critical path analysis
    ├── risk_matrix_diagram.svg                     # Probability x Impact risk exposure matrix
    ├── sequence_diagram.svg                        # End-to-end soft-hold, approval & event bus sequence
    ├── srs_context_architecture.svg                # System context & layered architecture diagram
    ├── state_diagram.svg                           # Dual-coupled Proposal & VenueSlot state machines
    ├── test_bva_dre_diagram.svg                    # BVA boundary partitions & phase-wise DRE metrics
    ├── use_case_diagram.svg                        # Actor-goal use case model across 5 campus roles
    ├── velocity_forecast_chart.svg                 # Sprint velocity burn-up & scope cut forecast (96 SP)
    └── wbs_tree_diagram.svg                        # Work Breakdown Structure (WBS) hierarchy
```

---

## Software Engineering and Project Management (SEPM) Deliverables Summary

| Deliverable | Document File | Core Engineering Artifact and Quantitative Baseline |
| :--- | :--- | :--- |
| **Deliverable 01** | `Deliverable_01_SRS.pdf` | 11 Functional Requirements (`FR-01` to `FR-11`), 6 Non-Functional Requirements (`NFR-01` to `NFR-06`), context architecture, and quantified KPI targets (72.73% approval time reduction, 0.0531 -> 0.0000 clashes/event). |
| **Deliverable 02** | `Deliverable_02_UML_Package.pdf` | Use Case, Class, Sequence, State, and Activity diagrams enforcing **Loose Coupling** (Stamp/Message coupling via `IVenueBookingPort` and `HoldToken`) and **High Functional Cohesion**. |
| **Deliverable 03** | `Deliverable_03_Project_Plan.pdf` | Work Breakdown Structure (WBS), Activity-on-Node (AON) Critical Path Method (CPM) schedule, and resource allocation for a 4-student final-year engineering team. |
| **Deliverable 04** | `Deliverable_04_Release_Forecast.pdf` | 96 Story Point (SP) backlog across 8 two-week sprints (16 weeks). Empirical velocity from Sprints 1-3 (`11, 14, 13 SP`; mean `12.67 SP/sprint`) forecasting `84 + 12 = 96 SP` capacity with MoSCoW scope governance. |
| **Deliverable 05** | `Deliverable_05_Test_Plan_and_Evidence.pdf` | Requirements Traceability Matrix (RTM), Boundary Value Analysis (BVA) on attendees and budget, Decision Table routing (`R1`-`R8`), and **95.0% Defect Removal Efficiency (DRE)** (`38` pre-release defects caught out of `40` total). |
| **Deliverable 06** | `Deliverable_06_Risk_Register_and_Closure_Note.pdf` | Risk Mitigation, Monitoring, and Management (RMMM) register addressing the **9-month team graduation constraint**, Dockerized deployment runbooks, and junior maintainer knowledge transfer (KT) sign-off. |

---

## Running the Interactive Prototype Locally

The prototype in `index.html` is built with zero external runtime dependencies using standard HTML5, CSS3, and vanilla ES6+ JavaScript (utilizing the Web Crypto API for real-time SHA-256 hash chaining).

### Option 1: Direct Browser Launch
Open `index.html` directly in any modern web browser:
```bash
open index.html
```

### Option 2: Local HTTP Server
Serve the repository root over a local static server:
```bash
python3 -m http.server 8000
```
Then navigate to `http://localhost:8000` in your browser.

---

## Verification and Testing Walkthrough

1. **Role Switching:** Use the top navigation bar to switch between **Club Leader**, **Faculty Coordinator**, **Dean of Student Affairs**, **Campus Security Desk**, and **System Auditor** views.
2. **Fast-Track Rule (`FR-05`) Verification:** In the **Propose -> Approve Stepper** tab, click **Test INR 0 Classroom Fast-Track** to verify automatic Tier-2 Dean bypass for zero-budget classroom sessions (`<= 50` attendees).
3. **Atomic Venue Clash Prevention (`FR-03`):** Advance to **Step 2 (Venue Soft-Hold)** and click **Simulate Competing Club Booking Attempt** to trigger the optimistic locking / GiST overlap rejection.
4. **Cryptographic Audit Verification (`NFR-03`):** Navigate to the **SHA-256 Audit Verify** tab and toggle **Simulate Database Tampering (+INR 1)** to observe real-time hash-chain invalidation across subsequent blocks.
5. **SEPM Metrics Inspection:** Open the **SEPM Forecast, BVA & Risks** tab to test live Boundary Value Analysis inputs and inspect the sprint velocity math, decision tables, and RMMM closure criteria.
