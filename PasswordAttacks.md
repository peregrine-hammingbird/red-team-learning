# THM Today's Progress -Red Teaming-Initial Access-Password Attacks

## Overview
Completed the **Password Attacks** room in the TryHackMe Red Teaming module.  
This room focused on practical password attack techniques used during red team engagements, including wordlist generation, mutation rules, and online brute-force attacks.

---

## Key Topics & Skills Learned

### 🔐 Password Attack Methodology
- Understanding when and how password attacks are used in red team operations
- Differences between offline cracking and online brute-force attacks
- Recognizing protocol-specific limitations and failure conditions

### 🧩 Wordlist Generation
- Creating custom wordlists using **Crunch**
- Pattern-based generation using masks (`-t`)
- Learning how small syntax differences drastically affect output

### 🔄 Password Mutation & Rules
- Using **John the Ripper** for rule-based password generation
- Applying custom rule sets (e.g. `Single-Extra`)
- Troubleshooting rule syntax and option conflicts
- Generating candidate passwords using `--stdout` for reuse in other tools

### ⚔️ Online Attacks
- Performing brute-force attacks with **Hydra**
- Attacking multiple services:
  - SSH
  - SMTP / SMTPS
  - Web login forms (`http-post-form`)
- Interpreting authentication errors vs. connectivity issues
- Using flags such as `-f` and `-v` appropriately

### 🧠 Troubleshooting & Lessons Learned
- Understanding why attacks fail (not just how to run tools)
- Common pitfalls:
  - Incorrect form parameters
  - Regex/PCRE errors
  - Service-side authentication restrictions
- Importance of patience and methodical debugging

---

## Takeaways
- Password attacks are rarely “fire and forget”
- Tool output interpretation is a core skill
- Custom wordlists outperform generic ones in realistic scenarios
- Persistence and troubleshooting are essential red team traits

---

🛠 Tools used:
- Crunch
- John the Ripper
- Hydra

📚 Platform: TryHackMe
