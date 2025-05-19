---
title: Tokenizer
tags: Tokenizer
date: 2024-07-21 20:25:28
cover:
---

## Tokenizer

在NLP中，**tokenizer**是一个软件工具或函数，用于将文本字符串分割成更小的单元，这些单元被称为“tokens”。这些tokens可以是单词、标点符号、数字或任何有意义的文本单元，具体取决于tokenizer的设计和目标应用场景。例如，一个简单的空格分隔的tokenizer会将句子分割成单个的单词。

Tokenization是文本预处理的第一步，几乎所有NLP任务都需要这一步骤，包括但不限于机器翻译、情感分析、文本分类、命名实体识别（NER）、问答系统等。

Tokenizer可以根据不同的语言特性、语法结构和任务需求进行定制。例如：

- **基础分词器**：仅仅基于空格或标点符号分割文本。
- **高级分词器**：考虑连字符、缩写、数字和标点符号，甚至能处理复合词。
- **子词分词器**：如Byte Pair Encoding (BPE)，WordPiece，SentencePiece，它们将单词分割成子词单位，有助于处理未见过的词汇。
- **预训练模型的分词器**：如BERT、RoBERTa、GPT等模型自带的分词器，它们通常使用子词分词技术，并且包含特殊标记如`[CLS]`、`[SEP]`、`[PAD]`等，用于标识句子的开始、结束或填充。

在实践中，Hugging Face的Transformers库提供了各种预训练模型对应的tokenizer，这些tokenizer可以很容易地加载和使用。例如，加载BERT的tokenizer可以这样写：

```python
from transformers import BertTokenizer

tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
```

这将创建一个BERT模型的tokenizer实例，可以用来将文本转换为适合模型输入的格式，包括分词、添加特殊标记和转换为token IDs。

如果您在寻找特定的"rokenizer"工具或概念，建议提供更多的上下文或描述，以便给出更准确的解释或指导。

## 高级分词器（tokenizer）

在自然语言处理（NLP）中扮演着至关重要的角色，尤其是在处理复杂语言结构和实现更高精度的文本分析时。与基础分词器仅依赖于空格或标点符号来分割文本不同，高级分词器能够理解和处理更复杂的文本结构，包括但不限于：

1. **复合词处理**：在某些语言中，如德语，单词可以由多个部分组成，高级分词器能够识别并正确处理这些复合词，将其分割成构成部分。

2. **缩写和首字母缩略词**：高级分词器可以识别常见的缩写和首字母缩略词，并避免错误地将其分割。

3. **标点和特殊字符处理**：除了空格之外，高级分词器还会考虑到标点符号和特殊字符的作用，确保它们被正确地处理，例如，区分句号在句子结束和缩写词中的作用。

4. **数字和货币符号**：能够理解数字和货币符号的含义，避免错误地将它们与周围的文本分开。

5. **情感符号和表情符号**：在社交媒体和即时通讯中，表情符号和特殊字符非常普遍，高级分词器能够识别并保留这些符号的意义。

6. **上下文感知分词**：一些高级分词器能够根据上下文来决定如何分词，例如，同一个词在不同上下文中可能有不同的含义或用法。

7. **子词分词**：现代的高级分词器，如BERT、RoBERTa、GPT等预训练模型使用的分词器，通常采用子词分词技术（如Byte Pair Encoding, WordPiece, SentencePiece），这有助于模型处理未知词汇和多语言环境。

8. **语言特定规则**：高级分词器通常会包含针对特定语言的规则，以适应该语言的语法和书写习惯。

### 实现和使用
在实践中，Hugging Face的Transformers库提供了许多高级分词器，它们与预训练的模型紧密集成。例如，BERT的分词器不仅能够处理基本的分词，还能处理特殊标记，如`[CLS]`、`[SEP]`、`[PAD]`等，这些标记对于模型的输入格式至关重要。

使用高级分词器通常涉及以下步骤：

1. **加载分词器**：从预训练模型的仓库加载分词器。
2. **分词文本**：使用分词器将文本分割成tokens。
3. **转换为模型输入格式**：将tokens转换为模型所需的ID序列，并添加必要的特殊标记。

例如，使用BERT的分词器：

