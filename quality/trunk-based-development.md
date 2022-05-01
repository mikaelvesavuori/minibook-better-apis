---
description: TODO
---

# 🛣 Trunk-Based Development

> There are two main patterns for developer teams to work together using version control. One is to use feature branches, where either a developer or a group of developers create a branch usually from trunk (also known as main or mainline) and then work in isolation on that branch until the feature they are building is complete. When the team considers the feature ready to go, they merge the feature branch back to trunk.
>
> The second pattern is known as trunk-based development, where each developer divides their own work into small batches and merges that work into trunk at least once (and potentially several times) a day. The key difference between these approaches is scope. Feature branches typically involve multiple developers and take days or even weeks of work. In contrast, branches in trunk-based development typically last no more than a few hours, with many developers merging their individual changes into trunk frequently.

From Google's article [DevOps tech: Trunk-based development](https://cloud.google.com/architecture/devops/devops-tech-trunk-based-development)

For me, Trunk Based Development (TBD) captures an essential truth: That most branching models are just too complicated, and have too many adverse effects when it comes to merging code and staying agile. Even if certain models (IMHO) balance those aspects fairly well, like [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow) (not to be mixed up with [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/)!), nothing is simpler and more core to the agile values than TBD: Just push to `main` (or `master` if you aren't up with the times) and trust your tooling to handle it.

{% hint style='info' %}

Note that even Vincent Driessen, the original conceiver of GitFlow, nowadays [actively discourages the use of GitFlow in modern circumstances](https://nvie.com/posts/a-successful-git-branching-model/).

{% endhint %}

[Trunk Based Development](https://trunkbaseddevelopment.com) is worth reading about, if nothing else because it seems misunderstood by some.

**🎯 Example 1**: I can't easily point to some evidence here, but the full history of this project (both the code and the book/guide) has been handled this way.

The pros and cons of TBD are of course only truly, fully visible when seen together with other practices and tools you have. There needs to be certain maturity before doing this while remaining safe. This project should represent perfectly valid conditions for which TBD can be used instead of less agile strategies.

{% hint style='info' %}

Tip: You can usually set up various branching strategies and restrictions in your CI tool, to effectively require TBD as part of the workflow.

{% endhint %}

**🎯 Example 2**: To truly support TBD, I've added [husky](https://github.com/typicode/husky) to run pre-commit hooks for various activities, such as testing. This way we get to know even before the code is "sent off" if it's in shape to reach the CI stage.

Read more about Git workflow anti-patterns at [Jason Swett's blog](https://www.codewithjason.com/git-workflow-anti-patterns/).
