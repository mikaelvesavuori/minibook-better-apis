---
description: TODO
---

# 📜 Generate documentation

Documentation... Love it or hate it, but professionally produced software absolutely requires documentation at least as good as the software itself.

Some of the different types of docs we might need to produce include:

- **Customer-facing documentation**: Guides, tutorials, how-to's...
- **API documentation**
- **Technical documentation**: Class diagrams, deployment diagrams, dependency graphs...
- **Code documentation**: Comments...
- **Project/product documentation**: Internal stuff like tickets, roadmaps, call sheets...

So being lax about _what_ types of docs we are talking about is maybe not too helpful. But we can slice the cake differently, and try to bucket the above into two major categories:

- **Requires manual labor**: Intellectual work needed, not deterministic
- **Better when generated**: Context is complicated, dynamic, but deterministic

{% hint style='tip' %}

Just like code, documentation is a liability and a responsibility. The more of it you have, the higher the price of upkeep can be.

{% endcode %}

We should attempt to reach the point where as much as possible of the documentation can be generated and output through automation. Good candidates for automated document generation would be dependency graphs, some of our architecture views, and technical documentation from our comments, types, and code structure.

Now let's look at how we've done a bit in the code part of this project.

**🎯 Example 1**: Open up any TS file in the `src` folders. Here, we use the [JSDoc](https://jsdoc.app) standard for documenting source code. Since we are using TypeScript, the style I am using discards "params" instead mostly focusing on the descriptive and referential aspects of JSDoc.

{% code title="src/FakeUser/frameworks/callExternal.ts" %}

```typescript
/**
 * @description Check if JSON is really a string.
 * @see https://stackoverflow.com/questions/3710204/how-to-check-if-a-string-is-a-valid-json-string-in-javascript-without-using-try
 */
const isJsonString = (str: string): Record<string, unknown> | boolean => {
  try {
    JSON.parse(str);
  } catch (e) {
    return false;
  }
  return true;
};
```

{% endcode %}

**🎯 Example 2**: Then, we use [TypeDoc](https://typedoc.org) for generating docs from the source code. You can generate documentation into the `typedoc-docs` folder by running `npm run docs`. These are the documents uploaded to [the static website](https://better-apis-workshop.pages.dev) as well. This brings us full circle on certain documentation being automatically updated and uploaded, as soon as we push any changes!

**🎯 Example 3**: For diagrams, we use [Arkit](https://arkit.pro) for generating diagrams from your software architecture. See the [Arkit diagrams](workshop/architecture-diagrams). As with the docs, these are also generated with `npm run docs` and presented on the published website.

**🎯 Example 4**: Taken together, this means that you can easily make rich documentation available in a central, visible location (like a static website in this case) in the CI stage. See [`.github/workflows/main.yml`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/.github/workflows/main.yml) for the CI script itself. Note that Cloudflare Pages is setup outside of GitHub so you won't see too much of that integration in the script.

{% hint style='info' %}

For deeper reading on the topic of documentation, I highly recommend:

- [Documenting APIs: A guide for technical writers and engineers](https://idratherbewriting.com/learnapidoc/)
- [Google: Technical Writing One](https://developers.google.com/tech-writing/one)
- [Principles of technical documentation](https://www.innoq.com/en/articles/2022/01/principles-of-technical-documentation/)

{% endhint %}
