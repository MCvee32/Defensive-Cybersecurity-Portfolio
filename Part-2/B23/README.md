# Part 2 - B23: Test an intrusion detection system and discuss its effectiveness
### IDS Used: Snort
A free, open-source network intrusion detection system (NIDS) and intrusion prevention system (IPS) that monitors network traffic in real time. It analyzes packets to detect malicious activity, such as scans and attacks, by comparing them against a set of user-defined rules.

## 1. Snort Installation 
<img width="759" height="315" alt="Screenshot 2026-04-06 at 5 38 40 pm" src="https://github.com/user-attachments/assets/faff4589-c214-4c8e-8f16-06fbfbfeae75" />

## 2. Setup Rules
<img width="476" height="109" alt="Screenshot 2026-04-06 at 5 41 54 pm" src="https://github.com/user-attachments/assets/c0298bab-8fd8-4dc4-a55f-3067fb41d116" />

## 3. Run IDS

## 4. Simulate Attack  
- Using nmap to simulate reconaissance
<img width="616" height="216" alt="Screenshot 2026-04-06 at 5 59 28 pm" src="https://github.com/user-attachments/assets/f270a94c-2ddc-45cc-8b0e-c3332d27a54d" />

## 5. Snort IDS Alerts

### Effectiveness of Snort
**Strengths:**  
- Real-time monitoring: detects attacks as they happen
- Signature-based detection
- Low cost

**Limitations:**  
- Only detects attacks, cannot block them
- False positives: can flag normal traffic as malicious
- Limited against new/unknown attacks
