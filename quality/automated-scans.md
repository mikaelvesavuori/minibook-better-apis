---
description: TODO
---

# 🔁 Automated scans

In this age of DevOps, we don't want to forget the security portion of our responsibility. Perhaps the more proper term is DevSecOps, which is more and more making headway in the developer community.

To support Dev(Sec)Ops, a best practice is to use various types of scans to automate often boring, sometimes hard, sometimes also mandated requirements e.g. for compliance and security aspects.

Since there is no DevOps without automation the tooling we adopt needs to work both in CI and locally, and provide meaningful confidence and improvements to our delivery. [GitLab writes about their view on "5 benefits of automated security"](https://about.gitlab.com/blog/2020/07/08/devsecops-security-automation/), which they summarize as:

> - Reduced human error.
> - Early security intervention.
> - Streamlined vulnerability triage.
> - Repeatable security checks.
> - Responsibility clarification.

This is all gold. We want this.

There exists a lot of tooling in this space these days. We are going to use some of the more well-known, free and open-source options.

{% hint style='info' %}

You can see the below tools running in the [CI script](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/.github/workflows/main.yml).

{% endhint %}

**🎯 Example 1**: We use [Trivy](https://github.com/aquasecurity/trivy) to check for vulnerabilities in packages (among other places).

**🎯 Example 2**: Then, we use [Checkov](https://www.checkov.io) to scan for misconfigurations, and also create an infrastructure-as-code SBOM (["software bill-of-materials"](https://en.wikipedia.org/wiki/Software_bill_of_materials)).

**🎯 Example 3**: Finally, we use [gitleaks](https://github.com/zricethezav/gitleaks) for detecting hard-coded secrets in our repository.
