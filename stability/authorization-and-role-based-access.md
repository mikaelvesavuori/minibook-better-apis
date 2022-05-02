---
description: TODO
---

# 🎭 Authorization and role-based access

You'll often see tutorials and such talking about [authentication](https://auth0.com/intro-to-iam/what-is-authentication/), which is about how we can verify that a person really is the person they claim to be. This tends to be mostly a technical exercise.

[**Authorization**](https://www.osohq.com/academy)**, on the other hand, is knowing what this person is allowed to do.**

Only trivial systems will require no authorization, so prepare to think about how you want your model to work and how to construct your permission sets. Rather than being a technical concern, this becomes more logical than anything else.

{% hint style="warning" %}
Authentication is entirely out of scope here, whereas trivial authorization _is_ in scope.
{% endhint %}

**🎯 Example 1**: In [`serverless.yml`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/serverless.yml) (lines 60-62) we define an authorizer function, located at [`src/FeatureToggles/controllers/AuthController.ts`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/src/FeatureToggles/controllers/AuthController.ts). Then on lines 70-74, we attach it to the `FakeUser` function. Now, the authorizer will run before the FakeUser function when called.

{% code title="serverless.yml" %}
```yml
functions:
  Authorizer:
    handler: src/FeatureToggles/controllers/AuthController.handler
    description: ${self:service} authorizer
  FakeUser:
    handler: src/FakeUser/controllers/FakeUserController.handler
    description: Fake user
    events:
      - http:
          method: GET
          path: /fakeUser
          authorizer:
            name: Authorizer
            resultTtlInSeconds: 30 # See: https://forum.serverless.com/t/api-gateway-custom-authorizer-caching-problems/4695
            identitySource: method.request.header.Authorization
            type: request
```
{% endcode %}

**🎯 Example 2**: In our application we use a handcrafted [RBAC](https://en.wikipedia.org/wiki/Role-based\_access\_control) (role-based access control) to attach a user group to each of the users. The user group is added as a flag in the subsequent call to the actual service.

In [`src/FeatureToggles/usecases/getUserFeatureToggles`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/4c28f13ce65cb0d05cb09154a27b8949bcd1641a/src/FeatureToggles/usecases/getUserFeatureToggles.ts) users are matched like so:

{% code title="src/FeatureToggles/usecases/getUserFeatureToggles.ts" %}
```typescript
/**
 * @description Get user's authorization level keyed for their name. Fallback is "standard" features.
 */
function getUserAuthorizationLevel(user: string): string {
  const authorizationLevel = userPermissions[user];
  if (!authorizationLevel) return "standard";
  else return authorizationLevel;
}
```
{% endcode %}

{% code title="src/FeatureToggles/config/userPermissions.ts" %}
```typescript
export const userPermissions: Record<string, UserGroup> = {
  "erroruser@company.com": "error",
  "legacyuser@company.com": "legacy",
  "betauser@company.com": "beta",
  "standarduser@company.com": "standard",
  "devuser@company.com": "dev",
  "devnewfeatureuser@company.com": "devNewFeature",
  "qauser@company.com": "qa",
};
```
{% endcode %}

{% hint style="info" %}
For more full-fledged implementations, consider tools like [Oso](https://www.osohq.com) to authorize on the overall "access level". [See for example this demo](https://www.osohq.com/post/add-authorization-to-a-serverless-nodejs-app) using a similar serverless stack, or the plain [quick start version](https://docs.osohq.com/node/getting-started/quickstart.html).
{% endhint %}
