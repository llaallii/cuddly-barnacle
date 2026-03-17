# MBSE Framework for Electromechanical Medical Devices — Master Outline

## Document purpose

This outline defines the complete structure of the MBSE Framework Definition — the reference document that will guide every subsequent Enterprise Architect build phase. Each Part (A through G) will be delivered as a separate `.md` file with embedded Mermaid diagrams, reviewed and confirmed before moving to the next.

The framework targets **electromechanical medical devices with active actuation** — devices using pumps, motors, stepper motors, sensors, electronics, and embedded firmware — and covers the full product lifecycle under ISO 13485, IEC 60601-1, ISO 14971, IEC 62304, IEC 62366-1, and ISO 11608-4.

A running **case study** (stepper-motor-driven peristaltic dosing module) is introduced in Part F and threaded through Parts A–E retroactively during the EA build phases.

---

## Framework overview — how the seven parts connect

```mermaid
---
config:
  layout: elk
---
graph TD
    A["<b>Part A</b><br/>Framework Scope<br/>& Purpose"] --> B["<b>Part B</b><br/>Framework<br/>Architecture"]
    B --> C["<b>Part C</b><br/>Standards-to-Model<br/>Mapping Rules"]
    B --> D["<b>Part D</b><br/>Modeling Conventions<br/>& Guidelines"]
    C --> E["<b>Part E</b><br/>Lifecycle<br/>Integration"]
    D --> E
    E --> F["<b>Part F</b><br/>Case Study<br/>Definition"]
    E --> G["<b>Part G</b><br/>Governance<br/>& SOPs"]
    F -.->|"traced through<br/>all layers"| A
    F -.->|"traced through<br/>all layers"| B
    F -.->|"traced through<br/>all layers"| C
    G -.->|"controls &<br/>governs"| B

    style A fill:#2d6a4f,stroke:#1b4332,color:#fff
    style B fill:#2d6a4f,stroke:#1b4332,color:#fff
    style C fill:#40916c,stroke:#2d6a4f,color:#fff
    style D fill:#40916c,stroke:#2d6a4f,color:#fff
    style E fill:#52b788,stroke:#40916c,color:#000
    style F fill:#74c69d,stroke:#52b788,color:#000
    style G fill:#74c69d,stroke:#52b788,color:#000
```

**Reading order:** A → B → C & D (parallel, both depend on B) → E → F & G (parallel, both depend on E).

**Dependency logic:**
- Part A defines *what* the framework covers and *why*.
- Part B defines *how the model is structured* — package hierarchy, projection principle, DHF/DMR backbone.
- Parts C and D define *what goes into the model* — C translates standards into model patterns, D defines the syntax and conventions.
- Part E defines *when things happen* — lifecycle phases, review gates, V-model mapping, baseline strategy.
- Part F applies everything to a concrete subsystem so the framework is practical, not theoretical.
- Part G defines *who controls the model and how* — ownership, SOPs, change control.

---

## Part A — Framework Scope and Purpose

**File:** `Part_A_Framework_Scope_and_Purpose.md`

### A.1 Problem statement
- Why document-centric development fails for electromechanical medical devices
- The multi-domain challenge: mechanical, electrical, firmware, sensor, fluid path — all coupled
- Traceability gaps, siloed safety analysis, slow root cause investigation, fragile design transfer
- Regulatory audit vulnerability when product knowledge lives in disconnected documents

### A.2 What MBSE solves in this context
- Single authoritative model with controlled projections (not one monolithic diagram)
- Bidirectional traceability from stakeholder needs through design, risk, verification, and manufacturing
- Model-based impact analysis for change control, CAPA, and root cause investigation
- Queryable compliance evidence: "show all EP-tagged requirements without verification" becomes a model query, not a manual hunt

### A.3 Framework scope boundaries
- **In scope:** Concept definition, requirements, architecture (structural + behavioral), interface control, risk management, verification/validation, manufacturing transfer, issue management, root cause analysis, post-market hooks
- **In scope device types:** Electromechanical devices with active actuation — infusion pumps, electronic injection systems, on-body delivery systems, motorized auto-injectors, and similar pump/motor/sensor/firmware combinations
- **Out of scope:** Pure software-as-medical-device (SaMD), passive devices, in vitro diagnostics, clinical trial design, full PLM/ERP implementation

```mermaid
graph LR
    subgraph IN_SCOPE["In Scope"]
        direction TB
        S1["Concept & Intended Use"]
        S2["Requirements Hierarchy"]
        S3["System Architecture<br/>Structural + Behavioral"]
        S4["Interface Control"]
        S5["Risk Management<br/>ISO 14971 + FMEA"]
        S6["Verification & Validation"]
        S7["Manufacturing Transfer"]
        S8["Issue Management<br/>& Root Cause Analysis"]
        S9["Post-Market Hooks"]
    end

    subgraph OUT_SCOPE["Out of Scope"]
        direction TB
        O1["Pure SaMD"]
        O2["Passive Devices"]
        O3["IVD"]
        O4["Clinical Trials"]
        O5["Full PLM/ERP<br/>Implementation"]
    end

    style IN_SCOPE fill:#d8f3dc,stroke:#2d6a4f,color:#000
    style OUT_SCOPE fill:#fce4e4,stroke:#c0392b,color:#000
```

