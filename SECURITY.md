# Security Policy

The KOLEKTA team takes the security of our software and the privacy of our users seriously. This document outlines our security policy, how to report vulnerabilities, and the security considerations specific to our platform.

## Supported Versions

> [!NOTE]
> KOLEKTA has not yet made any official releases. Once releases begin, this section will list which versions receive security updates.

| Version | Supported |
|---|---|
| (no releases yet) | N/A |

When releases begin, the project intends to provide security updates for:

- The latest stable release
- The previous minor release (for a limited transition period)

## Reporting a Vulnerability

If you discover a security vulnerability in KOLEKTA, **please report it responsibly**. Do not open a public GitHub Issue for security vulnerabilities.

### How to Report

Send a detailed report to:

**creatorerasmo@gmail.com**

> [!NOTE]
> Please include `[SECURITY]` in the email subject line. Mark your message as security-sensitive. Do NOT open a public GitHub Issue for security vulnerabilities.

### What to Include

Please provide as much of the following information as possible to help us understand and reproduce the issue:

- **Description**: A clear description of the vulnerability and its potential impact.
- **Affected component**: Which part of the system is affected (e.g., API, frontend, GPS ingestion, database).
- **Steps to reproduce**: Detailed steps to reproduce the vulnerability, including any prerequisites.
- **Proof of concept**: Code snippets, screenshots, or logs that demonstrate the vulnerability (if available).
- **Environment**: The version of KOLEKTA, operating system, browser, or other relevant software versions.
- **Severity assessment**: Your assessment of the severity (Critical, High, Medium, Low) and why.
- **Suggested fix**: If you have ideas for how to address the vulnerability, we welcome them.

### Response Timeline

We commit to the following response timeline for security reports:

| Milestone | Target |
|---|---|
| **Acknowledgement** | Within 48 hours of receipt |
| **Initial assessment** | Within 7 days of receipt |
| **Status update** | At least every 14 days until resolved |
| **Fix release** | As soon as practical, prioritised by severity |

These timelines are goals, not guarantees. Complex vulnerabilities may require more time to investigate and address properly. We will keep you informed of progress throughout the process.

## Responsible Disclosure

We follow a responsible disclosure policy:

1. **Report privately.** Do not disclose vulnerability details publicly until a fix has been released and an appropriate disclosure timeline has been agreed upon.

2. **Allow reasonable time.** Give the project team reasonable time to investigate, develop, and release a fix before any public disclosure.

3. **Coordinate disclosure.** We will work with you to coordinate the timing and content of any public disclosure. We believe in transparency and will credit reporters (with their permission) in security advisories.

4. **No retaliation.** We will not take legal action against or retaliate against anyone who reports a vulnerability in good faith, even if the report turns out to be invalid.

5. **Act in good faith.** Do not access, modify, or delete data belonging to other users during your research. Use test accounts and environments where possible.

## Security Considerations for KOLEKTA

KOLEKTA handles several categories of sensitive data that require careful security design. Contributors and developers should be aware of these considerations when working on any part of the system.

### GPS & Vehicle Tracking Data

- **Vehicle position data** must be treated as operationally sensitive. Real-time fleet positions could be exploited if exposed to malicious actors.
- GPS data should be transmitted over encrypted channels (TLS) and stored with appropriate access controls.
- Historical trajectory data should have retention policies — data older than a defined period should be aggregated or purged.
- API endpoints exposing vehicle positions must implement rate limiting and, where appropriate, access controls.

### User Location Data

- Location data and its linked push subscription are **sensitive**. The server stores them for up to 30 days only when a user explicitly enables background notifications; otherwise, location remains client-side.
- Collect only the precision needed for the selected notification radius. Do not collect a location history.
- Explain server-side storage and retention before opt-in. Users must be able to revoke location permission or unsubscribe; unsubscribing deletes the associated zone and push subscription.
- Restrict access to notification-zone data to the services that evaluate proximity and deliver push messages. Do not expose the coordinates or push keys through public APIs or logs.
- Do not share location data with third parties except the push-delivery provider required to send a requested notification, and disclose that processing to the user.

### Notification Permissions

- Push notification tokens (FCM, Web Push) are linked to individual devices and must be stored securely.
- Notification payloads should not contain sensitive location details — they should reference general areas (e.g., neighbourhood names) rather than precise coordinates.
- Users must be able to unsubscribe from notifications easily.

### Authentication & Authorisation

- The system should support anonymous usage for basic features (viewing vehicle positions on a map).
- Features that require user-specific state (saved locations, notification preferences) will require authentication.
- Administrative and operator APIs must implement role-based access control (RBAC).
- API keys for GPS data ingestion from vehicle operators must be managed securely and rotated regularly.

### Data Privacy Compliance

- While Angola's data protection regulatory framework is evolving, KOLEKTA should adopt privacy-by-design principles aligned with international best practices.
- Data collection should follow the **principle of data minimisation** — only collect what is necessary for the platform to function.
- Users should be informed about what data is collected, how it is used, and how long it is retained.
- A privacy policy should be published before the platform becomes publicly available.

### Infrastructure Security

- All services should run in isolated containers with minimal privileges.
- Database access should be restricted to application services only, with no direct external access.
- Secrets (API keys, database credentials, notification service keys) must be managed through environment variables or a secrets manager — never committed to the repository.
- Dependencies should be regularly audited for known vulnerabilities (e.g., using `npm audit`, `pip audit`, or similar tools).

---

> [!NOTE]
> This security policy will be updated as the project evolves and specific security controls are implemented. Contributors are encouraged to flag security considerations in their pull requests and design proposals.
