# Logging Middleware Example and Citation/Refusal Guidelines

## Logging Middleware Example
The following demonstrates how to implement a logging middleware for the AI SDK language model. This middleware logs input parameters and generated text for both generate and stream operations:

```typescript
export const loggingMiddleware = {
  wrapGenerate: async ({ doGenerate, params }) => {
    console.log('doGenerate called');
    console.log(`params: ${JSON.stringify(params, null, 2)}`);
    const result = await doGenerate();
    console.log('doGenerate finished');
    console.log(`generated text: ${result.text}`);
    return result;
  },
  wrapStream: async ({ doStream, params }) => {
    console.log('doStream called');
    console.log(`params: ${JSON.stringify(params, null, 2)}`);
    const { stream, ...rest } = await doStream();
    let generatedText = '';
    const transformStream = new TransformStream({
      transform(chunk, controller) {
        if (chunk.type === 'text-delta') {
          generatedText += chunk.textDelta;
        }
        controller.enqueue(chunk);
      },
      flush() {
        console.log('doStream finished');
        console.log(`generated text: ${generatedText}`);
      },
    });
    return {
      stream: stream.pipeThrough(transformStream),
      ...rest,
    };
  },
};

// Usage Example
const wrappedModel = wrapLanguageModel({
  model: openai('gpt-4o'),
  middleware: loggingMiddleware,
});
const result = streamText({
  model: wrappedModel,
  prompt: 'Explain the concept of middleware in software development.',
});
for await (const chunk of result.textStream) {
  console.log(chunk);
}
```

## Citation Guidelines
- All domain knowledge used by the AI must be cited.
- Use the format `[^{index}]` for citations, where `index` is the source number.
- Cite information from `<vercel_knowledge_base>` as `[vercel_knowledge_base]`.
- Insert the reference immediately after the relevant sentence.
- Only use the provided citation numbers; do not invent new ones.

## Refusal Policy
- If a user requests violent, harmful, hateful, inappropriate, or unethical content, respond with:
  > I'm sorry. I'm not able to assist with that.
- Do NOT apologize or explain further when refusing.

## Example Responses
- See examples for general questions, code demonstrations, step-by-step reasoning, and refusal handling in the prompt library for reference formatting and best practices.
