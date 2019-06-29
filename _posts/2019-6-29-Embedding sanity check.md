---
layout: post
title: Embedding sanity check
subtitle: Big fixes done!
gh-repo: franklee26/QEmbed
gh-badge: [star, fork, follow]
tags: [QEmbed, Quantum Computing]
---

{: .box-warning}
**Warning:** Code listed here is not finalised and is updated as of June 29 2019.

A lot of the work done here is based on Goodrich's et al. paper *Optimizing Adiabatic Quantum Program Compilation using a Graph-Theoretic Framework*. In particular, they provide the following problem graph embedding process as an example:

![paper](https://i.imgur.com/NZbX3Ne.png)

I've recently implemented a complete embedding implementation for QEmbed. More specifically, you can now get a full Chimera embedding provided that you input a problem graph. 

The code is rather messy and I need to work on the syntax and mechanics of it all. However, what is doing is simple and compartmentalised. Take a look:

{% highlight python linenos %}
# import QEmbed
import QEmbed.Embed as qe 

# input problem graph
e = qe.Embedding(graph = [(1,2),(1,5),(1,3),(2,4),(2,5),(3,5),(3,4)])

# plot problem graph
e.plotGraph()

# get left, right bipartite set as well as the OCT set
L,R,OCT = e.greedyBipartiteSets(getOCT = True)

# get the virtual hardware embedding as well as the Chimera topology dimensions 
newL, newR, LL, MM, NN = e.OCTEmbed(left = L, right = R, oct = OCT, getChimeraDimensions = True)

# plot the virtual hardware
e.plotBipartite(left = newL, right = newR)

# plot the Chimera graph
e.plotChimeraFromBipartite(left= newL, right = newR, showMappings = False, L = LL, M = MM, N = NN, isBipartite = False)
{% endhighlight %}

Here's what the problem graph looks like:

![Problem graph example](https://i.imgur.com/qNym2Wn.png)

and the virtual hardware:

![Virtual hardware](https://i.imgur.com/QOCvJ87.png)

and finally the embedded Chimera graph:

![Chimera graph](https://i.imgur.com/eS82YGp.png)