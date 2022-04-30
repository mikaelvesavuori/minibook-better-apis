---
description: TODO
---

# Weighing between a hardware- or a software-oriented approach

## 🖥️ The drawbacks of hardware-segregated environments

- You start expecting environmental parity between any other systems
- You start expecting that all systems are in similar, co-deployed stages
- The implicit reasoning starts becoming that you "should" or "can" only have a low degree of variability in configuration
- As a consequence, such "pre-baked" configuration tests may start becoming large-scale blocking, manual tests
- There may be significant cost overhead with a higher count of static environments
- There is most likely a significant complexity overhead with a higher count of static environments

## 🧑‍💻 The drawbacks of a software-defined, dynamic environment

- Can add complexity
- Will become harder to work with in the local scope (i.e. the actual code), and more so if there are many branches
- Requires some degree of cleanliness and pruning (governance even) to control, so things don't grow out of hand

## 🎨 Yes, you can mix these patterns with a hardware-separated environment!

You can certainly use the patterns seen in this project in a more "traditional" hardware-separated environment. However, the benefits become more pronounced as you also shed some of the overhead and weight of classical environments.
