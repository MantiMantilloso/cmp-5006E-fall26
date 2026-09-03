# STRIDE — one threat per category, specific to DVWA

| Category | Threat |
|---|---|
| Spoofing | An attacker steals another user's PHPSESSID cookie (for example via the XSS module) and reuses it to impersonate that logged-in user without knowing their password. |
| Tampering | In the SQL Injection module, the `id` parameter is concatenated into the query without sanitization at low security, allowing an attacker to modify the query logic to retrieve rows they should not see. |
| Repudiation | DVWA keeps no audit log of who performed an action, so a user who changes a password or triggers an action via CSRF can deny having done it because there is no record tying the action to their identity. |
| Information Disclosure | The SQL Injection module returns raw database contents (usernames, password hashes) directly in the HTTP response, and at low or medium security, PHP error messages can leak file paths or query structure. |
| Denial of Service | The Command Injection module lets an attacker pass OS commands to a shell function (for example `ping`), and a payload can be crafted to consume server resources or crash the application for other users. |
| Elevation of Privilege | A low-privileged or authenticated user manipulates the security level cookie or exploits a broken access control path on an admin-only page (such as the DVWA security settings page) to gain functionality reserved for an admin role. |