```python
from transformers import BertTokenizer

tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
text = "This is a sample sentence."
tokens = tokenizer.tokenize(text)
ids = tokenizer.convert_tokens_to_ids(tokens)
```

这将创建一个tokens列表和一个对应的token ID列表，准备供BERT模型使用。高级分词器的使用能够显著提高NLP任务的准确性和效果，尤其是在处理复杂和多样化的文本数据时。

## **特殊标记**（Special Tokens）

扮演着至关重要的角色。这些标记被加入到输入文本中，以帮助模型理解文本的结构、边界和其他重要信息。以下是几种常见的特殊标记及其用途：

1. **`[CLS]`（Classification）**:
   - 出现在输入序列的开始，用于表示整个序列的开始。在某些任务中，如句子级别的情感分类，`[CLS]`标记的最终隐藏状态通常被用作整个序列的固定长度表示。

2. **`[SEP]`（Separator）**:
   - 用于分隔输入序列中的不同部分。例如，在BERT中，如果模型接收两个句子作为输入，`[SEP]`将被放在两个句子之间，以及序列的末尾，以明确句子的边界。

3. **`[PAD]`（Padding）**:
   - 用于填充，当序列长度不一致时，较短的序列会被`[PAD]`标记填充至统一长度，以适应模型的批处理要求。

4. **`[MASK]`（Masked）**:
   - 在预训练阶段，特别是掩码语言建模（Masked Language Modeling, MLM）中使用。随机遮挡一部分输入序列中的单词，然后让模型预测被遮挡的单词。`[MASK]`标记被放置在被遮挡的单词的位置。

5. **`[UNK]`（Unknown）**:
   - 代表未知的或不在模型词汇表中的单词。当模型遇到它未曾见过的单词时，会将其替换为`[UNK]`标记。

6. **`[EOS]`（End Of Sentence）**:
   - 虽然在某些模型中与`[SEP]`的功能类似，但在其他模型中，如一些生成模型，`[EOS]`标记用于指示序列的结束。

7. **`<s>` 和 `</s>`**:
   - 这些标记在一些模型（如RoBERTa和XLM-RoBERTa）中用于标记句子的开始和结束。

特殊标记的使用对于模型的训练和下游任务的执行至关重要。它们帮助模型理解输入数据的结构，区分不同部分的文本，以及处理序列的边界条件。在使用预训练模型进行微调或直接应用时，确保正确使用这些标记是非常重要的。

例如，在使用BERT进行序列分类任务时，输入序列通常会被构造为`[CLS]` + 文本A + `[SEP]` + 文本B + `[SEP]`的形式，其中文本A和文本B可能分别代表两个要对比的句子。这样，模型可以正确地理解文本的结构，并生成相应的嵌入表示用于后续的分类任务。

## **Byte Pair Encoding (BPE)** 

是一种用于自然语言处理的子词分割技术，最初由 Rico Sennrich、Barry Haddow 和 Alexandra Birch 在神经机器翻译系统中提出。它的设计目的是为了处理未见过的词（OOV words）问题，同时减少词汇表的大小，这对于处理低资源语言或者具有大量词汇的语言特别有用。

BPE 的核心思想是将词分解成一系列的子词单位（subword units），这些子词单位可以是单个字符、常见词根、词缀或其他常见组合。这个过程基于统计学习，从一个初始化的字符级词汇表开始，逐步合并最常见的相邻字符对，直到达到预定的词汇表大小。

BPE 的算法步骤大致如下：

1. **初始化词汇表**：
   - 开始时，词汇表仅包含所有出现过的字符（或字节）。

2. **统计并合并**：
   - 遍历语料库，统计所有相邻字符对的频率。
   - 找出最常出现的字符对，并将它们合并为一个新的子词单位。
   - 更新词汇表，添加新的子词单位。
   - 重复此过程，直到词汇表达到预定的大小或满足其他停止标准。

3. **编码和解码**：
   - 编码时，一个词被分解成一系列的子词单位。
   - 解码时，这些子词单位重新组合成原来的词。

BPE 的优势包括：

- **处理 OOV 词**：由于 BPE 使用的是子词单位，即使是未见过的新词也可以通过其组成部分来表示。
- **词汇表大小**：相比于完整的词级词汇表，BPE 可以显著减少词汇表的大小，从而降低存储需求和计算复杂度。
- **泛化能力**：BPE 提供了更好的泛化能力，特别是在处理形态学丰富的语言时。

