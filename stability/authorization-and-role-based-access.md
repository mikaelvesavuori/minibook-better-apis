# Authorization and role-based access

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
