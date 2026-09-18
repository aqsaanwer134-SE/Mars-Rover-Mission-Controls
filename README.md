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

| FR-07 *(implied)* | The system shall support command routing to a specific rover among multiple rovers. |

