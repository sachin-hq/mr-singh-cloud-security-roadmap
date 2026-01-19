PART 4: INTERVIEW BLOCK — 40 MIN (ENGLISH ONLY)

Q1. What is the difference between identity and credential?

Answer (English):
"Identity is the principal — the 'who' — such as a user, service account, or role. It represents the entity requesting access. A credential is the secret proof used to authenticate that identity, such as a password, access key, token, or certificate. Identity can be publicly known, but credentials must always remain confidential."

Answer (Hindi explanation):
"Identity matlab 'kaun hai' — user, service, ya role. Credential matlab 'proof' — password, key, token jo prove karta hai ki tum wahi ho. Identity public ho sakti hai, par credential hamesha secret rehna chahiye."

Follow-up interviewer might ask:
"Can you give examples from AWS?"

Answer:
"Sure. In AWS, an IAM user like `admin-john` is an identity. The corresponding credentials could be a password for console login or an access key pair (access key ID and secret access key) for programmatic access. The IAM role `EC2-WebServer-Role` is another identity, and when an EC2 instance assumes it, temporary session credentials are generated."

Q2. Why are credentials the main perimeter in cloud security?

Answer (English):
"In cloud environments, resources are controlled through APIs and management consoles rather than physical access. If an attacker obtains valid credentials, they can authenticate as a legitimate user and perform actions without needing to exploit vulnerabilities. The cloud infrastructure trusts the credentials, not the person. This makes credentials the primary security boundary — once they're compromised, the attacker essentially becomes an authorized user."

Answer (Hindi explanation):
"Cloud mein sab kuch API aur console se control hota hai. Agar attacker ke paas valid credentials aa gaye, to woh legitimate user ban jata hai. Cloud system credentials ko trust karta hai, person ko nahi. Isliye credentials hi boundary hain — agar woh leak ho gaye, attacker authorized user ban jata hai."

Architect-level addition:
"That's why we design for assumed breach — we implement least privilege, time-bound sessions, and anomaly detection to limit damage even if credentials are compromised."

Q3. Give examples of credential compromise in cloud.

Answer (English):
"Common credential compromise scenarios include:

1. Phishing — Attacker sends fake login page, user enters credentials thinking it's legitimate
2. GitHub secrets leak — Developers accidentally commit AWS access keys or API tokens to public repositories
3. Session hijacking — Malware steals browser cookies or OAuth tokens
4. Weak MFA — SMS-based OTP can be intercepted through SIM swapping
5. SSRF attacks — Exploiting vulnerable applications to access cloud metadata endpoints and steal instance role credentials
6. Credential stuffing — Using previously leaked passwords from other breaches to try cloud accounts"

Real-world example to mention:
"For instance, the Uber breach in 2016 involved an attacker finding AWS credentials in a GitHub repository, which led to the exposure of 57 million user records."

Q4. What is the best practice to reduce credential risk?

Answer (English):
"A multi-layered approach is necessary:

1. Strong authentication — Enforce phishing-resistant MFA, preferably hardware security keys like YubiKeys rather than SMS
2. Least privilege — Grant only the minimum permissions required, avoid wildcards in policies
3. Short-lived credentials — Use temporary session tokens and IAM roles instead of long-lived access keys
4. Eliminate root usage — Never use root or global admin accounts for daily operations
5. Secret management — Store credentials in dedicated secret managers, never in code
6. Logging and monitoring — Enable comprehensive audit logging and anomaly detection to identify compromised credentials quickly
7. Regular rotation — Rotate credentials every 90 days or less
8. Automated scanning — Implement pre-commit hooks and CI/CD secret scanning to prevent credential leaks"

Q5. Explain account takeover (ATO) in cloud context.

Answer (English):
"Account takeover occurs when an attacker gains unauthorized access to a cloud account by obtaining valid credentials. Unlike traditional infrastructure attacks that require exploiting vulnerabilities, cloud ATO simply requires valid authentication. Once inside, the attacker can perform any action the compromised identity is authorized for — data exfiltration, resource manipulation, privilege escalation, or deploying malicious workloads. The challenge is that these actions appear legitimate in logs since valid credentials are being used."

How to prevent:
"Prevention requires defense-in-depth: MFA to prevent initial compromise, least privilege to limit blast radius, anomaly detection to identify suspicious behavior, and rapid incident response to minimize damage."

Q6. What is blast radius and why does it matter?

Answer (English):
"Blast radius is the scope of damage an attacker can cause if a specific set of credentials is compromised. For example, if a service account with full S3 access has its credentials stolen, the blast radius is limited to S3 operations. However, if an IAM user with AdministratorAccess is compromised, the blast radius is the entire AWS account.

