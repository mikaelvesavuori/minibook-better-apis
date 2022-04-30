### 🗂️ API client version using headers

The first way to define expectations is to use API versioning. While there are several ways to do this—for example, refer to [this article from Nordic APIs](https://nordicapis.com/everything-you-need-to-know-about-api-versioning/)—we are going to use a header to define which API version to use.

**🎯 Example**: See `src/FakeUser/controllers/FakeUserController.ts` and note how the header is handled in `checkInput()`.

Note: An alternative, perhaps more commonly used, approach would be to deploy a new instance of the API (like `api.com/v2`), but of course this would create further hardware segregation, which we want to avoid.

So, while it may be non-standard, in this context version 2 of the API represents the beta, meaning that version 1 represents the current (or "stable", "old") variant.

With this, we have created a way to define output simply through a header, without resorting to separate codebases or separate deployments.

_[GraphQL is a different beast when it comes to versioning](https://graphql.org/learn/best-practices/#versioning). See this section as REST-specific in its details, but a good idea overall as long as you adapt to what versioning means for your protocol._

### 📄 API schema

An API schema describes our API in a standardized way. You can write schemas by hand, use tooling to generate them, or go with services like [Stoplight](https://stoplight.io), [Bump](https://bump.sh), [Readme](https://readme.com), and API clients like [Insomnia](https://insomnia.rest/product/design) that sometimes have capabilities to design APIs, too.

**When you actually have a schema, make sure to make it accessible and visible (that's our reason for using Bump in this demo).**

There are a few ways to think about schemas, like ["API design-first"](https://www.infoq.com/articles/api-mocking-break-dependencies/), in which we design the API and generate the actual code from the schema. Our way is more traditional since we create the code and keep the schema mostly as a representation (however: a very important representation!).

We use the [OpenAPI 3](https://swagger.io/specification/) standard. The approach is manual, and hardly the most forward-facing option, but it's easy to understand and lets us (for what it's worth) implement the API as we need while trying to stay true to the schema specification. Learn more about it over at [Swagger](https://swagger.io/docs/specification/basic-structure/) and [Documenting APIs](https://idratherbewriting.com/learnapidoc/docapis_introtoapis.html).

**🎯 Example**: See `api/schema.yml` for the OpenAPI 3 schema. Since our approach is manual, we have to implement any security and/or validations on our end, in code. In our case, this is both for ingoing and outgoing data. Ingoing data can be seen handled at `src/FakeUser/controllers/FakeUserController.ts` in `checkInput()`, and outgoing data is handled in `src/FakeUser/entities/User.ts` and its various validation functions like `validateName()`.

_Strongly consider using security tooling like 42Crunch's [VS Code plugin for OpenAPI](https://marketplace.visualstudio.com/items?itemName=42Crunch.vscode-openapi)._

Note that because this is intended as a public API, the OAS security object is not present.

_For GraphQL, consider if something like [Apollo Studio](https://www.apollographql.com/docs/studio/) might be a way to cover this area for your needs._

### 👌 API schema validation

AWS API Gateway offers schema validation. Schema validation is done with [JSON schemas](https://json-schema.org), which are similar but ultimately not the same as OpenAPI schemas.

These validators allow our gateway to respond to incorrect requests without us needing to do much of anything about them, other than provide the validator. Also, we have the benefit of our Lambda functions not running if the ingoing input is not looking the way we expect it to.

Once again, since we have a manual approach, any validation schemas need to be handled separately from code and the OpenAPI schema.

We only use validators for POST requests, which means the FeatureToggles function is in scope, but not the FakeUser function.

**🎯 Example**: See `api/FeatureToggles.validator.json`. It's attached to our feature toggles function in `serverless.yml` on lines 96-98.

### 🧬 Branch by abstraction

In Paul Hammant's words:

> [Branch] by abstraction instead of by [code] branching in source control. And no, that doesn't mean sprinkle conditionals into your source code, it means to use an abstraction concept that's idiomatic for the programming language you are using.

It's a methodical, very practical thing too:

```
1. Introduce an abstraction to methodically chomp away at that time consuming non-functional change
2. Start with a single most depended on and least depending component
3. To not jeopardize anyone else's throughput, work in a place in the codebase that is separate from the existing code
4. Methodically complete the work, temporarily duplicating tens or hundreds of components
5. Go live in a 'dark deployment' mode part complete as many times as needed
6. Scale up your CI infrastructure to guard old and new implementations
7. When ready, switch over in production (a toggle flip to end 'dark deployment' mode)
8. Lastly, delete old implementations and the abstraction itself (essential follow up work)
```

**🎯 Example**: While our example might be too lightweight, we do have a "beta" version and a "current" version as two code paths (see `src/FakeUser/controllers/FakeUserController.ts`, lines 37-44), abstracted via the controller. Effectively, we are—even in this contrived example—making it easy and possible to work together with both new and old code without too much risk. Taking this thinking further, it's possible to solve vastly more complex situations just as well.

Refer to the [Branch by Abstraction website](https://www.branchbyabstraction.com).

### 🆕 Beta functionality

We have built-in a "beta functionality" concept that is propagated from toggles into our services. This is a catch-all for new features that we want to test, and which may not yet be ready for wider release. This means that our services need to have distinct checks for this.

As you can see further below in the feature toggles section, we can also use user groups to segment features, as well as classic, individual toggles that can be used on a per-user basis.

What gives? Aren't these beta features just like other toggles? **Yes**. In this project, we define ”beta features” as a _feature-level, wide bucket across user groups_, while user grouping is a dynamically wide bucket (basically just audience segmentation). This way, we can define granularly both on user groups and on beta usage.

**🎯 Example**: See for example `src/FakeUser/usecases/createFakeUser.ts` that uses the provided toggles when calling the User entity which dynamically creates different types of users from this toggle.

### 🏁 Feature toggles ("feature flags")

This demo uses a handcrafted, simple feature flags engine and a small toggle configuration.

While a technically trivial solution, this enables a dynamic configuration that can be detached from the source code itself. [In effect, this means we are one big step closer to separating a (technical) deployment from a (business-oriented) release.](https://medium.com/turbine-labs/deploy-not-equal-release-part-two-acbfe402a91c)

The feature function is located at `src/FeatureToggles`. For the configuration part, we should put it on [Mockachino](https://www.mockachino.com) so that we can approximate a more conventional feature toggles service, getting (and updating) flags without having to re-deploy our code. The set of feature toggles is ordered under each respective user group.

Unknown (or missing, non-existing, null...) users get the `standard` user group access.

**🎯 Example**: The full configuration you can use as your template looks like the one below. Notice how it's segmented on the group level:

```json
{
  "error": {
    "enableBetaFeatures": false,
    "userGroup": "error"
  },
  "legacy": {
    "enableBetaFeatures": false,
    "userGroup": "legacy"
  },
  "beta": {
    "enableBetaFeatures": true,
    "userGroup": "beta"
  },
  "standard": {
    "enableBetaFeatures": false,
    "userGroup": "standard"
  },
  "dev": {
    "enableBetaFeatures": true,
    "userGroup": "dev"
  },
  "devNewFeature": {
    "enableBetaFeatures": true,
    "enableNewUserApi": true,
    "userGroup": "devNewFeature"
  },
  "qa": {
    "enableBetaFeatures": false,
    "userGroup": "qa"
  }
}
```

You can update the flags as you want, to get the effects you need, without requiring any redeployment!

_Note that different feature toggle services or implementations may differ in how they exactly apply toggles, and how they work. In our case here, we apply group-level toggles rather than request individual flags, if nothing else than for simplicity of demonstration._

Refer to [FeatureFlags.io](https://featureflags.io) and to [Unleash](https://www.getunleash.io)'s documentation on [activation strategies](https://docs.getunleash.io/user_guide/activation_strategy) for inspiration.

### 🎭 Authorization and role-based access

You'll often see tutorials and such talking about [authentication](https://auth0.com/intro-to-iam/what-is-authentication/), which is about how we can verify that a person really is that person.

[Authorization](https://www.osohq.com/academy), on the other hand, is knowing what this person is allowed to do.

_Authentication is entirely out of scope here, whereas trivial authorization IS in scope._

**🎯 Example 1**: In `serverless.yml` (lines 60-62) we define an authorizer function, located at `src/FeatureToggles/controllers/AuthController.ts`. Then on lines 70-74, we attach it to the `FakeUser` function. Now, the authorizer will run before the FakeUser function when called.

In our self-contained, basic demo we use a handcrafted [RBAC](https://en.wikipedia.org/wiki/Role-based_access_control) (role-based access control) to attach a user group to each of the users. The user group is added as a flag in the subsequent call to the actual service.

**🎯 Example 2**: In `src/FeatureToggles/config/userPermissions.ts` users are matched like so:

```TypeScript
export const userPermissions: Record<string, UserGroup> = {
  'erroruser@company.com': 'error',
  'legacyuser@company.com': 'legacy',
  'betauser@company.com': 'beta',
  'standarduser@company.com': 'standard',
  'devuser@company.com': 'dev',
  'devnewfeatureuser@company.com': 'devNewFeature',
  'qauser@company.com': 'qa'
};
```

For more full-fledged implementations, consider tools like [Oso](https://www.osohq.com) to authorize on the overall "access level". [See for example this demo](https://www.osohq.com/post/add-authorization-to-a-serverless-nodejs-app) using a similar serverless stack, or the plain [quick start version](https://docs.osohq.com/node/getting-started/quickstart.html).

### 🗺️ Lifecycle management and roadmap

We can follow the convention where we divide an API's lifecycle into design, lifetime, sunset, and deprecation phases (see resources below). Beyond these, we add the notion of being "removed" which is the point at which a feature has been completely purged from the source code.

**🎯 Example**: We'd keep a roadmap like the below in our docs. Imagine that today is 25 November 2021 and see below what our code base represents:

| Feature                                   | Group/Flag            | Beta start      | Stable          | Sunset         | Deprecated       | Removed         |
| ----------------------------------------- | --------------------- | --------------- | --------------- | -------------- | ---------------- | --------------- |
| Use hardcoded response                    | N/A                   | 1 August 2021   | 8 August 2021   | 1 October 2021 | 15 November 2021 | 15 January 2022 |
| Use RandomUser instead of JSONPlaceholder | Group `devNewFeature` | 1 November 2021 | 15 January 2022 | N/A            | N/A              | N/A             |

Refer to the pattern [Aggressive Obsolescence](https://microservice-api-patterns.org/patterns/evolution/AggressiveObsolescence.html) and articles by [Nordic APIs](https://nordicapis.com/how-to-smartly-sunset-and-deprecate-apis/) and [Stoplight](https://blog.stoplight.io/deprecating-api-endpoints).

### 🦺 Canary deployment

The traditional way to deploy software is as one big block that becomes instantly activated whenever it's deployed to a machine. If the code was a complete bomb, then you have zero time to verify this. This notion is what makes managers ask for counter-intuitive things like code freeze and all-hands-on-deck deployments. Let's forever end those days!

In `serverless.yml` at line ~85, you'll see `type: AllAtOnce`. This means that we get a classic "deploy === release" pattern. When the deployment is done, the function version is active, with a clear cut-off between the previous and the current (new) version.

There are considerations and problems with this approach. In our AWS circumstances, running on Lambda, we won't face downtime while the instance switches over, and a half-good PaaS solution won't create massive headaches either. However, we should primarily concern ourselves by considering application-level issues, rather than low-level technical issues. So when that "all at once" deployment is done, if something problematic surfaces that was not caught by testing, there is no obvious way how to proceed. Rollback? Roll forward? Hotfix, yay or nay? Sarcastic note: It's hardly unheard of that even manual testing and code freezes slip up.

Let's be honest: **Shit happens anyway**.

![Words to live by, as told by Mike Tyson](/images/tyson.jpg)

Using a [canary release](https://martinfowler.com/bliki/CanaryRelease.html) (formally in AWS CodeDeploy: "[blue/green](https://martinfowler.com/bliki/BlueGreenDeployment.html)") is one way to get those [unknown unknown effects](https://en.wikipedia.org/wiki/Cynefin_framework) happening with real production traffic in a safe and controlled manner. This is where [testing-in-production](https://increment.com/testing/i-test-in-production/) really kicks in—trying to answer questions no staging environment or typical test can address. Instead of being overly defensive, let's simply embrace the uncertainty, as it's already there anyway.

**🎯 Example**: Setting the line to `type: Canary10Percent5Minutes` makes the deployment happen through AWS CodeDeploy in a more controlled, bi-directed fashion: 90% of the traffic will pass to whatever function version that was already deployed and active, while the remaining 10% of traffic will be directed to the "canary" version of the function. The alarm configuration (defined on lines 75-83) looks for a static value of 3 or more errors on the function (I assume all versions here?) during the last 60-second window. After 5 minutes, given nothing has fired the alarm, then the new version takes all of the traffic.

You can either manually send "error traffic" with the `erroruser@company.com` Authorization header, or use the AWS CLI to manually toggle the alarm state. See [AWS docs for how to set the alarm state](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudwatch/set-alarm-state.html), similar to:

```bash
aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM --state-reason "testing purposes"
```

_Tip: Use the above with the `OK` state value to reset the alarm when done._

This specific solution is rudimentary, but indicative enough for how a canary solution might begin to look.

_See this article at [Google Cloud Platform for more information on deployment and test strategies](https://cloud.google.com/architecture/application-deployment-and-testing-strategies)._
