---
description: TODO
---

# 🏗 Continuous Integration and Continuous Deployment

We're at the very basics of classic [agile](https://en.wikipedia.org/wiki/Agile_software_development) (and [extreme programming](https://en.wikipedia.org/wiki/Extreme_programming)) when we state that Continuous Integration (or [CI](https://explainagile.com/agile/xp-extreme-programming/practices/continuous-integration/)) and Continuous Delivery or Deployment ([CD](https://www.atlassian.com/continuous-delivery/continuous-deployment)) are things to strive for.

However, even 20 years later it still seems like these notions are pretty far away in many organizations. To be frank then, the problem is as [Atlassian writes](https://www.atlassian.com/continuous-delivery/principles/why-agile-development-needs-continuous-delivery), "agile isn't agile without continuous delivery".

{% hint style='info' %}

For more on the relation between delivery and deployment (and more), [read this article by Atlassian](https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment).

{% endhint %}

Thankfully, there's beginning to be some standardization around what high performance in technology delivery actually means, driven primarily by the [DORA research program](https://www.devops-research.com/research.html) and related books like [Accelerate](https://www.amazon.com/Accelerate-Software-Performing-Technology-Organizations/dp/1942788339), by Nicole Forsgren, Jez Humble, and Gene Kim.

{% hint style='info' %}

Tip: You can take a simple test, called the [DORA DevOps Quick Check](https://www.devops-research.com/quickcheck.html) to check your DORA performance metrics.

{% endhint %}

When it comes to our practices, we can make meaningful steps towards this by opting for [smaller releases](https://explainagile.com/agile/xp-extreme-programming/practices/small-releases/) and having a [limit on work-in-progress](https://www.atlassian.com/agile/kanban/wip-limits). This keeps the focus tighter, features smaller, the release cadence flowing smoother and faster. Everyone wins.

**🎯 Example**: You can see that we have CI running in GitHub with our script located at `.github/workflows/main.yml`. Every commit runs the full pipeline and deploys new code to AWS. The other "soft practices", are somewhat less valid for a single author and can't be easily demonstrated.

CI/CD is nowadays 20% a technology problem (good and easy tooling is available in abundance) and 80% a people and process problem. Using DORA metrics and CI/CD, you can start setting a high mark on the objective delivery improvements that these tools and practices help enable when compared to traditional waterfall teams. This is all of course contingent on you having a situation that allows such flexibility of operating (potentially) differently than your organization already does things.
