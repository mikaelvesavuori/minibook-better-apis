---
description: TODO
---

# ✅ Open source license compliance

Open source is fantastic, you don't need me to tell you about it! However, consuming (and sometimes redistributing) open source is not always a very trivial matter.

To deal with potential legal issues, we'll set up checks to allow only permissive, good, and well-established open source licenses to be used in our own software.

**🎯 Example**: In [`package.json`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/package.json) we have two scripts that run as part of the pre-commit hook and in CI:

{% code title="package.json" %}

```json
"licenses": "npx license-compliance --direct --allow 'MIT;ISC;0BSD;BSD-2-Clause;BSD-3-Clause;Apache-2.0;Unlicense;CC0-1.0'",
"licenses:checker": "npx license-compatibility-checker",
```

{% endcode %}

These verify that we only use a set of allowed open-source licenses, using [license-compliance](https://www.npmjs.com/package/license-compliance), and can also use [license-compatibility-checker](https://www.npmjs.com/package/license-compatibility-checker) to check for compatibility between our license and our used ones.

{% hint style='info' %}

Some good resources for open source and licensing include:

- [TLDRLegal](https://tldrlegal.com)
- [GitHub: Open Source Guides](https://opensource.guide)
- [Google Open Source](https://opensource.google/documentation/reference)

{% endhint %}