然而，BPE 也存在一些限制，比如可能会产生过分割的问题，导致较长的输出序列，以及在处理非常罕见的字符组合时可能出现的不确定性。

BPE 在现代自然语言处理中得到了广泛的应用，尤其是在预训练语言模型（如 GPT 系列、RoBERTa 和 BART）中，作为词汇表构建和文本分词的重要组成部分。

## **WordPiece** 和 **SentencePiece** 

是两种流行的子词（subword）分词技术，它们被设计用于处理自然语言处理（NLP）任务中的未知词（Out-of-Vocabulary，OOV）问题，同时也减少了词汇表的大小，提高了模型的训练效率和泛化能力。

### WordPiece

**WordPiece** 是由Google提出的一种子词分词方法，最初在Google的神经机器翻译系统中使用。WordPiece 的核心思想是将词分割成一系列子词单元，这些子词单元可以是词的一部分或完整的词。WordPiece 的训练过程涉及构建一个子词词汇表，这个词汇表是基于输入文本语料库的统计信息生成的。

WordPiece 的训练步骤如下：

1. 初始化一个包含所有字符的词汇表。
2. 统计语料库中所有子词单元的频率。
3. 选择最频繁的子词单元对，并将这对子词合并成一个新的子词单元。
4. 更新词汇表和子词单元的频率。
5. 重复步骤3和4，直到达到预定的词汇表大小。

### SentencePiece

**SentencePiece** 也是由Google开发的，旨在提供一种无需依赖语言特定的知识就能处理任何脚本和语言的通用子词分词器。SentencePiece 的独特之处在于它可以处理多种语言和脚本，而不仅仅是基于拉丁字母的语言。

SentencePiece 的主要特点包括：

- **无监督学习**：SentencePiece 使用无监督学习算法来构建子词单元的词汇表，这意味着它不需要任何预处理或语言特定的知识。
- **跨语言兼容性**：它适用于所有脚本和语言，包括混合脚本和混合语言的文本。
- **灵活性**：SentencePiece 允许用户指定生成的子词单元的大小，这使得它能够平衡词汇表的大小和模型的复杂性。

SentencePiece 的训练过程与WordPiece 类似，但它是基于更广泛的字符集和语言，因此更加灵活和通用。

### WordPiece vs SentencePiece

尽管两者都是子词分词技术，但它们之间存在一些关键差异：

- **语言独立性**：SentencePiece 更加独立于特定语言，而WordPiece 在设计时考虑了特定语言的特征。
- **训练和使用**：SentencePiece 的训练和使用通常更加简单和直观，因为它不需要语言特定的配置。
- **适用性**：SentencePiece 由于其语言独立性，更适合多语言和混合脚本的文本处理。

这两种分词技术都在现代NLP模型中得到了广泛应用，如BERT、RoBERTa、XLM-R等预训练语言模型都采用了WordPiece或SentencePiece作为其分词策略。通过使用这些子词分词技术，模型能够在处理未知词和多语言数据时表现出更好的性能和泛化能力。

## 中文文本的分词

对于中文自然语言处理（NLP）任务至关重要，因为中文不同于英文等基于空格分隔的语言，没有明显的词边界。因此，中文分词器（Tokenizer）需要能够识别出有意义的词语单位，以便后续的NLP流程可以正确地理解和处理文本。

中文分词器通常会面临以下几个挑战：

1. **词边界模糊**：中文词语之间没有空格分隔，识别词的开始和结束位置较为困难。
2. **歧义词组**：一些词组可能有多种分词方式，例如“研究生活动中心”可以分为“研究生活动/中心”，也可以分为“研究/生/活动/中心”。
3. **未登录词**：新词、专业术语、人名、地名等可能不在词典中，但需要被正确识别。

下面是一些常用的中文分词器：

1. **Jieba分词**：
   - Jieba是最广泛使用的开源中文分词库之一，支持精确模式、全模式以及搜索引擎模式的分词。Jieba分词基于前缀树（Trie Tree）实现高效查找，并使用HMM（隐马尔科夫模型）进行未登录词识别。

2. **THULAC**：
   - THULAC是清华大学自然语言处理与社会人文计算实验室开发的一个中文分词和词性标注工具，具有较高的准确率。

