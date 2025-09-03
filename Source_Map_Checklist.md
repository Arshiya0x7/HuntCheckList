# 🕵️ Source Map Pentesting Checklist

A practical checklist for penetration testers after extracting source code from exposed JavaScript **source maps** (`.map` files).  
This guide helps identify sensitive data, hidden logic, and potential vulnerabilities.

---

## 🔍 Step 1: Recon & Structure
- [ ] List all files and directories:
  ```bash
  tree -L 3 ./reconstructed > structure.txt

- [ ] Identify critical files (config.js, constants.ts, auth.js, api.js).

- [ ] Extract external URLs and domains:
  ```bash
    rg -o 'https?://[^\")\']+' ./reconstructed | sort -u > endpoints.txt

# 🔑 Step 2: Secret Hunting

- [ ] Scan for secrets using tools:

  ```bash
  trufflehog filesystem ./reconstructed
  gitleaks detect --source=./reconstructed

- [ ] Manual grep searches:
  ```bash
    grep -R -i "api_key" ./reconstructed
    grep -R -i "secret" ./reconstructed
    grep -R -i "token" ./reconstructed
    grep -R -i "password" ./reconstructed

# 🌐 Step 3: API Endpoint Discovery

- [ ] Search for fetch, axios, or graphql calls:
  ```bash
  rg -i "fetch\(|axios\." ./reconstructed > api_calls.txt

- [ ] Test endpoints in BurpSuite/Postman:

  * Auth bypass
  * IDOR (userId, orderId replacement)
  * Rate-limit bypass
  * GraphQL introspection

# 🛡 Step 4: Authentication & Session

- [ ] Review login/signup flows.

- [ ] Check for hardcoded JWT secrets or algorithms.

- [ ] Verify token storage (localStorage/sessionStorage).

- [ ] Test for open redirects in login/logout/redirect flows.

# ⚡ Step 5: XSS & Input Handling

- [ ] Search for dangerous functions:

      rg -i "innerHTML" ./reconstructed
      rg -i "eval" ./reconstructed
      rg -i "dangerouslySetInnerHTML" ./reconstructed

- [ ] Analyze regex-based validation → attempt bypass.

# 📦 Step 6: Dependency Vulnerabilities

- [ ] Extract all packages and versions (from pnpm, package.json, etc.).

- [ ] Run vulnerability scans:

      npm audit --production --directory ./reconstructed

- [ ] Cross-check with Snyk or Exploit-DB.

# 🔥 Step 7: Business Logic Testing

- [ ] Inspect cart, discount, payment related code.

- [ ] Check conditions like:

      if (discount > 50) { ... }

- [ ] Test if validations are only client-side or enforced server-side.

# 🧪 Step 8: Feature Discovery

Search for feature flags (process.env.FEATURE_*).

Look for hidden routes or disabled code paths.

    Test endpoints for availability even if hidden in UI.

# 📊 Step 9: Logging & Debugging

- [ ] Search for debug functions:

      rg -i "console.log" ./reconstructed
      rg -i "debug" ./reconstructed
      rg -i "logger" ./reconstructed

- [ ] Identify if developer/debug flags are left enabled.

# ⚔️ Step 10: Tools


* fast searching  →   `ripgrep (rg)`

* secret hunting  →   trufflehog / gitleaks

* dependency vulns  →  npm audit / Snyk

* API testing   →  BurpSuite / Postman

# ✅ Deliverables

At the end of the assessment, prepare a findings report including:

* Secrets exposed (API keys, tokens, credentials)
* Sensitive or undocumented API endpoints
* Business logic flaws (e.g., cart/discount/payment manipulation)
* Client-side XSS opportunities
* Dependency vulnerabilities
