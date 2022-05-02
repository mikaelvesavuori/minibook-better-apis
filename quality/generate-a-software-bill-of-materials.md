---
description: What went into making your software, besides swearing and broken deadlines?
---

# 🧾 Generate a software bill of materials

We need to understand what our software is composed of—this is called "software composition analysis" (SCA).

For certain cases (such as regulated industries) this is _extremely_ important, down to the requirement of knowing each and every dependency and what they themselves are built out of... For our case, though, we are creating the SBOM to understand at "face value" what software (and risks) we are bundling together.

**🎯 Example**: We create a Software Bill of Materials (or `SBOM`) similarly to how we ran automated scans, except this time we do it for our packages using [Syft](https://github.com/anchore/syft). This is, yet again, visible together with the other tools running in the [CI script](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/.github/workflows/main.yml).
