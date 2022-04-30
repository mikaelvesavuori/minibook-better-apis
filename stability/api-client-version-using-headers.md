# 🗂️ API client version using headers

The first way to define expectations is to use API versioning. While there are several ways to do this—for example, refer to [this article from Nordic APIs](https://nordicapis.com/everything-you-need-to-know-about-api-versioning/)—we are going to use a header to define which API version to use.

**🎯 Example**: See `src/FakeUser/controllers/FakeUserController.ts` and note how the header is handled in `checkInput()`.

Note: An alternative, perhaps more commonly used, approach would be to deploy a new instance of the API (like `api.com/v2`), but of course this would create further hardware segregation, which we want to avoid.

So, while it may be non-standard, in this context version 2 of the API represents the beta, meaning that version 1 represents the current (or "stable", "old") variant.

With this, we have created a way to define output simply through a header, without resorting to separate codebases or separate deployments.

_[GraphQL is a different beast when it comes to versioning](https://graphql.org/learn/best-practices/#versioning). See this section as REST-specific in its details, but a good idea overall as long as you adapt to what versioning means for your protocol._
