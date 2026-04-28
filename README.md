# trade-process-automation-showcase
Export Workflow Automation Case Study

## Table of Contents

- [1. System Overview & Architecture](#1-system-overview--architecture)
  - [1.1 Introduction](#11-introduction)
  - [1.2 Core Technologies](#12-core-technologies)
  - [1.3 High-Level System Architecture](#13-high-level-system-architecture)
    - [Input Layer – Excel Data Engine](#1-input-layer--excel-data-engine)
    - [Driver Management Layer](#2-driver-management-layer)
    - [Task Execution Layer (Modules)](#3-task-execution-layer-modules)
    - [Logging & Output Layer](#4-logging--output-layer)
    - [Network & Fault Tolerance Layer](#5-network--fault-tolerance-layer)
    - [Control & Monitoring Layer](#6-control--monitoring-layer)
  - [1.4 Architecture Diagram](#14-architecture-diagram)
- [2. Confidentiality Note](#2-confidentiality-note)


# Export Form Processor (EFP)

## 1. SYSTEM OVERVIEW & ARCHITECTURE

---

### 1.1 Introduction

**What the System Is**  
The Export Form Processor (EFP) is a multi-threaded automation solution designed to bulk-process Zimbabwean export documentation, specifically Forms CD1s & CD3s, through the Reserve Bank of Zimbabwe’s CEPECS (Computerized Export Payments Exchange Control System) web platform.  

Developed using Python and Selenium WebDrivers, the system simulates human interactions with the CEPECS interface to automate three core operations:
- Form CD1 Pre-acquittals (for goods-based export proceeds)
- Form CD3 Pre-acquittals (for service-based export proceeds)
- Babyline Shipment Entries (detailed shipment lines under Form CD1s)

The system leverages parallel browser execution to process high volumes of records concurrently, significantly reducing turnaround times. Its execution is fully traceable through structured logging, and it is designed to be resilient, network-aware, and Excel-driven for seamless integration into banking environments.

---

**Who the Intended Users Are**  
The system is specifically built for:
- Authorized Dealer Banks (ADBs) in Zimbabwe that are mandated to manage export form submissions and pre-acquittals within the CEPECS platform on behalf of exporters

The system abstracts the complexity of CEPECS interaction into just the provision of data to Excel templates, making it usable even by non-technical staff.

---

**What Problem It Solves**

**Key Problems in the Manual CEPECS Workflow:**  
Banks that rely on CEPECS for processing Forms CD1/CD3s, face several persistent challenges due to the platform’s lack of batch processing capabilities:

1. **Manual Data Entry Fatigue**  
Export officers are required to fill out dozens or even hundreds of forms manually on the platform. This repetitive process leads to physical and mental fatigue, especially during high-volume reporting periods.

2. **Time-Consuming Task Execution**  
Since CEPECS lacks batch processing capabilities, each form or shipment must be handled one by one. This consumes time that could otherwise be allocated to other time-sensitive banking operations or client-facing tasks.

3. **Error-Prone Manual Input**  
The manual process increases the risk of inconsistencies, such as incorrect remittance figures, dates, or shipment details. These errors can result in form rejection or require time-consuming corrections.

4. **Non-Compliance Risk with RBZ Timelines**  
CEPECS operations are governed by strict timelines set by the Reserve Bank of Zimbabwe:
- “The pre-acquittal of export documents needs to be effected in CEPECS within 48 hours of receipt of export proceeds.”
- “Authorized Dealers are always urged to be mindful of the need for timeous facilitation of export business by ensuring that exporters receive requested documents within 24 hours of their requests.”

These strict timelines mean that any delays caused by manual entry can result in compliance violations.

---

**Impact:**

The automation framework directly addresses these issues with a highly targeted solution that delivers by:

- Eliminating manual data entry through real-time browser-based simulation.
- Reducing processing times by over 85% through concurrent processing of multiple Forms/shipments.
- Providing 100% accuracy during the automation course assuming correct and validated Excel input is used.
- Supporting compliance through faster turnaround times which meet RBZ’s 24–48hr regulatory windows

---

**Tested Performance: Real-World Results**  
The automation system was tested using real export data in a controlled yet realistic environment and the testing was performed across multiple sessions and dates to account for network variability and workload diversity  
While the automation was tested under optimal runtime conditions, the manual processing time was calculated assuming a highly efficient human operator working continuously without breaks, distractions, or fatigue, a scenario that is nearly impossible to achieve in real workplace settings.

**CD1 Pre-acquittal Performance**  
- Manual benchmark: ~26 seconds per CD1 form (idealized human operator)

| Batch | CD1s | Time Taken | Manual Time (Est.) | Time Saved |
|-------|------|------------|--------------------|------------|
| A     | 382  | 5 min 32 sec | ~2 hours 45 minutes | ~2 hours 40 minutes |
| B     | 165  | 3 min 6 sec  | ~1 hour 11 minutes  | ~1 hour 8 minutes   |
| C     | 114  | 2 min 1 sec  | ~49 minutes         | ~47 minutes        |
| D     | 61   | 55 seconds   | ~26.5 minutes       | ~25.5 minutes      |

Accuracy: 100% across all sessions

**CD3 Pre-acquittal Performance**  
- Manual benchmark: ~22 seconds per CD3 form (idealized human operator)

| Batch | CD3s | Time Taken | Manual Time (Est.) | Time Saved |
|-------|------|------------|--------------------|------------|
| A     | 225  | 4 min 53 sec | ~1 hour 22 minutes | ~1 hour 17 minutes |
| B     | 225  | 2 min 11 sec | ~1 hour 22 minutes | ~1 hour 20 minutes |
| C     | 89   | 1 min 56 sec | ~32 minutes        | ~30.5 minutes      |
| D     | 469  | 10 min 23 sec| ~2 hours 51 minutes| ~2 hours 40 minutes|
| E     | 401  | 8 min 9 sec  | ~2 hours 27 minutes| ~2 hours 19 minutes|
| F     | 1,158| 36 min 20 sec| ~7 hours 5 minutes | ~6 hours 29 minutes|

Accuracy: 100% across all sessions

**Babyline Entry Performance**  
- Manual benchmark: ~30 seconds per babyline entry (idealized human operator)

| Session | Babyline Entries | Time Taken | Manual Time (Est.) | Time Saved |
|---------|------------------|------------|--------------------|------------|
| A       | 176              | 7 min 31 sec | ~1 hour 28 minutes | ~1 hour 20 minutes |
| B       | 176              | 11 min 19 sec| ~1 hour 28 minutes | ~1 hour 16 minutes |
| C       | 153              | 6 min 8 sec  | ~1 hours 16 minutes| ~1 hour 10 minutes |
| D       | 146              | 6 min 14 sec | ~1 hours 13 minutes| ~1 hour 7 minutes  |
| E       | 122              | 5 min 3 sec  | ~1 hours 1 minute  | ~56 minutes        |
| F       | 102              | 5 min 24 sec | ~51 minutes        | ~45 minutes        |
| G       | 102              | 6 min 2 sec  | ~51 minutes        | ~45 minutes        |
| H       | 102              | 7 min 5 sec  | ~51 minutes        | ~44 minutes        |
| I       | 40               | 1 min 27 sec | ~20 minutes        | ~18.5 minutes      |
| J       | 30               | 1 min 3 sec  | ~15 minutes        | ~14 minutes        |
| K       | 14               | 59 seconds   | ~7 minutes         | ~6 minutes         |

Accuracy: 100% across all sessions

---


### 1.2 Core Technologies

This automation tool relies on a focused set of technologies that drive its functionality across web automation, data processing, and parallel execution:

- **Python**  
  Python serves as the main engine of the automation system. It hosts and orchestrates all logic, coordinates tasks across multiple modules, and integrates various libraries into a seamless workflow. Its flexibility and mature ecosystem make it ideal for both rapid development and production-level automation.

- **Selenium**  
  Selenium is the browser automation driver at the heart of the system. It simulates user interactions with the CEPECS web platform, clicking buttons, filling forms, waiting for page elements, as if a human was performing the actions. It provides full control over the browser’s behavior during the automation process.

- **Openpyxl**  
  Openpyxl powers the system’s Excel integration. It reads structured CD1, CD3, and Babyline data from input workbooks and writes real-time results back into Excel logs. This Excel-based interface makes the system easy to use for banking professionals who are already familiar with spreadsheet workflows.

- **concurrent.futures**  
  This module enables parallel processing through a ThreadPoolExecutor, allowing multiple browser sessions to run independently and simultaneously. This dramatically boosts throughput by processing multiple forms or shipment entries at once, turning hours of manual effort into minutes.

---

### 1.3 High-Level System Architecture


The system is organized into 6 major layers, each responsible for a distinct function in the automation pipeline:


#### 1. Input Layer – Excel Data Engine
- Accepts structured CD1, CD3 and Babyline inputs from preformatted Excel sheets
- Parses, validates data structures for the inputs, and segments the data per task module

---

#### 2. Driver Management Layer
- Initializes multiple browser sessions for Edge using Edge drivers
- Each browser runs in isolation with the other sessions and each with its own temporary profile
- The browser drivers are distributed across available data segments for parallel processing

---

#### 3. Task Execution Layer (Modules)
This layer contains dedicated modules for each operation:
- **CD1 Pre-Acquittal Module** – Handles pre-acquittals and status transitions based on payment data
- **CD3 Pre-Acquittal Module** – Applies similar logic for service-based transactions
- **Babyline Entry Module** – Submits structured shipment records to a Master CD1 Form

Each module:
- Simulates human UI actions using Selenium Webdrivers
- Interacts only with visible fields and buttons (no backend injection)

---

#### 4. Logging & Output Layer
- All driver actions are logged per task and per Form reference
- Three types of logs are generated:
    - `.xlsx` result files for user review
    - `.log` files for developer/system-level traceability
    - `.json` files for audit teams
- Logs include timestamps and outcomes, with the `.log` files also including background errors occurred, timeouts occurred, and retries attempted during the automation course.

---

#### 5. Network & Fault Tolerance Layer
This layer detects and mitigates network or system failures to ensure continuous operation.
- Includes network readiness checks and latency profiling before execution
- Incorporates layered network checks (ping, HTTP, DNS).
- Handles DOM waiting, and on timeouts, it retries element lookups after further network and latency checks
- Recovery procedures for interrupted sessions.

---

#### 6. Control & Monitoring Layer
- A user friendly CLI interface is used to:
    - Choose task
    - Input login credentials
    - Monitor progress in real time
- Includes fail-safe commands:
    - `[ESC]` – Safely terminate execution
    - `[F5]` – Restart an operation if it suddenly hangs

---

### 1.4 Architecture Diagram

<img width="1217" height="667" alt="image" src="https://github.com/user-attachments/assets/80f0f148-a03d-42ce-831b-f2c09bf39475" />


---

## 2. Confidentiality Note
- Full source code remains private due to operational confidentiality and proprietary implementation details.



