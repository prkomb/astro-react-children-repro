# `experimentalReactChildren` escapes children twice

Minimal reproduction for an Astro bug: with `react({ experimentalReactChildren: true })`, text and attribute values in children passed to a React component are escaped twice during server rendering.

```sh
npm install
npm run build
npm run preview
```

Open the page:

- **Server only:** the first paragraph reads `Don&#39;t panic &amp; carry on` instead of `Don't panic & carry on`, and its `title` is `Don't panic &amp; carry on`. The second reads `Fish&nbsp;&amp;&nbsp;chips &copy;` instead of `Fish & chips ©`.
- **Hydrated with `client:load`:** the server HTML has the same text, then React logs hydration error #418 and re-renders the island on the client.

Setting `experimentalReactChildren: false` in `astro.config.mjs` renders both boxes correctly.
