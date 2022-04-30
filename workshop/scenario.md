---
description: TODO
---

# Scenario

You are supporting your friends, who are making a new social online game, by providing an API service that churns out fake users. For their initial MVP, they asked you to create a service that just returns hardcoded feedback; an object like so, `{ "name": "Someguy Someguyson" }`. This was enough while they built their network code and first Non-Player Character engine.

The code is as simple as:

```TypeScript
import { APIGatewayProxyEvent, Context, APIGatewayProxyResult } from 'aws-lambda';

/**
 * @description The controller for our "fake user" service, in its basic or naive shape.
 */
export async function handler(
  event: APIGatewayProxyEvent,
  context: Context
): Promise<APIGatewayProxyResult> {
  try {
    return {
      statusCode: 200,
      body: JSON.stringify({
        name: 'Someguy Someguyson'
      })
    };
  } catch (error) {
    return {
      statusCode: 500,
      body: JSON.stringify(error)
    };
  }
}
```

_Refer to [src/FakeUserBasic/index.ts](/src/FakeUserBasic/index.ts) for the above code._

Now, things are starting to become more involved:

- They are ready for a bit more detailed user data, meaning more fields.
- They are also considering pivoting the game to use cats, instead of humans.
- Of course, it's also ideal if the team can be sensibly isolated from any work on new features by individual contributors in the open-source community, who will need to have the work thoroughly documented and coded to a level that marks the high ambitions and makes it easy to contribute.
- Oh, and it would be good with a dedicated beta feature set, too.

For the near future, the **API must be able to handle all these use-cases**. It would also be perfect if the API can have **stable interfaces (for new clients and old), and not use several endpoints**, as the development overhead is already big for the two weekend game programmers.

How can you solve this situation?
