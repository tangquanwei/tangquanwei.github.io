---
title: Classic NLP
tags: Classic NLP
date: 2024-05-24 20:08:17
cover:
---

NLP 是一门研究如何使计算机能够理解、处理和生成人类语言的学科。


基本概念：

1. 语言模型：用于对语言的概率进行建模，包括n-gram模型、神经网络语言模型等。
2. 分词：将连续的文本切分成有意义的词语。
3. 词性标注：为文本中的每个词语标注其词性，如名词、动词、形容词等。
4. 命名实体识别：识别文本中的具体实体，如人名、地名、组织机构等。
5. 语义角色标注：对句子中的每个词语进行语义角色的标注，如施事者、受事者等。

基本技术： 
1. 文本预处理：包括分词、去除停用词、词干化等，用于清洗和规范化文本数据。
2. 文本分类：将文本分为不同的类别或标签，如情感分类、主题分类等。
3. 信息抽取：从文本中提取结构化的信息，如实体关系、事件等。
4. 机器翻译：将一种语言的文本翻译成另一种语言的文本。
5. 问答系统：根据用户的问题，从文本中找到相关的答案。
6. 对话系统：与用户进行自然语言交互，实现智能对话。

应用场景：
1. 搜索引擎：通过分析用户的查询意图，提供相关的搜索结果。
2. 情感分析：分析文本中的情感倾向，如正面、负面或中性。
3. 社交媒体分析：分析社交媒体上的用户评论和观点。
4. 机器翻译：实现跨语言的文本翻译，如中英互译。
5. 语音识别：将语音转换为文本，实现语音助手和语音输入等功能。
6. 自动摘要：从大量文本中自动提取关键信息，生成摘要。
7. 智能客服：通过对话系统实现自动化的客户服务。

## 文本预处理

1. 分词（Tokenization）：将连续的文本切分成有意义的词语（token）。常见的分词方法包括基于规则的分词、基于统计的分词和基于机器学习的分词。
  

2. 去除停用词（Stopword Removal）：停用词是指在文本中频繁出现但没有实际意义的词语，如“的”、“是”、“在”等。去除停用词可以减少文本中的噪音，提高后续处理的效果。
  

3. 词干化（Stemming）：将词语还原为其词干形式，去除词语的词缀。例如，将“running”和“runs”都还原为“run”。常用的词干化算法有Porter算法和Lancaster算法。
  

4. 词形归一化（Lemmatization）：将词语还原为其基本形式（词元），即词的词性和时态都统一。与词干化不同，词形归一化考虑了词语的上下文和语法规则。例如，将“better”还原为“good”。

5. 去除特殊字符和标点符号：将文本中的特殊字符、标点符号和HTML标签等去除或替换为空格，以便后续处理。

6. 文本规范化：对文本进行一些规范化操作，如转换为小写字母、处理缩写词、数字的处理等，以保持文本的一致性。

7. 文本向量化：将文本转换为数值向量的形式，以便机器学习算法的处理。常用的文本向量化方法有词袋模型（Bag-of-Words）、TF-IDF（Term Frequency-Inverse Document Frequency）等。


## 分词

安装nltk：
``` pip3 install nltk ``` 

分词： 
```python 
import nltk 
# 下载nltk中的punkt模块，用于分句 
nltk.download('punkt') 
# 分词 
text = "Hello, world! How are you today?" 
tokens = nltk.word_tokenize(text) print(tokens) 
``` 
输出结果为： 
``` ['Hello', ',', 'world', '!', 'How', 'are', 'you', 'today', '?'] ``` 
这样就完成了基本的分词操作。

## 去除停用词

在自然语言处理中，停用词（Stopwords）是指在文本中频繁出现但没有实际意义的词语，如“的”、“是”、“在”等。

去除停用词可以减少文本中的噪音，提高后续处理的效果。下面是使用NLTK进行停用词去除的示例代码：

