# Tool Usage and Editing Guidelines

## Tool Usage Examples

### Listing Code Definitions
- **List definitions from a specific file:**
  ```xml
  <list_code_definition_names>
  <path>src/main.ts</path>
  </list_code_definition_names>
  ```
- **List definitions from all files in a directory:**
  ```xml
  <list_code_definition_names>
  <path>src/</path>
  </list_code_definition_names>
  ```

### Applying Diffs
- **Purpose:** Replace existing code using a search and replace block for precise, surgical edits.
- **Usage:**
  - Specify the file path and provide the exact lines to search and replace, including line numbers.
  - Always make as many changes as possible in a single request using multiple search/replace blocks.
  - Only use a single line of `=======` between search and replacement content.

#### Example Diff Format
```xml
<apply_diff>
<path>File path here</path>
<diff>
<<<<<<< SEARCH
:start_line:1
:end_line:5
-------
def calculate_total(items):
    total = 0
    for item in items:
        total += item
    return total
=======
def calculate_total(items):
    """Calculate total with 10% markup"""
    return sum(item * 1.1 for item in items)
>>>>>>> REPLACE
</diff>
</apply_diff>
```

### Writing to Files
- **Purpose:** Write full content to a file, creating or overwriting as needed.
- **Usage:**
  - Always provide the complete content of the file.
  - Specify the number of lines.

#### Example
```xml
<write_to_file>
<path>frontend-config.json</path>
<content>
{
  "apiEndpoint": "https://api.example.com",
  "theme": {
    "primaryColor": "#007bff",
    "secondaryColor": "#6c757d",
    "fontFamily": "Arial, sans-serif"
  },
  "features": {
    "darkMode": true,
    "notifications": true,
    "analytics": false
  },
  "version": "1.0.0"
}
</content>
<line_count>14</line_count>
</write_to_file>
```

### Search and Replace
- **Purpose:** Perform search and replace operations with optional regex and line range restrictions.
- **Usage:**
  - Specify the path and a JSON array of operations.
  - Use for both string and regex replacements.

#### Example
```xml
<search_and_replace>
<path>example.ts</path>
<operations>[
  {
    "search": "foo",
    "replace": "bar",
    "start_line": 1,
    "end_line": 10
  }
]</operations>
</search_and_replace>
```

### Executing Commands
- **Purpose:** Run CLI commands on the system for project operations.
- **Usage:**
  - Provide a clear explanation of the command.
  - Prefer complex CLI commands over scripts.

#### Example
```xml
<execute_command>
<command>Your command here</command>
</execute_command>
```

## General Editing and Task Completion Criteria
- Always consider the context and compatibility of code changes.
- Use available tools to accomplish tasks efficiently; avoid unnecessary questions.
- Present results using the appropriate tool and await user feedback.
- Only ask questions using the designated follow-up tool, providing actionable suggestions.
- If terminal output is missing, assume success unless verification is critical.
- Use file contents provided by the user directly if available.
- Wait for user confirmation after each tool use before proceeding.

## Proactiveness Policy
- Be proactive in using tools, gathering context, and resolving tasks.
- Ask for secrets only when necessary for external integrations.
- Always follow the user's framework and tool preferences.
- For interactive CLI or web applications, use the appropriate feedback or browser tools to verify functionality.

(Continue with further anonymized operational guidelines as needed...)
