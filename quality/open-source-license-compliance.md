---
description: >-
  Make sure you make all the open source angels out there happy by complying
  with obligations and license restrictions.
---

# ✅ Open source license compliance

Open source is fantastic, you don't need me to tell you about it! However, consuming (and sometimes redistributing) open source is not always a very trivial matter. Especially when we start having to comply with license obligations, like providing license files, and attributing people and so on.

{% hint style="info" %}
I've previously written on this topic on Medium, [Open source license compliance, the TL;DR version](https://medium.com/wearehumblebee/open-source-license-compliance-the-tl-dr-version-b52ca78df957).

Some other good resources for open source and licensing include:

* [TLDRLegal](https://tldrlegal.com)
* [GitHub: Open Source Guides](https://opensource.guide)
* [Google Open Source](https://opensource.google/documentation/reference)
{% endhint %}

To deal with potential legal issues, we'll set up checks to allow only permissive, good, and well-established open source licenses to be used in our own software.

**🎯 Example**: In [`package.json`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/package.json) we have two scripts that run as part of the pre-commit hook and in CI:

{% code title="package.json" %}
```json
"licenses": "npx license-compliance --direct --allow 'MIT;ISC;0BSD;BSD-2-Clause;BSD-3-Clause;Apache-2.0;Unlicense;CC0-1.0'",
"licenses:checker": "npx license-compatibility-checker",
```
{% endcode %}

These verify that we only use a set of allowed open-source licenses, using [license-compliance](https://www.npmjs.com/package/license-compliance), and can also use [license-compatibility-checker](https://www.npmjs.com/package/license-compatibility-checker) to check for compatibility between our license and our used ones.

Because we are doing server-side applications (i.e. a backend or API), we are not redistributing any code, making our obligations easier to handle and less messy. Webpack will bundle all licenses as well, so we should be all set.
