---
description: TODO
---

# 🏗 Continuous Integration and Continuous Deployment

We're at the very basics when it comes to classic [agile](https://en.wikipedia.org/wiki/Agile_software_development) (and [extreme programming](https://en.wikipedia.org/wiki/Extreme_programming)) when we state that [CI](https://explainagile.com/agile/xp-extreme-programming/practices/continuous-integration/) and [CD](https://www.atlassian.com/continuous-delivery/continuous-deployment) are things to strive for. Even 20 years later, it still seems like these notions are pretty far away in many organizations. To be frank, as [Atlassian writes](https://www.atlassian.com/continuous-delivery/principles/why-agile-development-needs-continuous-delivery), "agile isn't agile without continuous delivery".

_For more on the relation between delivery and deployment etc,_ [_read this article_](https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment)_._

Thankfully, there's beginning to be some standardization around what high performance in technology delivery actually means, driven primarily by the [DORA research program](https://www.devops-research.com/research.html) and related books like [Accelerate](https://www.amazon.com/Accelerate-Software-Performing-Technology-Organizations/dp/1942788339), by Nicole Forsgren, Jez Humble, and Gene Kim.

_Tip: You can take a simple test, called the_ [_DORA DevOps Quick Check_](https://www.devops-research.com/quickcheck.html) _to check your DORA performance metrics._

To get here we should also opt for [small releases](https://explainagile.com/agile/xp-extreme-programming/practices/small-releases/) and having a [limit on work-in-progress](https://www.atlassian.com/agile/kanban/wip-limits).

**🎯 Example**: You can see that we have CI running in GitHub, with our script located at `.github/workflows/main.yml`. Every commit runs the full pipeline and deploys new code to AWS. The other "soft practices", are somewhat less valid for a single author and can't be easily demonstrated.
