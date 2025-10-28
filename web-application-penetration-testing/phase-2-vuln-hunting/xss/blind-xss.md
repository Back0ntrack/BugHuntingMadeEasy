# Blind XSS

* **What it is:** Blind XSS is a stored XSS where the attacker’s payload is saved by the app but **not** immediately reflected back to the attacker. Instead it executes later in another context (admin panel, email viewer, log viewer, etc.), so the attacker sees no immediate browser pop-up — hence “blind.”
* **Typical flow:** Attacker injects payload → payload is stored (DB, logs, ticket system, file metadata) → a different user or backend process opens the stored data → payload executes in that victim’s browser/context → attacker receives out-of-band evidence (callback/DNS/HTTP).
* **Common sinks:** admin dashboards, support/CRM systems, email previews, log viewers, scheduled reports, file previewers, notification systems.
* **Why it’s dangerous:** Execution happens in a higher-privilege context (admins), can steal session tokens, perform actions, pivot, or exfiltrate data via OOB channels — often unnoticed for longer.
* **How defenders detect it:** instrument out-of-band (OOB) testing using safe collaborator services (DNS/HTTP callbacks), monitor server logs for unexpected script activity, add alerting on suspicious HTML in stored fields, and review rendered outputs in admin UIs.

{% hint style="info" %}
_Even though it seems that the payload is percent encoded in the source code it might execute XSS somewhere else like in the admin panel etc._&#x20;
{% endhint %}

## Target - 1&#x20;