导入NLTK库并下载必要的数据集：

```python
import nltk

# 下载必要的数据集
nltk.download('stopwords')
```

接下来，使用NLTK的`stopwords`模块加载英文停用词表：

```python
from nltk.corpus import stopwords

# 加载英文停用词表
stop_words = set(stopwords.words('english'))

# 打印停用词表
print(stop_words)
```

运行以上代码，将会输出以下结果：

```

{'itself', 'mustn', 'herself', 'other', 'mightn', 'down', 'during', 'where', 'out', 'has', 'as', 'wasn', 'or', 'have', 'now', 'no', 'yourselves', 'had', 'when', 'weren', 'hasn', 'the', 't', 'above', 'under', 'those', 'before', 're', 'am', 'll', 'doe...', 'an', 'been', 'doesn', 'been', 'isn', 'y', 'for', 'we', 'did', 'ourselves', 'ma', 'than', 'they', 'should', 'he', 'ours', 'are', 'will', 'just', 'her', 'myself', 'wouldn', 'through', 'himself', 'shouldn', 'my', 'so', 'who', 'him', 'most', 'each'}

```

可以看到，NLTK的`stopwords`模块加载了英文停用词表，并返回了一个包含停用词的集合。

接下来，可以使用Python的列表推导式和条件语句，对文本进行停用词去除：

```python
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize

# 加载英文停用词表
stop_words = set(stopwords.words('english'))

# 定义要处理的文本
text = "This is an example sentence to demonstrate stopword removal."

# 对文本进行分词
tokens = word_tokenize(text)

# 去除停用词
filtered_tokens = [token for token in tokens if token.lower() not in stop_words]

# 输出结果
print(filtered_tokens)
```

  

运行以上代码，将会输出以下结果：
```

['example', 'sentence', 'demonstrate', 'stopword', 'removal', '.']

```

  

可以看到，经过停用词去除后，文本中的停用词“is”、“an”、“to”、“this”等被成功去除，只保留了实际有意义的词语。

## 词干化

将词语还原为其词干形式，去除词语的词缀。词干是指词语的基本形式，可以通过去除词缀来获得。词干化可以减少词语的变体，将具有相同词根的词语归并为同一形式，从而简化文本处理和分析。下面是使用NLTK进行词干化的示例代码：

在Python中导入NLTK库并下载必要的数据集：

```python
import nltk

# 下载必要的数据集
nltk.download('punkt')
nltk.download('averaged_perceptron_tagger')
nltk.download('wordnet')
```

接下来，使用NLTK的`PorterStemmer`类进行词干化：

```python
from nltk.stem import PorterStemmer
from nltk.tokenize import word_tokenize

# 创建PorterStemmer对象
stemmer = PorterStemmer()

# 定义要进行词干化的文本
text = "running runs"

# 对文本进行分词
tokens = word_tokenize(text)

# 对分词结果进行词干化
stemmed_tokens = [stemmer.stem(token) for token in tokens]


# 输出词干化结果
print(stemmed_tokens)
```

  

运行以上代码，将会输出以下结果：

  

```
['run', 'run']
```

  

可以看到，NLTK的`PorterStemmer`类将输入的文本中的词语进行了词干化，并返回了一个包含词干化结果的列表。

  

除了`PorterStemmer`类，NLTK还提供了其他的词干化类，如`LancasterStemmer`和`SnowballStemmer`，可以根据具体的需求选择适合的词干化类进行文本处理。

  

需要注意的是，词干化是一种基于规则的简单处理方法，可能会存在一些不准确的情况，如将“running”和“runs”都还原为“run”。如果需要更精确的词形还原，可以考虑使用词形归一化（Lemmatization）等更复杂的技术。


## 词形归一化

将词语还原为其标准形式，即词元（Lemma），从而减少词语的变体，将具有相同词根的词语归并为同一形式，从而简化文本处理和分析。与词干化不同，词形归一化考虑了词语的上下文语境和词性，因此可以得到更精确的结果。下面是使用NLTK进行词形归一化的示例代码：


