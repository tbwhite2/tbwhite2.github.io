---
  layout: post
title: What's Cooking - Data Exploration Part 1
---

While *super* late to the game, the kaggle competition [What's Cooking]() sounded too fun to miss. 
The premise is simple - can you design a model to predict a recipe's cuisine type based on the
ingredients present in the recipe?  Intuitively, most people would guess that buttermilk + grits +
collard greens would be southern American cuisine - but teaching a machine learning model that could 
be tricky as there are [] unique ingredients in the dataset, with the average number of ingredients 
per recipe of [].  I'll be focusing on the data prep and cleaning in this post, see the
[full repo here!](https://github.com/tbwhite2/WhatCookin)

{% highlight r %}
apple = 2 + 2
{% endhighlight %}

One of the first things you'll notice about this dataset is that the cuisine type classifications are very imbalanced.

![cusine_freq_plot](https://github.com/tbwhite2/WhatCookin/blob/master/plots/cuisine_freq_plot.png)

