 # Technical Analysis of CVE-2024-3094: XZ Utils Supply Chain Backdoor

#### Video Demo: https://youtu.be/3xBtabSkusE?si=ChN3nBgybK4nqNLz

#### Description:
An in-depth technical and architectural breakdown of **CVE-2024-3094** (CVSS 10.0 Critical), the historic open-source supply chain backdoor embedded into `xz-utils` / `liblzma` (versions 5.6.0 and 5.6.1) discovered in March 2024. 

This presentation was developed as the Final Project for **CS50’s Introduction to Cybersecurity** by Harvard University / edX.

---

### 👤 Project Metadata
* **Student Name:** Namegabe Mulokwa Victoire
* **edX Username:** namegabevictoire01
* **GitHub Username:** namegabevictoire01-sys
* **Date:** September 15, 2026
* **Course:** CS50 Cybersecurity (Final Project)

---

### 📌 Executive Summary
In March 2024, a critical supply chain attack was discovered inside `xz-utils`, a fundamental compression utility utilized across major Linux distributions (Debian, Fedora, Ubuntu, Arch, RHEL). Assigned the maximum vulnerability score of **CVSS 10.0**, the backdoor targeted `liblzma.so` during compilation to hijack OpenSSH (`sshd`) processes, allowing unauthorized remote code execution (RCE) with full `root` privileges.

This project examines the complete lifecycle of the vulnerability across five key dimensions:
1. **Scope & Impact:** Global footprint of `liblzma` and core system security concepts.
2. **Social Engineering:** The 2-year multi-persona infiltration campaign by threat actor 'Jia Tan'.
3. **Technical Mechanics:** Obfuscated test files, build-time `m4` execution, and GNU IFUNC symbol hijacking.
4. **Detection & Payload Execution:** Cryptographic signature verification (ED448) and Andres Freund’s anomaly discovery.
5. **Systemic Mitigation:** Multi-maintainer governance, reproducible builds, and privilege decoupling.

---

### 🛠️ Key Technical Findings

* **Social Engineering / Human Infiltration:** The threat actor established trust over 24 months through genuine contributions before leveraging coordinated sockpuppet accounts to pressure the sole maintainer, eventually gaining administrative commit and release-signing permissions.
* **Build-Time Obfuscation:** Malicious binary objects were hidden inside disguised test archives (`bad-3-corrupt_lzma2.xz`). An injected `build-to-host.m4` macro verified environment conditions (x86_64 Linux target packaging) before extracting and linking the payload during binary generation.
* **OpenSSH Interception via GNU IFUNC:** On systemd-enabled Linux distributions, systemd links OpenSSH with `liblzma`. The backdoor utilized GNU Indirect Functions (IFUNC) to hook into memory during dynamic symbol resolving, replacing `RSA_public_decrypt`.
* **Covert Remote Execution:** The hooked function evaluated incoming SSH payloads against a hardcoded ED448 key. Valid signatures triggered instant root execution without leaving traces in system logs (`syslog`), while non-matching attempts passed through seamlessly to standard authentication.

---

### 📂 Repository & Project Structure

```text
.
├── presentation_slides/     # High-resolution 16:9 presentation slides (PNG)
│   ├── slide1_title.png
│   ├── slide2_social_engineering.png
│   ├── slide3_technical_details.png
│   ├── slide4_discovery.png
│   └── slide5_mitigation.png
├── audio_transcripts/      # AI Voiceover transcripts and timing scripts
│   └── narration_script.txt
├── README.md                # Project documentation and summary
└── project_details.txt      # Course submission metadata