在Python中导入NLTK库并下载必要的数据集：


```python
import nltk

# 下载必要的数据集
nltk.download('punkt')
nltk.download('averaged_perceptron_tagger')
nltk.download('wordnet')
```

接下来，使用NLTK的`WordNetLemmatizer`类进行词形归一化：

```python
from nltk.stem import WordNetLemmatizer
from nltk.tokenize import word_tokenize
from nltk.corpus import wordnet  

# 创建WordNetLemmatizer对象
lemmatizer = WordNetLemmatizer()  

# 定义要进行词形归一化的文本
text = "The striped bats are hanging on their feet for best"  

# 对文本进行分词和词性标注
tokens = word_tokenize(text)

tagged_tokens = nltk.pos_tag(tokens)

# 定义函数将词性标注转换为WordNet词性标记

def get_wordnet_pos(tag):
	if tag.startswith('J'):
		return wordnet.ADJ
	elif tag.startswith('V'):
		return wordnet.VERB
	elif tag.startswith('N'):
		return wordnet.NOUN
	elif tag.startswith('R'):
		return wordnet.ADV
	else:
		return wordnet.NOUN

# 对分词结果进行词形归一化
lemmatized_tokens = [lemmatizer.lemmatize(token, get_wordnet_pos(tag)) for token, tag in tagged_tokens]

# 输出词形归一化结果
print(lemmatized_tokens)
```

  

运行以上代码，将会输出以下结果：

  

```

['The', 'striped', 'bat', 'be', 'hang', 'on', 'their', 'foot', 'for', 'best']

```

  

可以看到，NLTK的`WordNetLemmatizer`类将输入的文本中的词语进行了词形归一化，并返回了一个包含词形归一化结果的列表。在进行词形归一化之前，需要先对文本进行分词和词性标注，并将词性标注转换为WordNet词性标记，以便`WordNetLemmatizer`类能够正确地识别词语的词性和上下文语境。

  

需要注意的是，词形归一化是一种相对复杂的处理方法，需要考虑词语的上下文语境和词性等因素，因此可能会比词干化更慢，但得到的结果更加准确。如果需要更快速的处理方法，可以考虑先使用词干化等简单的处理方法进行预处理，再使用词形归一化进行进一步处理。

## 向量化 word2vec

1. 词袋模型（Bag-of-Words）：将文本看作是一个词语的集合，忽略了词语的顺序和语法结构，只关注词汇的出现频率。可以使用CountVectorizer或TfidfVectorizer等工具库来实现词袋模型。

2. TF-IDF向量化：TF-IDF（Term Frequency-Inverse Document Frequency）是一种用于评估一个词语在文档中的重要程度的统计方法。TF-IDF向量化将文本表示为每个词语的TF-IDF权重，其中TF（词频）表示词语在文档中出现的频率，IDF（逆文档频率）表示词语在整个文档集合中的重要程度。可以使用TfidfVectorizer来实现TF-IDF向量化。

3. Word2Vec向量化：Word2Vec是一种基于神经网络的词向量表示方法，它将每个词语映射到一个固定长度的向量空间中，使得具有相似语义的词语在向量空间中距离较近。Word2Vec向量化可以捕捉到词语之间的语义关系，常用于文本相似度计算等任务。可以使用gensim库来实现Word2Vec向量化。
  [[Word2Vec向量化]]

4. 文档嵌入（Document Embedding）：文档嵌入是将整个文档表示为一个固定长度的向量的方法，常用于文本分类和文本聚类等任务。常见的文档嵌入方法包括Doc2Vec和BERT（Bidirectional Encoder Representations from Transformers）等。可以使用gensim库来实现Doc2Vec向量化，使用Hugging Face的transformers库来实现BERT向量化。

