---
description: TODO
---

# 📄 API schema

An API schema describes our API in a standardized way. You can write schemas by hand, use tooling to generate them, or go with services like [Stoplight](https://stoplight.io), [Bump](https://bump.sh), [Readme](https://readme.com), and API clients like [Insomnia](https://insomnia.rest/product/design) that sometimes have capabilities to design APIs, too.

**When you actually have a schema, make sure to make it accessible and visible (that's our reason for using Bump in this demo).**

There are a few ways to think about schemas, like ["API design-first"](https://www.infoq.com/articles/api-mocking-break-dependencies/), in which we design the API and generate the actual code from the schema. Our way is more traditional since we create the code and keep the schema mostly as a representation (however: a very important representation!).

We use the [OpenAPI 3](https://swagger.io/specification/) standard. The approach is manual, and hardly the most forward-facing option, but it's easy to understand and lets us (for what it's worth) implement the API as we need while trying to stay true to the schema specification. Learn more about it over at [Swagger](https://swagger.io/docs/specification/basic-structure/) and [Documenting APIs](https://idratherbewriting.com/learnapidoc/docapis_introtoapis.html).

**🎯 Example**: See `api/schema.yml` for the OpenAPI 3 schema. Since our approach is manual, we have to implement any security and/or validations on our end, in code. In our case, this is both for ingoing and outgoing data. Ingoing data can be seen handled at [`src/FakeUser/controllers/FakeUserController.ts`](src/FakeUser/controllers/FakeUserController.ts) in `checkInput()`, and outgoing data is handled in [`src/FakeUser/entities/User.ts`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/src/FakeUser/entities/User.ts) and its various validation functions like `validateName()`.

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

_Strongly consider using security tooling like 42Crunch's_ [_VS Code plugin for OpenAPI_](https://marketplace.visualstudio.com/items?itemName=42Crunch.vscode-openapi)_._

Note that because this is intended as a public API, the OAS security object is not present.

_For GraphQL, consider if something like_ [_Apollo Studio_](https://www.apollographql.com/docs/studio/) _might be a way to cover this area for your needs._
