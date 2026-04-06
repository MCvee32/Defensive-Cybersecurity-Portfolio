<img width="549" height="582" alt="Screenshot 2026-04-06 at 9 26 58 am" src="https://github.com/user-attachments/assets/7038dd3e-1188-4efc-b9df-9d153cfa8870" /># Part 2 - B29: Find a CVE in this year and fix it using three different generative AI systems (e.g., ChatGPT, Gemini), comparing the consistency
### CVE-2026-3644: Incomplete control character validation in http.cookies
**Description:**
The fix for CVE-2026-0672, which rejected control characters in http.cookies.Morsel, was incomplete. The Morsel.update(), |= operator, and unpickling paths were not patched, allowing control characters to bypass input validation. Additionally, BaseCookie.js_output() lacked the output validation applied to BaseCookie.output().

### 1. Claude
**Vulnerability:**
Claude provides a description of the three vulnerable paths in the code:
1. Morsel.update() — directly updated the internal dictionary without validation. 
2. The __ior__ method (implementing |=) was entirely missing, falling back to default behavior with no validation.
3. BaseCookie.js_output() generated JavaScript output without the same sanitization applied to output().
**Impact:**
<img width="696" height="205" alt="Screenshot 2026-04-06 at 9 37 03 am" src="https://github.com/user-attachments/assets/835deb67-8ed3-4774-b095-bcd83d1a2d8f" />

**The Fix:**
<img width="680" height="394" alt="Screenshot 2026-04-06 at 9 37 05 am" src="https://github.com/user-attachments/assets/25db08f5-d865-4c4e-b9ca-0ff9ae3d714c" />

### 2. ChatGPT
**Vulnerability:**
ChatGPT highlight four problems with the code.
Python tried to block control characters in cookies, but missed some code paths:
1. Morsel.update()
2. |= operator
3. unpickling
3. BaseCookie.js_output() (no output validation)
**Impact:**
Attackers can bypass validation, leading to:
- HTTP response splitting
- Session manipulation
- Possible XSS / header injection
**The Fix:**
ChatGPT highlights one primary fix and three mitigations (when primary fix isn't possible)
Primary Fix: Upgrading Python to a patched version
<img width="696" height="623" alt="Screenshot 2026-04-06 at 9 18 30 am" src="https://github.com/user-attachments/assets/1937a66b-9cb4-4610-97ae-4cc678036fcd" />
Mitigations:
1. Sanitise Cookie Input Manually
<img width="693" height="359" alt="Screenshot 2026-04-06 at 9 16 40 am" src="https://github.com/user-attachments/assets/2f462aac-0c01-4814-a55f-6d139e46782d" />
2. Avoide Unsafe Paths
<img width="695" height="471" alt="Screenshot 2026-04-06 at 9 16 43 am" src="https://github.com/user-attachments/assets/e79f9abd-e367-4634-b81e-c330a9664641" />
3. Block Malicious Input at The Edge
<img width="693" height="319" alt="Screenshot 2026-04-06 at 9 16 46 am" src="https://github.com/user-attachments/assets/c285ac1d-4360-457b-9214-358032f375c4" />

### 3. Gemini
**Vulnerability:**
Gemini highlights four specific "leaks" where validation is missing:
1. Morsel.update(): Updating cookie attributes via dictionary methods.
2. The |= operator: Merging cookie attributes.
3. Unpickling: Deserializing Morsel objects from a pickle stream.
4. BaseCookie.js_output(): Generating JavaScript-formatted cookie strings.

**Impact:**
Possible:
- HTTP Response Splitting
- Session Hijacking

**The Fix:**
Provides a patch similar to the official CPython fix, for when upgrading isn't possible. As well as mitigations.
Fix:
<img width="549" height="582" alt="Screenshot 2026-04-06 at 9 26 58 am" src="https://github.com/user-attachments/assets/424bd17c-d41a-4bcc-8ac0-edaf3406d267" />
Mitigations:
<img width="539" height="349" alt="Screenshot 2026-04-06 at 9 27 09 am" src="https://github.com/user-attachments/assets/1106db2b-f12e-4e0a-ba43-f805463fc5bf" />

### Conclusion
All three AI assitants highlight Morsel.update, the |= operator, unpickling, and BaseCookie.js_output() as vulnerabilities in the code. Except for Claude, which didn't highlight unpickling as a vulnerability in the code. ChatGPT and Gemini both outputed similar results with the primary fix being an upgrade to a patched version of Python. Whereas, Claude made sure all three vulnerable paths are passed through the _has_control_character() validator, but still mentioned an upgrade to a patched version as a mitigation strategy. All three also had similar mitigation strategies and impacts.