3. **HanLP**：
   - HanLP是由哈工大社会计算与信息检索研究中心开发的中文自然语言处理工具包，提供了词性标注、命名实体识别、依存句法分析等功能。

4. **LTP（Language Technology Platform）**：
   - LTP是哈工大社会计算与信息检索研究中心开发的一套中文语言处理平台，包括分词、词性标注、命名实体识别、依存句法分析等多个模块。

5. **MiNLP-Tokenizer**：
   - 这是小米AI实验室NLP团队自研的中文分词工具，基于深度学习序列标注模型实现，具有很好的分词效果和轻量级模型。

6. **NLPIR**：
   - NLPIR是中科院计算所信息检索实验室开发的中文分词工具，适合新闻、论坛、微博等多种文本类型。

7. **THUCTC**：
   - THUCTC是清华大学开发的中文词性标注和分词工具，特别适合于社交媒体和网络文本。

8. **基于Transformer的Tokenizer**：
   - 预训练模型如BERT、RoBERTa等使用的Tokenizer，这些模型通常采用WordPiece或SentencePiece算法，能够处理未登录词和多语言文本。

9. **Stanford NLP**：
   - Stanford NLP工具包也提供了中文分词功能，适合学术研究和工业应用。

在选择中文分词器时，应该考虑具体的应用场景、对分词精度的需求、处理速度、是否需要词性标注或其他NLP功能等因素。不同的分词器可能在不同的应用场景下表现最佳，因此可能需要根据实际需求进行实验和比较。

## `transformers`库中的tokenizer


### 加载Tokenizer

```python
from transformers import AutoTokenizer

# 加载预训练模型对应的tokenizer
tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")
```

### 分词和编码

#### 分词（Tokenization）

```python
# 将文本分词
text = "Hello, my dog is cute"
tokens = tokenizer.tokenize(text)
print(tokens)
```

#### 编码（Encoding）

```python
# 将文本编码为模型可以接受的格式
encoded_input = tokenizer.encode(text, return_tensors='pt')  # 返回PyTorch tensors
print(encoded_input)
```

#### 批量编码（Batch Encoding）

```python
texts = ["Hello, my dog is cute", "I am playing football"]
# 批量编码文本，自动填充或截断到相同长度
encoded_inputs = tokenizer(texts, padding=True, truncation=True, max_length=512, return_tensors='pt')
print(encoded_inputs)
```

### 解码（Decoding）

```python
# 解码模型的输出
decoded_output = tokenizer.decode(encoded_input[0].tolist())
print(decoded_output)
```

### 特殊标记

```python
# 获取特殊标记的ID
cls_token_id = tokenizer.cls_token_id
sep_token_id = tokenizer.sep_token_id
pad_token_id = tokenizer.pad_token_id
unk_token_id = tokenizer.unk_token_id

# 或者获取标记本身
cls_token = tokenizer.cls_token
sep_token = tokenizer.sep_token
pad_token = tokenizer.pad_token
unk_token = tokenizer.unk_token
```

### 附加信息

`tokenizer`还提供了很多附加信息，如`tokenizer.model_max_length`，`tokenizer.vocab_size`等，可以帮助你更好地理解tokenizer的配置和限制。

### 使用`__call__`方法

`tokenizer.__call__`方法是tokenizer的主要入口点，它整合了编码和一些额外的参数设置，如padding和truncation。你可以直接使用这个方法来处理文本：

```python
# 使用__call__方法编码文本
inputs = tokenizer(text, return_tensors='pt', padding=True, truncation=True, max_length=512)
```

### 注意事项

- 不同的预训练模型可能需要不同的特殊标记，例如BERT使用`[CLS]`和`[SEP]`，而GPT系列则使用``。
- 当处理中文文本时，确保使用适合中文的tokenizer，如`bert-base-chinese`。
- 在使用`tokenizer`进行编码时，`return_tensors`参数可以设置为`'pt'`（PyTorch tensors）、`'tf'`（TensorFlow tensors）或`'np'`（NumPy arrays）以适应不同的深度学习框架。

以上这些操作涵盖了`transformers`库中tokenizer的大部分常见使用场景。根据具体需求，你可能还需要查阅官方文档或源代码以获取更详细的配置选项和高级功能。