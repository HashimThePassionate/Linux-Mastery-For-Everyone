# 🌩️ Planning for Disaster Recovery & Risk Management

Managing risks is a vital asset for every business and individual. For system administrators, this responsibility is immense. Managing risks should be an integral part of a broader **risk management strategy**.

IT footprints within companies have grown exponentially over the last decade. Today, almost every activity—whether in small businesses, large corporations, government agencies, or the health and education sectors—relies on IT operations. Because every activity is unique, it requires a specific type of assessment. However, in information security, risk management has often become a "one-size-fits-all" practice based on checklists.


## 🛡️ A Brief Introduction to Risk Management

**What is risk management?**
In a nutshell, it comprises specific operations set to mitigate any possible threat that could impact the overall continuity of a business. The risk management process is crucial for every IT department.

### 📜 Origins and Standards
Risk management frameworks initially emerged in the United States following the **Federal Information Systems Modernization Act (FISMA)** laws, which began in 2002. During this time, the **National Institute of Standards and Technology (NIST)** started creating new standards and methods for cybersecurity assessments across US government agencies.


Security certifications and compliances are paramount for every Linux distribution provider competing in the corporate and governmental space. Major distributions like **Red Hat**, **SUSE**, and **Canonical** hold certifications from:
* 🇺🇸 **NIST** (USA)
* 🇬🇧 **NCSC** (National Cyber Security Centre - UK)
* 🇷🇺 **FSTEC** (Federal Service for Technic and Export Control - Russia)

### 🏗️ The Risk Management Framework (NIST SP 800-37r2)
According to NIST, the framework consists of seven steps, ranging from preparation to daily monitoring. In summary, the framework is built upon several important branches:

* **📋 Inventory:** A thorough inventory of all on-premises systems and a complete list of software solutions.
* **🏷️ System Categorization:** Assessing the impact level for each data type regarding availability, integrity, and confidentiality.
* **🔒 Security Control:** Detailed procedures regarding the security of hundreds of computer systems (NIST SP 800-53r4).
* **🔍 Risk Assessment:** A series of steps covering threat source identification, vulnerability identification, impact determination, information sharing, risk monitoring, and periodic updates.
* **📝 System Security Plan:** A report based on every security control, detailing how future actions are assessed, implemented, and their effectiveness.
* **✅ Certification, Accreditation, Assessment, & Authorization:** The process of reviewing security assessments, highlighting issues, and detailing effective resolutions in a future plan of action.
* **🛠️ Plan of Action:** A tool used to track security weaknesses and apply the correct response procedures.


## ⚠️ Types of IT Risks

Risks in information technology vary widely, ranging from physical hazards to malicious acts:

* **Hardware Failure:** Equipment breakdown or malfunction.
* **Software Errors:** Bugs, glitches, and compatibility issues.
* **Spam and Viruses:** Malicious software and unwanted communications.
* **Human Error:** Mistakes made by users or administrators.
* **Natural Disasters:** 🔥 Fires, 🌊 floods, 🌍 earthquakes, 🌀 hurricanes, etc.
* **Criminal Activities:** Security breaches, employee dishonesty, corporate espionage, and cybercrime.


## 🔄 The Risk Management Strategy: 5 Distinct Steps

To address these risks effectively, an appropriate management strategy must be implemented. This strategy generally follows five key steps:

### 1. 🕵️ Identifying Risk
Identify possible threats and vulnerabilities that could impact your ongoing IT operations.

### 2. 📊 Analyzing Risk
Decide the magnitude of the risk (how big or small it is) based on thorough studies and data.

### 3. ⚖️ Evaluating Risk
Evaluate the potential impact on operations. The immediate action is to respond based on this impact assessment. This requires real actions to be performed at every level of operations.

### 4. 🛡️ Responding to Risk
Activate your **Disaster Recovery Plans (DRPs)**, combined with strategies for prevention and mitigation.

### 5. 📡 Monitoring and Reviewing Risk
Trigger a drastic monitoring and reviewing strategy. This ensures that all IT teams:
* Know how to respond to the risk.
* Have the necessary tools and abilities to isolate the risk.
* Can enforce and strengthen the company’s infrastructure.

> **Note:** Risk assessment is extremely important to any business and should be taken very seriously by IT management.

---

# 📉 Risk Calculation & Assessment

Risk assessment (also known as risk calculation or risk analysis) is the process of identifying, evaluating, and estimating the levels of risks involved in a situation. It focuses on finding and calculating solutions to potential threats and vulnerabilities.


