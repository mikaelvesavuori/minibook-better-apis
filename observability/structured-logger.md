# 🪵 Structured logger

Another best practice is to treat logs as a source of data rather than as individual strings. To do so, we need to have a structured approach to outputting them. In this project, we'll use a handcrafted logging utility to help us do this in an easy way that nobody can fail using correctly.

I've provided a basic one that also uses `getUserMetadata()` to get metadata (correlation ID and user ID) that has been set in the environment at an early stage in the controller.

**🎯 Example**: See [`src/FakeUser/frameworks/Logger.ts`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/src/FakeUser/frameworks/Logger.ts). Implementation is as simple as importing it and then:

{% code title="src/FakeUser/frameworks/Logger.ts" %}

```typescript
const logger = new Logger();
logger.log("My message!");
logger.warn("My warning!");
logger.error("My error!");
```

{% endcode %}

_Note: As opposed to some solutions, in our case, the Logger will not replace the vanilla `console.log` (etc) so you will need to import it everywhere you want to use it._

Using it, your logs will then all follow the format:

{% code title="src/FakeUser/frameworks/Logger.ts" %}

```typescript
{
  message: "My message!",
  level: "INFO" | "WARN" | "ERROR" <based on the call, i.e. logger.warn() etc.>,
  timestamp: <timestamp>,
  userId: <userId from metadata>,
  correlationId: <correlationId from metadata>
};
```

{% endcode %}

Good folks like [Yan Cui have written and presented on this matter many times](https://www.slideshare.net/Codemotion/yan-cui-how-to-build-observability-into-a-serverless-application-codemotion-amsterdam-2019) and you can certainly also opt-in to turnkey solutions like [`dazn-lambda-powertools`](https://github.com/getndazn/dazn-lambda-powertools).
