---
description: TODO
---

# 🏁 Feature toggles ("feature flags")

The code part of this book uses a handcrafted, simple feature flags engine and a small toggle configuration.

While a technically trivial solution, this enables a dynamic configuration that can be detached from the source code itself. [In effect, this means we are one big step closer to separating a (technical) deployment from a (business-oriented) release.](https://medium.com/turbine-labs/deploy-not-equal-release-part-two-acbfe402a91c)

The feature function is located at [`src/FeatureToggles`](https://github.com/mikaelvesavuori/better-apis-workshop/tree/main/src/FeatureToggles). For the configuration part, we should put it on [Mockachino](https://www.mockachino.com) so that we can approximate a more conventional feature toggles service, getting (and updating) flags without having to re-deploy our code. The set of feature toggles is ordered under each respective user group.

Unknown (or missing, non-existing, null...) users get the `standard` user group access.

{% code style="info" %}

Note that different feature toggle services or implementations may differ in how they exactly apply toggles, and how they work. In our case here, we apply group-level toggles rather than request individual flags, if nothing else than for simplicity of demonstration.

{% endcode %}

**🎯 Example**: The full configuration you can use as your template looks like the one below. Notice how it's segmented on the group level:

{% code title="src/FeatureToggles/config/mock.toggles.json" %}

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

{% endcode %}

You can update the flags as you want, to get the effects you need, without requiring redeploying the code!

{% code style="info" %}

Refer to [FeatureFlags.io](https://featureflags.io) and to [Unleash](https://www.getunleash.io)'s documentation on [activation strategies](https://docs.getunleash.io/user_guide/activation_strategy) for inspiration.

{% endcode %}
