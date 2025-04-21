# Domain Knowledge, React Hooks, and Custom Hook Guidelines

## Domain Knowledge
- The assistant uses Retrieval Augmented Generation (RAG) to access up-to-date domain knowledge for accurate responses.
- Defaults to the latest technology (e.g., Next.js App Router, Next.js 15, Server Components) unless otherwise specified.
- Follows App Router conventions: file-based routing with folders, `layout.js`, `page.js`, and `loading.js` files.
- Prioritizes best practices and new features in modern frameworks.

## Sources and Citations
- Cite domain knowledge using the `[ ^index ]` format.
- Provide source links when referencing documentation or official resources.

## React Hooks: Effect and Performance
- Use `useEffect` to synchronize with external systems (network, DOM, third-party widgets, etc.).
- Prefer custom hooks to encapsulate common effect logic (e.g., `useChatRoom`, `useWindowListener`).
- Use `useMemo` and `useCallback` to optimize performance by caching calculations and function definitions.
- Use `useTransition` and `useDeferredValue` to prioritize and defer UI updates for better responsiveness.

### Example: Custom Hook
```js
function useChatRoom({ serverUrl, roomId }) {
  useEffect(() => {
    const options = { serverUrl, roomId };
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, serverUrl]);
}
```

## Wrapping Effects in Custom Hooks
- Extract repeated effect logic into custom hooks for reusability and declarative APIs.
- Example: `useChatRoom` hides effect logic behind a simple interface.

## Controlling Non-React Widgets
- Use effects to synchronize external widgets (e.g., maps, video players) with React state.
- Example: Use `useRef` and `useEffect` to manage widget instances and keep them in sync with props.

## Image Component Guidelines (Next.js)
- Use the latest `next/image` API for performance and developer experience.
- Reference legacy components only when necessary for backward compatibility.
- Demo intrinsic and fixed layouts to illustrate responsive image behavior.

## Best Practices
- Prove to linters that non-reactive values/functions do not need to be dependencies in effects.
- Always declare all dependencies in effect hooks to avoid bugs.
- Separate blocking and non-blocking UI updates for optimal performance.
