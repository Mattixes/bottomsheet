# Get started

## Installation

### NPM

```bash
npm install @mattixes/bottomsheet

or

npm install --save @mattixes/bottomsheet
```

### YARN

```bash
yarn add @mattixes/bottomsheet
```

### PNPM

pnpm install @mattixes/bottomsheet

## Basic usage

```jsx
import { useState } from 'react'
import { BottomSheet } from '@mattixes/bottomsheet'

// if setting up the CSS is tricky, you can add this to your page somewhere:
// <link rel="stylesheet" href="https://unpkg.com/@mattixes/bottomsheet@0.1.0/dist/style.css" crossorigin="anonymous">
import '@mattixes/bottomsheet/dist/style.css'

export default function Example() {
  const [open, setOpen] = useState(false)
  return (
    <>
      <button onClick={() => setOpen(true)}>Open</button>
      <BottomSheet open={open}>My awesome content here</BottomSheet>
    </>
  )
}
```

## TypeScript

TS support is baked in, and if you're using the `snapTo` API use `BottomSheetRef`:

```tsx
import { useRef } from 'react';
import { BottomSheet, BottomSheetRef } from '@mattixes/bottomsheet';

export default function Example() {
  const sheetRef = useRef<BottomSheetRef | any>();
  return (
    <BottomSheet open ref={sheetRef}
      // the first snap points height depends on the content, while the second one is equivalent to 60vh
      snapPoints={({ minHeight, maxHeight }) => [minHeight, maxHeight / 0.6]}
      // Opens the largest snap point by default, unless the user selected one previously
      defaultSnap={({ lastSnap, snapPoints }) =>
        lastSnap ?? Math.max(...snapPoints)
      }
    >
      <button
        onClick={() => {
          // Full typing for the arguments available in snapTo, yay!!
          sheetRef.current?.snapTo(({ maxHeight }: { maxHeight: number; }) => maxHeight);
        }}
      >
        Expand to full height<br />
      </button>
    </BottomSheet>
  );
}
```

## Customizing the CSS

### Using CSS Custom Properties

These are all the variables available to customize the look and feel when using the [provided](/src/style.css) CSS.

```css
:root {
  --rsbs-backdrop-bg: rgba(0, 0, 0, 0.6);
  --rsbs-bg: #fff;
  --rsbs-handle-bg: hsla(0, 0%, 0%, 0.14);
  --rsbs-max-w: auto;
  --rsbs-ml: env(safe-area-inset-left);
  --rsbs-mr: env(safe-area-inset-right);
  --rsbs-overlay-rounded: 16px;
}
```

### Custom CSS

It's recommended that you copy from [style.css](/src/style.css) into your own project, and add this to your `postcss.config.js` setup (`npm i postcss-custom-properties-fallback`):

```js
module.exports = {
  plugins: {
    // Ensures the default variables are available
    'postcss-custom-properties-fallback': {
      importFrom: require.resolve('@mattixes/bottomsheet/defaults.json'),
    },
  },
}
```
