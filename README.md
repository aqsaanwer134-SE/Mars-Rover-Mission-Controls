#  Mars Rover Mission Control

A requirements-engineering case study for a Mars rover command-and-control system. 
Covers functional & non-functional requirement extraction from engineering notes, 
traceability, and requirement change management (CR-01, CR-02, CR-03) for a 
bandwidth-constrained, high-latency, multi-rover deployment.

**Topics:** requirements-engineering · software-engineering · systems-design · 
functional-requirements · non-functional-requirements · change-management · 
mission-control · embedded-systems


## 📋 Mission Brief

Mars Rover Mission Control is a software system that remotely controls exploration 
rovers on Mars. The rover communicates with Mission Control through a communication 
link with **limited bandwidth** and a **delay of several minutes**, so commands 
cannot be sent repeatedly without confirmation.

Engineers must be able to:
- Send movement commands to the rover
- Receive rover location and health data
- Detect communication failures
- Prevent unauthorized commands
- Automatically place the rover into a safe state when a critical fault is detected
- Store mission events for later investigation

---

## 1️⃣ Functional Requirements (FRs)

Functional requirements describe **what the system must do**.

| ID | Requirement |
|----|-------------|
| FR-01 | The rover shall receive commands from Mission Control and execute valid commands. |
| FR-02 | The rover shall report its current position, battery level, temperature, and communication status. |
| FR-03 | The system shall reject invalid or unauthorized commands. |
| FR-04 | If the rover detects a critical battery or thermal condition, it shall enter Safe Mode. |
| FR-05 | Mission Control shall receive command execution status (success/failure feedback). |
| FR-06 | All commands and critical rover events shall be recorded with timestamp and operator ID. |

## 2️⃣ Non-Functional Requirements (NFRs)

Non-functional requirements describe **how well** the system performs.

| ID | Requirement | Category |
|----|-------------|----------|
| NFR-01 | The system shall continue operating despite temporary communication interruptions. | Reliability / Fault Tolerance |
| NFR-02 | Only authenticated Mission Control operators shall be allowed to issue commands. | Security |
| NFR-03 | Command processing should normally complete within 5 seconds after a command is received by the rover. | Performance |
| NFR-04 | The system shall support communication with multiple rovers simultaneously. | Scalability |
| NFR-05 *(implied)* | The system shall operate reliably under low-bandwidth, high-latency links without requiring repeated command retransmission. | Reliability / Network Constraint |

## 3️⃣ Change Requests from Mission Control

### 🔴 CR-01  Emergency Safety
**Original FR-04:** The rover shall enter Safe Mode when a critical battery or thermal condition is detected.

**Updated FR-04:** The rover shall enter Safe Mode **within 3 seconds** when battery temperature exceeds the critical threshold **or** battery capacity falls below the defined emergency level.

**Analysis:**
- Adds a measurable timing constraint (previously untestable).
- Ties into NFR-03 (5s normal command processing)  safety response (3s) is correctly faster than routine operations.
- Requires new defined parameters: *critical temperature threshold* and *emergency battery capacity level*, which should live in a config/parameters table, not hardcoded.

### 🟢 CR-02  Mission Expansion
**Original NFR-04:** The system shall support communication with multiple rovers simultaneously.

**Updated NFR-04:** The system shall support **at least 20** simultaneously connected rovers.

**Analysis:**
- "Multiple" was unverifiable  even 2 rovers would technically pass.
- New version is a hard, testable acceptance criterion.
- Ripple effect: connection pooling, bandwidth allocation, and authentication (NFR-02) must now scale to 20 concurrent sessions.

### 🔵 CR-03  Security Upgrade
**Original NFR-02:** Only authenticated Mission Control operators shall be permitted to issue rover commands.

**Updated NFR-02:** The system shall require **authenticated and role-authorized** operators before accepting rover commands.

**Analysis:**
- Adds **authorization** (role-based access control) on top of **authentication** (identity check).
- Enforces least-privilege access  e.g., a "Monitor" role can view telemetry but not send movement commands.
- Ripple effect: FR-03 (reject unauthorized commands) must now check role, and FR-06 (event logging) should log operator role alongside operator ID.

---

## 📊 Updated Requirements Baseline

| ID | Status | Updated Statement |
|----|--------|--------------------|
| FR-04 | Modified (CR-01) | Enter Safe Mode within 3s of critical battery/thermal threshold breach |
| NFR-04 | Modified (CR-02) | Support ≥ 20 simultaneous rover connections |
| NFR-02 | Modified (CR-03) | Require authentication **and** role-based authorization before command acceptance |
| FR-03, FR-06 | Indirectly affected | Must be updated to reflect role-checking and role logging |


| FR-07 *(implied)* | The system shall support command routing to a specific rover among multiple rovers. |

