# Broken Authentication + OAuth 2.0

A comprehensive lecture covering broken authentication patterns, insecure session management, and OAuth 2.0 integration pitfalls. Includes 10 practical labs and defense strategies.

## Objectives
- Understand authentication vs authorization and session management fundamentals
- Identify and exploit common broken authentication flaws
- Learn OAuth 2.0 roles, flows (Authorization Code + PKCE, Client Credentials), and endpoints
- Execute common OAuth attack paths and design mitigations

## Topics
- Password attacks: brute force, credential stuffing, spraying
- Session issues: fixation, hijacking, missing rotation, cookie flags
- Password reset flaws: predictable tokens, missing ownership verification
- JWT pitfalls: none algorithm, key confusion, weak secret storage
- OAuth: redirect_uri manipulation, code interception, PKCE weaknesses, account linking, mix-up attack

## Labs (10)
1. Brute Force + Rate Limiting
2. Session Fixation
3. Weak Reset Token
4. JWT None/HS256 Confusion
5. OAuth Code Interception (redirect_uri)
6. PKCE Downgrade
7. Scope Escalation
8. Account Linking Abuse
9. Refresh Token Misuse
10. 2FA Bypass Logic

## Defense
- Enforce MFA and rate limiting; lockout with exponential backoff
- Strong hashing (Argon2/Bcrypt), password policies, breach checks
- Secure cookies (HttpOnly, Secure, SameSite=strict), CSRF tokens
- Session rotation on login, logout everywhere, device binding
- OAuth: strict redirect_uri allowlist, PKCE required, minimize scopes, short-lived access tokens, refresh token rotation and revoke 