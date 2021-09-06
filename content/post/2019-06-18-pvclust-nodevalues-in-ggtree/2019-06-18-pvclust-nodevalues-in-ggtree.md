---
title: "pvclust Node Values In ggtree"
summary: "How to display bootstrap values from pvclust on a phylogenetic tree made in ggtree"
date: 2019-06-18
tags:
  - bioinformatics
  - ggtree
  - pvclust
  - bootstrap_values
---

I recently was using the R package [`pvclust`](http://stat.sys.i.kyoto-u.ac.jp/prog/pvclust/) to test the
'robusticity' of clusters in a microbiome-related clustering analysis. While `pvclust` provides it's own plots via `plot()`  on a `pvclust` object, this plots the dendrogram in  base R. For readability and customisability reasons I prefer using the packages `ggplot2` and `ggtree` for making my figures. However, I was having a hard time to extract the node uncertainty values from the `pvclust` object  and integrate them into a generic R `phylo` object for plotting the dendrogram in ggtree.

Fortunately, a bit of googling (a couple of days...) showed me someone else had already solved the problem of transferring additional `hclust` information into a `phylo` object but in a different context. The `fastbap` package has the function [`as.phylo.hclust.node.attributes()`](https://github.com/gtonkinhill/fastbaps/blob/master/R/as.phylo.hclust.node.attributes.R)` which essentially does what I needed to do - i.e. when calculating the node numbers from each merge event, also store with the same node number the corresponding attribute, or in this case, `pvclust` AU value.

I then modified this function slightly to make it more consistent with how `pvclust` will display the values in the base R plot (rounding and converting to a 'percentage'). Note that this outputs in the `phylo` object the metadata `node.label`, not as `node.attributes` as in the original `fastbaps` function.

So in summary:

```r
## 1. Make pvclust object e.g.
hclust_boot <- pvclust::pvclust(otu_matrix,
                 method.hclust = selected_method,
                 method.dist = "euclidean",
                 nboot = 1000,
                 parallel = T)

## 2. Set modified fastbaps function
as.phylo.pvclust.node.attributes <- function(x, attribute)
{
  N <- dim(x$merge)[1]
  edge <- matrix(0L, 2*N, 2)
  edge.length <- numeric(2*N)
  ## `node' gives the number of the node for the i-th row of x$merge
  node <- integer(N)
  node[N] <- N + 2L
  node.attributes <- rep(NA, N)
  cur.nod <- N + 3L
  j <- 1L
  for (i in N:1) {
    edge[j:(j + 1), 1] <- node[i]
    for (l in 1:2) {
      k <- j + l - 1L
      y <- x$merge[i, l]
      if (y > 0) {
        edge[k, 2] <- node[y] <- cur.nod
        cur.nod <- cur.nod + 1L
        edge.length[k] <- x$height[i] - x$height[y]
        node.attributes[edge[k, 1] - (N + 1)] <- attribute[i]
      } else {
        edge[k, 2] <- -y
        edge.length[k] <- x$height[i]
        node.attributes[edge[k, 1] -  (N + 1)] <- attribute[i]
      }
    }
    j <- j + 2L
  }

  if (is.null(x$labels))
    x$labels <- as.character(1:(N + 1))
  
  ## MODIFICATION: clean up node.attributes so they are in same format in 
  ## pvclust plots
  node.attributes <- as.character(round(node.attributes * 100, 0))
  node.attributes[1] <- NA
  
  obj <- list(edge = edge, edge.length = edge.length / 2,
              tip.label = x$labels, Nnode = N, node.label = node.attributes)
  class(obj) <- "phylo"
  stats::reorder(obj)
}

## 3. Use the modified fastbaps function by accessing the hclust object in first 
## position, and the corresponding au values from the edges list entry.
hclust_boot_phylo <- as.phylo.pvclust.node.attributes(hclust_boot$hclust, 
                                                     hclust_boot$edges$au)

```

To display the values on the tree with `ggtree` you can then run

```r
ggtree(hclust_boot_phylo  aes(x, y)) +
    geom_text2(aes(subset = !isTip, label = label)) 
```

as described in the `ggtree` [FAQ](https://guangchuangyu.github.io/software/ggtree/faq/#) under the section "bootstrap values from newick format".
