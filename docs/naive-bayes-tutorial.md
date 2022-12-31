# 教程:用朴素贝叶斯预测电影评论情绪

> 原文：<https://www.dataquest.io/blog/naive-bayes-tutorial/>

March 17, 2015[Sentiment analysis](https://en.wikipedia.org/wiki/Sentiment_analysis) is a field dedicated to extracting subjective emotions and feelings from text. One common use of sentiment analysis is to figure out if a text expresses negative or positive feelings. Written reviews are great datasets for doing sentiment analysis because they often come with a score that can be used to train an algorithm. [Naive Bayes](https://en.wikipedia.org/wiki/Naive_Bayes_classifier) is a popular algorithm for classifying text. Although it is fairly simple, it often performs as well as much more complicated solutions. In this post, we’ll use the naive Bayes algorithm to predict the sentiment of movie reviews. We’ll also do some [natural language processing](https://en.wikipedia.org/wiki/Natural_language_processing) to extract features to train the algorithm from the text of the reviews.

## 在我们分类之前

我们有一个包含电影评论的 csv 文件。数据集中的每一行包含评论的文本，以及评论的语气是否被分类为正面(

`1`)，或者负(`-1`)。我们希望在给定文本的情况下，预测评论是负面的还是正面的。为了做到这一点，我们将使用`train.csv`中的评论和分类来训练一个算法，然后对`test.csv`中的评论进行预测。然后我们将能够使用`test.csv`中的实际分类来计算我们的误差，并看看我们的预测有多好。对于我们的分类算法，我们将使用[朴素贝叶斯](https://en.wikipedia.org/wiki/Naive_Bayes_classifier)。朴素贝叶斯分类器的工作原理是计算出数据的不同属性与某个类相关联的概率。这是基于[贝叶斯定理](https://en.wikipedia.org/wiki/Bayes%27_theorem)。定理为\(P(A \mid B) = \frac{P(B \mid A)，P(A)}{P(B)}\)。这基本上说明了“给定 B 为真的概率等于给定 A 为真的概率乘以 A 为真的概率，除以 B 为真的概率。”让我们做一个快速练习来更好地理解这条规则。

```py
 # Here's a running history for the past week.
# For each day, it contains whether or not the person ran, and whether or not they were tired.
days = [["ran", "was tired"], ["ran", "was not tired"], ["didn't run", "was tired"], ["ran", "was tired"], ["didn't run", "was not tired"], ["ran", "was not tired"], ["ran", "was tired"]]

# Let's say we want to calculate the odds that someone was tired given that they ran, using bayes' theorem.
# This is P(A).
prob_tired = len([d for d in days if d[1] == "was tired"]) / len(days)
# This is P(B).
prob_ran = len([d for d in days if d[0] == "ran"]) / len(days)
# This is P(B|A).
prob_ran_given_tired = len([d for d in days if d[0] == "ran" and d[1] == "was tired"]) / len([d for d in days if d[1] == "was tired"])

# Now we can calculate P(A|B).
prob_tired_given_ran = (prob_ran_given_tired * prob_tired) / prob_ran

print("Probability of being tired given that you ran: {0}".format(prob_tired_given_ran))
```

```py
Probability of being tired given that you ran: 0.6
```

## 朴素贝叶斯简介

让我们尝试一个稍微不同的例子。假设我们还有一个分类——你是否累了。假设我们有两个数据点——你是否跑步，以及你是否早起。贝叶斯定理在这种情况下不适用，因为我们有两个数据点，而不是一个。这就是朴素贝叶斯可以帮忙的地方。朴素贝叶斯通过假设每个数据点都是独立的，扩展了贝叶斯定理来处理这种情况。公式看起来是这样的:\(P(y \mid x_1，\dots，x _ n)= \ frac { p(y)\prod_{i=1}^{n} p(x _ I \ mid y)} { p(x _ 1，\dots，x_n)}\。也就是说“给定特征\(x1 \)、\(x2 \)等，分类 y 正确的概率等于 y 乘以给定 y 的每个 x 特征的乘积，再除以 x 特征的概率”。要找到“正确的”分类，我们只需用公式找出哪个分类\( (P(y \mid x_1，\dots，x_n))\)的概率最高。

```py
 # Here's our data, but with "woke up early" or "didn't wake up early" added.
days = [["ran", "was tired", "woke up early"], ["ran", "was not tired", "didn't wake up early"], ["didn't run", "was tired", "woke up early"], ["ran", "was tired", "didn't wake up early"], ["didn't run", "was tired", "woke up early"], ["ran", "was not tired", "didn't wake up early"], ["ran", "was tired", "woke up early"]]

# We're trying to predict whether or not the person was tired on this day.new_day = ["ran", "didn't wake up early"]

def calc_y_probability(y_label, days):
  return len([d for d in days if d[1] == y_label]) / len(days)

def calc_ran_probability_given_y(ran_label, y_label, days):
  return len([d for d in days if d[1] == y_label and d[0] == ran_label]) / len(days)

def calc_woke_early_probability_given_y(woke_label, y_label, days):
  return len([d for d in days if d[1] == y_label and d[2] == woke_label]) / len(days)

denominator = len([d for d in days if d[0] == new_day[0] and d[2] == new_day[1]]) / len(days)

# Plug all the values into our formula.  Multiply the class (y) probability, and the probability of the x-values occuring given that class.
prob_tired = (calc_y_probability("was tired", days) * calc_ran_probability_given_y(new_day[0], "was tired", days) * calc_woke_early_probability_given_y(new_day[1], "was tired", days)) / denominator

prob_not_tired = (calc_y_probability("was not tired", days) * calc_ran_probability_given_y(new_day[0], "was not tired", days) * calc_woke_early_probability_given_y(new_day[1], "was not tired", days)) / denominator

# Make a classification decision based on the probabilities.
classification = "was tired"

if prob_not_tired > prob_tired:
   classification = "was not tired"

print("Final classification for new day: {0}. Tired probability: {1}. Not tired probability: {2}.".format(classification, prob_tired, prob_not_tired))
```

```py
Final classification for new day: was tired. Tired probability: 0.10204081632653061\. Not tired probability: 0.054421768707482984.
```

## 查找字数

我们试图确定一个数据行应该归类为负数还是正数。正因为如此，我们可以忽略分母。正如您在最后一个代码示例中看到的，它在每个可能的类中都是一个常数，因此对每个概率的影响是相等的，所以它不会改变哪个是最大的(用 5 除以 2 和用 10 除以 2 不会改变第二个数字更大的事实)。因此，我们必须计算每个分类的概率，以及每个特征落入每个分类的概率。在上一个例子中，我们使用了几个独立的特性。这里，我们只有一根长绳子。从文本生成要素的最简单方法是将文本分割成单词。评论中的每个单词都将成为我们可以使用的功能。为了做到这一点，我们将根据空白分割评论。然后，我们将统计每个词在负面评价中出现的次数，以及每个词在正面评价中出现的次数。这将允许我们最终计算属于每个类的新评论的概率。

```py
 # A nice python class that lets you count how many times items occur in a list
from collections import Counter
import csv
import re

# Read in the training data.
with open("train.csv", 'r') as file:
  reviews = list(csv.reader(file))

def get_text(reviews, score):
  # Join together the text in the reviews for a particular tone.
  # We lowercase to avoid "Not" and "not" being seen as different words, for example.
  return " ".join([r[0].lower() for r in reviews if r[1] == str(score)])

def count_text(text):
  # Split text into words based on whitespace.  Simple but effective.
  words = re.split("\s+", text)
  # Count up the occurence of each word.
  return Counter(words)

negative_text = get_text(reviews, -1)
positive_text = get_text(reviews, 1)
# Generate word counts for negative tone.
negative_counts = count_text(negative_text)
# Generate word counts for positive tone.
positive_counts = count_text(positive_text)

print("Negative text sample: {0}".format(negative_text[:100]))
print("Positive text sample: {0}".format(positive_text[:100])) 
```

```py
 Negative text sample: plot : two teen couples go to a church party drink and then drive . they get into an accident . one 
Positive text sample: films adapted from comic books have had plenty of success whether they're about superheroes ( batman
```

## 做出预测

现在我们有了字数，我们只需要将它们转换成概率，然后乘以它们就可以得到预测的分类。假设我们想找出评论

`didn't like it`表示消极的情绪。我们将找到单词`didn't`在负面评论中出现的总次数，并用它除以负面评论中的单词总数，得到给定 y 时 x 的概率。然后我们将对`like`和`it`做同样的事情。我们将所有三个概率相乘，然后乘以任何表达负面情绪的文档的概率，以获得句子表达负面情绪的最终概率。对于积极情绪，我们也会这样做，然后无论哪个概率更大，都将是该评论被分配到的类别。要做到这一点，我们需要计算数据中每个类出现的概率，然后创建一个函数来计算分类。

```py
 import re
from collections import Counter

def get_y_count(score):
  # Compute the count of each classification occuring in the data.
  return len([r for r in reviews if r[1] == str(score)])

# We need these counts to use for smoothing when computing the prediction.
positive_review_count = get_y_count(1)
negative_review_count = get_y_count(-1)

# These are the class probabilities (we saw them in the formula as P(y)).
prob_positive = positive_review_count / len(reviews)
prob_negative = negative_review_count / len(reviews)

def make_class_prediction(text, counts, class_prob, class_count):
  prediction = 1
  text_counts = Counter(re.split("\s+", text))
  for word in text_counts:
      # For every word in the text, we get the number of times that word occured in the reviews for a given class, add 1 to smooth the value, and divide by the total number of words in the class (plus the class_count to also smooth the denominator).
      # Smoothing ensures that we don't multiply the prediction by 0 if the word didn't exist in the training data.
      # We also smooth the denominator counts to keep things even.
      prediction *=  text_counts.get(word) * ((counts.get(word, 0) + 1) / (sum(counts.values()) + class_count))
  # Now we multiply by the probability of the class existing in the documents.
  return prediction * class_prob

# As you can see, we can now generate probabilities for which class a given review is part of.
# The probabilities themselves aren't very useful -- we make our classification decision based on which value is greater.
print("Review: {0}".format(reviews[0][0]))
print("Negative prediction: {0}".format(make_class_prediction(reviews[0][0], negative_counts, prob_negative, negative_review_count)))
print("Positive prediction: {0}".format(make_class_prediction(reviews[0][0], positive_counts, prob_positive, positive_review_count))) 
```

```py
Review: plot : two teen couples go to a church party drink and then drive . they get into an accident . one of the guys dies but his girlfriend continues to see him in her life and has nightmares . what's the deal ? watch the movie and " sorta " find out . . . critique : a mind-fuck movie for the teen generation that touches on a very cool idea but presents it in a very bad package . which is what makes this review an even harder one to write since i generally applaud films which attempt
Negative prediction: 3.0050530362356505e-221
Positive prediction: 1.3071705466906793e-226
```

## 预测测试集

现在我们可以预测了，让我们来预测

`test.csv`。如果你对`train.csv`中的评论进行预测，你会得到误导性的好结果，因为概率是从中产生的(而且，算法对它预测的数据有先验知识)。在训练集上获得好的结果可能意味着你的模型是[过度拟合](https://en.wikipedia.org/wiki/Overfitting)，并且只是拾取随机噪声。只有在模型未被训练的集合上进行测试，才能告诉你它是否正常运行。

```py
 import csv

def make_decision(text, make_class_prediction):
    # Compute the negative and positive probabilities.
    negative_prediction = make_class_prediction(text, negative_counts, prob_negative, negative_review_count)
    positive_prediction = make_class_prediction(text, positive_counts, prob_positive, positive_review_count)

    # We assign a classification based on which probability is greater.
    if negative_prediction > positive_prediction:
      return -1
    return 1

with open("test.csv", 'r') as file:
    test = list(csv.reader(file))

predictions = [make_decision(r[0], make_class_prediction) for r in test] 
```

## 计算错误

现在我们知道了预测，我们将使用下面的面积来计算误差

[ROC](https://en.wikipedia.org/wiki/Receiver_operating_characteristic#Area_under_curve) 曲线。这将告诉我们模型有多“好”——越接近 1 意味着模型越好。计算误差对于知道你的模型什么时候是“好的”，什么时候变得更好或更差非常重要。

```py
 actual = [int(r[1]) for r in test]

from sklearn import metrics

# Generate the roc curve using scikit-learn.
fpr, tpr, thresholds = metrics.roc_curve(actual, predictions, pos_label=1)

# Measure the area under the curve.  The closer to 1, the "better" the predictions.
print("AUC of the predictions: {0}".format(metrics.auc(fpr, tpr))) 
```

```py
AUC of the predictions: 0.680701754385965
```

## 一种更快的预测方法

我们可以对这个算法进行许多扩展，使它的性能更好。我们可以看看

[n-grams](https://en.wikipedia.org/wiki/N-gram) 而不是 unigrams。我们可以删除标点符号和其他非字符。我们可以移除[停用词](https://en.wikipedia.org/wiki/Stop_words)。我们也可以执行[词干](https://en.wikipedia.org/wiki/Stemming)或词条化。但是，我们不希望每次都必须将整个算法编码出来。使用朴素贝叶斯的一个更简单的方法是使用 scikit-learn 中的实现。Scikit-learn 是一个 python 机器学习库，包含了所有常见机器学习算法的实现。

```py
 from sklearn.naive_bayes import MultinomialNB
from sklearn.feature_extraction.text import CountVectorizer
from sklearn import metrics

# Generate counts from text using a vectorizer.  There are other vectorizers available, and lots of options you can set.
# This performs our step of computing word counts.
vectorizer = CountVectorizer(stop_words='english')
train_features = vectorizer.fit_transform([r[0] for r in reviews])
test_features = vectorizer.transform([r[0] for r in test])

# Fit a naive bayes model to the training data.
# This will train the model using the word counts we computer, and the existing classifications in the training set.
nb = MultinomialNB()
nb.fit(train_features, [int(r[1]) for r in reviews])

# Now we can use the model to predict classifications for our test features.
predictions = nb.predict(test_features)

# Compute the error.  It is slightly different from our model because the internals of this process work differently from our implementation.
fpr, tpr, thresholds = metrics.roc_curve(actual, predictions, pos_label=1)
print("Multinomial naive bayes AUC: {0}".format(metrics.auc(fpr, tpr))) 
```

```py
Multinomial naive bayes AUC: 0.6509287925696594
```

## 接下来的步骤

要了解更多信息，请查看我们的 Dataquest 课程

[朴素贝叶斯](https://app.dataquest.io/m/27)。