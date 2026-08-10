# Final Project: Technical Analysis of CVE-2024-3094 (XZ Utils Backdoor)

## Author Information
- **Name:** Namegabe Mulokwa Victoire
- **edX Username:** namegabevictoire01
- **GitHub Username:** namegabevictoire01-sys
- **Location:** Bukavu, Democratic Republic of the Congo
- **Date:** August 10, 2026

## Project Overview
This project provides a comprehensive cybersecurity analysis of the critical supply chain attack identified under CVE-2024-3094, commonly known as the XZ Utils backdoor incident. Discovered in late March 2024, this backdoor targeted the `liblzma` library, a core compression component widely deployed across major Linux distributions.

## Background and Chronology
The attack was executed through a multi-year social engineering campaign. Starting around 2022, an account under the name 'Jia Tan' contributed legitimate patches to the XZ Utils repository to build trust within the developer community. Over time, the threat actor gained maintainer permissions and pushed hidden malicious files. In March 2024, Microsoft engineer Andres Freund identified a 500-millisecond delay during SSH authentication on Debian testing machines. Further investigation revealed that release tarballs for versions 5.6.0 and 5.6.1 contained modified build scripts not present in the public Git repository.

## Technical Mechanism
The injection mechanism was highly sophisticated and heavily obfuscated:
1. **Extraction:** During the build process, hidden M4 macros extracted an encrypted payload contained within binary test files.
2. **Hooking:** The compromised `liblzma` library hooked into OpenSSH (`sshd`) via systemd notification structures.
3. **Execution:** The injected code intercepted RSA signature verification routines during SSH authentication.
4. **Impact:** Any remote client sending a specifically crafted payload signed with a designated private RSA key could achieve arbitrary command execution (RCE) with full root privileges, bypassing normal authentication mechanisms completely.

## Recommendations and Mitigations
To protect open-source ecosystems from similar supply chain compromises, several measures must be implemented:
- **Governance:** Critical utility projects must avoid single-maintainer dependencies by establishing multi-maintainer governance structures.
- **CI/CD Security:** Automated auditing tools must verify that release tarballs match the source code in official Git repositories.
- **System Isolation:** Systems should limit unnecessary linking between system management daemons like systemd and exposed networking services like OpenSSH.