### A.4 Standards baseline
- ISO 13485:2016 — QMS and design controls
- IEC 60601-1 (Ed 3.2) — Basic safety, essential performance, PEMS (Clause 14)
- IEC 60601-1-2 — EMC
- IEC 60601-1-8 — Alarm systems
- IEC 60601-1-11 — Home healthcare
- ISO 14971:2019 — Risk management
- IEC 62304:2006/Amd 1:2015 — Software lifecycle
- IEC 62366-1:2015/Amd 1:2020 — Usability engineering
- ISO 11608-1 — Needle-based injection systems (dose accuracy)
- ISO 11608-4:2022 — Electronic needle-based injection systems
- SysML 1.6 / UML 2.5.1 — Modeling languages (OMG)
- OSLC — Cross-tool lifecycle linking (optional, enabling)

### A.5 Engineering objectives
- Objective 1: Every requirement traces to a source, a design element, and a verification artifact
- Objective 2: Every essential performance function is tagged, risk-linked, and independently verifiable
- Objective 3: Every risk control traces to a design block, a verification test, and a residual risk judgement
- Objective 4: Every interface is defined with typed ports, acceptance criteria, and integration test targets
- Objective 5: Impact analysis for any change or field issue can be executed as a model query
- Objective 6: Manufacturing acceptance criteria trace back to design requirements and risk controls
- Objective 7: The model serves as the authoritative index for DHF, DMR, and risk management file

### A.6 Diagram: Framework coverage mapped to product lifecycle

```mermaid
---
config:
  layout: elk
---
flowchart TB
 subgraph LIFECYCLE["Product Lifecycle"]
    direction LR
        L2["Architecture"]
        L1["Concept"]
        L3["Detailed<br>Design"]
        L4["Verification"]
        L5["Validation"]
        L6["Transfer"]
        L7["Production"]
        L8["Post-Market"]
  end
 subgraph FRAMEWORK["MBSE Framework Coverage"]
    direction TB
        F1["Stakeholder Needs<br>&amp; Intended Use"]
        F2["Requirements<br>Hierarchy"]
        F3["Structural &amp;<br>Behavioral Models"]
        F4["Interface<br>Control"]
        F5["Risk &amp;<br>FMEA"]
        F6["V&amp;V<br>Plans &amp; Evidence"]
        F7["Manufacturing<br>Specs &amp; BOM"]
        F8["Issue Mgmt<br>&amp; CAPA"]
  end
    L1 --> L2
    L2 --> L3
    L3 --> L4
    L4 --> L5
    L5 --> L6
    L6 --> L7
    L7 --> L8
    L1 --- F1 & F2
    L2 --- F3 & F4
    L3 --- F3 & F5
    L4 --- F6
    L5 --- F6
    L6 --- F7
    L7 --- F7
    L8 --- F8
    F5 -.- L1 & L2 & L3 & L4 & L8

    style LIFECYCLE fill:#edf6f9,stroke:#006d77,color:#000
    style FRAMEWORK fill:#fefae0,stroke:#606c38,color:#000
```

### A.7 Intended audience and usage
- Systems engineers, firmware engineers, hardware engineers, quality/regulatory, verification leads, manufacturing engineers
- Used as a reference during EA model construction
- Used as a training/onboarding resource for team members new to MBSE in medical devices

### Diagrams in Part A
| Diagram | Mermaid Type | Purpose |
|---------|-------------|---------|
| Scope boundary | Flowchart (LR) | Show in-scope vs out-of-scope |
| Lifecycle coverage | Flowchart (LR) | Map framework elements to lifecycle phases |
| Standards ecosystem | Flowchart (TD) | Show how the six standards relate to each other |

---

## Part B — Framework Architecture

**File:** `Part_B_Framework_Architecture.md`

### B.1 Core principle: single model with controlled projections
- The model is not one diagram — it is an organized repository of interconnected elements
- "Projections" are views generated for specific audiences (reviewers, auditors, manufacturing, firmware team)
- Each projection is a filtered, frozen snapshot — the model is the living source
- Analogy: the model is a database; documents are reports generated from queries

### B.2 Package hierarchy — the backbone of the EA project
- 10-package structure mapped to regulated artifacts
- Each package has a defined owner, defined contents, and defined relationships to other packages
- Package hierarchy directly supports DHF structure, DMR structure, and risk management file structure

```mermaid
---
config:
  layout: elk
---
flowchart LR
    ROOT["📁 DrugDeliveryDevice_Project<br><i>Root Model</i>"] --> P01["📁 01_StakeholderNeeds<br><i>Use cases, user profiles,<br>intended use, environments</i>"] & P02["📁 02_Requirements<br><i>4-tier hierarchy: StRS → SyRS<br>→ SubSys → Component</i>"] & P03["📁 03_Architecture<br><i>System/Subsystem/Component<br>BDD, IBD, Interface Blocks</i>"] & P04["📁 04_Behavior<br><i>State machines, activities,<br>sequences, parametric</i>"] & P05["📁 05_RiskManagement<br><i>Hazards, FMEA, risk controls,<br>residual risk, risk matrix</i>"] & P06["📁 06_Verification<br><i>Test cases, test suites,<br>V&amp;V matrices, evidence refs</i>"] & P07["📁 07_SoftwareArchitecture<br><i>IEC 62304: SW system/items/<br>units, SOUP, anomalies</i>"] & P08["📁 08_ManufacturingTransfer<br><i>Production specs, BOM,<br>process validation, CTQs</i>"] & P09["📁 09_ReusableLibrary<br><i>Value types, standard blocks,<br>interface blocks, stereotypes</i>"] & P10["📁 10_TraceabilityViews<br><i>Matrices, dashboards,<br>gap analysis, compliance reports</i>"]

    style ROOT fill:#1b4332,stroke:#081c15,color:#fff
    style P01 fill:#2d6a4f,stroke:#1b4332,color:#fff
    style P02 fill:#2d6a4f,stroke:#1b4332,color:#fff
    style P03 fill:#40916c,stroke:#2d6a4f,color:#fff
    style P04 fill:#40916c,stroke:#2d6a4f,color:#fff
    style P05 fill:#52b788,stroke:#40916c,color:#000
    style P06 fill:#52b788,stroke:#40916c,color:#000
    style P07 fill:#40916c,stroke:#2d6a4f,color:#fff
    style P08 fill:#74c69d,stroke:#52b788,color:#000
    style P09 fill:#95d5b2,stroke:#74c69d,color:#000
    style P10 fill:#95d5b2,stroke:#74c69d,color:#000
```

