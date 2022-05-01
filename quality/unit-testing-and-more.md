---
description: TODO
---

# 🧪 Unit testing (and more)

> More code is bad. Less code is good. Only add code when the other options have been exhausted. This also applies to tests.
>
> You are writing tests to increase your confidence in your system. If you are not confident about aspects of your system, do more testing. When you have to make compromises due to time and budget constraints, always prioritise testing the areas where you need the most confidence.

— Source: [Scott Logic](https://blog.scottlogic.com/2018/03/12/testing-confidence-engineering.html)

Testing is fundamentally about building confidence in our code. We need to have enough tests to accurately be able to say that our code is covered for its critical use-cases, and that we feel confident about the code (and tests!) we wrote.

**🎯 Example**: You'll see that the tests under [`tests/`](https://github.com/mikaelvesavuori/better-apis-workshop/tree/main/tests) are segmented into several wider categories. The tests under [`tests/unit`](https://github.com/mikaelvesavuori/better-apis-workshop/tree/main/tests/unit) are for the individual relevant layers such as controllers, entities, and frameworks.

If you are writing tests _after_ having created the initial code—functions, classes, etc—then a good (and fast!) way to create baseline coverage and confidence is by writing tests for the high-level layers, such as the controllers. Some people call these fuller, outer-boundary tests "component tests" since they address full use cases that likely encapsulate multiple smaller functions.

Testing this way, you have the immediate benefit of understanding your code as an API, as this is the closest to—or even the exact same—code that will be "the real deal" going out into production. These tests therefore tend to be very similar when writing for the integration and contract test use-cases: After all, they all try to address the same, or at least similar, things through more or less the exact same API.

Such coarse-grained API/integration/contract/component/unit tests also typically exercise a wider portion of your codebase, though normally not all of the error handling and such. _This is when you want to start testing on a finer level and not only at the very edge of your code._

For "frameworks" (such as utilities and idempotent calculations) these should be written so that they are very easy to test in isolation. These tend to be the easiest and least messy parts to test. Test both "success" and "failure" states as far as logically possible and meaningful.

While there is no such thing as "untestable" code, in practice if something is hard as nails to test, then it's probably the code (not the test) that needs refactoring.

If you later discover unknown test cases that need to be added, to guard against those, we just add them to the unit test collection. Some people call these "regression tests" but it's all the same in the end.

_Spending even a bit of time raising the overall coverage to 90% or more will provide valuable confidence to you and your team. Remember that there are diminishing returns after a certain point, and you should feel comfortable about just leaving some things untested, and especially so if the uncovered areas are not able to create meaningful problems._
