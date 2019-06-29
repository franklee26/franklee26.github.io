---
layout: post
title: Embedding sanity check
subtitle: More big fixes!
gh-repo: franklee26/QEmbed
gh-badge: [star, fork, follow]
tags: [QEmbed, Quantum Computing]
---

{: .box-warning}
**Warning:** Code listed here is not finalised and is updated as of June 29 2019.

A lot of the work done here is based on Goodrich's et al. paper *Optimizing Adiabatic Quantum Program Compilation using a Graph-Theoretic Framework*. In particular, they provide the following problem graph embedding process as an example:

![paper](https://i.imgur.com/NZbX3Ne.png)

This screenshot was taken directly from their paper. There does appear to be a mistake; there should be an edge connecting nodes 4 and 8. 

A good sanity to check if QEmbed is working properly is to simply use their example. Previous versions had trouble with this and it finally looks like a lot of the bugs are taken care of, but of course not all of them.

Take a look at the QEmbed implementation: 

{% highlight python linenos %}
# QEmbed v1.8
# import QEmbed
import QEmbed.Embed as qe 

# copying problem graph from paper
g = [(1,2),(1,3),(1,4),(1,5),(1,6),(1,7),(1,8),(2,3),(2,4),(2,7),(3,4),(3,5),(3,6),(3,7),(3,8),(4,7),(5,8),(5,7),(6,7),(6,8),(4,8)]
e = Embedding(graph = g)

# plot problem graph
e.plotGraph()

# get left, right bipartite set as well as the OCT set
L,R,OCT = e.greedyBipartiteSets(getOCT = True, ensurance = True)

# get the virtual hardware embedding
newL, newR = e.OCTEmbed(left = L, right = R, oct = OCT, getChimeraDimensions = False)

# plot the virtual hardware
e.plotBipartite(left = newL, right = newR)

# plot a 2x2x4 Chimera graph
e.plotChimeraFromBipartite(left= newL, right = newR, showMappings = False, L = 2, M = 2, N = 4, isBipartite = False)
{% endhighlight %}

Here's what the problem graph looks like (it should be the same as the problem graph provided in the paper, but with an extra edge added between 4 and 8):

![problem](https://i.imgur.com/yTUMj7j.png)

and the virtual hardware:

![virtual hardware](https://i.imgur.com/lM2gSIz.png)

and finally the embedded 2x2x4 Chimera graph:

![chimera](https://i.imgur.com/QYrzGrA.png)

We get the same Chimera mappings! Looks like the embedding process is working a lot better.

Notice that there is now a new argument for `greedyBipartiteSets()` called `ensurance`. When this field is flagged true, it simply checks that the computed left and right sets in fact form a bipartite graph. I'm not sure why (or if this is to be expected), but sometimes `greedyBipartiteSets()` does not find bipartite partitions, which is why I added such an option. 

If `ensurance` is set to be true, then the left and right sets will be recomputed at most 101 times until a correct partition is found. This will slow down the process so this will need to be addressed later.

{: .box-error}
**Issue:** The `ensurance` argument can slow down computation by quite a bit and does not necessarily guarantee the correct answer. This method may be deprecated later.

Also, notice that I specified chimera dimensions to be 2x2x4 instead of using `getChimeraDimensions = True` in the `octEmbed()` method. This is because the way QEmbed computed the dimentions came out to be 3x3x3, which works but is not what the paper used.