### B.3 Inter-package relationships — how packages connect
- Detailed description of what links to what and via which SysML relationship
- Requirements → Architecture (satisfy), Requirements → Verification (verify), Requirements → Requirements (deriveReqt)
- Risk elements → Architecture blocks (mitigate/trace), Risk controls → Verification (verify)
- Architecture → Manufacturing (allocate/trace), Architecture → Software Architecture (allocate)
- Issue/CAPA → Architecture + Requirements + Verification (trace)

```mermaid
---
config:
  layout: elk
---
flowchart TB
    P01["01<br>Stakeholder<br>Needs"] -- refine /<br>deriveReqt --> P02["02<br>Requirements"]
    P02 -- satisfy --> P03["03<br>Architecture"]
    P02 -- verify --> P06["06<br>Verification"]
    P02 -- deriveReqt --> P07["07<br>Software<br>Architecture"]
    P03 -- allocate --> P07
    P03 -- trace --> P08["08<br>Manufacturing"]
    P04["04<br>Behavior"] -- owned by<br>blocks in --> P03
    P05["05<br>Risk"] -- mitigate →<br>blocks in --> P03
    P05 -- verify --> P06
    P05 -- trace to<br>hazards from --> P01
    P08 -- acceptance<br>criteria from --> P02
    P10["10<br>Traceability"] -. queries<br>across all .-> P01
    P10 -.-> P02 & P03 & P05 & P06

    style P01 fill:#2d6a4f,stroke:#1b4332,color:#fff
    style P02 fill:#2d6a4f,stroke:#1b4332,color:#fff
    style P03 fill:#40916c,stroke:#2d6a4f,color:#fff
    style P06 fill:#52b788,stroke:#40916c,color:#000
    style P07 fill:#40916c,stroke:#2d6a4f,color:#fff
    style P08 fill:#74c69d,stroke:#52b788,color:#000
    style P04 fill:#40916c,stroke:#2d6a4f,color:#fff
    style P05 fill:#52b788,stroke:#40916c,color:#000
    style P10 fill:#95d5b2,stroke:#74c69d,color:#000
```

### B.4 Model as DHF/DMR backbone
- How the model replaces (or more precisely, indexes and structures) the Design History File
- Each DHF section maps to a model package and a generated document output
- The model is the authoritative source; documents are controlled projections
- DMR generation: BOM from BDD hierarchy, production specs from tagged blocks, acceptance criteria from verification links

### B.5 Discipline-specific views from one shared model
- Firmware team view: state machines, activity diagrams, software architecture blocks, IEC 62304 traceability
- Hardware team view: BDD component blocks with value properties, IBD interfaces, parametric constraints
- Quality/Risk team view: hazard chains, FMEA elements, risk control verification status, residual risk aggregation
- Verification team view: test cases, requirement coverage matrices, pass/fail dashboards
- Manufacturing team view: production spec blocks, BOM, CTQ characteristics, acceptance criteria

### B.6 Three-tier decomposition principle
- System → Subsystem → Component
- Each tier has defined SysML elements and required relationships
- Subsystem boundary rules: a subsystem owns components, interfaces with other subsystems via typed ports, and carries allocated requirements

### Diagrams in Part B
| Diagram | Mermaid Type | Purpose |
|---------|-------------|---------|
| Package hierarchy | Flowchart (TD) | Show the 10-package EA structure |
| Inter-package relationships | Flowchart (LR) | Show SysML relationship types between packages |
| DHF backbone mapping | Flowchart (LR) | Map DHF sections to model packages |
| Discipline views | Flowchart (TD) | Show how each team gets their view from one model |

---

## Part C — Standards-to-Model Mapping Rules

**File:** `Part_C_Standards_to_Model_Mapping.md`

### C.1 Standards interconnection diagram
- Visual showing how ISO 13485, IEC 60601-1, ISO 14971, IEC 62304, IEC 62366-1, and ISO 11608-4 relate
- Which standards invoke which other standards
- Where risk management (ISO 14971) acts as the spine connecting all others

