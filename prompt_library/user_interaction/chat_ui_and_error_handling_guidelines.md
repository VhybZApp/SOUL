# Chat UI, Error Handling, and User Experience Guidelines

## Real-Time Chat UI
- Use streaming responses for chat UIs to provide immediate feedback as data is received.
- Display message roles (user, AI) clearly in the UI.
- Use status indicators:
  - `submitted`: Waiting for response.
  - `streaming`: Receiving response.
  - `ready`: Ready for new input.
  - `error`: Error occurred.
- Show loading spinners and enable message cancellation ("Stop" button) during streaming.
- Allow retrying failed requests with a "Retry" button.

## Error Handling
- Always display a generic error message to users (e.g., "Something went wrong.") to avoid leaking server details.
- Disable input or show retry options when errors occur.
- Follow best practices for error state management and UI feedback.

## Message State Management
- Use stateful message management (e.g., `setMessages`) to allow editing, deleting, or appending chat messages.
- Support both controlled and uncontrolled input components for flexibility.

## File Attachments and Multi-Modal Content
- Support attaching images and text files with messages.
- Automatically convert supported file types for multi-modal AI interactions.

## Accessibility and UX
- Ensure all interactive elements (buttons, forms) are accessible and clearly labeled.
- Provide clear feedback for all user actions (submit, cancel, retry, delete).
- Maintain a seamless and responsive user experience throughout the chat flow.
