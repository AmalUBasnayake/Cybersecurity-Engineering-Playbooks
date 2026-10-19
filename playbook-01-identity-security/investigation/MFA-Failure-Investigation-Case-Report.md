# MFA Failure Investigation — Case Report

**Project:** Amal Cyber Lab — Playbook 01: Microsoft Entra ID Security Engineering  
**Case ID:** P01-IR-001  
**Investigation date:** 13 October 2026  
**Environment:** Dedicated lab tenant  
**Identity under investigation:** `lab-reader@amaludayangalive.onmicrosoft.com`  
**Primary event time:** 2 October 2026, 02:01:51 UTC  
**Status:** Investigation documented; no confirmed compromise

> **Lab/publication note:** This report intentionally excludes public IP addresses, request IDs, tenant identifiers, and other unnecessary identifiers. Keep raw screenshots in the private evidence folder, and redact account/tenant details further if publishing this report publicly.

## 1. Executive summary

A Microsoft Entra ID interactive sign-in for the lab reader identity was interrupted. The recorded authentication details indicate that the password step succeeded, while the mobile-app authentication step did not succeed. The event's security evaluation showed that the Microsoft Entra **Security Defaults** MFA grant control returned **Failure**.

The evidence supports classifying this as an **interrupted sign-in with an unsuccessful MFA step**. It does not, by itself, establish malicious activity or account compromise. Further correlation with surrounding sign-ins, audit events, user confirmation, and any available endpoint or risk telemetry would be needed to assess whether the activity was suspicious.

## 2. Objective and scope

The objective was to review an interrupted interactive sign-in, determine which authentication step did not complete, inspect the associated security-control evaluation, and record evidence-based conclusions.

The review used the Microsoft Entra sign-in event details previously captured in the lab. A later attempt to query the 2–3 October 2026 date range in the portal returned **No results**, even with the user filter removed. This report therefore relies on the previously saved screenshots; the empty later query is not treated as proof that the historical event never existed or as proof of a particular retention cause.

## 3. Evidence reviewed

| Evidence | Artifact | What it supports |
|---|---|---|
| Evidence 25 | Interrupted sign-in — basic details | Records an interrupted Azure Portal sign-in for the lab reader identity; the displayed details included an expired password/session-related explanation. Use only the exact wording visible in the saved screenshot when presenting this evidence. |
| Evidence 26 | Authentication details for an interrupted event | Shows the password step as successful and an additional authentication step as unsuccessful; Security Defaults was shown as applied. |
| Evidence 27 | Security Defaults evaluation | Shows the MFA registration grant control result as Failure for the relevant event. |
| Evidence 31–35 | Successful sign-in and supporting details | Provides comparison evidence for a successful sign-in, including MFA satisfied by a claim in the token, a successful MFA evaluation, and device context. |
| Evidence 36 | Interrupted vs. successful comparison graphic | Summarizes the differences between the recorded interrupted and successful sign-in outcomes. |
| Evidence 40 | Conditional access filter results | The saved query returned no results for `Conditional access = Failure`. This does not contradict Evidence 27, which records a Security Defaults evaluation. |
| Evidence 54 | `evidence-54-mfa-authentication-failure-details.png` | For the event at 2 October 2026, 02:01:51 UTC: correct password succeeded; the mobile-app authentication step did not succeed; Security Defaults was shown. |
| Evidence 55 | `evidence-55-conditional-access-mfa-failure.png` | The event detail's security evaluation showed the Security Defaults MFA grant control with result Failure. The portal tab is labelled “Conditional access,” but this should not be described as proof that a custom Conditional Access policy failed. |

**Evidence handling:** Preserve original screenshots without editing. Create redacted copies for public documentation. Ensure each evidence filename and number matches the actual file in the repository before committing.

## 4. Findings

### Finding 1 — Password step succeeded

The authentication details in Evidence 54 show that the correct password step succeeded. This means the event was not recorded as a password-step failure in that authentication sequence.

### Finding 2 — Mobile-app authentication did not succeed

The same evidence shows that the mobile-app authentication step did not succeed. The overall sign-in was interrupted rather than completed successfully.

### Finding 3 — Security Defaults MFA evaluation returned Failure

Evidence 55 records the Security Defaults MFA grant control with a result of Failure. This is the most direct evidence for the failed MFA-related control evaluation in the investigated event.

### Finding 4 — A successful event provides a useful comparator, not proof of device trust

Evidence 31–36 includes a successful sign-in where the MFA requirement was satisfied by a claim in the token. The recorded device context for successful sign-ins was unmanaged and non-compliant. Consequently, the available evidence does not support treating device compliance alone as the explanation for the different outcomes.

### Finding 5 — The available evidence is insufficient to confirm compromise

An interrupted sign-in and unsuccessful MFA step can occur for benign reasons as well as during suspicious activity. The screenshots reviewed do not, by themselves, establish attacker involvement, successful unauthorized access, or compromise of the account.

## 5. Assessment

**Observed outcome:** Interrupted interactive sign-in.  
**Authentication detail:** Password step succeeded; mobile-app authentication step did not succeed.  
**Control evaluation:** Security Defaults MFA grant control returned Failure.  
**Confirmed compromise:** Not established by the reviewed evidence.  
**Custom Conditional Access policy failure:** Not established by these screenshots.

The result should be recorded as an **MFA authentication interruption requiring context**, not automatically labelled a confirmed attack.

## 6. Recommended follow-up

1. If historical logs become available, correlate the event with nearby sign-ins for the same identity, including status, authentication details, application, client, location, device, and risk signals.
2. Review Entra audit logs for relevant changes to the test user, authentication methods, group membership, or security configuration around the event time.
3. Ask the test user/operator whether they initiated the sign-in and whether they received or approved an authentication prompt. Record the response as operator-supplied context, not as a portal finding.
4. If a comparable event recurs, capture the event's basic details, authentication details, and security-control evaluation once each, avoiding duplicate screenshots.
5. Do not claim that logs expired because of licensing unless the tenant's applicable retention configuration and licence are verified. The later query's “No results” message alone does not establish the cause.

## 7. Conclusion

The saved evidence shows an interrupted Microsoft Entra ID sign-in in which the password step succeeded, the mobile-app authentication step did not succeed, and the Security Defaults MFA grant control evaluation returned Failure. A separate successful sign-in provides comparison context, but neither event alone confirms malicious activity. The appropriate outcome is to document the MFA interruption, retain the evidence, and correlate additional telemetry before making a compromise determination.

## 8. Evidence checklist

- [x] Evidence 54 — MFA authentication failure details saved.
- [x] Evidence 55 — Security Defaults MFA grant control failure saved.
- [x] Earlier successful/interrupted sign-in comparison evidence recorded (Evidence 31–36).
- [ ] Verify evidence filenames and numbers against the actual private evidence folder before publication.
- [ ] Redact public copies of screenshots and remove request IDs, IP addresses, and tenant-specific identifiers.
- [ ] Add any additional corroborating logs only if they become available; do not fabricate missing records.