```mermaid
---
config:
  layout: elk
---
flowchart TB
    ISO13485["<b>ISO 13485:2016</b><br>QMS &amp; Design Controls<br><i>Overarching process framework</i>"] -- requires risk<br>management per --> ISO14971["<b>ISO 14971:2019</b><br>Risk Management<br><i>Lifecycle risk process</i>"]
    ISO13485 -- design controls<br>frame --> IEC60601["<b>IEC 60601-1</b><br>Basic Safety &amp;<br>Essential Performance<br><i>Electrical/PEMS safety</i>"]
    IEC60601 -- requires risk<br>process per --> ISO14971
    IEC60601 -- Clause 14 links<br>SW lifecycle to --> IEC62304["<b>IEC 62304</b><br>Software Lifecycle<br><i>SW development process</i>"]
    IEC60601 -- requires usability<br>process per --> IEC62366["<b>IEC 62366-1</b><br>Usability Engineering<br><i>Use-related risk &amp; HMI</i>"]
    ISO11608["<b>ISO 11608-4:2022</b><br>Electronic Injection<br>Systems<br><i>Actuation-specific safety</i>"] -- draws electrical<br>safety from --> IEC60601
    ISO11608 -- requires risk<br>approach per --> ISO14971
    ISO11608 -- invokes usability<br>from --> IEC62366
    ISO11608 -- EMC testing per --> IEC60601_12["IEC 60601-1-2<br><i>EMC</i>"]
    IEC60601 --- IEC60601_12 & IEC60601_18["IEC 60601-1-8<br><i>Alarms</i>"] & IEC60601_111["IEC 60601-1-11<br><i>Home Healthcare</i>"]

     ISO14971:::riskStyle
    classDef riskStyle fill:#e63946,stroke:#d00000,color:#fff
    style ISO13485 fill:#003049,stroke:#001d3d,color:#fff
    style IEC60601 fill:#d62828,stroke:#9d0208,color:#fff
    style IEC62304 fill:#f77f00,stroke:#e36414,color:#fff
    style IEC62366 fill:#fcbf49,stroke:#f77f00,color:#000
    style ISO11608 fill:#606c38,stroke:#283618,color:#fff
    style IEC60601_12 fill:#d62828,stroke:#9d0208,color:#fff
    style IEC60601_18 fill:#d62828,stroke:#9d0208,color:#fff
    style IEC60601_111 fill:#d62828,stroke:#9d0208,color:#fff
```

### C.2 ISO 13485 design controls → model structures
- Clause 7.3.1 (Planning) → Lifecycle Plan element, milestone breakdown, review gates
- Clause 7.3.2 (Inputs) → Requirement blocks with source metadata, EP tags, risk links
- Clause 7.3.3 (Outputs) → BDD/IBD blocks with satisfy links, acceptance criteria
- Clause 7.3.4 (Review) → Model baselines, review projection generation, findings as model elements
- Clause 7.3.5 (Verification) → Test cases with verify links, parametric verification
- Clause 7.3.6 (Validation) → Use case execution, clinical workflow validation
- Clause 7.3.7 (Change Control) → Baseline comparison, impact analysis, re-verification
- Clause 7.3.8 (Transfer) → Production spec blocks, BOM generation, process validation links
- Clause 7.3.9 (Post-transfer changes) → Version-controlled elements, engineering change notice
- Clause 7.3.10 (Design files) → Model as DHF index, generated document outputs
- Full clause-by-clause mapping table

### C.3 IEC 60601-1 → model elements
- Essential performance (Clause 4.3) → EP-tagged requirements, functions, parameters
- Single fault safety (Clause 4.7, 13.2) → Fault injection scenarios, state machine fault transitions
- PEMS (Clause 14) → Embedded control architecture blocks, PEMS requirement specs, defensive design allocation
  - Clause 14.2: PEMS lifecycle and milestone definition
  - Clause 14.3: Problem resolution system as model workflow
  - Clause 14.6: PEMS requirement specification → model requirements package
  - Clause 14.8: PEMS architecture → BDD/IBD with risk control allocation
  - Clause 14.10: PEMS verification → test cases for BS/EP/risk control functions
  - Clause 14.11: PEMS validation → system-level validation test cases
- Electrical safety (Clause 8) → MOOP/MOPP requirements, insulation boundary modeling in IBD
- Alarm systems (via IEC 60601-1-8) → alarm priority state machine, alarm requirement blocks
- Verification expectations → verification plan elements, coverage criteria, independence

### C.4 ISO 14971 → risk model structure
- Clause 4 (Risk analysis) → Hazard identification, hazardous situation elements, severity/probability
- Clause 5 (Risk evaluation) → Risk matrix as parametric constraint, acceptability determination
- Clause 6 (Risk control) → Risk control elements linked to design blocks via satisfy/mitigate
- Clause 7 (Residual risk evaluation) → Post-control risk assessment, verification evidence links
- Clause 8 (Overall residual risk) → Aggregation constraint block, benefit-risk analysis
- Clause 9 (Production and post-production) → Post-market monitoring hooks, CAPA linkage
- Risk as the model's spine: how every hazard connects to architecture, verification, and post-market

### C.5 IEC 62304 → software model structure
- Software safety classification (A/B/C) → SafetyClassification stereotype on SW blocks
- Software system → software items → software units hierarchy → package/block decomposition
- SOUP management → SOUP blocks with version, anomaly, and requirement tagged values
- Software architecture (Clause 5.3) → BDD for SW items, IBD for SW interfaces
- Software verification (Clause 5.5–5.7) → Test cases linked to SW requirements
- Software anomaly resolution (Clause 9) → Anomaly elements with state machine lifecycle

### C.6 IEC 62366-1 → usability model links
- Use specification (Clause 5.1) → Use case diagrams, user profiles, use environments
- Hazard-related use scenarios (Clause 5.3–5.4) → Activity diagrams for task flows, linked to hazards
- UI specification (Clause 5.6) → HMI subsystem BDD/IBD, display/button/alarm blocks
- Formative/summative evaluation (Clause 5.5) → Validation test cases linked to usability requirements
- Usability engineering file → model package with generated document outputs

