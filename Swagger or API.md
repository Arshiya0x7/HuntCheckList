# quick checklist for API and Endpoint 

# 🛠 Swagger / OpenAPI Pentest Checklist

## 🔍 1. Recon
- Extract all Endpoints (GET/POST/PUT/DELETE)
- Identify parameters (Query, Path, Header, Body)
- Review Schemas / Models for sensitive fields (isAdmin, role, balance)
- Check API versioning (/api/v1/, /api/v2/ ...)
- Look for internal comments / developer notes

## 🔑 2. Authentication & Access Control
- Test endpoints without tokens (bypass auth)
- Test user token on admin endpoints
- Modify Role in request body (role=admin)
- Check for IDOR (/users/{id} → change IDs)
- JWT manipulation (decode/modify claims, test without signature verification)

## 💉 3. Injection
- SQL Injection on parameters
- NoSQL Injection in JSON payloads
- Command Injection (system commands)
- Path Traversal (../../etc/passwd)
- Header Injection (X-Forwarded-For, Host)

## 🔄 4. Unexpected Input (Fuzzing)
- Send wrong data type (string instead of int)
- Oversized payloads (DoS testing)
- Extra fields not defined in schema
- Null / empty values
- Negative or abnormal values (-1, 999999999)

## 🔐 5. Sensitive Data Exposure
- Inspect swagger.json for API Keys, sample JWTs, developer emails
- Check API responses for sensitive data (stack trace, internal errors)

## 🌐 6. HTTP Methods & Misconfigurations
- Test all HTTP methods (GET, POST, PUT, DELETE)
- Check CORS configuration (Access-Control-Allow-Origin: *)
- Test OPTIONS method (information disclosure)
- Check for missing rate limiting (DoS potential)

## 🛠 7. Tools
- Postman → Import swagger.json → manual testing
- Burp Suite → Import swagger → fuzzing / scanning
- OWASP ZAP (OpenAPI plugin) → automated scanning
- sqlmap on parameters
- ffuf / wfuzz for brute-forcing hidden endpoints

## 🎯 8. Special Tricks
- Check /swagger.json, /openapi.json, /v2/swagger.json
- Look for /swagger-ui/, /swagger-ui.html, /redoc/
- Host Header Injection (SSRF)
- Hidden or undocumented endpoints
