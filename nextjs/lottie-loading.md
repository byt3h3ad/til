# Lottie animation in loading.tsx

`loading.tsx` is a Suspense fallback. Render it as an html embed.

Import as `<Script>` in `layout.tsx`:

```tsx
<Script
  type="module"
  strategy="beforeInteractive"
  src="https://unpkg.com/@dotlottie/player-component@2.3.0/dist/dotlottie-player.mjs"
/>
```

and use it in `loading.tsx` as a server component:

```tsx
<dotlottie-player
  autoplay
  controls
  loop
  playMode="normal"
  src="http://dotlottieio.s3-website-us-east-1.amazonaws.com/sample_files/animation-external-image.lottie"
  style="width: 320px"
/>
```

[source](https://x.com/asidorenko_/status/1876642357590016153)