### C.7 ISO 11608-4 → actuation-specific model requirements
- Essential performance for dose delivery → EP-tagged dose accuracy requirements
- Single fault safety for moving parts (Clause 8.10.4) → Locked rotor/jam failure modes, fault injection scenarios
- Motor-specific test conditions → Test case blocks with environmental constraints
- EMC by risk approach → EMC risks and mitigations linked to interfaces and behaviors
- Environmental stress (shock, vibration) → Requirements allocated to mechanical housing blocks

### C.8 Comprehensive standards mapping table
- Full clause-by-clause table: Standard | Clause | Requirement Topic | SysML Element Type | EA Artifact | DHF Document | Review Gate
- Covers all six primary standards plus collateral standards

### Diagrams in Part C
| Diagram | Mermaid Type | Purpose |
|---------|-------------|---------|
| Standards interconnection | Flowchart (TD) | Show how standards invoke/reference each other |
| ISO 13485 clause-to-model flow | Flowchart (LR) | Map design control clauses to model elements |
| Risk spine diagram | Flowchart (TD) | Show ISO 14971 as the connecting thread |
| IEC 62304 SW hierarchy | Flowchart (TD) | Show SW system → items → units decomposition |
| EP identification flow | Flowchart (LR) | Show how essential performance is identified and tagged |

---

## Part D — Modeling Conventions and Guidelines

**File:** `Part_D_Modeling_Conventions_and_Guidelines.md`

### D.1 Custom stereotypes (MDG Technology profile)
- Complete stereotype catalog with definitions, tagged values, and base metaclass
- «EPR» (Essential Performance Requirement) — extends Requirement
- «RiskControl» — extends Class
- «failureMode» — extends Class
- «hazard», «hazardousSituation», «harm» — extends Class
- «SOUP» — extends Class
- «SafetyClassification» — extends Class
- «productionSpec» — extends Class
- «anomaly» — extends Class
- «problemReport» — extends Class

```mermaid
classDiagram
    class Requirement {
        <<SysML metaclass>>
    }
    class Class_Block {
        <<SysML metaclass>>
    }

    class EPR {
        <<stereotype>>
        +clinicalFunction : String
        +degradationLimit : String
        +singleFaultBehavior : String
        +sourceHazard : String
    }

    class RiskControl {
        <<stereotype>>
        +controlType : enum
        +riskLevel : String
        +residualRisk : String
        +verificationStatus : enum
        +implementationStatus : enum
    }

    class FailureMode {
        <<stereotype>>
        +severity : Integer
        +occurrence : Integer
        +detection : Integer
        +RPN : Integer
        +fmeaID : String
    }

    class SOUP {
        <<stereotype>>
        +version : String
        +manufacturer : String
        +knownAnomalies : String
        +safetyClass : enum
    }

    class ProductionSpec {
        <<stereotype>>
        +manufacturingMethod : enum
        +materialSpecification : String
        +dimensionalTolerances : String
        +acceptanceCriteria : String
        +AQL : String
    }

    Requirement <|-- EPR : extends
    Class_Block <|-- RiskControl : extends
    Class_Block <|-- FailureMode : extends
    Class_Block <|-- SOUP : extends
    Class_Block <|-- ProductionSpec : extends
```

### D.2 Naming conventions
- Requirements: `STK-XXX` (stakeholder), `SYS-REQ-XXX` (system), `ACT-REQ-XXX` (subsystem-actuation), etc.
- Hazards: `H-XXX`, Hazardous situations: `HS-XXX`, Risk controls: `RC-XXX`
- Test cases: `TC-SYS-XXX`, `TC-INT-XXX`, `TC-RSK-XXX`
- Failure modes: `FM-ACT-XXX`, `FM-SNS-XXX`, `FM-IF-XXX`
- Interface blocks: `IFB_Source_Target` (e.g., `IFB_MCU_MotorDriver`)
- Baselines: `BL-[Gate]-v[Major].[Minor]-[Date]`

### D.3 Traceability relationship rules
- The "three-link minimum" rule: every requirement must have (1) upward trace, (2) satisfy link from design, (3) verify link from test case
- Five SysML requirement relationships: satisfy, verify, deriveReqt, refine, trace — when to use each
- Cross-package relationship patterns

```mermaid
---
config:
  layout: elk
---
flowchart TB
    STD["Standard<br>Clause"] -- trace --> REQ["System<br>Requirement"]
    REQ -- deriveReqt --> SUBREQ["Subsystem<br>Requirement"]
    SUBREQ -- satisfy<br>(from block) --> BLOCK["Design<br>Block"]
    SUBREQ -- verify<br>(from test) --> TEST["Test<br>Case"]
    HAZ["Hazard"] -- mitigate<br>(via risk control) --> RC["Risk<br>Control"]
    RC -- satisfy<br>(from block) --> BLOCK
    RC -- verify<br>(from test) --> TEST

    style STD fill:#003049,stroke:#001d3d,color:#fff
    style REQ fill:#2d6a4f,stroke:#1b4332,color:#fff
    style SUBREQ fill:#40916c,stroke:#2d6a4f,color:#fff
    style BLOCK fill:#f77f00,stroke:#e36414,color:#fff
    style TEST fill:#fcbf49,stroke:#f77f00,color:#000
    style HAZ fill:#e63946,stroke:#d00000,color:#fff
    style RC fill:#d62828,stroke:#9d0208,color:#fff
```

