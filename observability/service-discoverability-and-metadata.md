# 🗂 Service discoverability and metadata

[Service discovery](https://stackoverflow.com/questions/37148836/what-is-service-discovery-and-why-do-you-need-it) seems to be a very hot topic among the Kubernetes crowd. With serverless FaaS like we are using here (AWS Lambda), that's not really an interesting discussion. However, discoverability is not just a technical question—it's also something that absolutely relates to the social, organic, and management layers.

At some point that _single_ function will be one service, which will soon maybe become hundreds of services and then thousands of functions. **How to keep track of them?** To some extent, it's possible to get a high-level picture inside of AWS or by being a CLI crusader. Unfortunately, if you are not, then there is no option unless you buy, build, or adapt some open-source solution.

{% hint style="info" %}

One such open-source solution is Spotify's [Backstage](https://backstage.io) which offers a broad set of ideas—no wonder since it's labeled as "an open platform for building developer portals". It's a bit heavy, but some pretty big players are starting to use it.

For a super-lightweight AWS-based and flexible solution, you might want to consider [catalogist](https://github.com/mikaelvesavuori/catalogist) written by yours truly.

{% endhint %}

For a lighter-weight system, I'd think the reasonable data to pull in would be some of the metadata and processes and output we have generated and written previously. In a corporate context, we'd want to ensure the source of truth of the state of our systems/applications resides with the codebase (not in, say, Sharepoint or Confluence or similar), but that a fresh representation is always available in some less developer-oriented context, say on an internally accessible website.

**🎯 Example**: See `manifest.json` for a napkin sketch of how one could work with service metadata. Of course, this is not a functional solution, as you'd need something to extract that data and store it somewhere meaningful.
