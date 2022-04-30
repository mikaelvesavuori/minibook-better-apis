# Unit testing (and more)

At the end of the day, testing is about building confidence in our code. We need to have enough to adequately say our code is covered, and that we feel confident about what we wrote.

**🎯 Example**: You'll see that the tests under `tests/` are segmented into various categories. The tests under `tests/unit` are for the individual, relevant layers such as controllers, entities, and frameworks.

If you are writing tests _after_ having created the initial code—functions, classes, etc—then a good (and fast!) way to create baseline coverage and confidence is by writing tests for the high-level layers, such as the controllers. Some people call these fuller, outer-boundary tests "component tests" since they address full use cases. Testing this way, you have the immediate benefit of understanding your code as an API, as this is the closest to (or even the exact same) code that will be "the real deal" in production. These tests, therefore, tend to be very similar when writing for the integration and contract test use-cases: after all, they all try to address the same (or similar) things through more or less the exact same API.

Such coarse-grained API/integration/contract/component/unit tests also typically exercise a wider portion of your codebase, though normally not all of the error handling and such. _This is when you want to start testing on a finer level and not only at the very edge of your code._

For "frameworks" (such as utilities and idempotent calculations) these should be written so that they are very easy to test in isolation. These tend to be the easiest and least messy parts to test. Test both "success" and "failure" states as far as logically possible and meaningful.

While there is no such thing as "untestable" code, in practice if something is hard as nails to test, then it's probably the code (not the test) that needs refactoring.

If you later discover unknown test cases that need to be added, to guard against those, we just add them to the unit test collection. Some people call these "regression tests" but it's all the same in the end.

_Spending even a bit of time raising the overall coverage to 90% or more will provide valuable confidence to you and your team. Remember that there are diminishing returns after a certain point, and you should feel comfortable about just leaving some things untested, and especially so if the uncovered areas are not able to create meaningful problems._
