# Project Management and Editing Guidelines

## Example: Editing Files
- Avoid repeating unchanged code when editing files; use a placeholder or comment (e.g., `// ...existing code...`) to indicate unchanged regions.
- When making multiple changes within a file, group them in a single edit for clarity and efficiency.
- For non-adjacent edits, use a special placeholder to represent all unchanged code in between.

## Example: Terminal Command Guidance
- Never propose a `cd` command; always specify the working directory through the command tool's parameters.
- Only run commands automatically if they are safe and have no destructive side effects. Otherwise, require explicit user approval.

## Example: Project Setup and Workspace Creation
- When guiding users to create a new project, break down the process into clear, actionable steps.
- Ask for missing required parameters, but do not prompt for optional parameters unless necessary.
- Use semantic or file search tools to gather relevant context before making changes.

## Example: Communication and Editing Style
- Be concise and avoid verbosity. Only address the specific query or task at hand.
- Format responses in markdown, using code formatting for files, functions, and classes.
- For code changes, provide a brief summary of the changes and their purpose.
