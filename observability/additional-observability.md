# 👁️ Additional observability

We can go full monty and add better, richer observability tooling when we feel like we've outgrown the AWS standard tooling.

[Honeycomb](https://www.honeycomb.io) is a next-generation "true" observability solution. This can be bolted on as an addition if you want some more bells and whistles over the AWS stuff. It's also based on the OpenTelemetry standard, making it pretty future-proof. [Read about OpenTelemetry here](https://leaddev.com/monitoring-observability/rise-opentelemetry).

If you want to toy with Honeycomb, look no further than [their playground](https://www.honeycomb.io/play/).

If you want to try Honeycomb with this project, it's pretty easy if we use their [Lambda extension](https://github.com/honeycombio/honeycomb-lambda-extension):

1. [Get a free (generous!) Honeycomb account](https://www.honeycomb.io).
2. When you have the account set up and are logged in, get an API key (it's enough that it can send events and create datasets). [See the docs for more details](https://docs.honeycomb.io/api/api-keys/).
3. Modify the provided logger as per below, [since the regular `console.log()` does not seem to work correctly with Honeycomb](https://docs.honeycomb.io/getting-data-in/integrations/aws/aws-lambda/#javascript).
4. Modify `serverless.yml`, uncommenting and adding your values to the environment variables `LIBHONEY_DATASET` and `LIBHONEY_API_KEY` (lines 37-38). Note: Ideally these are encrypted or in a secrets manager, but for this project, let's just do it this way.
5. Deploy the stack.
6. Add the Honeycomb Lambda integration layer by running `sh honeycomb-layer.sh` (ensure the values correspond with your values first!).
7. Ready to go! You should see data coming into Honeycomb shortly if you start using your live endpoints.

#### Setting up Bunyan as a logger

Install [`bunyan`](https://github.com/trentm/node-bunyan) and its typings with `npm install bunyan @types/bunyan`.

Open up `src/FakeUser/frameworks/Logger.ts` and add this to the top of the file:

```
import bunyan from 'bunyan';
const log = bunyan.createLogger({ name: 'better-apis-workshop' });
```

For the `log()`, `warn()` and `error()` methods, change the existing `console.log()`-dependent implementation lines to: `log.info(createdLog);`, `log.warn(createdLog);` and `log.error(createdLog);`, respectively.

In the `createLog()` method, go ahead and remove the `level` field, as `bunyan` adds that itself.
