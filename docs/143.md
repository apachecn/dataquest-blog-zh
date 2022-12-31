# 教程:Python 中的 K 近邻

> 原文：<https://www.dataquest.io/blog/k-nearest-neighbors-in-python/>

July 27, 2015![nba-basketball-python-knn-tutorial-k-nearest-neighbors](img/19b194f4b1166269f42f2dabcf75c4e7.png)

在本帖中，我们将使用 K 近邻算法来预测 2013-2014 赛季 NBA 球员的得分。一路上，我们将了解欧几里德距离，并找出哪些 NBA 球员与勒布朗詹姆斯最相似。如果您想跟进，您可以在这里获取 csv 格式的数据集[。](https://www.dropbox.com/s/b3nv38jjo5dxcl6/nba_2013.csv?dl=0)

## 看一看数据

在我们深入研究算法之前，让我们看一下我们的数据。数据中的每一行都包含一名球员在 2013-2014 赛季的表现信息。

以下是从数据中选择的一些列:

*   `player` —玩家的名字
*   `pos` —玩家的位置
*   `g` —玩家参与的游戏数量
*   `gs` —玩家开始的游戏次数
*   `pts` —玩家的总得分

数据中有更多的列，主要包含赛季中平均玩家游戏表现的信息。其余的解释见[本网站](https://www.databasebasketball.com/about/aboutstats.htm)。

我们可以读入数据集，并确定存在哪些列:

```
 import pandas
with open("nba_2013.csv", 'r') as csvfile:
    nba = pandas.read_csv(csvfile)
print(nba.columns.values) # The names of all the columns in the data. 
```

```
['player' 'pos' 'age' 'bref_team_id' 'g' 'gs' 'mp' 'fg' 'fga' 'fg.' 'x3p' 'x3pa' 'x3p.' 'x2p' 'x2pa' 'x2p.' 'efg.' 'ft' 'fta' 'ft.' 'orb' 'drb' 'trb' 'ast' 'stl' 'blk' 'tov' 'pf' 'pts' 'season' 'season_end']
```

## KNN 概述

k-最近邻算法基于通过将未知值与最相似的已知值进行匹配来预测未知值的简单思想。

假设我们有三种不同类型的汽车。我们知道车的名字，马力，是否有赛车条纹，是否很快。：

```
car,horsepower,racing_stripes,is_fast
Honda Accord,180,False,False
Yugo,500,True,True
Delorean DMC-12,200,True,True
```

假设我们现在有另一辆车，但我们不知道它有多快:

```
car,horsepower,racing_stripes,is_fast
Chevrolet Camaro,400,True,Unknown
```

我们想弄清楚这车到底快不快。为了预测它是否与 k 最近邻，我们首先找到最相似的已知汽车。在这种情况下，我们将比较`horsepower`和`racing_stripes`的值，以找到最相似的汽车，即`Yugo`。由于 Yugo 很快，我们可以预测 Camaro 也很快。这是一个 1-最近邻的例子，我们只查看了最相似的汽车，给出了 1 的 k 值。

如果我们执行 2-最近邻，我们将得到 2 个`True`值(对于 Delorean 和 Yugo ),平均为`True`。Delorean 和 Yugo 是最相似的两辆车，我们的 k 值为 2。

如果我们做 3 个最近邻，我们将得到 2 个`True`值和一个`False`值，平均到`True`。

我们用于 k-最近邻的邻居数量(k)可以是小于数据集中行数的任何值。在实践中，只查看几个邻居会使算法执行得更好，因为邻居与我们的数据越不相似，预测就越差。

## 欧几里得距离

在使用 KNN 进行预测之前，我们需要找到一些方法来计算出哪些数据行“最接近”我们试图预测的行。

一个简单的方法是使用欧几里德距离。公式是\(\sqrt{(q_1-p_1)^2+(q_2-p_2)^2+\ cdots+(q_n-p_n)^2}\)

假设我们有这两行(True/False 已转换为 1/0)，我们希望找到它们之间的距离:

```
car,horsepower,is_fast
Honda Accord,180,0
Chevrolet Camaro,400,1
```

我们将首先只选择数字列。那么距离就变成了\(\sqrt{(180-400)^2 + (0-1)^2}\)，大约等于`220`。

我们可以利用欧几里德距离原理，找出与[勒布朗詹姆斯](https://en.wikipedia.org/wiki/LeBron_James)最相似的 NBA 球员。

```
 # Select Lebron James from our dataset
selected_player = nba[nba["player"] == "LeBron James"].iloc[0]

# Choose only the numeric columns (we'll use these to compute euclidean distance)
distance_columns = ['age', 'g', 'gs', 'mp', 'fg', 'fga', 'fg.', 'x3p', 'x3pa', 'x3p.', 'x2p', 'x2pa', 'x2p.', 'efg.', 'ft', 'fta', 'ft.', 'orb', 'drb', 'trb', 'ast', 'stl', 'blk', 'tov', 'pf', 'pts']

def euclidean_distance(row):
    """
    A simple euclidean distance function
    """
    inner_value = 0
    for k in distance_columns:
        inner_value += (row[k] - selected_player[k]) ** 2
    return math.sqrt(inner_value)

# Find the distance from each player in the dataset to lebron.
lebron_distance = nba.apply(euclidean_distance, axis=1)
```

## 规范化列

您可能已经注意到，汽车示例中的`horsepower`对最终距离的影响比`racing_stripes`大得多。这是因为`horsepower`值的绝对值要大得多，从而使`racing_stripes`值在欧几里德距离计算中的影响相形见绌。

这可能不好，因为具有较大值的变量不一定能更好地预测哪些行是相似的。

处理这个问题的一个简单方法是将所有列标准化，使其平均值为 0，标准差为 1。这将确保没有单个列对欧几里德距离计算有显著影响。

要将平均值设置为 0，我们必须找到一列的平均值，然后从该列的每个值中减去该平均值。为了将标准差设置为 1，我们将列中的每个值除以标准差。公式为\(x=\frac{x-\mu}{\sigma}\)。

```
 # Select only the numeric columns from the NBA dataset
nba_numeric = nba[distance_columns]
# Normalize all of the numeric columns
nba_normalized = (nba_numeric - nba_numeric.mean()) / nba_numeric.std() 
```

## 寻找最近的邻居

我们现在已经知道了足够的信息，可以在 NBA 数据集中找到给定行的最近邻。我们可以使用`scipy.spatial`中的`distance.euclidean`函数，这是一种计算欧几里德距离更快的方法。

```
 from scipy.spatial import distance

# Fill in NA values in nba_normalized
nba_normalized.fillna(0, inplace=True)

# Find the normalized vector for lebron james.
lebron_normalized = nba_normalized[nba["player"] == "LeBron James"]

# Find the distance between lebron james and everyone else.
euclidean_distances = nba_normalized.apply(lambda row: distance.euclidean(row, lebron_normalized), axis=1)

# Create a new dataframe with distances.
distance_frame = pandas.DataFrame(data={"dist": euclidean_distances, "idx": euclidean_distances.index})
distance_frame.sort("dist", inplace=True)
# Find the most similar player to lebron (the lowest distance to lebron is lebron, the second smallest is the most similar non-lebron player)
second_smallest = distance_frame.iloc[1]["idx"]
most_similar_to_lebron = nba.loc[int(second_smallest)]["player"] 
```

## 生成训练集和测试集

既然我们知道如何找到最近的邻居，我们可以在测试集上进行预测。我们将尝试使用`5`最近的邻居来预测一个玩家得了多少分。我们将通过使用数据集中的所有数字列生成相似性得分来查找邻居。

首先，我们必须生成测试和训练集。为了做到这一点，我们将使用随机抽样。我们将随机打乱`nba`数据帧的索引，然后使用随机打乱的值选择行。

如果我们不这样做，我们最终会在相同的数据集上进行预测和训练，这将会过度拟合。我们也可以做交叉验证，这样会稍微好一点，但是稍微复杂一点。

```
 import random
from numpy.random import permutation

# Randomly shuffle the index of nba.
random_indices = permutation(nba.index)

# Set a cutoff for how many items we want in the test set (in this case 1/3 of the items)
test_cutoff = math.floor(len(nba)/3)

# Generate the test set by taking the first 1/3 of the randomly shuffled indices.
test = nba.loc[random_indices[1:test_cutoff]]

# Generate the train set with the rest of the data.
train = nba.loc[random_indices[test_cutoff:]]
```

## 使用 sklearn 查找 k 个最近邻

我们可以使用 scikit-learn 中的 k-nearest neighbors 实现，而不必自己去做。这是文件。有一个回归变量和一个分类器可用，但我们将使用回归变量，因为我们有连续的值要预测。

Sklearn 自动执行标准化和距离查找，并让我们指定想要查看多少个邻居。

```
 # The columns that we will be making predictions with.
x_columns = ['age', 'g', 'gs', 'mp', 'fg', 'fga', 'fg.', 'x3p', 'x3pa', 'x3p.', 'x2p', 'x2pa', 'x2p.', 'efg.', 'ft', 'fta', 'ft.', 'orb', 'drb', 'trb', 'ast', 'stl', 'blk', 'tov', 'pf']
# The column that we want to predict.
y_column = ["pts"]

from sklearn.neighbors import KNeighborsRegressor
# Create the knn model.
# Look at the five closest neighbors.
knn = KNeighborsRegressor(n_neighbors=5)
# Fit the model on the training data.
knn.fit(train[x_columns], train[y_column])
# Make point predictions on the test set using the fit model.
predictions = knn.predict(test[x_columns])
```

## 计算误差

现在我们知道了我们的点预测，我们可以计算我们预测的误差。我们可以计算出[均方误差](https://en.wikipedia.org/wiki/Mean_squared_error)。公式是\(\frac{1}{n}\sum_{i=1}^{n}(\hat{y_{i}}-y_{i})^{2}\).

```
 # Get the actual values for the test set.
actual = test[y_column]

# Compute the mean squared error of our predictions.
mse = (((predictions - actual) ** 2).sum()) / len(predictions)
```

## 后续步骤

有关 k 近邻的更多信息，您可以查看我们的六部分交互式[机器学习基础课程](https://www.dataquest.io/course/machine-learning-fundamentals)，该课程使用 k 近邻算法教授机器学习的基础知识。

## 用正确的方法学习 Python。

从第一天开始，就在你的浏览器窗口中通过编写 Python 代码来学习 Python。这是学习 Python 的最佳方式——亲自看看我们 60 多门免费课程中的一门。

![astronaut floating over code](img/8cbc4821ae1245a9fd02da67c90ed420.png)

[尝试 Dataquest](https://app.dataquest.io/signup)