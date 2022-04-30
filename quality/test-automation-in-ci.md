---
description: TODO
---

# 🏭 Test automation in CI

First, let's look at what I call "defensive testing", that is, any typical testing that we do to test the resiliency and functionality of our solution.

We need to establish clear boundaries and expectations of where our work ends, and someone else's work begins. To not be overloaded with other's services (though we are dependent on them) we use API mocking to mock away external dependencies. For this purpose, we use [Mock Service Worker](https://mswjs.io).

**🎯 Example 1**: See `tests/mocks/index.ts`.

Next up, we set up contract testing, using [TripleCheck CLI](https://github.com/mikaelvesavuori/triplecheck-cli). Contract testing means that we can easily verify if our assumptions of services and their interfaces are correct.

_For wider scale and bigger system landscapes, consider using the_ [_TripleCheck broker_](https://github.com/mikaelvesavuori/triplecheck-broker) _to be able to store and load contracts from a centralized source._

**🎯 Example 2**: See `triplecheck.config.json` and the TripleCheck documentation linked above.

During the CI we will deploy a complete, realistic stack with the most recent version. First, we'll do some basic smoke tests, to verify we don't have a major malfunction on our hands. _In reality, smoke tests are just lighter types of integration tests._

**🎯 Example 3**: See `tests/synthetics/smoketest.sh`; it's just a very basic `curl` call!

When it comes to _actual_ integration testing of the real service we will do it when we've seen our smoke tests pass. My solution is a home-built thingy that makes some calls and evaluates the expected responses with the received data using [ajv](https://ajv.js.org).

**🎯 Example 4**: See `tests/integration/index.ts`.
