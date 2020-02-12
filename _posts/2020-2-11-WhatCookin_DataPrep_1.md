---
layout: post
title: What's Cooking - Data Exploration Part 1
---

While *super* late to the game, the kaggle competition [What's Cooking](https://www.kaggle.com/c/whats-cooking-kernels-only/data) sounded too fun to miss. 
The premise is simple - can you design a model to predict a recipe's cuisine type based on the
ingredients present in the recipe?  Intuitively, most people would guess that buttermilk + grits +
collard greens would be southern American cuisine - but teaching a model that could 
be tricky - there are 6714 unique ingredients in the dataset, with almost 11 ingredients on average per recipe! I'll be focusing on the data prep and cleaning in this post, see the
[full repo here!](https://github.com/tbwhite2/WhatCookin)



For the data cleaning process, I'm going to use R - this could be done in python - but I really like 
the flexibility and speed of the [data.table](https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html) package.  After reading this data into R, you'll notice
the data has the ingredients stored as a vector - which is a pretty inaccessible form for ML.  By using the few lines below you can split each ingredient out, making a long version of the data.

{% highlight r %}
train_data = train_data[,lapply(.SD,function(x){unlist(x)}),
                        .SDcols = 'ingredients',
                        by = .(id, cuisine)]
{% endhighlight %}

Once the data is in the long shape, its much more simple to work with.  One of the first things I noticed about this dataset is that the cuisine type classifications are very imbalanced. (See plot below) - 

![_config.yml](/images/cusine_freq_plot.png)