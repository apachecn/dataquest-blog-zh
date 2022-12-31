# 使用机器学习和自然语言处理工具进行文本分析

> 原文：<https://www.dataquest.io/blog/using-machine-learning-and-natural-language-processing-tools-for-text-analysis/>

February 28, 2022![](img/ca7bd0308cc1e6822b1d947812edc0a1.png)

这是关于引导项目反馈分析主题的第三篇文章。该主题的主要思想是分析学习者在论坛页面上收到的回复。Dataquest 鼓励其学习者在其论坛上发布他们的指导项目，在发布后，其他学习者或工作人员可以分享他们对该项目的意见。我们对这些观点的内容感兴趣。

在我们的[上一篇文章](https://www.dataquest.io/blog/how-to-clean-and-prepare-your-data-for-analysis/)中，我们已经对数字数据进行了基本的数据分析，并深入分析了反馈文章的文本数据。

在本文中，我们将尝试使用多个包来增强我们的文本分析。我们不会为一项任务设定目标，而是会尝试各种工具，这些工具使用自然语言处理和/或机器学习来提供输出。

下面是我们将在本文中尝试的一些好东西:

*   情感分析
*   关键词提取
*   主题建模
*   k 均值聚类
*   一致
*   查询回答模型

使用机器学习进行自然语言处理是一个非常广泛的话题，不可能包含在一篇文章中。您可能会发现，从您的角度来看，本文中描述的工具并不重要。或者它们被错误地使用，它们中的大多数没有被调整，我们只是使用了开箱即用的参数。请记住，这是对用于增强反馈数据分析的软件包、工具和模型的主观选择。但是你也许能找到更好的工具或不同的方法。我们鼓励您这样做并分享您的发现。

# ML 与 NLP

机器学习和自然语言处理是两个非常宽泛的术语，可以涵盖文本分析和处理领域。我们不会试图在这两个术语之间设定一条固定的界限，我们会把它留给哲学家。如果你有兴趣探究它们之间的差异，请看这里。

# 情感分析

情感分析是 ML power 的一个非常流行的应用。通过分析每个文本的内容，我们可以评估句子或整个文本的权重是积极还是消极。如果你想过滤掉对你产品的负面评价或者只展示好的评价，这将会有巨大的价值。

网上有大量的 python 情感分析模型和工具。我们将关注最简单的一个:执行一个基本的情感分析需要两行代码:

```py
# import the package:
from pattern.en import sentiment
# perform the analysis:
x = 'project looks amazing, great job'
sentiment(x)
```

输出:

```py
(0.7000000000000001, 0.825)
```

如上所示，导入包后，我们只需调用情感函数，为它提供一个字符串值，瞧！输出是一个元组，第一个值是“极性”(句子在从-1 到 1 的范围内有多积极)。第二个值是主观性，它告诉我们算法对其第一个值的评估有多确定，这次标度从 0 开始，到 1 结束。让我们再看几个例子:

```py
y = 'plot looks terrible, spines are too small'
sentiment(y)
```

输出:

```py
(-0.625, 0.7)
```

我们得到的是一个相当“低”的第一值，算法对其极性评估仍然很有信心。让我们试试更难的:

```py
z = 'improve the comment, first line looks bad'
sentiment(z)
```

输出:

```py
(-0.22499999999999992, 0.5)
```

我们可以注意到模型在这一点上更加犹豫。字符串有一个否定词，但它不再那么确定了。

让我们将情感函数应用于我们的反馈内容:

```py
df['sentiment'] = df['feedback_clean2_lem'].apply(lambda x: sentiment(x))
df['polarity'] = df['sentiment'].str[0]
df['subjectivity'] = df['sentiment'].str[1]
df = df.drop(columns='sentiment')
```

现在，我们已经衡量了每个反馈帖子的极性和主观性，让我们看看每个主题有何不同:

```py
top15 = df['short_title'].value_counts()[:15].index
df[df['short_title'].isin(top15)].groupby('short_title')[['polarity','subjectivity']].mean().sort_values('subjectivity')
```

| 简短标题 | 极性 | 主观性 |
| --- | --- | --- |
| sql 使用 | 0.227535 | 0.441175 |
| 中情局实况报道 | 0.232072 | 0.460941 |
| 汽车价格 | 0.206395 | 0.476976 |
| 易趣汽车 | 0.251605 | 0.498947 |
| 交通拥挤 | 0.219977 | 0.504832 |
| 星球大战 | 0.250845 | 0.50871 |
| 新闻黑客 | 0.288198 | 0.509783 |
| 离职员工 | 0.269066 | 0.512406 |
| 科普 | 0.276232 | 0.514718 |
| app 盈利 | 0.281144 | 0.514833 |
| 纽约高中 | 0.288988 | 0.519288 |
| fandango 收视率 | 0.265831 | 0.524607 |
| 性别差距 | 0.285667 | 0.534309 |
| 大学可视化 | 0.279269 | 0.547273 |
| 市场广告 | 0.279195 | 0.572073 |

不幸的是，结果非常相似。这并不奇怪，大多数反馈帖子都有非常相似的结构。开始时，他们通常会包含一两句对项目表示祝贺的话。这种正面的内容后面通常会跟着一些批评性的备注(通常被视为带有负极性的内容)。

帖子通常以一些对未来编码有益的信息结尾。本质上，这是一堆积极和消极情绪交织在一起的信息。不像产品评论那么简单，我们经常会遇到一个开心的客户或一个非常不开心的客户。大多数时候，对他们的评论进行分类并不困难。不幸的是，我们的内容更加复杂。但是我们不会这么轻易放弃。

# 关键词

从给定的字符串中提取关键字是另一个重要的技巧，可以改进我们的分析。

Rake 包提供了从文本中提取的所有 n 元文法及其权重的列表。该值越高，所考虑的 n-gram 就越重要。解析完文本后，我们可以只过滤掉具有最高值的 n 元语法。

请注意，该模型使用停用词来评估句子中哪些词是重要的。如果我们给这个模型输入清除了停用词的文本，我们不会得到任何结果。

```py
from rake_nltk import Rake
# set the parameteres (length of keyword phrase):
r = Rake(include_repeated_phrases=False, min_length=1, max_length=3)
text_to_rake = df['feedback'][31]
r.extract_keywords_from_text(text_to_rake)
# filter out only the top keywords:
words_ranks = [keyword for keyword in r.get_ranked_phrases_with_scores() if keyword[0] > 5]
words_ranks
```

输出:

```py
[(9.0, '“ professional ”'),
 (9.0, 'avoiding colloquial language'),
 (8.0, 'nicely structured project'),
 (8.0, 'also included antarctica'),
 (8.0, 'add full stops')]
```

在这个例子中，Rake 认为“专业”或“避免口语化语言”是输入文本中最重要的关键词。为了进一步分析，我们不会对关键字的数值感兴趣。我们只想收到每个帖子的几个热门关键词。我们将设计一个简单的函数，只提取顶部的关键字，并将其应用于“反馈”列:

```py
def rake_it(text):
    r.extract_keywords_from_text(text)
    r.get_ranked_phrases()
    keyword_rank = [keyword for keyword in r.get_ranked_phrases_with_scores() if keyword[0] > 5]
    # select only the keywords and return them:
    keyword_list = [keyword[1] for keyword in keyword_rank]
    return keyword_list

df['rake_words'] = df['feedback'].apply(lambda x: rake_it(x))
```

把每个帖子的关键词都提取出来了，我们来看看哪些是最受欢迎的！记住它们是作为一个列表存储在一个单元格中的，所以我们必须处理这个障碍:

```py
# function from: towardsdatascience.com/dealing-with-list-values-in-pandas-dataframes-a177e534f173
def to_1D(series):
    return pd.Series([x for _list in series for x in _list])

to_1D(df['rake_words']).value_counts()[:10]
```

输出:

```py
jupyter file menu        24
guided project use       24
happy coding :)          22
everything looks nice    20
sql style guide          16
first guided project     16
everything looks good    15
new topic button         15
jupyter notebook file    14
first code cell          13
dtype: int64
```

# 主题建模

主题建模可以让我们快速洞察文本的内容。与从文本中提取关键词不同，主题建模是一种更高级的工具，可以根据我们的需要进行调整。

我们不打算冒险深入设计和实现这个模型，它本身可以填充几篇文章。我们将针对每个反馈内容快速运行该模型的基本版本。我们的目标是为每个帖子提取指定数量的主题。

我们将从一个反馈帖子开始。让我们导入必要的包，编译文本并创建所需的字典和矩阵(这是机器学习，因此每个模型都需要特定的输入):

```py
# Importing:
import gensim
from gensim import corpora
import re

# compile documents, let's try with 1 post:
doc_complete = df['feedback_clean2_lem'][0]
docs = word_tokenize(doc_complete)
docs_out = []
docs_out.append(docs)

# Creating the term dictionary of our courpus, where every unique term is assigned an index. dictionary = corpora.Dictionary(doc_clean)
dictionary = corpora.Dictionary(docs_out)

# Converting a list of documents (corpus) into Document Term Matrix using dictionary prepared above.
doc_term_matrix = [dictionary.doc2bow(doc) for doc in docs_out]
```

做了所有的准备工作之后，就该训练我们的模型并提取结果了。您可能已经注意到，我们可以手动设置许多参数。举几个例子:话题的数量，每个话题我们用了多少单词。但是清单还在继续，就像许多 ML 模型一样，你可以花很多时间调整这些参数来完善你的模型。

```py
# Running and Trainign LDA model on the document term matrix.
Lda = gensim.models.ldamodel.LdaModel
ldamodel = Lda(doc_term_matrix, num_topics=5, id2word = dictionary, passes=50, random_state=4)

# Getting results:
ldamodel.print_topics(num_topics=5, num_words=4)
```

输出:

```py
[(0, '0.032*"notebook" + 0.032*"look" + 0.032*"process" + 0.032*"month"'),
 (1, '0.032*"notebook" + 0.032*"look" + 0.032*"process" + 0.032*"month"'),
 (2, '0.032*"important" + 0.032*"stay" + 0.032*"datasets" + 0.032*"process"'),
 (3,
  '0.032*"httpswww1nycgovsitetlcabouttlctriprecorddatapage" + 0.032*"process" + 0.032*"larger" + 0.032*"clean"'),
 (4, '0.113*"function" + 0.069*"inside" + 0.048*"memory" + 0.048*"ram"')]
```

我们可以注意到，我们的模型为我们提供了一个元组列表，每个元组包含单词及其权重。数字越高，单词越重要(根据我们的模型)。如果我们想深入研究，我们可以提取某个值以上的所有单词…只是一个想法🙂

让我们继续将该模型应用于每个反馈帖子。为了简化我们的生活，我们将只提取主题词，而不是“权重”值。这样，我们可以轻松地对提取的主题执行 value_counts，并查看哪些主题是最受欢迎的(根据模型)。为了对列中的每个单元格进行主题建模，我们将设计一个函数。作为输入值，我们将使用单元格的内容(文本)和主题的字数:

```py
def get_topic(x,n):
    """
    extract list of topics given text(x) and number(n) of words for topic
    """
    docs = word_tokenize(x)
    docs_out = []
    docs_out.append(docs)
    dictionary = corpora.Dictionary(docs_out)
    doc_term_matrix = [dictionary.doc2bow(doc) for doc in docs_out]
    Lda = gensim.models.ldamodel.LdaModel
    # Running and Trainign LDA model on the document term matrix.
    ldamodel = Lda(doc_term_matrix, num_topics=5, id2word = dictionary, passes=50, random_state=1)
    topics = ldamodel.print_topics(num_topics=2, num_words=n)
    topics_list = []
    for elm in topics:
        content = elm[1]
        no_digits = ''.join([i for i in content if not i.isdigit()])
        topics_list.append(re.findall(r'\w+', no_digits, flags=re.IGNORECASE))
    return topics_list 
```

让我们看看最大长度为 4 个单词的主题是什么样子的:

```py
df['topic_4'] = df['feedback_clean2_lem'].apply(lambda x: get_topic(x,4))
to_1D(df['topic_4']).value_counts()[:10]
```

输出:

```py
[keep, nice]                                               8
[nice, perform]                                            4
[nice]                                                     4
[nan]                                                      4
[sale, take, look, finish]                                 2
[learning, keep, saw, best]                                2
[library, timezones, learning, httpspypiorgprojectpytz]    2
[job, graph, please, aesthetically]                        2
[thanks, think, nice, learning]                            2
[especially, job, like, nice]                              2
dtype: int64
```

现在，让我们检查最多 3 个词的主题:

```py
df['topic_3'] = df['feedback_clean2_lem'].apply(lambda x: get_topic(x,3))
to_1D(df['topic_3']).value_counts()[:10]
```

输出:

```py
[keep, nice]                       8
[share, thank, please]             4
[nan]                              4
[nice]                             4
[nice, perform]                    4
[guide, documentation, project]    3
[]                                 3
[cell]                             3
[guide, project, documentation]    3
[plot]                             3
dtype: int64
```

我们的结果不是很好，但是记住我们只是触及了这个工具的表面。谈到 lda 模型，有很大的潜力。我鼓励您至少阅读下面的一篇文章，以扩大您对这个库的了解:

*   [机械瓶颈](https://www.machinelearningplus.com/nlp/topic-modeling-gensim-python/)
*   [走向数据科学](https://towardsdatascience.com/nlp-extracting-the-main-topics-from-your-dataset-using-lda-in-minutes-21486f5aa925)

# k 均值聚类

Kmeans 模型可以基于各种输入对数据进行聚类，这可能是最流行的无监督机器学习模型。只需选择您希望数据分配到多少个集群，基于什么功能和瞧。作为一个 ML 模型，我们不能只给它提供原始文本，我们必须对文本数据进行矢量化，然后给模型提供原始文本。本质上，我们将文本数据转换成数字数据。我们如何做取决于我们自己，有许多方法可以对数据进行矢量化，让我们试试 TfidfVectorizer:

```py
# imports:
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.cluster import KMeans

# vectorize text:
tfidfconverter = TfidfVectorizer(max_features=5000, min_df=0.1, max_df=0.7, stop_words=stopwords.words('english'))  
X = tfidfconverter.fit_transform(df['feedback']).toarray()

# fit and label:
Kmean = KMeans(n_clusters=8, random_state=2)
Kmean.fit(X)
df['label'] = Kmean.labels_
```

现在，让我们看看不同的集群是否有很大的极性差异:

```py
df.groupby('label')['polarity'].mean()
```

输出:

```py
label
0    0.405397
1    0.312328
2    0.224152
3    0.210876
4    0.143431
5    0.340016
6    0.242555
7    0.241244
Name: polarity, dtype: float64
```

实际上，让我们根据我们的模型分配的集群编号检查一些其他的编号:

```py
 #group data:
polar = df.groupby('label')['polarity'].mean()
subj = df.groupby('label')['subjectivity'].mean()
level = df.groupby('label')['level'].mean()
val_cnt = df['label'].value_counts()
length = df.groupby('label')['len'].mean()

#create a df and rename some of the columns, index:
cluster_df = pd.concat([val_cnt,polar,subj,level, length], axis=1)
cluster_df = cluster_df.rename(columns={'label':'count'})
cluster_df.index.name = 'label'
cluster_df
```

| 标签 | 数数 | 极性 | 主观性 | 水平 | 低输入联网（low-entry networking 的缩写） |
| --- | --- | --- | --- | --- | --- |
| Zero | Eighty-seven | 0.405397 | 0.635069 | 1.49425 | Three hundred and fourteen point two eight seven |
| one | One hundred and fifty | 0.312328 | 0.536363 | 1.55333 | Seven hundred and forty-two point two five three |
| Two | Sixty | 0.224152 | 0.469265 | One point five | Five hundred and ninety-four point two six seven |
| three | One hundred and thirty-six | 0.210876 | 0.513048 | 1.46324 | One thousand four hundred and twenty-nine point one |
| four | Sixty-six | 0.143431 | 0.34258 | 1.4697 | Two hundred and fifty-one point two two seven |
| five | One hundred and eighteen | 0.340016 | 0.581554 | 1.29661 | Nine hundred and three point one one |
| six | Three hundred and two | 0.242555 | 0.495008 | 1.45033 | Seven hundred and twenty-four point two zero nine |
| seven | Ninety-two | 0.241244 | 0.431905 | 1.52174 | Three hundred and ninety-eight point two two eight |

我们可以在该表中注意到一些有趣的趋势，例如，0 号聚类具有相当积极的内容(高极性平均值)，此外，我们之前在该聚类中使用的情感模型对其建模相当确定(高主观性值)。这可能是由该簇(314)中非常短的平均文本长度引起的。看着上面的表格，你有没有发现其他有趣的事实？

请记住，我们已经向 Kmeans 模型提供了使用 Tfidf 矢量化的数据，在将文本数据提供给模型之前，有多种方式[对文本数据进行矢量化。你应该试试它们，看看它们对结果有什么影响。](https://neptune.ai/blog/vectorization-techniques-in-nlp-guide)

# 句子聚类

如前所述:每个反馈帖子的内容是赞美和建设性批评的相当复杂的混合。这就是为什么我们的聚类模型在被要求对帖子进行聚类时表现不佳。但是，如果我们将所有的帖子分成句子，并要求模型将句子聚集起来，我们应该会改善我们的结果。

```py
from nltk.corpus import stopwords
stop = set(stopwords.words('english'))
from nltk import sent_tokenize
from nltk import pos_tag
import string
from nltk.stem import WordNetLemmatizer
lemmatizer = WordNetLemmatizer()

# set up for cleaning and sentence tokenizing:
exclude = set(string.punctuation)
def clean(doc):
    stop_free = " ".join([i for i in doc.lower().split() if i not in stop])
    punc_free = ''.join(ch for ch in stop_free if ch not in exclude)
    return punc_free

# remember this function from article no 2?
def lemmatize_it(sent):
    empty = []
    for word, tag in pos_tag(word_tokenize(sent)):
        wntag = tag[0].lower()
        wntag = wntag if wntag in ['a', 'r', 'n', 'v'] else None
        if not wntag:
            lemma = word
            empty.append(lemma)
        else:
            lemma = lemmatizer.lemmatize(word, wntag)
            empty.append(lemma)
    return ' '.join(empty)

# tokenize sentences and clean them:
doc_complete = sent_tokenize(df['feedback'].sum())
doc_clean = [clean(doc) for doc in doc_complete] 
doc_clean_lemmed = [lemmatize_it(doc) for doc in doc_clean] 

# create and fill a dataframe with sentences:
sentences = pd.DataFrame(doc_clean_lemmed)
sentences.columns = ['sent']
sentences['orig'] = doc_complete
sentences['keywords'] = sentences['orig'].apply(lambda x: rake_it(x))

# vectorize text:
tfidfconverter = TfidfVectorizer(max_features=10, min_df=0.1, max_df=0.9, stop_words=stopwords.words('english'))  
X = tfidfconverter.fit_transform(sentences['sent']).toarray()

# fit and label:
Kmean = KMeans(n_clusters=8)
Kmean.fit(X)
sentences['label'] = Kmean.labels_
```

现在让我们来看看每个标签最受欢迎的关键词:

```py
to_1D(sentences[sentences['label'] == 0]['keywords']).value_counts()[:10]
```

输出:

```py
everything looks nice     15
everything looks good     13
sql style guide           10
print () function          8
social love attention      7
data cleaning process      7
looks pretty good          6
screen shot 2020           5
everything looks great     5
jupyter notebook file      5
dtype: int64
```

```py
to_1D(sentences[sentences['label'] == 2]['keywords']).value_counts()[:10]
```

输出:

```py
first code cell            8
avoid code repetition      7
items (): spine            4
might appear complex       4
may appear complex         4
might consider creating    3
1st code cell              3
print () function          3
little suggestion would    3
one code cell              3
dtype: int64
```

# 聚类 n 元文法

类似于对帖子和句子进行聚类，我们可以对 n 元语法进行聚类:

```py
from nltk.util import ngrams 
import collections

# extract n-grams and put them in a dataframe:
tokenized = word_tokenize(sentences['sent'].sum())
trigrams = ngrams(tokenized, 3)
trigrams_freq = collections.Counter(trigrams)
trigram_df = pd.DataFrame(trigrams_freq.most_common())
trigram_df.columns = ['trigram','count']
trigram_df['tgram'] = trigram_df['trigram'].str[0]+' '+trigram_df['trigram'].str[1]+' '+trigram_df['trigram'].str[2]

# vectorize text:
tfidfconverter = TfidfVectorizer(max_features=100, min_df=0.01, max_df=0.9, stop_words=stopwords.words('english'))  
X = tfidfconverter.fit_transform(trigram_df['tgram']).toarray()

# fit and label:
Kmean = KMeans(n_clusters=8, random_state=1)
Kmean.fit(X)
trigram_df['label'] = Kmean.labels_
```

# 一致

[https://avidml . WordPress . com/2017/08/05/natural-language-processing-concordance/](https://avidml.wordpress.com/2017/08/05/natural-language-processing-concordance/)

如果我们想检查一个特定的单词在文本中是如何使用的呢？我们想看看这个特定单词前后的单词。在 concordance 的一点帮助下，我们可以很快地看一看:

```py
from nltk.text import Text 
text = nltk.Text(word_tokenize(df['feedback'].sum()))
text.concordance("plot", lines=10)
```

输出:

```py
Displaying 10 of 150 matches:
ive precision . it is better to make plot titles bigger . about the interactiv
 '' ] what is the point of your last plot ? does it confirm your hypothesis th
ou use very similar code to create a plot – there is an opportunity to reduce 
er plots ” try to comment after each plot and not at the end so the reader doe
ur case saleprice , and after it you plot correlation only between remaining f
tation then possible and correlation plot may be have different colors and val
tting the format of the grid on your plot setting the ylabel as ‘ average traf
 line_data = series that you want to plot # start_hour = beginning hour of the
#start_hour = beginning hour of the plot **in 24hr format** # end_hour = end 
ormat** # end_hour = end hour of the plot **in 24hr format** def plot_traffic_
```

# 查询回答模型

老实说，从一个简单的查询回答模型开始，你不需要了解太多关于模型内部发生的巫术的具体机制。有必要了解一些基本知识:

*   你必须把文本转换成向量和数组
*   该模型比较该数字输入，并找到与该输入最相似的内容
*   就这样，在您成功运行了一个基本模型之后，您应该开始尝试不同的参数和矢量化方法

```py
# imports:
from sklearn.metrics import pairwise_distances
from sklearn.decomposition import TruncatedSVD
from sklearn.feature_extraction.text import CountVectorizer
from nltk.corpus import stopwords

stopword_list = stopwords.words('english')

# create a model:
dtm = CountVectorizer(max_df=0.7, min_df=5, token_pattern="[a-z']+", 
                      stop_words=stopword_list, max_features=6000)

dtm.fit(df['feedback_clean2_lem'])
dtm_mat = dtm.transform(df['feedback_clean2_lem'])
tsvd = TruncatedSVD(n_components=200)
tsvd.fit(dtm_mat)
tsvd_mat = tsvd.transform(dtm_mat)

# let's look for "slow function solution"
query = "slow function solution"
query_mat = tsvd.transform(dtm.transform([query]))

# calculate distances:
dist = pairwise_distances(X=tsvd_mat, Y=query_mat, metric='cosine')
# return the post with the smallest distance:
df['feedback'][np.argmin(dist.flatten())]
```

输出:

```py
' processing data inside a function saves memory (the variables you create stay inside the function and are not stored in memory, when you are done with the function) it is important when you are working with larger datasets - if you are interested with experimenting: https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page try cleaning 1 month of this dataset on kaggle notebook (and look at your ram usage) outside the function and inside the function, compare the ram usage in both examples '
```

请记住，我们正在解析以寻找答案的数据集相当小，因此我们不能期待令人兴奋的答案。

# 最后一句话

这并不是用于文本分析的一长串工具的结尾。我们几乎没有触及表面，我们使用的工具还没有得到最有效的利用。你应该继续寻找更好的方法，调整模型，使用不同的矢量器，收集更多的数据。

自然语言处理和[机器学习](https://www.dataquest.io/course/machine-learning-fundamentals/)的疯狂混合，是一个可以研究几十年的永无止境的话题。就在过去的 20 年里，这些工具给我们带来了惊人的应用，你还记得谷歌之前的世界吗？在网上搜索内容和看黄页非常相似。用我们的智能手机助手怎么样？这些工具不断变得更有效，值得你关注它们如何变得更好地理解我们的语言。

## 有什么问题吗？

随便伸手问我什么:
[Dataquest](https://community.dataquest.io/u/adam.kubalica/summary) ， [LinkedIn](https://www.linkedin.com/in/kubalica/) ， [GitHub](https://github.com/grumpyclimber/portfolio)