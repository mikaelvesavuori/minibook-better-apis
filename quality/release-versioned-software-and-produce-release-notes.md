---
description: I wish the song "It's a Sin" was about releasing unversioned and undocumented software, but alas, it is not.
---

# 📝 Release versioned software and produce release notes

Each release should be uniquely versioned. We should also keep release notes, but it's one of those things that can be easy to miss.

...I really wish this was something more enterprise teams did, but I find it to be less common than in the open-source or product development communities. It kind of makes sense, but this needn't be the case.

So, how do we make it easy "to do the right thing"? We'll add a tool to help us!

**🎯 Example**: Instead of writing manual release notes, we use [Standard Version](https://github.com/conventional-changelog/standard-version) to output them for us from our commits into the [`CHANGELOG.md`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/CHANGELOG.md) file. Practically, this works simply by running `npm run release`.

{% hint style="info"%}

The standard convention we want to use is called [semantic versioning](https://semver.org), which is the format you've probably seen a million times with versions like `1.0.3` and `3.10.2`, representing `MAJOR.MINOR.PATCH` in the version number. Using this tool you get that automatically. For a Node project like the one we are using, you could simply update the number in `package.json` and then regenerate any files (`npm run build` or similar and build the docs) and you're all set prior to publishing on NPM or similar package libraries.

{% endhint %}

This should be easy and powerful enough to help set an example in this area.
