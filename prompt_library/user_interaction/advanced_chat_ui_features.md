# Advanced Features: Empty Submissions, Reasoning Tokens, Sources, and Attachments

## Empty Submissions
- Allow users to submit empty messages by enabling the `allowEmptySubmit` option in chat UI hooks.
- Useful for triggering AI responses or actions without explicit user input.

## Reasoning Tokens
- Some AI models provide reasoning tokens, sent before the main message content.
- Forward reasoning tokens to the client using a dedicated option (e.g., `sendReasoning`).
- Render reasoning parts distinctly in the UI for transparency and explainability.

## Source Citations
- Certain providers (e.g., search-based models) can include source citations in responses.
- Forward sources to the client (e.g., `sendSources` option) and render as links or references in the UI.
- Clearly distinguish between message content and cited sources for user trust.

## Attachments and Multi-Modal Content
- Support sending attachments (images, text files) with messages using file input elements or URL lists.
- Automatically convert supported file types (e.g., `image/*`, `text/*`) for multi-modal AI interactions.
- Render attachments (e.g., images) inline with chat messages for a seamless user experience.
- Handle unsupported file types gracefully, providing user feedback as needed.

## Implementation Notes
- Use event callbacks and configuration options to enable these advanced features in your chat UI and backend.
- Ensure accessibility and clarity for all advanced UI elements and features.
