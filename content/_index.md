---
title: Intro
type: docs
mermaid_theme: neo
---

In my opinion, we can view data storage systems through many lenses.
We can explore it by analyzing the underlying data structures and their 
interactions to store, manage and retrieve data and metadata.
We can also explore at as a I/O stack consisting of different sub-systems or 
layers and the interfaces through which these sub-systems interact.
We can also take a hybrid approach. 
I'll follow a hybrid approach. 

## 4 Degrees of Data Storage Systems

{{<mermaid>}}
%%{init:{"theme":"neo"}}%%
mindmap
((Data Storage))
  Components
  Communication
  Data Structure
  Request Processing
{{</mermaid>}}

Don't discard the above categorization because you don't see performance anywhere. In my opinion, performance is a cross-cutting issue present in each degree separately and across the whole system.


## Things to remember

1. We always need metadata to be manage data efficiently. Sometimes, metadata can be larger than data itself! 
2. The same problem can be solved in different ways. Each design choice has some trade-offs. There are multiple theorems which talks about this in the context of data storage and databases - RUM conjecture, CAP theorem. Interestingly, there are also works trying to achieve the optimal data structure design.
3. Optimization efforts can focus on optimizing a layer in the storage stack(e.g., FTL design in SSD, index design for database), or streamlining the storage stack (e.g., SNIA key-value stack), or bypassing one or more layers completely (e.g., SPDK, SNIA key-value stack). 
4. When optimizing a layer, make it efficient without adding overheads in other layers. Look at the KV-SSD experience. The host-side storage stack was streamlined, but many jobs were now delegated to the SSD, without adding compute power. Some computational SSD designs do this delegation by adding special hardware inside the storage device. However, it is still not clear if such devices provided good end-to-end benefits.
5. Similar to 4, when you are analyzing a new solution, think in terms of the whole stack. Is the problem solved by making a particular layer efficient, or by delegating to another layer? Does I/O performance improves end-to-end?
6. A lot of times, the cool new feature that solves a problem is not available commercially or widely enough in most systems. So the problem it solves is still out there and needs to be solved in a different way. If you design a solution that is for the systems without this cool new feature, be extra careful to convey this information, especially when writing an academic paper. Systems people are highly sensitive about this, in my experience. I once got a review that a problem we identified regarding persistent memory devices has been solved by the introduction of eADR feature in Intel Optane, and thus the problem we identified is no longer relevant. However, I have seen a number of high-end machines using Intel Optane persistent memory devices over the years, and yet to encounter a system that supports the eADR feature. Hence, the problem we identified is still valid for most systems using Intel Optane, except for the device has been discontinued by Intel!
7. Different layers in the storage stack employ their own cache. Hence, the entire stack tends to have a multi-layer caching system, which does not interact with each other directly. This can incur additional data copy and overheads. Some optimization techniques also bypass certain caches to avoid these overheads, and may use their own cache.