### D.4 Diagram selection decision tree
- When to use BDD vs IBD vs STM vs ACT vs SEQ vs PAR vs REQ
- BDD: structural decomposition and composition hierarchy
- IBD: interface definition, internal connectivity, port/flow specification
- STM: control logic, firmware states, fault transitions
- ACT: algorithms, process flows, operational sequences
- SEQ: time-ordered interactions between blocks (especially for safety protocols)
- PAR: engineering constraints, parametric relationships, simulation binding
- REQ: requirement hierarchy, traceability visualization

### D.5 Essential performance tagging rules
- What qualifies as EP: performance whose absence or degradation produces unacceptable risk
- EP is identified through risk analysis, not assumed
- EP tag applies to: requirements, functions (activity nodes), behaviors (state machine regions), parameters (constraint values)
- EP-tagged elements carry stricter verification requirements and change control

### D.6 Interface modeling standards
- Every cross-subsystem interface gets an interface block
- Interface blocks define signal names, data types, directions, protocols, timing constraints
- Proxy ports on blocks are typed by interface blocks
- Item flows on IBD connectors annotate what crosses each interface
- Each interface becomes an integration test target

### Diagrams in Part D
| Diagram | Mermaid Type | Purpose |
|---------|-------------|---------|
| Stereotype class diagram | Class diagram | Show all custom stereotypes with tagged values |
| Traceability relationship pattern | Flowchart (LR) | Show the three-link minimum and risk chain |
| Diagram selection decision tree | Flowchart (TD) | Guide for which SysML diagram to use when |

---

## Part E — Lifecycle Integration

**File:** `Part_E_Lifecycle_Integration.md`

### E.1 V-model mapping within the MBSE model
- Left side: decomposition (Stakeholder → System → Subsystem → Component)
- Right side: integration/verification (Unit Test → Integration Test → System Verification → Validation)
- Each layer has defined SysML artifacts on both sides
- Review gates at each transition

```mermaid
---
config:
  layout: elk
---
flowchart TB
 subgraph LEFT["Decomposition (Design)"]
    direction TB
        L1["Stakeholder Needs<br><i>Use Cases, StRS</i>"]
        L2["System Requirements<br><i>SyRS, System Context BDD</i>"]
        L3["Subsystem Design<br><i>BDD/IBD, State Machines</i>"]
        L4["Component Design<br><i>Leaf blocks, value properties</i>"]
  end
 subgraph RIGHT["Integration (Verification)"]
    direction TB
        R1["System Validation<br><i>Clinical workflow, summative eval</i>"]
        R2["System Verification<br><i>IEC 60601-1 type tests</i>"]
        R3["Integration Testing<br><i>Interface verification, IBD tests</i>"]
        R4["Unit Testing<br><i>Component acceptance criteria</i>"]
  end
    L1 -- SRR --- L2
    L2 -- PDR --- L3
    L3 -- CDR --- L4
    L4 -. build &amp;<br>integrate .-> R4
    R4 -- DVR --- R3
    R3 --- R2
    R2 -- Validation<br>Review --- R1
    L1 -. verify .- R1
    L2 -. verify .- R2
    L3 -. verify .- R3
    L4 -. verify .- R4

    style LEFT fill:#edf6f9,stroke:#006d77,color:#000
    style RIGHT fill:#fefae0,stroke:#606c38,color:#000
```

### E.2 Review gate definitions
- SRR (System Requirements Review): completeness of requirements, standards coverage, initial risk
- PDR (Preliminary Design Review): architecture frozen, all interfaces defined, satisfy links, preliminary FMEA
- CDR (Critical Design Review): detailed design frozen, full FMEA, 100% requirement-to-test coverage
- DVR (Design Verification Review): all requirements verified, all anomalies dispositioned
- Validation Review: clinical workflow validated, summative usability evaluation
- MRR (Manufacturing Readiness Review): production specs, BOM, process validation, acceptance tests
- Gate criteria expressed as model queries (e.g., "0 requirements without verify link at DVR")

### E.3 Baseline strategy
- Baselines aligned to review gates
- System Architecture Baseline (after PDR)
- Design Verification Baseline (after DVR)
- Manufacturing Transfer Baseline (after MRR)
- Post-release baselines for change control
- Baseline comparison for change impact analysis

### E.4 Verification strategy derived from standards
- IEC 60601-1 verification expectations for BS/EP/risk control functions
- Separation of type-testing (design verification) from manufacturing lot acceptance
- ISO 11608-4 dose accuracy verification with defined sample sizes
- PEMS verification plan: milestones, strategies, independence, tools, coverage, results

### E.5 Manufacturing transfer as an engineered transition
- Design outputs → production specifications (not a document handover)
- Manufacturing process blocks with CTQs linked to product requirements
- Test fixtures as system elements with calibration requirements
- Acceptance criteria linked to risk controls
- Production Specification Baseline as a controlled model snapshot

### E.6 Post-market integration hooks
- Complaint → Issue model element → impacted blocks/interfaces/behaviors
- CAPA workflow within the model: cause → corrective action → re-verification
- Post-market surveillance data feeding back to risk model updates

### Diagrams in Part E
| Diagram | Mermaid Type | Purpose |
|---------|-------------|---------|
| V-model mapping | Flowchart (TD) | Show decomposition/integration alignment |
| Review gate flow | Flowchart (LR) | Show gate sequence with criteria |
| Baseline timeline | Flowchart (LR) | Show baseline strategy across lifecycle |
| Issue-to-CAPA workflow | Flowchart (TD) | Show post-market feedback loop |

---

## Part F — Case Study Definition

**File:** `Part_F_Case_Study_Definition.md`

