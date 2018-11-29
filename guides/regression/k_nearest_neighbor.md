---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
title: Lurn | K Nearest Neighbor Regression
layout: guides
active_dropdown: regression
active_guide: knn_regression
---

<header>
  <h2>K Nearest Neighbor Regression</h2>
</header>

K Nearest Neighbor (KNN) is among the simplest types of regression models to understand and is therefore a great starting
point for anyone new to machine learning. The concept is simple, all of the predictors for a training set are
stored as-is along with their target value (the thing you're trying to predict). To predict the target value
of a new observation you simply find the k training observationos that are most like the new data and average
the target values.

#### How KNN Regression Works

The [Iris Data Set](https://archive.ics.uci.edu/ml/datasets/iris) provides data from a sample of Iris plants. Variables
included are the length and with of each plant's petal and sepal (the leafy supporting structure at the base of the
flower). This is example will attempt to use this data to fill in missing data for a newly observed plant.

The plot below shows the sepal length and width for all plants in the data set.

<iframe id="datawrapper-chart-TX66t" src="//datawrapper.dwcdn.net/TX66t/4/" scrolling="no" frameborder="0" allowtransparency="true" style="width: 0; min-width: 100% !important;" height="500"></iframe><script type="text/javascript">if("undefined"==typeof window.datawrapper)window.datawrapper={};window.datawrapper["TX66t"]={},window.datawrapper["TX66t"].embedDeltas={"100":525,"200":525,"300":500,"400":500,"500":500,"700":500,"800":500,"900":500,"1000":500},window.datawrapper["TX66t"].iframe=document.getElementById("datawrapper-chart-TX66t"),window.datawrapper["TX66t"].iframe.style.height=window.datawrapper["TX66t"].embedDeltas[Math.min(1e3,Math.max(100*Math.floor(window.datawrapper["TX66t"].iframe.offsetWidth/100),100))]+"px",window.addEventListener("message",function(a){if("undefined"!=typeof a.data["datawrapper-height"])for(var b in a.data["datawrapper-height"])if("TX66t"==b)window.datawrapper["TX66t"].iframe.style.height=a.data["datawrapper-height"][b]+"px"});</script>

Using this as a training set, KNN Regression can be applied to predict the petal length of new plants. Suppose a new 
observation is found with a sepal length of 6.1 and sepal width 3.3. To estimate its petal length with knn regression 
(with k = 5) find the 5 training observations that are most similar to the newly observed plant and average their petal 
lengths. The plot below shows the new observation in red with its 5 nearest neighbors highlighted in green. Averaging 
the neighbors’ petal lengths, the estimated value for the new observation would be 
(5.6 + 6.0 + 5.4 + 4.5 + 4.8) / 5 = 5.26.

<iframe id="datawrapper-chart-ex8kB" src="//datawrapper.dwcdn.net/ex8kB/1/" scrolling="no" frameborder="0" allowtransparency="true" style="width: 0; min-width: 100% !important;" height="500"></iframe><script type="text/javascript">if("undefined"==typeof window.datawrapper)window.datawrapper={};window.datawrapper["ex8kB"]={},window.datawrapper["ex8kB"].embedDeltas={"100":575,"200":525,"300":525,"400":500,"500":500,"700":500,"800":500,"900":500,"1000":500},window.datawrapper["ex8kB"].iframe=document.getElementById("datawrapper-chart-ex8kB"),window.datawrapper["ex8kB"].iframe.style.height=window.datawrapper["ex8kB"].embedDeltas[Math.min(1e3,Math.max(100*Math.floor(window.datawrapper["ex8kB"].iframe.offsetWidth/100),100))]+"px",window.addEventListener("message",function(a){if("undefined"!=typeof a.data["datawrapper-height"])for(var b in a.data["datawrapper-height"])if("ex8kB"==b)window.datawrapper["ex8kB"].iframe.style.height=a.data["datawrapper-height"][b]+"px"});</script>

Most real world examples will involve more than two predictors and are more difficult to visualize but the concept is 
the same.

#### Using lurn's KNN Regression
To apply lurn's KNN regression, download the [Iris Data Set](https://archive.ics.uci.edu/ml/datasets/iris). Fire up
a ruby console or Jupyter Notebook and load the data into a Daru data frame.

```ruby
# replace this with the path you downloaded the data to
file_path = "PATH_TO_IRIS_DATA"
headers = ["sepal_length", "sepal_width", "petal_length", "petal_width", "class"]
iris_plants = Daru::DataFrame.from_csv(file_path, headers: headers)
```

Now instantaite a new instance of `Lurn::Neighbors::KNNRegression` with k = 5, and train (fit) the model with the predictors and
target variable.

```ruby
predictors = iris_plants[:sepal_length, :sepal_width].map_rows { |row| row.to_a }
target = iris_plants[:petal_length].to_a

model = Lurn::Neighbors::KNNRegression.new(5)
model.fit(predictors, target)
```

The model is now ready to predict the petal length of new observations. Using the example before, call `predict` on the
model with sepal length of 6.1 and sepal width of 3.3.

```ruby
new_plant = [6.6, 3.1]
model.predict(new_plant)
```