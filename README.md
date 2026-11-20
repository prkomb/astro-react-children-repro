# `experimentalReactChildren` loses text in children

Minimal reproduction for an Astro bug: with `react({ experimentalReactChildren: true })`, the server drops whitespace inside elements in children (a space between two inline elements, a line break in `<pre>`), and the client drops text that sits next to an element at the top level of the children. Islands then hit a hydration mismatch.

```sh
npm install
npm run build
npm run preview
```

Open the page. Every box should read `Hello world` (the `<pre>` box on two lines).

- **Server only:** the first box reads `Helloworld`. The built HTML is `<p><strong>Hello</strong><em>world</em></p>`: the space between `</strong>` and `<em>` is gone. The `<pre>` box reads `Helloworld` on one line: the line break between the two `<b>` elements is gone too.
- **Hydrated, space between elements (`#space`):** the server HTML reads `Helloworld` as above. On hydration React throws error #418 and re-renders the island on the client, which keeps the space.
- **Hydrated, text next to an element (`#text`):** the server HTML reads `Hello world`. On hydration React throws error #418 and re-renders the island, which then reads only `world`.

Setting `experimentalReactChildren: false` in `astro.config.mjs` renders every box as `Hello world`, and the two islands hydrate without an error.

The reproduction for the earlier, separate bug (children escaped twice, withastro/astro#18785) is on the `main` branch.