We minimize blast radius through:
- Least privilege policies
- Permission boundaries
- Resource-based policies
- Separate accounts for different environments
- Time-bound elevated access

This way, even if one credential leaks, the damage is contained."

Q7. How do you detect credential compromise?

Answer (English):
"Detection relies on behavioral analytics and anomaly detection:

1. Impossible travel — Same user logging in from geographically distant locations within a short time
2. New device or IP — Login from previously unknown sources
3. Unusual API patterns — Sudden spike in API calls or calls to sensitive APIs
4. Mass data access — Large volume downloads or listings
5. Privilege escalation attempts — Calls to IAM modification APIs
6. Failed authentication spikes — Indicating credential stuffing or brute force

Tools like AWS GuardDuty, Azure Sentinel, and Google Security Command Center use machine learning to identify these patterns and alert on potential compromise."

Q8. What is MFA fatigue attack?

Answer (English):
"MFA fatigue, also called push bombing, is when an attacker who has obtained a user's password repeatedly sends MFA push notifications hoping the user will eventually approve one out of annoyance or confusion. This exploits human psychology rather than technical vulnerabilities.

Defenses include:
- Number matching MFA (user must enter displayed number)
- FIDO2 hardware keys that can't be fatigued
- Rate limiting push notifications
- Alerting on multiple rejected MFA attempts
- User training to never approve unexpected MFA requests"

Q9. Why are long-lived access keys risky?

Answer (English):
"Long-lived access keys remain valid until manually rotated or deleted, which creates several risks:

1. Persistent exposure — If leaked, they remain usable indefinitely
2. Rotation burden — Manual rotation is often forgotten or delayed
3. No session controls — Can't be revoked without deleting the key
4. Hard to track — Difficult to know where keys are stored and used

Short-lived credentials like STS session tokens are better because they:
- Auto-expire after set duration (1-12 hours)
- Reduce attack window
- Force periodic re-authentication
- Enable automatic rotation

Best practice is to eliminate long-lived keys entirely in favor of IAM roles with temporary credentials."

Q10. Explain SSRF metadata credential theft.

Answer (English):
"Cloud instances like AWS EC2 have a metadata service accessible at `http://169.254.169.254`. This service provides configuration data and, crucially, temporary credentials for the instance's IAM role. If an application running on the instance has an SSRF (Server-Side Request Forgery) vulnerability, an attacker can force it to query the metadata endpoint and retrieve the credentials.

For example:
Returns temporary access keys, secret keys, and session tokens.

Defenses:
- IMDSv2 — Requires a token-based request that SSRF attacks can't typically perform
- Least privilege roles — Limit what the instance role can do
- Egress restrictions — Block outbound connections to metadata IPs
- Application security — Fix SSRF vulnerabilities"



 Q11. What are the three A's of security, and how do they relate to credentials?

Answer (English):
"The three A's are:

1. Authentication — Verifying identity using credentials (who are you?)
2. Authorization — Determining permissions (what can you do?)
3. Accounting/Auditing — Logging actions (what did you do?)

Credentials are critical to all three:
- In authentication, credentials prove identity
- In authorization, the authenticated identity's permissions are evaluated
- In auditing, logs record which credentials were used for which actions

If credentials are compromised, an attacker passes authentication, gains the identity's authorization, and their malicious actions appear legitimate in audit logs."



 Q12. How would you respond to a discovered access key leak on GitHub?

Answer (English - Step-by-step incident response):

"Immediate actions (within 5 minutes):
1. Disable the leaked access key in IAM console
2. Identify which IAM user/role it belongs to

Short-term (within 1 hour):
1. Review CloudTrail logs for all activity with that access key
2. Identify any unauthorized actions
3. Create new access key for legitimate use
4. Delete the compromised key
5. Notify security team

Assessment (within 4 hours):
1. Determine blast radius — what permissions did the key have?
2. Check for data exfiltration, new resources, policy changes
3. Review GuardDuty findings
4. Document timeline of exposure

Remediation (same day):
1. Revert any unauthorized changes
2. Scan all repositories for other exposed secrets
3. Implement git-secrets or similar pre-commit hooks
4. Rotate related credentials as precaution

Long-term (within 1 week):
1. Eliminate long-lived keys — migrate to IAM roles
2. Implement automated secret scanning in CI/CD
3. Conduct post-mortem and update runbooks
4. Train developers on secret management best practices

This demonstrates both technical knowledge and incident response methodology."



 English Bypass Strategy for Interviews (BONUS SECTION)

Problem: English weak hai, par interview English mein hoga

Solution Strategy:

 Strategy 1: Template-based Answers

Pre-memorize these sentence structures: