---
description: TODO
---

# 📂 API client version using headers

One typical way to define expectations on an API is to use versioning. While there are several ways to do this—for example, refer to [this article from Nordic APIs](https://nordicapis.com/everything-you-need-to-know-about-api-versioning/)—we are going to use a header to decide which API backend to actually activate for the request.

**🎯 Example**: See [`src/FakeUser/controllers/FakeUserController.ts`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/src/FakeUser/controllers/FakeUserController.ts) and note how the header is handled in `checkInput()`.

{% code title="src/FakeUser/controllers/FakeUserController.ts" %}

```typescript
/**
 * @description Check and validate input.
 */
function checkInput(event: APIGatewayProxyEvent): string {
  const clientVersion =
    event?.headers["X-Client-Version"] || event?.headers["x-client-version"];
  const isClientVersionValid = validateClientVersion(clientVersion || "");
  const userId = event?.requestContext?.authorizer?.principalId;
  const isUserValid = validateUserId(userId || "");

  if (!isClientVersionValid || !isUserValid)
    throw new Error("Invalid client version or user!");

  return clientVersion || "";
}
```

{% endcode %}

{% hint style="warning" %}
An alternative and perhaps more commonly used approach would be to deploy a new instance of the API—like `api.com/v2`—but of course this would create further hardware segregation (having a separate `v1` and `v2` instance), which we want to avoid.
{% endhint %}

So, while it may be non-standard, in this context version 2 of the API represents the beta, meaning that version 1 represents the current (or "stable", "old") variant.

With this we have created a way to dynamically define our response simply through a header, without resorting to separate codebases or separate deployments.

{% hint style="warning" %}

[GraphQL is a different beast when it comes to versioning](https://graphql.org/learn/best-practices/#versioning). See this section as REST-specific in its details, but a good idea overall as long as you adapt to what versioning means for your protocol.

{% endhint %}
