---
description: TODO
---

# 📝 Release versioned software and produce release notes

Each release should be uniquely versioned. We should also keep release notes, but it's one of those things that can be easy to miss.

...I really wish this was something many enterprise teams did, but I find it to be less common than in the open source or product development communities. Kind of makes sense, but this needn't be the case.

So, how do we make it easy "to do the right thing"? We'll add a tool to help us!

**🎯 Example**: Instead of writing manual release notes, we use [Standard Version](https://github.com/conventional-changelog/standard-version) to output them for us from our commits into the `CHANGELOG.md` file. Practically, this works by running `npm run release`.

This should be easy and powerful enough to help set an example in this area.
