---
description: TODO
---

# 📜 Generate documentation

While it would be unrealistic to say that you shouldn't need to write documentation, we can at least get to the point where documentation (given that some exist) can be generated and output through automation.

Here, we use the [JSDoc](https://jsdoc.app) standard for documenting source code.

**🎯 Example 1**: Open up any TS file in the `src` folders.

Then, we use [TypeDoc](https://typedoc.org) for generating docs from the source code.

**🎯 Example 2**: You can generate documentation into the `typedoc-docs` folder by running `npm run docs`. These are the documents uploaded to [the static website](https://better-apis-workshop.pages.dev) as well.

For diagrams, we use [Arkit](https://arkit.pro) for generating diagrams from your software architecture.

**🎯 Example 3**: See the [Arkit diagrams](workshop/architecture-diagrams). As with the docs, these are also generated with `npm run docs`.

Taken together, this means that you can easily make rich documentation available in a central, visible location (like a static website in this case) in the CI stage. This is precisely what I've done on my end for this project: These diagrams also get uploaded to Cloudflare Pages on each commit to `main`.

**🎯 Example 4**: See `.github/workflows/main.yml` for the CI script.
