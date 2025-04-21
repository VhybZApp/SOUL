# Code Project, AI SDK, and UI Component Guidelines

## Code Project Structure
- Group related files and React/Next.js components inside a single project structure for clarity and maintainability.
- Use the provided syntax for defining React components and project files, following kebab-case naming conventions (e.g., `login-form.tsx`).
- Do not output or write a `package.json` file; dependencies are inferred from imports.
- Never output `next.config.js`; only use `next.config.mjs` if it already exists.
- Use Tailwind CSS, shadcn/ui, and Lucide React icons for styling and icons.
- Always generate responsive designs and use wrapper elements for custom backgrounds.
- Set the `dark` class manually for dark mode; it will not be applied automatically.
- For placeholder images, use `/placeholder.svg?height={height}&width={width}`.
- Only use icons from Lucide React, never output `<svg>` directly.
- For images, set `crossOrigin` to `anonymous` when rendering on `<canvas>`.

## AI Model and SDK Usage
- Use the AI SDK via `ai` and `@ai-sdk` (JavaScript only, not Python).
- Access models like GPT-4o via the AI SDK (e.g., `openai('gpt-4o')`).
- Use core functions like `streamText` for streaming and `generateText` for single completions.
- Do not use unsupported libraries (e.g., `langchain`, `openai-edge`).
- Do not use `runtime = 'edge'` in API routes.
- Environment variables must be prefixed with `NEXT_PUBLIC` to be used on the client.

## UI Capabilities and Constraints
- Users can attach images and text files in the prompt form.
- Users can execute JavaScript code and preview React, Next.js, HTML, and Markdown.
- Use QuickEdit only for small, simple changes (1-20 lines, 1-3 steps). For larger changes, rewrite the complete code.
- Never use QuickEdit for renaming files or projects; always provide full edits.
- Use the `<MoveFile />` component for moving/renaming files and update all imports accordingly.
- Ensure all interactive controls and advanced features are accessible and clearly documented.
