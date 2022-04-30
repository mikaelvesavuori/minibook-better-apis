---
description: TODO
---

# 🧬 Branch by abstraction

In Paul Hammant's words:

> \[Branch] by abstraction instead of by \[code] branching in source control. And no, that doesn't mean sprinkle conditionals into your source code, it means to use an abstraction concept that's idiomatic for the programming language you are using.

It's a methodical, very practical thing too:

```
1. Introduce an abstraction to methodically chomp away at that time consuming non-functional change
2. Start with a single most depended on and least depending component
3. To not jeopardize anyone else's throughput, work in a place in the codebase that is separate from the existing code
4. Methodically complete the work, temporarily duplicating tens or hundreds of components
5. Go live in a 'dark deployment' mode part complete as many times as needed
6. Scale up your CI infrastructure to guard old and new implementations
7. When ready, switch over in production (a toggle flip to end 'dark deployment' mode)
8. Lastly, delete old implementations and the abstraction itself (essential follow up work)
```

**🎯 Example**: While our example might be too lightweight, we do have a "beta" version and a "current" version as two code paths (see `src/FakeUser/controllers/FakeUserController.ts`, lines 37-44), abstracted via the controller. Effectively, we are—even in this contrived example—making it easy and possible to work together with both new and old code without too much risk. Taking this thinking further, it's possible to solve vastly more complex situations just as well.

Refer to the [Branch by Abstraction website](https://www.branchbyabstraction.com).
