# 🏦 Architecture & Execution Blueprint: AI-Assisted Digital Onboarding & V-KYC

[![Scale: 2M+ Profiles](https://img.shields.io/badge/Scale-2M%2B_Active_Profiles-blue?style=for-the-badge)](#)
[![TAT: 82% Reduction](https://img.shields.io/badge/Latency-82%25_TAT_Reduction-success?style=for-the-badge)](#)
[![Compliance: RBI Master Direction](https://img.shields.io/badge/Compliance-RBI_Zero_Tolerance-critical?style=for-the-badge)](#)

**Client:** Federal Bank (Sanitized) | **Role:** Lead Product Manager | **Timeline:** Dec 2022 – March 2025

## 📌 Executive Summary & Business Outcomes
Architected and executed the multi-year transformation of Federal Bank’s consumer onboarding infrastructure. We sunsetted a fragmented manual KYC process and deployed a "Generate & Freeze" onboarding architecture powered by Aadhaar APIs and a proprietary Computer Vision engine. 

By separating the **Account Generation** (instant, automated) from **Account Activation** (Human-in-the-Loop Video KYC), we eradicated frontend friction while maintaining uncompromising regulatory defense.

**The ROI & Scale Impact:**
*   **Throughput Unlocked:** Scaled the system to process **2,000,000+** compliant profiles.
*   **Friction Eradicated:** Slashed digital journey Turnaround Time (TAT) from 11 minutes to **2 minutes (82% reduction)**.
*   **Conversion Multiplied:** Lifted application completion rates from a baseline of **28% to 85%**.
*   **Premium Segment Capture:** The unblocked funnel directly enabled the rapid acquisition of **1,000+ High Net-Worth Individual (HNI)** priority accounts.

---

## 🛑 1. Baseline Constraints: The "11-Minute Bottleneck"
Prior to intervention, the onboarding funnel was structurally incapable of scaling:
1.  **Operational Ceiling:** Manual KYC verifications created linear operational costs; we could only acquire users as fast as human compliance officers could review documents.
2.  **User Drop-off:** The 11-minute frontend TAT resulted in a 72% abandonment rate, fundamentally breaking the unit economics of digital customer acquisition.
3.  **Regulatory Risk vs. Speed:** Instant account activation posed massive AML risks, yet manual reviews destroyed the user experience. 

---

## ⚙️ 2. Solution Architecture: The "Generate & Freeze" Model
To balance high-speed acquisition with RBI compliance, I designed a two-phase decoupled architecture: **Instant Provisioning** followed by **ML-Assisted Verification**.

### The Technical Pillars:
*   **Phase 1: API Gateway (Account Generation):** Integrated a native biometric/OTP layer connected directly to UIDAI. Upon successful demographic match, the Core Banking System (CBS) provisions an account immediately, but places it in a **FROZEN (Restricted) state**. This secures the customer acquisition and prevents drop-off.
*   **Phase 2: The Vision ML Engine:** Built a proprietary Computer Vision model trained on millions of historical customer photographs. The engine asynchronously processes the live video feed against the UIDAI source photo, extracting a deterministic **Facial Accuracy Score** and **Age Estimation**.
*   **Phase 3: Human-in-the-Loop (HITL) Dashboard:** The ML outputs do not auto-approve. Instead, they serve as a high-fidelity decision support layer for the Video KYC agent. The human agent reviews the ML confidence scores to authorize the final unfreezing of the account.

---

### 🔄 System Handshake (Sequence Architecture)
The following Mermaid.js diagram maps the separation of Account Generation and the ML-assisted Video KYC activation.
```mermaid
sequenceDiagram
    autonumber
    actor U as User (Mobile Client)
    participant B as API Gateway
    participant UIDAI as UIDAI / AUA
    participant CBS as Core Banking Ledger
    participant ML as Computer Vision ML Engine
    participant VKYC as Human V-KYC Agent

    rect rgb(240, 248, 255)
        Note over U, CBS: Phase 1: High-Speed Acquisition (2-Minute TAT)
        U->>B: Input Aadhaar & Grant Consent
        B->>UIDAI: Request Authentication
        UIDAI-->>B: 200 OK (Demographic Payload & Source Photo)
        B->>CBS: POST /provision-account
        CBS-->>U: Return Account Number (Status: FROZEN)
    end
    
    rect rgb(255, 245, 238)
        Note over B, VKYC: Phase 2: Asynchronous ML Processing
        U->>B: Initiate Video KYC Call
        B->>ML: Stream Live Frame + UIDAI Source Photo
        ML->>ML: Run Facial Match % & Age Spoof Detection
        ML-->>VKYC: Pre-load Dashboard with ML Confidence Scores
    end

    rect rgb(245, 255, 250)
        Note over U, CBS: Phase 3: Human-in-the-Loop Authorization
        VKYC->>U: Conduct RBI Mandated Live Interaction
        VKYC->>VKYC: Agent validates against ML Decision Layer
        VKYC->>CBS: Auth Token: APPROVE & UNFREEZE
        CBS-->>U: Account Upgraded (Status: ACTIVE)
    end
