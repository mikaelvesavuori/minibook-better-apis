---
description: >-
  Sometimes the first question to answer is simply the one posed in Pulp
  Fiction—"English, motherf***er, do you speak it?"—and actually write down how
  you work.
---

# 📝 Make your processes known

This is step zero, so to speak.

Any non-trivial software development context requires some form of common ground to keep all of the work together, and for the team to correctly claim they have done their preparation work.

**🎯 Example 1**: See [`PROJECT.md`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/PROJECT.md) which is the start page for the generated website. It uses a basic template structure to aid in a team providing the right information. The file provides (among other things) information on the project governance model with key roles in the project and their responsibilities, outlines the requirements process, and points out where and how to follow work on the tasks/requirements. This file is just an opinionated starter to ensure that base-level questions around the project/product are answered.

**🎯 Example 2**: [`LICENSE.md`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/LICENSE.md) describes the license of this project. In an open-source project, a license should exist to make it clear what type of use is acceptable. For corporate projects, this is also valid so that you can clearly mark the software as proprietary (or open-source if you want to go that way!). [GitHub has a lot more on this](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository), if you are interested.

**🎯 Example 3**: Reading [`CONTRIBUTING.md`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/CONTRIBUTING.md) makes it clear how the team, and external parties, can contribute to the code base. This document also states some basic principles around code standards, code reviews, bug reporting, and similar "soft tech" issues. Don't underestimate the need to be clear on expectations, whether these are highly detailed nitpicky bits or general guidance: Your project will do better with a good contribution document. See [Mozilla's guide](https://mozillascience.github.io/working-open-workshop/contributing/) for more information.

**🎯 Example 4**: For our [`CODE_OF_CONDUCT.md`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/CODE\_OF\_CONDUCT.md), we are using the [Contributor Covenant](https://www.contributor-covenant.org), which is one standard to communicate baseline values and norms. Of course, enforcement and such are still in your hands. While it's made for open source circles, something like this makes sense to have in corporate contexts as well.

**🎯 Example 5**: [`SECURITY.md`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/SECURITY.md) describes how we address vulnerabilities and any reports of them, and how we are supposed to track them. Our tooling should use a risk-based remediation strategy based on [CVSS](https://www.first.org/cvss/user-guide), which basically boils down to setting ourselves up to have an informed view on the actual risks we have, have these valued, prioritize the most dangerous risks, and adapt our mitigation based on the severity of them.