## 🧮 Basic Terms for Risk Impact

To effectively discuss and calculate risk, you must understand the following key metrics.

### Financial Metrics
* **Annual Loss Expectancy (ALE):**
    * Defines the total monetary loss expected over the course of **1 year**.
    * **Formula:** `SLE x ARO = ALE`
* **Single Loss Expectancy (SLE):**
    * Represents the monetary loss expected from a **single** risky event at any given time.
* **Annual Rate of Occurrence (ARO):**
    * Represents the likelihood (probability) of a risky event occurring within a **1-year** period.

> **The Golden Formula:**
> $$SLE \times ARO = ALE$$
> Since each element provides a monetary value or rate, the final result (ALE) is expressed as a monetary value. This formula is essential for budgeting security measures.

### Operational & Recovery Metrics
* **Mean Time Between Failures (MTBF):**
    * Measures the average time elapsed between anticipated and reparable failures of a system.
* **Mean Time To Failure (MTTF):**
    * Measures the average time a system operates before experiencing an **irreparable** failure (total death of the component).
* **Mean Time To Restore (MTTR):**
    * Measures the average time needed to repair a failed system and get it back online.
* **Recovery Time Objective (RTO):**
    * Represents the **maximum** acceptable amount of time allocated for downtime.
* **Recovery Point Objective (RPO):**
    * Defines the specific point in time to which data must be restored (i.e., how much data loss is acceptable).


## 🛡️ Risk Assessment Strategies

Risk assessment relies on two major categories of action: Proactive (preventative) and Non-active (reactive/passive).

### 1. Proactive Actions
These are steps taken *before* an incident occurs.

* **Risk Avoidance:**
    * Based on identifying a risk and implementing a quick solution to completely avoid its occurrence.
* **Risk Mitigation:**
    * Based on taking specific actions to **reduce** the likelihood or impact of a possible risk.
* **Risk Transference:**
    * Transferring the potential outcome (and financial burden) of the risk to an external entity, such as an insurance company.
* **Risk Deterrence:**
    * Based on implementing systems and policies (like warning banners or security cameras) that discourage attackers from attempting to exploit the system.

### 2. Non-Active Actions
These are decisions made regarding risks that cannot or will not be mitigated.

* **Risk Acceptance:**
    * Accepting the risk without action. This is chosen if the cost of proactive measures (mitigation) exceeds the cost of the potential harm caused by the risk.


## ☁️ Risk in Cloud Computing

The strategies above apply to generic, on-premises computing. However, cloud computing is dominating the modern IT landscape.

**How does risk apply to the cloud?**
In cloud computing, you use a third party's infrastructure to store and process your own data. This is effectively **outsourcing**. Consequently, risks that were previously internal threats in an on-premises environment are now transferred to third-party providers (like Amazon, Microsoft, or Google).

### The Three Major Cloud Paradigms

Different cloud models shift different levels of responsibility (and risk) from the customer to the provider.

#### 1. Software as a Service (SaaS)
* **Definition:** A software solution for companies aimed at reducing IT maintenance costs by relying on subscriptions.
* **Examples:** Slack, Microsoft 365, Google Apps, Dropbox.
* **Risk Profile:** The provider manages almost everything; user responsibility is minimal.

#### 2. Platform as a Service (PaaS)
* **Definition:** A platform that allows you to deploy software applications to clients using another entity's infrastructure, runtimes, and dependencies. This can exist on public, private, or hybrid clouds.
* **Examples:** Microsoft Azure, AWS Lambda, Google App Engine, SAP Cloud Platform, Heroku, Red Hat OpenShift.
* **Risk Profile:** You manage the application and data; the provider manages the runtime and infrastructure.

#### 3. Infrastructure as a Service (IaaS)
* **Definition:** Online services that provide high-level APIs to access physical computing resources (servers, networking).
* **Notable Example:** OpenStack.
* **Risk Profile:** You manage the OS, applications, and data; the provider manages the physical hardware.

### Managing Cloud Risks
While many infrastructure risks are transferred to the third party, new risks emerge:
* **Data Integration:** Ensuring data flows correctly between systems.
* **Compatibility:** Ensuring legacy systems work with cloud environments.

**Comparison of Risk Challenges:**
* **On-Premises:** You manage all components in-house. Risk assessment is **challenging** because you own every risk.
* **Cloud (IaaS/PaaS/SaaS):** Risk assessment becomes **less challenging** as responsibilities are gradually transferred to the external entity.

