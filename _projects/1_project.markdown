---
layout: page
title: SDLHC
description: Segmentation - Dictionary - Levenshten Hierachical Clustering
img: /assets/img/instanceSegmentedTimeSerie.png
importance: 1
category: work
---

Time series unsupervised classification, or "clustering", designates a set of methods that partition a dataset into groups of "similar" temporal observations, without ground truth supervision.

In many real-life use cases, time series are composed of several phases, where each phase is temporal state that can be represented with a different model. If the dataset is composed of equal-length time series and if they share the same phase transition cutpoints, it is be possible to estimate global cutpoints on the whole dataset. This model assumption is used by the method [1].

However, in many real-life situations, time series length are unequal (For instance, when comparing flight records, or race results, or any events with possibly varying durations), and/or the transitions cutpoints vary from one time series to the other.

We propose an unsupervised classification of univariate time series, that addresses the case of a latent scenario construction (Fig.1), for unequal length time series and without hypothesis on transition cutpoint synchronicity.

<div class="row align-items-center">
    <div class="col-sm-1">
    </div>
    <div class="col-sm-9 mt-3 mt-md-2">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/scenarioHypothesis.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Figure 1: An instance of time series constructed with a scenario.
</div>

The method is composed of three parts: 
1) The (independent) segmentation of each time series; 
2) A clustering of the obtained segments produces a dictionary of patterns. Each segment can then be associated to a pattern, and the time series are recoded as chains of symbols; 
3) A weighted hierarchical clustering based on the Levenshtein distance produces the final clustering. The complete process is described on Fig.2.

<div class="row">
    <div class="col-sm-1">
    </div>
    <div class="col-sm-9 mt-3 mt-md-2">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/workflowSegmentation.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Figure 2: Illustration of the three-steps workflow.
</div>


The segmentation step is performed with a piecewise polynomial regression model [1] and a model selection (number of segments, polynomial complexity) using the BIC score [2].

The dictionary construction step is a Multivariate Gaussian Mixture Model on the polynomial coefficients associated to each segments, and (optionally) the mean, variance, and segment duration information.

In the final step, the symbol chains are regrouped with a hierarchical clustering and the Levenshtein distance. This distance measures the number of editions that need to be performed to transform one chain of symbols into another.

In our case, we add weights to the Levenshtein Distance, computed on the basis of the similarity between dictionary pattern segments. This weightage system improves the final clustering accuracy byt making use of the dictionary pattern proximity information. In addition, this weightage may reduce the impact of a potential dictionary size overestimation by associating similar or duplicate patterns together.

The associated paper is available in paper [3] (c.f. publication section for pdf), and the Scala / R open source code on github repository https://github.com/EtienneGof/SDLHC.

[1] Samé, A., Chamroukhi, F., Govaert, Gérard and Aknin, P.. Model-based clustering and segmentation of time series with changes in regime. Advances in Data Analysis and Classification Springer Berlin / Heidelberg., Vol. 5, pages:301–321, 2011

[2] Schwarz, G. (1978). Estimating the dimension of a model. The annals of statistics, 461-464.

[3] Goffinet, E., Lebbah, M., Azzag, H., & Giraldi, L. (2020). Autonomous Driving Validation with Model-Based Dictionary Clustering. In ECML/PKDD (4) (pp. 323-338).


