**Defender XDR: Alert Correlation & Incident Generation.**

Lab objective

This lab demonstrates how Microsoft Defender XDR takes multiple related alerts and correlates them into a single incident, giving a SOC analyst a unified investigation rather than requiring each alert to be investigated independently.

Conceptually:

Suspicious activity → Multiple alerts → Correlation → One incident → SOC investigation

Introduction

This lab demonstrates alert correlation and incident generation in Microsoft Defender XDR from a Security Operations Center (SOC) analyst perspective. The objective is to understand how multiple security alerts associated with a common entity can be investigated together within a single incident, rather than treating every alert as an isolated event.

The lab begins by examining an existing single-alert incident involving the SOC Test User and establishing a baseline using the incident timeline, activities, assets, and alert details. Advanced Hunting is then used with the AlertInfo and AlertEvidence tables to examine the alert and its associated evidence.

A second related training alert is introduced for the same SOC Test User. The lab then demonstrates how the alerts can be associated within the same incident and how an analyst can use the Alerts, Activities, Assets, and Summary views to examine their relationship and shared entity context.

Finally, the incident is reviewed and resolved as expected administrative activity, documenting the analyst's determination that the training activity was non-malicious.

Lab Objectives

By completing this lab, the analyst will gain hands-on experience with:

- Investigating single-alert and multi-alert incidents

- Understanding alert-to-incident relationships

- Identifying shared entities across related alerts

- Querying AlertInfo and AlertEvidence with KQL

- Reviewing incident activities, assets, and alert context

- Understanding how alert correlation supports SOC investigations

- Documenting an analyst's classification and resolution decision

Platform: Microsoft Defender XDR\
Primary Tools: Incidents, Alerts, Advanced Hunting, KQL\
Key Tables: AlertInfo, AlertEvidence\
Lab Scenario: Two related administrative-activity alerts involving the same SOC Test User are investigated together as a multi-alert incident.

Step 1 — Open the Microsoft Defender XDR Incident Queue

I navigate to Microsoft Defender → Incidents.

The incident queue was reviewed to identify existing security incidents and determine whether any incidents already contained multiple alerts.

It captures the Incidents page showing the Multi-alert incidents metric and the incident queue.(Image 1)

Step 2 — Establish the Initial Single-Alert Baseline

I open Incident ID 2 and I review its Attack story.

Initially, the incident contained:

Alerts: 1\
Assets: 1\
Affected user: SOC Test User\
Severity: Low

This established the baseline before introducing another related alert. (Image 2)

Step 3 — Examine the Original Alert

I select Alerts and I open:

SOC Lab – Expected Administrative Activity

I review the alert details, status, severity, associated entity, and available investigation information.

The alert represented expected administrative activity involving SOC Test User rather than malicious behavior. (Image 3)

Step 4 — Review the Incident's Existing Activities

I open the Activities tab.

The activity history was examined to distinguish actual security alerts from administrative changes such as assignment, classification, determination, and status changes.

This demonstrated an important SOC distinction:

Multiple incident activities do not necessarily mean multiple security alerts. (Image 4)

Step 5 — Verify the Affected Asset

I open: Assets → Users

The incident showed one affected identity: SOC Test Use. This identity became the common entity used for the correlation exercise. (Image 5)\

Step 6 — Validate Existing Alert Telemetry with Advanced Hunting

I open Advanced hunting and query the AlertInfo table:

AlertInfo

| where Timestamp \> ago(7d)

| project Timestamp, AlertId, Title, Severity, Category, ServiceSource, DetectionSource

| order by Timestamp desc

The query confirmed the alert information currently available to Defender XDR. (Image 6)

Step 7 — Examine Alert Evidence

Query the AlertEvidence table:

AlertEvidence

\| where Timestamp \> ago(7d)

\| project Timestamp, AlertId, Title, EntityType, EvidenceRole, AccountName, DeviceName, RemoteIP

\| order by Timestamp desc

This demonstrated how analysts can use Advanced Hunting to examine entities and evidence associated with Defender alerts. (Image 7)\

Step 8 — Introduce the Second Related Training Alert

A second benign training alert was introduced:

SOC Lab – Related Administrative Activity

The alert was associated with the same SOC Test User used by the original training alert.

No malicious activity was performed; the alert existed solely for SOC investigation and incident-correlation training.

It captures the second alert showing its name and SOC Test User. (Images 8 and 9)\

Step 9 — Associate the Related Alert with the Existing Incident

The second training alert was linked to Incident ID 2, producing a multi-alert incident.

The incident now contained:

SOC Lab – Expected Administrative Activity

and

SOC Lab – Related Administrative Activity

This demonstrated how related alerts can be investigated together as part of one security incident.

In this lab, the second alert was manually linked to the existing incident. Therefore, the lab demonstrates alert-to-incident association/correlation, rather than claiming that Defender XDR automatically performed the correlation. (Image 10)

Step 10 — Verify the Correlation in Incident Activities

I open the Activities tab.

The incident history recorded the alert association, including the alert-link/correlation activity.

This provided an audit trail showing how the incident changed when the additional alert was associated with it. (Image 11)

Step 11 — Verify the Shared Entity

I return to: Assets → Users

The incident continued to identify: SOC Test User as the impacted user.

This demonstrated the relationship: Alert 1 → SOC Test User ← Alert 2 (Image 12)

Step 12 — Review and Resolve the Multi-Alert Incident

I review the Summary page. The final incident contained:

2 alerts\
1 impacted user\
Severity: Low\
SOC Test User as the shared entity

After confirming that both alerts represented expected training activity, the incident was resolved and documented as expected activity.

The resolution note recorded that the incident was created for Defender XDR Lab 5 and that no malicious activity was identified.

Resolved\
Alerts (2)\
Both alert names\
SOC Test User (Images 13, 14, 15, 16, and 17)\
\
\
\
\
\
\
\
\
\
