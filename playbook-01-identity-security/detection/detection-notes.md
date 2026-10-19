
# Identity Security - Detection Engineering Notes

**Project:** Amal Cyber Lab  
**Playbook:** 01 - Identity Security  
**Focus:** Microsoft Entra ID Authentication Monitoring  
**Status:** Detection design in progress

## 1. Detection Objective

Define a repeatable process for identifying and investigating potentially suspicious authentication activity in Microsoft Entra ID.

The initial focus is interrupted sign-ins where password authentication succeeds but a subsequent authentication step, such as MFA, does not succeed.

## 2. Detection Scenario

**Scenario:** A user completes the password authentication step, but the subsequent mobile-app authentication step is unsuccessful.

Potential investigation signals include:

- Interrupted or failed sign-in outcomes
- Unsuccessful authentication steps
- MFA-related security-control evaluation failures
- Repeated authentication failures involving the same identity
- Unexpected sign-in context or related suspicious activity

An individual signal does not automatically establish malicious activity.

## 3. Existing Lab Evidence

| Evidence | Observation |
|---|---|
| Evidence 31–36 | Successful and interrupted sign-in comparison |
| Evidence 54 | Password step succeeded; mobile-app authentication step was unsuccessful |
| Evidence 55 | Security Defaults MFA grant-control evaluation returned Failure |

These observations provide a starting point for detection design and investigation.

## 4. Investigation Workflow

1. Identify the affected identity and sign-in event.
2. Review the sign-in status and application.
3. Inspect the individual authentication steps.
4. Review the applicable security-control evaluation.
5. Correlate related sign-ins and audit events when available.
6. Assess whether the activity is expected or suspicious.
7. Document the evidence, conclusion, and any required response.

## 5. Potential Data Sources

- Microsoft Entra interactive sign-in logs
- Microsoft Entra non-interactive sign-in logs, where relevant
- Microsoft Entra audit logs
- Microsoft Sentinel, if Entra sign-in data is connected to its workspace

## 6. Detection Logic - Initial Hypothesis

Investigate sign-in events where:

- The overall sign-in is interrupted or unsuccessful; and
- Authentication details show that an authentication step did not succeed.

Increase investigative priority when supported by additional signals, such as repeated attempts, unexpected context, unfamiliar activity, or related identity changes.

The authentication-step details must be examined before classifying an event specifically as an MFA failure. Sign-in status alone is insufficient.

## 7. Validation Requirements

- Confirm that the required sign-in data is available.
- Test the logic against known successful and unsuccessful events.
- Check for benign interruptions and false positives.
- Confirm that the result identifies the relevant authentication step.
- Record the query, test results, limitations, and any tuning.
- Do not mark a detection as validated until it has been tested against available data.

## 8. Current Limitations

The lab evidence documents an MFA-related authentication interruption. A later query for the historical event returned no results.

The cause of the missing historical results has not been established. A live detection rule has not yet been validated against ingested log data.

## 9. Next Steps

1. Verify access to the required sign-in telemetry.
2. Develop a query appropriate to the actual log schema and available data.
3. Validate the query against successful and unsuccessful sign-ins.
4. Tune the detection and document its limitations.
5. Consider an alerting rule only after successful validation.

## 10. Current Status

**Detection design:** In progress  
**Existing evidence reviewed:** Yes  
**Query validated:** Not yet  
**Automated alert configured:** Not yet
