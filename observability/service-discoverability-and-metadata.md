---
description: How to keep track of all of your services?
---

# 🗂 Service discoverability and metadata

[Service discovery](https://stackoverflow.com/questions/37148836/what-is-service-discovery-and-why-do-you-need-it) seems to be a very hot topic among the Kubernetes crowd. With serverless FaaS like we are using here (AWS Lambda), that's not really an interesting discussion. However, discoverability is not just a technical question—it's also something that absolutely relates to the social, organic, and management layers.

At some point that _single_ function will be one service, which will soon maybe become hundreds of services and then thousands of functions. **How to keep track of them?**

To some extent, it's possible to get a high-level picture inside of AWS or by being a CLI crusader. Unfortunately, if you are not, then there is no option unless you buy, build, or adapt some open-source solution.

{% hint style="info" %}
One such open-source solution is Spotify's [Backstage](https://backstage.io) which offers a broad set of ideas—no wonder since it's labeled as "an open platform for building developer portals". It's a bit heavy, but some pretty big players are starting to use it.

For a super-lightweight AWS-based and flexible solution, you might want to consider [catalogist](https://github.com/mikaelvesavuori/catalogist) written by yours truly.
{% endhint %}

For a lighter-weight system, I'd argue that the reasonable data to pull in would be a subset of the metadata (and processes and output) we have generated and written previously. In a corporate context, we'd want to ensure that the source of truth of the state of our systems/applications resides with the codebase—Not in a closed tool like Sharepoint or Confluence or similar.

**🎯 Example**: See [`manifest.json`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/manifest.json) for a napkin sketch of how one could work with service metadata if you had somewhere to send it and store it, during the CI stage. _This format is also very similar to the one used in_ [_catalogist_](https://github.com/mikaelvesavuori/catalogist).

{% code title="manifest.json" %}
```json
{
  "spec": {
    "lifecycle": "production",
    "type": "service",
    "name": "my-project",
    "team": "ThatTeam",
    "responsible": "Someguy Someguyson",
    "system": "something",
    "domain": "bigarea",
    "tags": ["typescript", "backend"],
    "securityClass": "Public",
    "dataSensitivity": "Sensitive",
    "l3ResolverGroup": "ThatTeam",
    "slo": "99.95"
  },
  "api": [
    {
      "FakeUser": "./api/schema.yml"
    }
  ],
  "metadata": {
    "annotations": {
      "sbom": "./outputs/sbom-output.txt",
      "typedoc": "./typedoc-docs/",
      "arkit": "./diagrams/"
    },
    "description": "The place to be, for great artists",
    "generation": 1,
    "labels": {
      "example.com/custom": "custom_label_value"
    },
    "links": [
      {
        "url": "https://admin.example-org.com",
        "title": "Admin Dashboard",
        "icon": "dashboard"
      }
    ]
  }
}
```
{% endcode %}
