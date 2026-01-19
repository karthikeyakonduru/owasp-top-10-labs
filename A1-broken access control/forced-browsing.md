# Insecure Direct Object Reference (IDOR) Vulnerability



## 1. Summary

An **Insecure Direct Object Reference (IDOR)** vulnerability was identified in the chat report download functionality of the application.  
By modifying the `file` parameter in an intercepted HTTP request, an authenticated user can access chat reports belonging to other users. This leads to exposure of sensitive information, including user credentials and private conversations.


## 2. Vulnerability Details

- **Vulnerability Type:** Insecure Direct Object Reference (IDOR)
- **OWASP Category:** A01 – Broken Access Control
- **Affected Endpoint:**  
```GET /download?file=2.txt```
- **Authentication Required:** Yes (but insufficient authorization checks)


## 3. Steps to Reproduce

1. Log in to the application as a normal authenticated user.
2. Navigate to the chatbot feature.
3. Start a chat and generate a chat report.
4. Download the generated chat report (e.g., `2.txt`).
5. Intercept the download request using Burp Suite.
6. Modify the `file` parameter value from `2.txt` to `1.txt`.
7. Forward the modified request to the server.


## 4. Proof of Concept (PoC)

### Original Request
```
 GET /download?file=2.txt HTTP/1.1 
 Host: target.com
```
### Modified Request
```
GET /download?file=1.txt HTTP/1.1
Host: target.com
```

### Result
The server responds with the contents of `1.txt`, which belongs to another user and contains sensitive information, including credentials.


## 5. Impact

An attacker can:
- Access private chat reports of other users
- Obtain sensitive information such as usernames and passwords
- Violate user privacy
- Potentially perform account takeover or further attacks


## 6. Severity

**High**

**CVSS v3.1 Score:** 7.5  
**Vector:** AV:N / AC:L / PR:L / UI:N / S:U / C:H / I:N / A:N


## 7. Recommendations

- Implement strict server-side authorization checks for all file access requests.
- Ensure users can access only resources that belong to their account.
- Avoid predictable or sequential identifiers (e.g., numeric filenames).
- Bind file access to the authenticated user’s session or user ID.
- Regularly review and test access control mechanisms.


## 8. References

- OWASP Top 10 – Broken Access Control  
- OWASP Testing Guide – IDOR

