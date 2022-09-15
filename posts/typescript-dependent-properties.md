---
title: Typescript interface dependent properties
date: 2022-10-01
layout: layouts/post.njk
---

I know there is no need in rewriting Typescript docs, but today I learned (and I spent about 1 hour to find proper information) how to make dependent properties in Typescript.

My goal was to supply React component with properties which will depend from one property. As an example I've created React text component with two types: `h1` and `p`. When you choose `h1` you will be additionally prompted to supply `fontWeight` and `fontSize` props. When you pick `p` type, you will be prompted to supply `fontStyle` prop.

## Typescript interfaces

```typescript
// Additional params for h1 prop
interface HeaderParams {
  fontWeight: "bold" | "normal";
  fontSize: "11px" | "14px" | "30px";
}

// Additional params for p prop
interface ParagraphParams {
  fontStyle: "italic" | "normal";
}

// Resulting type for h1 prop
interface HeaderType extends HeaderParams {
  type: "h1";
}

// Resulting type for p prop
interface ParagraphType extends ParagraphParams {
  type: "p";
}

// Text types variants, moved in separate type for readability
type TextTypes = HeaderType | ParagraphType;

// Type for React component
export type TextProps = TextTypes & {
  children: React.ReactNode;
};
```

Turned out that using [Typescript Unions](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) is the cleanest way. IMHO.

## React component

{% raw %}
```tsx
import * as React from "react";
import { TextProps } from "./Text.types";

const Text: React.FC<TextProps> = ({ children, type, ...props }) => {
  switch (type) {
    case "h1":
      return <h1 style={{ ...props }}>{children}</h1>;
    case "p":
      return <p style={{ ...props }}>{children}</p>;
    default:
      return <div>{children}</div>;
  }
};

export default Text;
```
{% endraw %}

I've built more complex [example](https://codesandbox.io/s/new-surf-v63w9c?file=/src/App.tsx) to play with.
