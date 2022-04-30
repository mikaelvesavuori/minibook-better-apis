# Beta functionality

We have built-in a "beta functionality" concept that is propagated from toggles into our services. This is a catch-all for new features that we want to test, and which may not yet be ready for wider release. This means that our services need to have distinct checks for this.

As you can see further below in the feature toggles section, we can also use user groups to segment features, as well as classic, individual toggles that can be used on a per-user basis.

What gives? Aren't these beta features just like other toggles? **Yes**. In this project, we define ”beta features” as a _feature-level, wide bucket across user groups_, while user grouping is a dynamically wide bucket (basically just audience segmentation). This way, we can define granularly both on user groups and on beta usage.

**🎯 Example**: See for example `src/FakeUser/usecases/createFakeUser.ts` that uses the provided toggles when calling the User entity which dynamically creates different types of users from this toggle.
