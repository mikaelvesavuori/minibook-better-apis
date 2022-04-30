---
description: TODO
---

# 🆕 Beta functionality

We have built-in a "beta functionality" concept that is propagated from toggles into our services. This is a catch-all for new features that we want to test, and which may not yet be ready for wider release. This means that our services need to have distinct checks for this.

As you can see further below in the feature toggles section, we can also use user groups to segment features, as well as classic, individual toggles that can be used on a per-user basis.

What gives? Aren't these beta features just like other toggles? **Yes**. In this project, we define ”beta features” as a _feature-level, wide bucket across user groups_, while user grouping is a dynamically wide bucket (basically just audience segmentation). This way, we can define granularly both on user groups and on beta usage.

**🎯 Example**: See for example [`src/FakeUser/usecases/createFakeUser.ts`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/src/FakeUser/usecases/createFakeUser.ts) that uses the provided toggles when calling the User entity which dynamically creates different types of users from this toggle.

{% code title="src/FakeUser/usecases/createFakeUser.ts" %}

```typescript
import { UserData, UserDataExtended, User } from "../entities/User";

import { getData } from "./interactors/getData";
import { getImage } from "./interactors/getImage";

/**
 * @description This is where we orchestrate the work needed to fulfill our use case "create a fake user".
 */
export async function createFakeUser(
  toggles: Record<string, unknown>
): Promise<UserData | UserDataExtended> {
  // Use of catAPI is same in all cases
  const user = new User(toggles.enableBetaFeatures as boolean | false);
  const imageResponse = await getImage("catAPI");
  user.applyUserImageFromCatApi(imageResponse);

  // Use code branching for new development feature
  if (toggles.enableNewUserApi) {
    const dataResponse = await getData("RandomUser");
    user.applyUserDataFromRandomUser(dataResponse);
  }
  // Else return regular response
  else {
    const dataResponse = await getData("JSONPlaceholder");
    user.applyUserDataFromJsonPlaceholder(dataResponse);
  }

  return user.viewUserData();
}
```

{% endcode %}
