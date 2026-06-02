# Collibra Intake Portal: Propose New Business Term

A lightweight, self-contained HTML, CSS, and JavaScript intake form designed to be embedded directly within your Collibra Data Intelligence Cloud environment (e.g., via a dashboard widget or text component).

This tool provides a streamlined, user-friendly interface for any business user to propose new data assets without having to navigate deep into the native Collibra operating model. It utilizes session-scoped cookie authentication and dynamically retrieves the active user's **CSRF token** to securely instantiate workflows via the Collibra REST API v2.

---

## 🚀 Features

* **Zero Dependencies:** Written in pure, vanilla HTML5, CSS3, and JavaScript. No external frameworks or heavy libraries required.
* **Native Security Alignment:** Leverages Collibra's active browser session (`credentials: "include"`) and programmatically fetches the required `X-CSRF-TOKEN` before submitting requests.
* **Dynamic Workflow Discovery:** Searches for the targeted workflow configuration by its string name rather than relying on hardcoded, environment-specific deployment IDs.
* **Clean UI/UX:** Styled using modern design principles that visually complement the standard Collibra user experience, complete with validation states and submission loading indicators.

---

## 🛠️ How It Works

1. **Authentication:** The script calls the `/rest/2.0/auth/sessions/current` endpoint to retrieve the active session's security context and caches the CSRF token.
2. **Workflow Lookup:** It queries `/rest/2.0/workflowDefinitions` using the explicit workflow name to find the current active process definition ID.
3. **Payload Construction:** It collects the text input from the form, maps it to the workflow's expected `signifier` variable, and merges it with the target asset type and domain configuration requirements.
4. **Instantiation:** It submits a `POST` request to `/rest/2.0/workflowInstances` under a `GLOBAL` context to trigger the asset creation pipeline.

---

## 📋 Prerequisites & Setup

### 1. Collibra Workflow Deployment

Ensure that the accompanying `propose.bpmn` BPMN process is fully deployed and activated inside your Collibra environment:

* **Process Name:** Must be exactly `Propose New Business Term` (or updated inside the JavaScript configuration constants).
* **Target Domain:** By default, the workflow targets a specific intake vocabulary (`00000000-0000-0000-0000-000000006013`). Ensure this domain exists or update the UUID to point to your designated target community/domain.

### 2. Form Configuration

Open the HTML file and review the configuration block inside the `<script>` tag to align with your instance's metadata model configuration:

```javascript
// Form properties and default UUID values extracted from your operating model
const FORM_PROPERTIES = {
  intakeVocabulary: "00000000-0000-0000-0000-000000006013", // Target Domain UUID
  conceptType: "00000000-0000-0000-0000-000000011001",      // Asset Type UUID (Business Term)
  usesRelationTypeUuid: "00000000-0000-0000-0000-000000007004",
  definitionAttributeTypeUuid: "00000000-0000-0000-0000-000000000202",
  noteAttributeTypeUuid: "00000000-0000-0000-0000-000000003116"
};

```