### F.1 Case study selection rationale
- Why a stepper-motor-driven peristaltic dosing module
- It exercises every framework layer: mechanical actuation, electronics, firmware, sensors, fluid path, safety logic
- It touches every applicable standard: ISO 13485 design controls, IEC 60601-1 PEMS/EP/single fault, ISO 14971 risk chain, IEC 62304 software lifecycle, IEC 62366-1 usability (dose setting UI), ISO 11608-4 actuator fault testing

### F.2 Subsystem scope and boundaries
- What is included: stepper motor, motor driver, pump mechanism, downstream pressure sensor, position encoder, application MCU (motor control + occlusion detection functions), safety MCU (MOTOR_KILL), relevant power rails
- What is excluded (modeled as external interfaces): HMI subsystem, BLE communication, reservoir/tubing consumables, battery management (treated as power input)
- System context boundary for the case study subsystem

```mermaid
---
config:
  layout: elk
---
graph TD
    subgraph CASE_STUDY["Case Study Subsystem Boundary"]
        direction TB
        SM["«block»<br/>StepperMotor"]
        SMD["«block»<br/>StepperMotorDriver"]
        PM["«block»<br/>PumpMechanism"]
        PS["«block»<br/>DownstreamPressureSensor"]
        PE["«block»<br/>PositionEncoder"]
        AMCU["«block»<br/>ApplicationMCU<br/><i>(motor control +<br/>occlusion detection)</i>"]
        SMCU["«block»<br/>SafetyMonitorMCU<br/><i>(MOTOR_KILL)</i>"]

        AMCU -->|"SPI: STEP, DIR,<br/>nFAULT"| SMD
        SMD -->|"PWM"| SM
        SM -->|"torque"| PM
        PS -->|"I2C: pressure<br/>data"| AMCU
        PE -->|"quadrature<br/>signal"| AMCU
        AMCU -->|"WDI heartbeat"| SMCU
        SMCU -->|"MOTOR_KILL"| SMD
    end

    subgraph EXTERNAL["External Interfaces (modeled as ports)"]
        HMI["HMI Subsystem<br/><i>dose commands</i>"]
        PWR["Power Subsystem<br/><i>VM rail, 3V3</i>"]
        FLD["Fluid Path<br/><i>tubing, reservoir</i>"]
    end

    HMI -->|"dose command"| AMCU
    PWR -->|"power"| SMD
    PWR -->|"power"| AMCU
    PM -->|"fluid delivery"| FLD

    style CASE_STUDY fill:#edf6f9,stroke:#006d77,color:#000
    style EXTERNAL fill:#fefae0,stroke:#606c38,color:#000
```

### F.3 Key interfaces to model
- MCU → Motor Driver (SPI + STEP/DIR/nFAULT): `IFB_MCU_MotorDriver`
- Pressure Sensor → MCU (I2C): `IFB_PressureSensor`
- Encoder → MCU (quadrature): `IFB_PositionEncoder`
- Application MCU → Safety MCU (heartbeat): `IFB_SafetyHeartbeat`
- Safety MCU → Motor Driver (MOTOR_KILL): `IFB_SafetyKill`
- Power Rail → Motor Driver (VM): `IFB_MotorPower`

### F.4 Key requirements to trace
- Dose accuracy (±5% for >10U) — EP-tagged, traced to ISO 11608-1
- Occlusion detection and alarm (within 30 seconds) — traced to IEC 60601-2-24
- Single fault safety for locked rotor/jam — traced to ISO 11608-4
- Safety MCU watchdog response (halt within 100ms) — traced to IEC 60601-1 Clause 14
- Motor stall detection — traced to ISO 11608-4

### F.5 Key behaviors to model
- Dosing state machine: IDLE → DELIVERING → SUSPENDED → FAULT
- Motor control activity: receive command → calculate steps → execute profile → check occlusion
- Safety heartbeat sequence: periodic WDI toggle, timeout detection, MOTOR_KILL assertion

### F.6 Key hazards and risk controls to trace
- H-001: Excessive drug delivery → RC: Safety MCU MOTOR_KILL → verified by TC-RSK-001
- H-002: Insufficient drug delivery (occlusion) → RC: Pressure differential algorithm + alarm → verified by TC-RSK-002
- H-003: Mechanical jam causing injury → RC: Current limiting + fault state → verified by TC-RSK-003

### F.7 How the case study threads through each framework part
- Table showing which case study element appears in each Part A–G section
- This becomes the verification that the framework is practical and connected

### Diagrams in Part F
| Diagram | Mermaid Type | Purpose |
|---------|-------------|---------|
| Subsystem context boundary | Flowchart (TD) | Show case study scope with external interfaces |
| Interface map | Flowchart (LR) | Show all six key interfaces |
| Requirement trace chain | Flowchart (LR) | Show dose accuracy requirement traced through all layers |
| Hazard-to-verification chain | Flowchart (TD) | Show one complete risk chain from hazard to test evidence |

---

## Part G — Governance and SOPs

**File:** `Part_G_Governance_and_SOPs.md`

### G.1 Model ownership and RACI
- System Architect: system-level BDD/IBD, requirement allocation, interface definitions
- Firmware Lead: state machines, activity diagrams, IEC 62304 software items
- Hardware Lead: electrical/mechanical subsystem blocks, PCB specifications
- Verification Lead: test case creation, execution tracking, V&V reporting
- Quality/Regulatory: risk model, standards mapping, CAPA linkage, DHF generation
- Manufacturing Engineering: production spec blocks, BOM, process validation

