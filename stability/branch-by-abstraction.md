---
description: TODO
---

# 🧬 Branch by abstraction

How do we do better than using branches? Well... not using branches! But how to deal with changes bigger than we want to contain in a single commit?

> "Branch by Abstraction" is a technique for making a large-scale change to a software system in gradual way that allows you to release the system regularly while the change is still in-progress.

— Source: [Martin Fowler: BranchByAbstraction](https://martinfowler.com/bliki/BranchByAbstraction.html)

[Paul Hammant](https://paulhammant.com) seems to be the originator, if not of the pattern, then at least of the term. He's also clear on this being smarter than using multiple branches.

> \[Branch] by abstraction instead of by \[code] branching in source control. And no, that doesn't mean sprinkle conditionals into your source code, it means to use an abstraction concept that's idiomatic for the programming language you are using.

— Source: [Branch By Abstraction?](https://www.branchbyabstraction.com)

This pattern works especially well when making significant changes to existing code. I might be harsh here, but there might well be severe code smells already present, since abstracting this way should be easy with well-engineered and nicely separated code.

It's all pretty simple, actually. The full eight steps are:

> 1. Introduce an abstraction to methodically chomp away at that time consuming non-functional change
> 2. Start with a single most depended on and least depending component
> 3. To not jeopardize anyone else's throughput, work in a place in the codebase that is separate from the existing code
> 4. Methodically complete the work, temporarily duplicating tens or hundreds of components
> 5. Go live in a 'dark deployment' mode part complete as many times as needed
> 6. Scale up your CI infrastructure to guard old and new implementations
> 7. When ready, switch over in production (a toggle flip to end 'dark deployment' mode)
> 8. Lastly, delete old implementations and the abstraction itself (essential follow up work)

**🎯 Example**: While our example might be too lightweight, and involved "new" and not old code, we do have a "beta" version and a "current" version as two code paths (see [`src/FakeUser/controllers/FakeUserController.ts`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/src/FakeUser/controllers/FakeUserController.ts), lines 37-44), abstracted in the controller.

Effectively, we are—even in this contrived example—making it easy and possible to work together with both new and old code without too much risk. Taking this thinking further, it's possible to solve vastly more complex situations just as well.

{% code title="src/FakeUser/controllers/FakeUserController.ts" %}

```typescript
/**
 * Run current version for:
 * - Legacy users
 * - If missing version header
 * - If version header is explictly set to an older version
 */
if (
  !clientVersion ||
  parseFloat(clientVersion) < BETA_VERSION ||
  toggles.userGroup === "legacy"
)
  return currentVersion();
// Run beta version for everyone else
else return await betaVersion(toggles);
```

{% endcode %}

If you need a hint, the encapsulation of the versions into their own "use-cases" makes it very easy to package completely different functionality into the same deployable artifact.

{% hint style="info" %}

Refer to the [Branch by Abstraction website](https://www.branchbyabstraction.com) for more.

{% endhint %}
