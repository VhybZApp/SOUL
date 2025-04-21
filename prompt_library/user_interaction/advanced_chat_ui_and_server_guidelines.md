# Advanced Chat UI Configuration and Server Interaction Guidelines

## Regeneration and Throttling
- Provide a "Regenerate" button to allow users to reprocess the last message.
- Throttle UI updates for streaming chat responses to improve performance (e.g., throttle to 50ms intervals).

## Event Callbacks
- Use event callbacks for chat lifecycle:
  - `onFinish`: Trigger actions when a message is fully streamed.
  - `onError`: Handle errors gracefully and log as needed.
  - `onResponse`: Process server responses for analytics or custom UI updates.
- You can abort processing by throwing an error in `onResponse` to trigger error handling.

## Request Customization
- Customize chat API requests with custom endpoints, headers, body fields, and credentials as needed.
- Pass additional custom fields per request for advanced server-side processing.

## Server-Side Streaming and Error Handling
- Mask error messages by default for security; optionally provide custom error messages.
- Optionally disable usage data in server responses.
- Support plain text streaming protocols for chat responses.

## Multi-File and Multi-Modal Support
- Allow users to attach and send multiple files (e.g., images, text) in chat.
- Automatically convert supported file types for multi-modal AI interactions.

## Project Structure and Code Block Policies
- Group related code blocks inside a single project structure for clarity.
- Maintain consistent project IDs across related responses.
- Use dedicated components for file moves, deletions, and quick edits.
- Do not use quick edit for renaming files or projects—always provide complete code for substantial changes.

## Accessibility and User Empowerment
- Ensure all interactive controls (regenerate, stop, retry) are accessible.
- Clearly document advanced configuration options for power users.
