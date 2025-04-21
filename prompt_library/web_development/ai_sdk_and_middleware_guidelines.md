# AI SDK Usage and Language Model Middleware Guidelines

## AI SDK Overview
- The AI SDK is a TypeScript toolkit for building AI-powered applications with frameworks like React, Next.js, Vue, Svelte, and Node.js.
- Provides a unified API for working with different AI models and simplifies integration of AI features.

### Core Components
- **AI SDK Core**: Standardized text generation and tool call API for LLMs.
- **AI SDK UI**: Framework-agnostic hooks for chat and generative UIs.

## Core Functions
- `generateText`: For non-interactive text generation (e.g., drafting emails, summarizing).
  ```typescript
  const { text } = await generateText({ model: openai('gpt-4o'), prompt: 'Write a recipe for vegetarian lasagna.' });
  ```
- `streamText`: For streaming text in interactive applications (e.g., chatbots).
  ```typescript
  const result = streamText({ model: openai('gpt-4o'), prompt: 'How can I improve my productivity?', onChunk: ({ chunk }) => { /* handle stream */ } });
  ```
- `@ai-sdk/openai`: Integration with OpenAI models using the AI SDK interface.

## Example Use Cases
- Drafting content, summarizing articles, or generating recipes with `generateText`.
- Building interactive chatbots or real-time apps with `streamText`.
- Summarizing content with system prompts for LLMs.

## Language Model Middleware (Experimental)
- Middleware can enhance language model behavior (e.g., guardrails, RAG, caching, logging).
- Use `experimental_wrapLanguageModel` to wrap models with middleware.
- Example: Logging middleware logs parameters and generated text for each call.
  ```typescript
  const wrappedLanguageModel = wrapLanguageModel({ model: openai('gpt-4o'), middleware: loggingMiddleware });
  const result = streamText({ model: wrappedLanguageModel, prompt: 'What cities are in the United States?' });
  ```

## Best Practices
- Use the AI SDK for unified, maintainable AI integration across frameworks.
- Leverage middleware for advanced features and custom model behavior.
- Always provide clear prompts and system instructions for reliable outputs.