### G.2 SOP-MDL-001: Model creation procedure
- Initiation from approved Design & Development Plan
- Package setup per framework architecture (Part B)
- Element creation with naming conventions (Part D)
- Relationship definition with three-link minimum enforcement
- Review submission

### G.3 SOP-MDL-002: Model review procedure
- Trigger at each review gate (SRR, PDR, CDR, DVR, MRR)
- Cross-functional review team composition
- Review checklist (naming, traceability, interface completeness, state coverage, risk linkage)
- Finding classification (Critical/Major/Minor/Observation)
- Approval and action tracking

```mermaid
graph LR
    A["Review<br/>Triggered<br/><i>(gate reached)</i>"] --> B["Generate<br/>Review<br/>Package"]
    B --> C["Distribute<br/>to Review<br/>Team"]
    C --> D["Conduct<br/>Review<br/>Meeting"]
    D --> E{"Findings?"}
    E -->|"Critical/<br/>Major"| F["Resolve<br/>& Resubmit"]
    F --> D
    E -->|"Minor/<br/>Observation"| G["Log &<br/>Track"]
    G --> H["Approve<br/>& Sign"]
    E -->|"None"| H
    H --> I["Baseline<br/>Model"]

    style A fill:#2d6a4f,stroke:#1b4332,color:#fff
    style D fill:#f77f00,stroke:#e36414,color:#fff
    style H fill:#40916c,stroke:#2d6a4f,color:#fff
    style I fill:#003049,stroke:#001d3d,color:#fff
```

### G.4 SOP-MDL-003: Model baselining procedure
- Trigger events (gate completion, pre-transfer, pre-submission, post-change)
- Pre-baseline validation checks
- Baseline creation in EA (menu path, naming convention)
- Approval, archival, and read-only lock

### G.5 SOP-MDL-004: Model change control procedure
- Change request submission (MCR with rationale, affected packages)
- Impact analysis via traceability queries
- Risk assessment of proposed change
- Implementation in development branch
- Verification of change, peer review
- Re-baseline and communication

```mermaid
graph TD
    A["Change<br/>Request<br/>(MCR)"] --> B["Impact<br/>Analysis<br/><i>query traceability</i>"]
    B --> C["Risk<br/>Assessment"]
    C --> D{"Significance?"}
    D -->|"Major"| E["Implement in<br/>Dev Branch"]
    D -->|"Minor"| E
    E --> F["Update<br/>Trace Links"]
    F --> G["Re-Verify<br/>Affected Items"]
    G --> H["Peer Review<br/><i>SOP-MDL-002</i>"]
    H --> I["Re-Baseline<br/><i>SOP-MDL-003</i>"]
    I --> J["Update DHF<br/>& Communicate"]

    D -->|"Major"| K["Re-Verification<br/>+ Re-Validation"]
    K --> H

    style A fill:#e63946,stroke:#d00000,color:#fff
    style B fill:#f77f00,stroke:#e36414,color:#fff
    style I fill:#003049,stroke:#001d3d,color:#fff
    style J fill:#2d6a4f,stroke:#1b4332,color:#fff
```

### G.6 Integration with existing QMS
- Model as a controlled document within the QMS (same review/approval/retention discipline)
- Model outputs (generated documents) as controlled records
- Audit trail via EA auditing feature
- 21 CFR Part 11 compliance considerations for electronic signatures

### G.7 Toolchain integration
- EA as the MBSE hub
- Links to ALM/issue tracker (CAPA records, defect tracking)
- Links to PLM (BOM synchronization, released design data)
- Links to test management (verification execution and results)
- OSLC-style linking patterns or proprietary API integration

### Diagrams in Part G
| Diagram | Mermaid Type | Purpose |
|---------|-------------|---------|
| RACI ownership diagram | Flowchart (TD) | Show who owns which model packages |
| Review workflow | Flowchart (LR) | Show SOP-MDL-002 process |
| Change control workflow | Flowchart (TD) | Show SOP-MDL-004 process |
| Toolchain integration | Flowchart (LR) | Show EA connections to ALM/PLM/test tools |

---

## Delivery sequence and checkpoints

| Step | Deliverable | Checkpoint before next |
|------|------------|----------------------|
| 1 | **This outline** — reviewed and confirmed | Agree on structure, scope, and sequencing |
| 2 | **Part A** notes (.md) + walkthrough | Scope, standards, objectives confirmed |
| 3 | **Part B** notes (.md) + walkthrough | Package hierarchy, inter-package links, DHF mapping confirmed |
| 4 | **Part C** notes (.md) + walkthrough | All standards mapped, comprehensive table reviewed |
| 5 | **Part D** notes (.md) + walkthrough | All stereotypes, naming, traceability rules confirmed |
| 6 | **Part E** notes (.md) + walkthrough | V-model, gates, baselines, verification strategy confirmed |
| 7 | **Part F** notes (.md) + walkthrough | Case study scope, interfaces, trace paths confirmed |
| 8 | **Part G** notes (.md) + walkthrough | SOPs, ownership, change control, QMS integration confirmed |
| 9 | **Begin EA build Phase 1** | All framework parts confirmed as the reference guide |

---

## What comes after the framework definition

Once all seven parts are confirmed, the framework definition becomes the **reference standard** for the EA build. Each EA build phase (Phase 1–10) will reference specific framework sections and implement them as model elements. The case study subsystem will be built incrementally through each phase, so by Phase 10 you have a complete, traced, verified example that proves the framework works in practice.
