Recall that one important part of [[Query Optimization|query optimization]] is cardinality estimation. Before, we relied on the [[Query Optimization#Selinger Selectivity|Selinger selectivity]] model, which uses an uniformity assumption.

## Single-Table Cardinality
### Histograms

However, a better method might be to construct an equal-width histogram for your data frequency. Then, within each bin, you can assume uniformity for more fine-grained estimation.

A better method might be equal-depth histograms, where bin widths are different but every bin has the same density. This avoids issues if, e.g. your smallest bin is 0 to 5k but you have a *ton* of zero values (since it is a default), and you would vastly underestimate the number of 0s using a uniformity assumption. This is more expensive to build and maintain, however.

Perhaps the best approach is to use a histogram and combine it with a most common values (MCV) table.

### Correlated Columns

So far, we've just used the independence assumption, but this could lead to large errors.

One solution is a 2D binned histogram combined with a MCV table. However, any more dimensions and this becomes computationally intractable. With a [[probabilistic graphical model]], you could reduce only to joint distributions between pairs of vertices and hopefully keep dimensions low, since you just need a 1D histogram per vertex and a 2D histogram per edge.

### Complicated Filters

For example, substring search, complex math expressions, or user-defined functions. In this case you can't do much. One way is to just pick a random constant (e.g. 7%). Or you can run on a small sample you keep in memory, but this might not be accurate.

## Join Cardinalities

### Uniformity Assumption

We can assume that all join keys are uniformly distributed. You just take the product of the number of records in each table, and then divide it by the max of the number of distinct values for each side.

### Joining Histograms

If you have the same bins, you only assume uniformity inside each bin. Allows you to multiple the sizes of the bins then divide by the width of the bin.





