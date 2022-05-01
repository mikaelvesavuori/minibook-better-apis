---
description: TODO
---

# 🏭 Test automation in CI

First, let's look at what I personally call "defensive testing", that is, any typical testing that we do to test the resiliency and functionality of our solution.

We need to establish clear boundaries and expectations of where our work ends, and someone else's work begins. I really this were a technical topic with clear answers, but the more I work with people, the more I keep getting surprised by how little teams do to precisely define their boundaries.

{% hint style='info' %}

It's well worth understanding (and practically implementing!) the concept [_bounded context_](https://martinfowler.com/bliki/BoundedContext.html) in your product/project.

{% endhint %}

**🎯 Example 1**: To not be overloaded with other's services (though we may be dependent on them) we can conduct API mocking to mock away external dependencies. For this purpose, we choose to use [Mock Service Worker](https://mswjs.io). See `tests/mocks/handlers.ts`. The generic pattern looks as easy as this:

{% code title="tests/mocks/handlers.ts" %}

```typescript
rest.get(API_ENDPOINT, (req, res, ctx) => {
  return res(ctx.status(200), ctx.json(apiResponse));
});
```

{% endcode %}

**🎯 Example 2**: Next up, we set up [contract testing](https://sqa.stackexchange.com/a/42064), using [TripleCheck CLI](https://github.com/mikaelvesavuori/triplecheck-cli). Contract testing means that we can easily verify if our assumptions of services and their interfaces are correct, but we skip verifying the exact semantics of the response. It's enough that the shape or syntax is right.

{% hint style='info' %}

For wider scale and bigger system landscapes, consider using the [TripleCheck broker](https://github.com/mikaelvesavuori/triplecheck-broker) to be able to store and load contracts from a centralized source.

{% endhint %}

See `triplecheck.config.json` and the TripleCheck documentation linked above.

**🎯 Example 3**: During the CI stage we will deploy a complete, realistic stack with the most recent version. First, we'll do some basic [smoke tests](<https://en.wikipedia.org/wiki/Smoke_testing_(software)>), to verify we don't have a major malfunction on our hands. _In reality, smoke tests are just lighter types of integration tests._

See `tests/synthetics/smoketest.sh`; it's just a very basic `curl` call!

**🎯 Example 4**: When it comes to _actual_ integration testing of the real service we will do it when we've seen our smoke tests pass. My solution is a home-built thingy that makes some calls and evaluates the expected responses with the received data using [ajv](https://ajv.js.org).

See `tests/integration/index.ts`.

**🎯 Example 5**: See [`.github/workflows/main.yml`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/.github/workflows/main.yml) for the CI script. You'll see all the overall steps covered:

```
- name: License check
- name: Scan for secrets
- name: Run Trivy vulnerability scanner
- name: Cache dependencies
- name: Install dependencies
- name: Test (units)
- name: Test (contracts)
- name: Test (IAC)
- name: Deploy services
- name: Bake time
- name: Smoke test
- name: Test (integrations)
- name: Test (load)
- name: Deploy API documentation
```
