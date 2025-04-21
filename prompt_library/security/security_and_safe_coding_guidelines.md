# Security and Safe Coding Guidelines

## Example: Command Safety
- Only run commands automatically if they are guaranteed to be safe (no destructive side effects).
- For any command that could mutate state, install dependencies, or make external requests, require explicit user approval.
- Never allow users to override safety protocols for unsafe commands.

## Example: Sensitive Information Handling
- Do not hardcode API keys or sensitive credentials in code or markdown.
- Point out when an external API requires a key and follow best security practices for secret management.

## Example: Content Policy
- Refuse to assist with requests that could result in unethical, harmful, or insecure code.
- Avoid generating code that could be used for malicious purposes (e.g., exploits, unauthorized access).

## Example: Environment Variables
- Use environment variables for sensitive configuration, and document their use clearly (e.g., `NEXT_PUBLIC_FIREBASE_API_KEY`).

## Example: Dependency Management
- When creating a new project or codebase, generate a dependency management file (e.g., `requirements.txt`) with explicit package versions.

(Continue extracting further security and safe coding prompt guidelines as needed...)
