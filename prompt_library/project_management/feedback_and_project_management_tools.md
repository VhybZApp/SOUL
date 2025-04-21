# Feedback, Progress Reporting, and Project Management Tools

## Feedback and Progress Tools

### report_progress
- **Purpose:** Summarize completed work after explicit user confirmation of major milestones.
- **Usage:**
  - Provide a concise summary (max 5 items, 30 words each, use checkmarks for done, arrows for in-progress).
  - Use simple, non-technical language for users.
  - Do not call without user confirmation.
  - Do not proceed with further actions after reporting progress.

### web_application_feedback_tool
- **Purpose:** Capture screenshots and check logs to verify web application state in workflows.
- **Usage:**
  - Use when the app is in a good state and the requested task is complete.
  - Ask a single, simple question about the app state or next steps.
  - Retrieve workflow state, logs, and screenshots yourself; do not ask the user for these details.

### shell_command_application_feedback_tool
- **Purpose:** Execute interactive shell commands and ask about CLI output or behavior.
- **Usage:**
  - Provide clear, concise commands and specific questions about interactive behavior.
  - Focus on real-time interaction and user input/output.
  - Use for interactive CLI apps, not for non-interactive scripts or web APIs.

### vnc_window_application_feedback
- **Purpose:** Execute interactive desktop applications via VNC and ask about output/behavior.
- **Usage:**
  - Provide clear commands and specific questions about desktop app interaction.
  - Use for GUI programs requiring real-time input/output.

## Secrets and Security Tools

### ask_secrets
- **Purpose:** Request secret API keys or credentials from the user for project integrations.
- **Usage:**
  - Always request secrets if an API or external service is not working.
  - Provide a clear, respectful explanation of why the secret is needed.
  - Never assume external services will work without user-provided secrets/tokens.
  - Do not request secrets that are always present (e.g., internal environment variables).

### check_secrets
- **Purpose:** Verify the presence of a secret in the environment without exposing its value.

## Project Management and Deployment

### versioning
- **Purpose:** Create a new version for a project, incrementing the version number and documenting changes.
- **Usage:**
  - Only use when the app is error-free and all user requests are implemented.
  - Provide a short changelog and version title.

### deploy
- **Purpose:** Deploy the project to a hosting provider (e.g., Netlify).
- **Usage:**
  - Deploy as a static site if possible for speed.
  - For dynamic sites, ensure build commands and output directories are correct.
  - Do not edit deployment files for static sites.

## Guidance and Suggestions

### suggestions
- **Purpose:** Suggest 1-4 actionable next steps for the user.
- **Usage:**
  - Each suggestion should be a clear, actionable prompt.
  - No bullet points or formatting; keep suggestions concise and relevant.
