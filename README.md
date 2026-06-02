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

Ensure that the accompanying `propose.xml` BPMN process is fully deployed and activated inside your Collibra environment:

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

---

## 📦 Deployment Instructions

Because this form relies on cookie-based session passing, **it must be executed from the same origin/domain as your Collibra instance** to avoid Cross-Origin Resource Sharing (CORS) blocks.

### Option A: Collibra Dashboard Widget (Recommended)

1. Navigate to your target dashboard in Collibra.
2. Add a new **Text/HTML Widget**.
3. Switch the widget editor into **Source Code Mode (`<>`)**.
4. Paste the complete contents of the HTML file directly into the source view and save.

### Option B: Custom Cloud Hosting / Reverse Proxy

If hosting outside of a direct dashboard widget, deploy the HTML file to an internal web server sitting behind the exact same enterprise reverse proxy or domain alias as your Collibra platform (e.g., `collibra.yourcompany.com/intake`).

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the issues page if you want to expand the form fields to capture descriptions (`definition`), notes (`note`), or relation targets.