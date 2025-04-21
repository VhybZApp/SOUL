# Next.js Image Component: Required and Optional Props

## Comparison: `next/image` vs `next/legacy/image`
- The new `next/image` component removes the `<span>` wrapper in favor of native aspect ratio.
- Supports canonical `style` prop; removes `layout`, `objectFit`, and `objectPosition` props in favor of `style` or `className`.
- Uses native lazy loading (removes `IntersectionObserver` and related props).
- Moves `loader` config to a prop.
- Makes `alt` prop required.
- Changes `onLoadingComplete` to receive a reference to the `<img>` element.

## Required Props
- **src**: Statically imported file, absolute external URL, or internal path. For external URLs, configure `remotePatterns`.
- **width**: Rendered or original width in pixels, depending on `layout` and `sizes`.
- **height**: Rendered or original height in pixels, depending on `layout` and `sizes`.

## Optional Props
- **layout**: Controls image behavior as viewport changes.
  - `intrinsic` (default): Scales down to fit container, up to image size.
  - `fixed`: Sized exactly to `width` and `height`.
  - `responsive`: Scales to fit container width.
  - `fill`: Grows to fill parent container (requires parent to be `position: relative`).
- **loader**: Custom function to resolve URLs (overrides default loader).
  - Example:
    ```js
    const myLoader = ({ src, width, quality }) => `https://example.com/${src}?w=${width}&q=${quality || 75}`;
    <Image loader={myLoader} src="me.png" alt="Author" width={500} height={500} />
    ```

## Layout Demos
- **intrinsic**: Maintains original dimensions for large viewports, scales down for small.
- **fixed**: Maintains fixed dimensions regardless of viewport.
- **responsive**: Scales up/down with container width.
- **fill**: Stretches to fill parent container.

## Best Practices
- Always provide `alt` text for accessibility.
- Use `layout` and `style`/`className` for responsive behavior.
- For SVGs, enable `unoptimized` or `dangerouslyAllowSVG` if needed.
- For external images, configure `remotePatterns` in `next.config.js`.
