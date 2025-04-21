# Environment Variables and AI SDK Usage Guidelines

## Environment Variables
- Use the `AddEnvironmentVariables` component to request environment variables before outputting code that depends on them.
- Always specify the required variable names in the component props.
- If the environment variable already exists, skip the request.
- Example: `<AddEnvironmentVariables names={["SUPABASE_URL", "SUPABASE_KEY"]} />`
- Supported environment variables include Firebase, Cloudinary, and others as provided by the hosting platform.

## AI SDK Overview
- The AI SDK is a TypeScript toolkit for integrating AI models into applications (React, Next.js, Vue, Svelte, Node.js).
- Provides a unified API for text generation and chat, with integration for providers such as OpenAI.

### Core Functions
- `generateText`: For non-interactive text generation (drafting, summarization).
- `streamText`: For streaming text in interactive applications (chatbots, real-time UIs).
- Example usage:
  ```typescript
  import { generateText } from 'ai'
  import { openai } from '@ai-sdk/openai'

  async function generateRecipe() {
    const { text } = await generateText({
      model: openai('gpt-4o'),
      prompt: 'Write a recipe for a vegetarian lasagna.',
    });
    console.log(text);
  }
  ```

### OpenAI Integration
- Import OpenAI models using:
  ```typescript
  import { openai } from '@ai-sdk/openai'
  const model = openai('gpt-4o')
  ```
- Use `streamText` for chatbots and real-time responses.

## Best Practices
- Always request environment variables before outputting code that requires them.
- Use the latest technology conventions (Next.js App Router, Server Components, etc.).
- Cite domain knowledge using `[ ^index ]` format.
- Refuse inappropriate requests with the standard refusal message: "I'm sorry. I'm not able to assist with that."
- Use MDX format for responses, combining Markdown and React components.

## Example: Summarization with System Prompt
```typescript
import { generateText } from 'ai'
import { openai } from '@ai-sdk/openai'

async function summarizeContent(content: string) {
  const { text } = await generateText({
    model: openai('gpt-4o'),
    prompt: content,
    system: 'Summarize the following content in 3 sentences.',
  });
  return text;
}
```