---
# 📝 Designing a Disaster Recovery Plan (DRP)

A **Disaster Recovery Plan (DRP)** is a structured document outlining the specific steps to take when an incident occurs. It is usually a subset of a broader **Business Continuity Plan (BCP)**, which ensures a company keeps operating despite infrastructure failures.



[Image of Disaster Recovery Plan Cycle]


## 🏗️ The Pillars of a Good DRP

To build an effective DRP, you must start with a solid foundation of knowledge about your current environment.

### 1. Comprehensive Inventory
You cannot protect what you don't know exists. A DRP starts with three accurate inventories:
* **Hardware Inventory:** Servers, storage devices, network gear, user devices.
* **Software Inventory:** OS versions, applications, licenses, dependencies.
* **Data Inventory:** Where data lives, who owns it, and how critical it is.

### 2. Standardization Policy
There should be a clear policy for **standardized hardware**.
* **Benefits:** Faulty hardware is easier to replace, drivers are better supported (crucial in Linux), and optimization is easier.
* **Trade-off:** This limits **BYOD (Bring Your Own Device)** practices. Employees must use company-provided hardware to ensure the IT department has full control over the software environment and security configurations.

### 3. Defining Responsibilities & Communication
* **IT Role:** The IT department designs recovery strategies based on **RPO** (Recovery Point Objective - max data loss) and **RTO** (Recovery Time Objective - max downtime).
* **Role Assignment:** Everyone must know exactly what to do during a crisis to reduce response time.
* **Communication Strategy:** Clear procedures for every organizational level ensure centralized decision-making. There must be a **succession plan** for backup personnel (if the lead admin is unavailable, who takes over?).

> **Critical Step:** DRPs must be tested **at least twice a year**. A plan that hasn't been tested is just a theory.

---

# 💾 Backing Up and Restoring the System

Disasters can strike at any time. Backing up your system is the ultimate safety net. It is always better to practice prevention than to learn the hard way through data loss.

Your strategy must be driven by your RTO and RPO requirements:
* **RTO (How fast?):** How quickly must we be back online to avoid hurting business operations?
* **RPO (How much?):** How much data can we afford to lose? (e.g., 1 hour's worth? 1 day's worth?)

## 🗂️ Types of Backups

Understanding the difference between these types is critical for balancing storage space vs. recovery speed.



| Backup Type | What is Backed Up | Pros | Cons |
| :--- | :--- | :--- | :--- |
| **Full Backup** | **All files** in the destination target. | Easiest to restore (you only need one file). | Slowest to run; takes the most storage space. |
| **Incremental Backup** | Only files changed since the **last backup** (whether it was full or incremental). | Fastest to run; uses the least storage. | Slowest to restore (must restore Full + all Incrementals in order). |
| **Differential Backup** | Only files changed since the **previous FULL backup**. | Faster to restore than Incremental (Full + last Differential). | Uses more storage than Incremental; slower to run as time goes on. |

## 🛠️ Methods of Backup

| Method | Description |
| :--- | :--- |
| **Manual Backup** | User-initiated (e.g., dragging files to a USB). Unreliable because it depends on human memory. |
| **Local Automated** | Scheduled scripts/software writing to local disks or tapes. Fast recovery but vulnerable to local disasters (fire/flood). |
| **Remote Automated** | Scheduled backups sent over the network to a remote server or cloud. Slower recovery but protects against local site disasters. |

---

# 🛡️ The Golden Rule: The 3-2-1 Backup Strategy

This is the industry standard for data protection. If you take away one thing from this section, let it be this rule.



### 3️⃣ Copies of Data
You should have at least **three** copies of your data:
1.  The primary (production) data.
2.  Backup copy #1.
3.  Backup copy #2.

### 2️⃣ Different Media
Store the copies on **two** different types of media.
* *Example:* One copy on an internal hard drive, one copy on an external tape drive or NAS (Network Attached Storage). This protects against a specific type of media failing (e.g., a batch of bad hard drives).

### 1️⃣ Off-Site
Keep at least **one** copy **off-site** (at a different geographical location).
* *Why?* If your office burns down or gets flooded, your local backups will be destroyed along with your servers. Cloud storage is a common solution for this today.

> **Note:** This rule can be adapted (e.g., 3-1-2, 3-2-2), but the core principle of redundancy and geographic separation remains checking **Backup Integrity** is also vital—a backup is worthless if it is corrupted and cannot be restored.

---