---
description: Looking at some time-honored engineering principles.
---

# 🧱 SOLID principles

The very first (technical) thing is to respect that good code, despite programming language, is good code even over time. Follow wise conventions like [SOLID](https://en.wikipedia.org/wiki/SOLID) to guide your daily work.

{% hint style='info' %}

See for example this [Stack Overflow article](https://stackoverflow.blog/2021/11/01/why-solid-principles-are-still-the-foundation-for-modern-software-architecture/) or [Khalil Stemmler's write-up](https://khalilstemmler.com/articles/solid-principles/solid-typescript/) for a concise introduction.

{% endhint %}

The principles are:

1. The `[S]ingle-responsibility principle`: "There should never be more than one reason for a class to change." (In other words, every class should have only one responsibility)
2. The `[O]pen–closed principle`: "Software entities ... should be open for extension, but closed for modification."
3. The `[L]iskov substitution principle`: "Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it."
4. The `[I]nterface segregation principle`: "Many client-specific interfaces are better than one general-purpose interface."
5. The `[D]ependency inversion principle`: "Depend upon abstractions, not concretions."

**🎯 Example**: In our project, one example of the dependency inversion principle comes into play when calling the `betaVersion()` function in [`src/FakeUser/controllers/FakeUserController.ts`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/src/FakeUser/controllers/FakeUserController.ts), as we send in the toggles for it to use.

The single responsibility principle should hopefully also be evident throughout most of the code.

{% code title="src/FakeUser/controllers/FakeUserController.ts" %}

```typescript
/**
 * @description Handle the new (v2) beta version.
 */
async function betaVersion(
  toggles: Record<string, unknown>
): Promise<APIGatewayProxyResult> {
  const response = await createFakeUser(toggles); // Run use case
  return {
    statusCode: 200,
    body: JSON.stringify(response),
  };
}
```

{% endcode %}
