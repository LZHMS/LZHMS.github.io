---
title: Transformer Concept Exploration and Practice in Pytorch
date: 2024-11-12 10:06:20
toc: true
tags:
    - Transformer
    - NLP
categories: projects
excerpt: This post explores the principles about the impressive transformer structure and for downstream tasks, such as machine translate, it achieves the full implementation and training details.
---
## Introduction
Transformer 是一种广泛应用与自然语言处理的神经网络架构，它基于自注意力机制，允许模型在做出预测时为句子中的不同单词赋予不同的重要性。它非常擅长处理序列任务，并且具有并行计算的优势，因此在大规模数据集上训练时非常高效。序列任务是任何将输入序列进行变换得到输出序列的任务，例如 machine translation, text summarization, and question answering. 而这种序列模型往往具有编码-解码的模型架构，Transformer 亦是如此：**编码器将输入的符号序列映射为提取的连续特征表示，而解码器负责一次生成一个符号，并在每一步将之前生成的符号再次添加到输入以此生成下一个符号，又称为自回归模型。** 这种依赖于过去和当前的输入的任务，也被称为**因果语言建模** (causal language modeling)。

在这篇文章中，我将探索对 Transformer 结构的学习以及在机器翻译任务上用Pytorch全流程实现Transformer。

## Understanding of Theories

### Tokenizer & Embedding

我们需要从原点出发理解整个处理过程，给定一个自然语言序列，需要做的工作包括对自然语言序列进行分词以及词嵌入，能够将自然语言的单词转换为Transformer模型需要处理的向量化表示。如下图所示，自然语言单词通过语法规则构造出规范的语句，而自然语句通过分词器将语句分级为 tokens，有时候为了处理方便，也会将自然语言单词进行拆分构成不同的token，这取决于分词器的实现。

分词后的tokens序列主要用来构造模型学习的语料库，而词嵌入 embedding 则是将tokens序列转换为连续的向量表示 embeddings，以便模型能够处理整个语句。经过这种变换后，自然语言单词能够转换为浮点数构成的数值向量，这不仅考虑了token的特异性，而且数值能够表示不同token之前的联系，即语境信息。

这种处理方式使得模型能够处理人类的自然语言，并且能够捕捉到不同单词之间的语义关系。

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/projects/ConceptPlot.drawio.svg" alt="Convert Pipline" width="70%"/>

在数据管理器中，基于 `torchtext` 实现了用于文本分词的 `tokenizer` 以及对应的 Vocabulary.

整体的流程是，通过预训练的 tokenizer 将输入的文本进行分词，并将单个 token 输出为 token_id，进一步通过输入的语料库来构建词汇表，在词汇表中可以通过 token_id 查找对应的 embedding，这是作为单词在句子中特殊语义的标记。

一些特殊的 token 标记：
+ `PAD_IDX`：由于在一个 batch 中不同的语句所转换后的 tokens 长度不一，为了能够统一转换为矩阵，需要对这些语句进行对齐，可以理解为以最长的 tokens 序列为标准，以一个特殊的标记填充其他语句。
+ `EOS_IDX`: 有填充就必定要有语句结束标记，指定一个语句在哪个位置已经结束。
+ `BOS_IDX`: 标记句子的开始，一般是以该 token 为解码器输入，然后逐渐生成我们想要的其他 tokens，所以可以认为这是解码器的特殊启动标记。


```python Data Manager
"""
@author: Zhihao Li
@date: 2024-11-11
@homepage: https://zhihaoli.top/
"""
import io
from collections import Counter
import torch
from torch.nn.utils.rnn import pad_sequence
from torch.utils.data import DataLoader
import torchtext
torchtext.disable_torchtext_deprecation_warning()
from torchtext.data.utils import get_tokenizer
from torchtext.vocab import vocab
from torchtext.utils import extract_archive


class DataManeger:
    """
    A integrated data manager with builded tokenizer and vocabulary.
    """
    def __init__(self, src_mode, tgt_mode, data_path):
        """
        Args:
            src_mode: source natural language, ('en': English, 'de': Deutsch / German', 'cs': Čeština / Czech, 'fr': Français / French).
            tgt_mode: target natural language, ('en': English, 'de': Deutsch / German', 'cs': Čeština / Czech, 'fr': Français / French).
            data_path: the path of dataset.
        """
        self.src_mode = src_mode
        self.tgt_mode = tgt_mode

        self.tokenize_src = get_tokenizer('spacy', language=src_mode)
        self.tokenize_tgt = get_tokenizer('spacy', language=tgt_mode)

        train_urls = ('train.'+ src_mode +'.gz', 'train.'+ tgt_mode +'.gz')
        val_urls = ('val.'+ src_mode +'.gz', 'val.'+ tgt_mode +'.gz')
        test_urls = ('test_2016_flickr.'+ src_mode +'.gz', 'test_2016_flickr.'+ tgt_mode +'.gz')

        self.train_filepaths = [extract_archive(data_path + url)[0] for url in train_urls]
        self.val_filepaths = [extract_archive(data_path + url)[0] for url in val_urls]
        self.test_filepaths = [extract_archive(data_path + url)[0] for url in test_urls]

        self.src_vocab = self.build_vocab(self.tokenize_src, self.train_filepaths[0])
        self.tgt_vocab = self.build_vocab(self.tokenize_tgt, self.train_filepaths[1])

        self.src_vocab.set_default_index(self.src_vocab['<unk>'])
        self.tgt_vocab.set_default_index(self.tgt_vocab['<unk>'])


    def make_dataset(self):
        """
        Process out the data through their zip files.
        """
        train_data = self.data_process(self.train_filepaths)
        val_data = self.data_process(self.val_filepaths)
        test_data = self.data_process(self.test_filepaths)

        return train_data, val_data, test_data

    def build_vocab(self, tokenizer, train_filepath):
        """
        Build the corresponding vocabulary for the two languages.
        """
        counter = Counter()
        with io.open(train_filepath, encoding="utf8") as f:
            for string_ in f:
                counter.update(tokenizer(string_))
        return vocab(counter, specials=['<unk>', '<pad>', '<bos>', '<eos>'])

    def data_process(self, filepaths):
        """
        Create the input_id tensors using tokenizer and vocabulary.
        """
        raw_src_iter = iter(io.open(filepaths[0], encoding="utf8"))
        raw_tgt_iter = iter(io.open(filepaths[1], encoding="utf8"))
        data = []
        for (raw_src, raw_tgt) in zip(raw_src_iter, raw_tgt_iter):
            src_tensor = torch.tensor([self.src_vocab[token] for token in self.tokenize_src(raw_src)],
                                    dtype=torch.long)
            tgt_tensor = torch.tensor([self.tgt_vocab[token] for token in self.tokenize_tgt(raw_tgt)],
                                    dtype=torch.long)
            data.append((src_tensor, tgt_tensor))
        return data

    def make_iter(self, train, validate, test, batch_size):
        """
        Create the iterater for sub-dataset using collection function.
        """
        train_iter = DataLoader(train, batch_size=batch_size,
                                shuffle=True, collate_fn=self.generate_batch)
        valid_iter = DataLoader(validate, batch_size=batch_size,
                                shuffle=False, collate_fn=self.generate_batch)
        test_iter = DataLoader(test, batch_size=batch_size,
                            shuffle=False, collate_fn=self.generate_batch)
        return train_iter, valid_iter, test_iter

    def generate_batch(self, data_batch):
        """
        Construct the batch input_id tensors, add the bos and eos tokens and padding the sentence.
        """
        SRC_PAD_IDX, TGT_PAD_IDX = self.src_vocab['<pad>'], self.tgt_vocab['<pad>']
        SRC_BOS_IDX, TGT_BOS_IDX = self.src_vocab['<bos>'], self.tgt_vocab['<bos>']
        SRC_EOS_IDX, TGT_EOS_IDX = self.src_vocab['<eos>'], self.tgt_vocab['<eos>']
        src_batch, tgt_batch = [], []
        for (src_item, tgt_item) in data_batch:
            src_batch.append(torch.cat([torch.tensor([SRC_BOS_IDX]), src_item, torch.tensor([SRC_EOS_IDX])], dim=0))
            tgt_batch.append(torch.cat([torch.tensor([TGT_BOS_IDX]), tgt_item, torch.tensor([TGT_EOS_IDX])], dim=0))

        # padding the sentence using PAD_IDX
        src_batch = pad_sequence(src_batch, padding_value=SRC_PAD_IDX)
        tgt_batch = pad_sequence(tgt_batch, padding_value=TGT_PAD_IDX)
        return src_batch.t(), tgt_batch.t()
```

### Position Embedding

+ 并行处理

其实可以发现，transformer 是并行处理一个语句中的所有 tokens，因为它同时接受这些 tokens 作为输入，接着直接计算注意力分数。

+ 位置信息

> 不同的 token 在语句的不同位置是语法体现，因此需要明确位置信息。

因此仅仅是单个 token 的嵌入向量，并不能表示在语句中的位置关系，这就需要额外引入能够表示 token 在语句中的位置信息。而位置信息需要满足的要求有如下两点，

1. It should be the same for a position irrespective of the token in that position. So while the sequence might change, the positional embeddings must stay the same. [1]
2. They should not be too large, or otherwise they will dominate semantic similarity. [1]

+ 函数选取
Position Embedding 不能够太大以免破坏 token 本身的语义信息。因此对于非周期函数例如线性函数，因为值域是无限的，并不容易控制随着维度增大引起的值域增大。

较好的选择就是正余弦函数，它们的值域都缩放在 [-1, 1] 之间，连续且具有周期性。相比于 _sigmoid_ 函数对较大的数基本已经保持平稳，三角函数能够对较大的数具有较大变换幅度，这对于处理长序列是非常有用的。

为了避免三角函数对于不同位置重复相同的结果，给定三角函数一个较低的频率，即具有较大的周期，这将对于最长的序列长度也不会不断重复。频率低就意味着相邻位置变化幅度比较小，这也不是我们想要的，因此对位置编码的奇数维度叠加低频 _sine_ 函数，而对偶数维度叠加低频 _cosine_ 函数。

对于一个单词的嵌入向量：`torch.size([1, 512])`，其中 512 嵌入向量的奇数位置采用低频 _sine_ 函数，偶数位置采用低频 _cosine_ 函数，这样能够保证每个单词的嵌入向量都包含位置信息。

$$
\begin{aligned}
PE(pos, 2i) &= \sin(\frac{pos}{1000^{2i/d_{model}}})\newline
PE(pos, 2i+1)& = \cos(\frac{pos}{1000^{2i/d_{model}}})
\end{aligned}
$$

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/blog/20241118114521.png" alt="pos=28 时对应的嵌入向量位置编码表示" width="40%"/>

从上图可以看到，这种交叉位置编码平衡了单独两个余弦函数的特性，能够在相邻位置保持变化性，并且对于长序列的位置编码也不会出现大量重复值。

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/blog/20241118114756.png" alt="dim=512 时交叉位置编码表示" width="40%"/>

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/blog/20241118114852.png" alt="dim=512 时正弦位置编码表示" width="40%"/>

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/blog/20241118114928.png" alt="dim=512 时余弦位置编码表示" width="40%"/>

对比交叉、正弦以及余弦位置编码可以看出，交叉位置编码在不同维度是不断变化的，而单独的正弦和余弦函数都出现了较为平滑的区域，即变换幅度都基本不变。

```python
class PositionalEncoding(nn.Module):
    # Implement the position encoding (PE) function.

    def __init__(self, d_model, dropout, max_len=5000):
        super(PositionalEncoding, self).__init__()
        self.dropout = nn.Dropout(p=dropout)

        # Compute the positional encodings once in log space.
        pe = torch.zeros(max_len, d_model)
        position = torch.arange(0, max_len).unsqueeze(1)
        div_term = torch.exp(
            torch.arange(0, d_model, 2) * -(math.log(10000.0) / d_model)
        )
        pe[:, 0::2] = torch.sin(position * div_term)
        pe[:, 1::2] = torch.cos(position * div_term)
        pe = pe.unsqueeze(0)
        self.register_buffer("pe", pe)

    def forward(self, x):
        # adds token embedding to its position embedding
        x = x + self.pe[:, : x.size(1)].requires_grad_(False)
        return self.dropout(x)
```

### Encoder

编码器负责从输入的 token 序列中提取出语义特征，其结构如下图所示：
<div style="display: flex; align-items: center; gap: 40px;">
  <img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/blog/20241118134352.png" style="flex: 1; width: 100%;" >
  <div>
    <h3>Residual Connection</h3>
    <p>
      残差连接是将该层的输入向量直接传递到输出而不做任何处理，并将其加到该层处理后得到的输出向量上面。这是一项简单高效的技术用于处理深度神经网络梯度消失的问题，以 ResNet 网络之名提出.
    </p>
    <h3>Layer Normalization</h3>
    <p>
      层归一化是在每层中对所有样本的输出进行规范化，而不是对每个批次进行规范化。如下图中对比，Layer Norm 对于单个样本的所有特征进行规范化，使得层内神经元输出的分布具有稳定的均值和方差。
    </p>
    <p>
      在 Transformer 中是对每个 token 形成的 embedding 进行规范化，而不是对整个序列进行规范化。
      然后使用可学习的参数（如 $\beta$ 和$\gamma$）对归一化后的输出进行缩放和平移。这样既可以保持数据的分布稳定性，又可以保留一定的灵活性。形式化的表示为：
      $$
      \text{LN}(x) = \frac{x - \mu}{\sigma + \epsilon} \cdot \gamma + \beta
      $$
      其中，$x$ 是输入向量，$\mu$ 和 $\sigma$ 是输入向量的均值和标准差，$\epsilon$ 是一个很小的常数，用于防止除以零，$\gamma$ 和 $\beta$ 是可学习的参数。
    </p>
    <p>
    在 Transformer 中，对于层归一化可以放置在 Attention 层和前馈神经网络层之后，也可以放置在它们之后。最初的 Transformer 论文中，层归一化采取的是第一种方法，但被证明很难训练到梯度收敛，而第二种方法训练时变得更加稳定且收敛更快。[1]
    </p>
  </div>
</div>


<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/blog/20241118143922.png" alt="Image source: Wu, Y., & He, K. (2018). Group normalization. ECCV" width="70%"/>

```python Layer Normalization
class LayerNorm(nn.Module):
    "Construct a layer norm module "

    def __init__(self, d_model, eps=1e-6):
        super(LayerNorm, self).__init__()
        self.a_2 = nn.Parameter(torch.ones(d_model))
        self.b_2 = nn.Parameter(torch.zeros(d_model))
        self.eps = eps

    def forward(self, x):
        mean = x.mean(-1, keepdim=True)
        std = x.std(-1, keepdim=True)
        return self.a_2 * (x - mean) / (std + self.eps) + self.b_2
```

### Multi-Head Attention
多头注意力机制实际上是包含多个自注意力头的一种机制，每个头都独立地学习输入序列中的不同模式。多头注意力机制可以捕获更多的信息，并且可以更好地处理长距离依赖关系。多头注意力机制的结构如下图所示：

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/blog/20241118145855.png" alt="Multi-Head Attention [1]" width="70%"/>

其中，$d_{model}$ 是设定的每个 embedding 所包含的特征数量，实际上对于该超参数的设定，有时候并不清楚是否特征表示冗余（即浪费了很多特征块），或者是特征表示不足（即特征块不够）。

面对这样的问题，与其单独计算一个有着冗余风险的超大自注意力头，不如将这些所有特征分组成 $h$ 组，每组包含 $d_{model}/h$ 个特征，然后分别对每组进行自注意力计算，最后将所有组的输出拼接起来。这样能够保证每个子注意力头完成一个子任务，即捕获子模式：不同位置和不同特征的信息，从而更好地处理输入序列中的复杂关系。

#### Self-Head Attention
子注意力头主要是关注于序列本身中每个token与序列中其他token的依赖关系以及相似度，计算的注意力也成为：Scaled dot-product attention。

首先，将序列的嵌入特征表示投影成不同的三个向量，记为 query, key and value。然后计算注意力分数，通过测量 query 和 key 的点积来衡量 query 和 key 之间的相似度。这是因为点积可以衡量向量之间的相似性，如果非常接近则点积结果会有一个较大的值。一个有 $n$ 个 token 的序列来计算相互之间的相似度，即 Pairwise Similarity 将会得到 $n\times n$ 的注意力分数。

在获得注意力分数之后，因为点积结果是两个高维向量相乘并求和的结果，取值范围属于无限大，如果直接参与后续计算，势必会扰乱特征信息。因此，需要对注意力分数进行缩放，即除以 $\sqrt{d_k}$，其中 $d_k$ 是 key 的维度。然后通过 softmax 将其转换为注意力权重，这样做的目的是为了平衡不同维度之间的差异，使得计算结果更加稳定。

真正表示 token 语义的一直是 value 向量，通过构建的 query 和 key 只是获取 token 之间的注意力权重，然后对 value 向量中的每一个 token 进行加权求和，可以得到依赖于**目前学习到的** token 间语义关系的**加权平均的**嵌入特征表示。这里有两个特定词，希望给出一些个人的理解：
+ 目前学习到的
可以看到，对 query, key and value 的投影矩阵都是不断学习的参数，transformer 训练过程中，会不断通过学习调整 query, key 以提取更加准确的 token 间的语义依赖关系，这也会是 value 向量再次更新的关键，等到学习基本完毕时，我们可以任务，value 向量已经集成了之前所探寻得到的语义关系，代表了能够真正理解这句话的真实含义。

+ 加权平均的
注意到注意力权重是通过 softmax 归一化的相似度分数，即对于注意力权重形如 $L\times L$，其中 $L$ 表示序列长度，每一行都表示对应的 token 与序列中其他 token 的语义关系（相似性），这样作用于 value 向量时，都会根据注意力分数提取其他相似的 token 的语义信息，从而得到一个加权平均的语义表示。

因此更加具体的实现还是自注意力头，假设输入的嵌入向量表示为 $E\in R^{B\times L\times D}$，其中 $B$ 表示批次大小，$L$ 表示序列长度，$D$ 表示每个 token 被编码表示的向量长度，那么具体的计算过程如下：

$$
\begin{aligned}
\text{Q} &= \text{W}_Q E \in R^{B\times L\times D} \newline
\text{K} &= \text{W}_K E \in R^{B\times L\times D} \newline
\text{V} &= \text{W}_V E \in R^{B\times L\times D} \newline
\text{Attention}(Q,K,V) &= \text{softmax}\left(\frac{QK^T}{\sqrt{D}}\right)V
\end{aligned}
$$

当采用多头注意力机制后，还需要对拼接每个子注意力头得到的注意力分数进行线性变换，这是因为多头注意力机制不仅学习序列的注意力特征，而且学习每一个子注意力头对注意力分数的贡献程度，具体计算如下：
$$
\begin{aligned}
\text{MultiHead}(Q,K,V) &= \text{Concat}(\text{head}_1, \text{head}_2, \ldots, \text{head}_h)W^O \newline
\text{where} \quad \text{head}_i &= \text{Attention}(Q, K, V)
\end{aligned}
$$
```python
def attention(query, key, value, mask=None, dropout=None):
    "Compute 'Scaled Dot Product Attention'"
    d_k = query.size(-1)
    scores = torch.matmul(query, key.transpose(-2, -1)) / math.sqrt(d_k)

    if mask is not None:
        scores = scores.masked_fill(mask == 0, -1e9)
    p_attn = scores.softmax(dim=-1)
    if dropout is not None:
        p_attn = dropout(p_attn)
    return torch.matmul(p_attn, value), p_attn


class MultiHeadedAttention(nn.Module):
    def __init__(self, n_head, d_model, dropout=0.1):
        # Take in model size and number of heads.
        
        super(MultiHeadedAttention, self).__init__()
        assert d_model % n_head == 0
        # We assume d_v always equals d_k
        self.d_k = d_model // n_head
        self.n_head = n_head
        self.linears = clones(nn.Linear(d_model, d_model), 4)
        self.attn = None
        self.dropout = nn.Dropout(p=dropout)

    def forward(self, query, key, value, mask=None):
        nbatches = query.size(0)

        # Do all the linear projections in batch from d_model => n_head x d_k
        query, key, value = [
            lin(x).view(nbatches, -1, self.n_head, self.d_k).transpose(1, 2)
            for lin, x in zip(self.linears, (query, key, value))
        ]

        # Apply attention on all the projected vectors in batch.
        x, self.attn = attention(
            query, key, value, mask=mask, dropout=self.dropout
        )

        # Concat using a view and apply a final linear.
        x = (
            x.transpose(1, 2)
            .contiguous()
            .view(nbatches, -1, self.n_head * self.d_k)
        )
        return self.linears[-1](x)
```
### Feed-Forward Network
前馈神经网络就是一个简单的两层全连接层，通常第一层的隐藏层大小设置为 $4d_{model}$，并且使用 ReLU 作为激活函数，具体实现如下：
```python
class FeedForward(nn.Module):
    
    def __init__(self, d_model, d_ff=2048, dropout=0.1):
        super().__init__() 
    
        # We set d_ff as a default to 2048
        self.linear_1 = nn.Linear(d_model, d_ff)
        self.dropout = nn.Dropout(dropout)
        self.linear_2 = nn.Linear(d_ff, d_model)
    
    def forward(self, x):
        x = self.dropout(F.relu(self.linear_1(x)))
        return self.linear_2(x)
```

### Decoder
解码器的任务是不断地生成文本，还记得上文中提到的，`BOS_IDX` token 这个特殊的 token 标记句子的开始，可以先理解为解码器最开始输入的句子就是只有一个开始标记，然后不断地往下生成 $n$ 个单词，组成一句完整的话。但是对于 Transformer 而言，由于其强大的并行处理能力，实际上是通过对目标句子加阶梯型掩码（表示token生成的顺序），然后通过注意力机制不断得到一个加权平均的嵌入向量。实际上，这个嵌入向量表示就是 transformer 生成的目标句子，而且是一次性生成的。

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/blog/20241118200337.png" alt="Decoder Architecture [2]" width="50%"/>


由于代码结果解释性比较强，为了深入地揭示 what happened 在 Decoder 中，下文主要结合代码执行结果进行说明。

+ Decoder 输入的目标语句信息
从下面可以看到，目标语句长度 padding 到了 40 tokens 而且对应的每一个序列的第一个 token 都是 bos，说明在处理的时候 Decoder 还是以 bos 开始处理。
```python
>>> target sentence length: 40
>>> target bos token id: 2
>>> target eos token id: 3
>>> target pad token id: 1
>>> target first token id:
 tensor([2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
        2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
        2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
        2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
        2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
        2, 2, 2, 2, 2, 2, 2, 2], device='cuda:1')
>>> target last token id:
 tensor([ 1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,
         1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1, 15,  1,
         1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,
         1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,
         1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,
         1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,
         1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,  1,
         1,  1], device='cuda:1')
```

+ Decoder 输入的 mask 信息
Decoder 需要考虑句子生成的先后顺序，在生成第 $i$ 个 token 的时候，只能看到第 $i$ 个 token 之前的 tokens，所以需要通过 mask 来实现，因此第一个 mask 记为 padding mask，第二个 mask 记为 subsequent mask，最后需要将这两个 mask 进行想与得到总的 mask，具体如下：
```python
>>> target padding mask shape: torch.Size([128, 1, 40, 1])
>>> target padding mask:
tensor([[[[ True,  True,  True,  ..., False, False, False]]],


        [[[ True,  True,  True,  ..., False, False, False]]],


        [[[ True,  True,  True,  ..., False, False, False]]],


        ...,


        [[[ True,  True,  True,  ..., False, False, False]]],


        [[[ True,  True,  True,  ..., False, False, False]]],


        [[[ True,  True,  True,  ..., False, False, False]]]], device='cuda:1')

>>> target sub mask shape: torch.Size([40, 40])
>>> target sub mask:
tensor([[1, 0, 0,  ..., 0, 0, 0],
        [1, 1, 0,  ..., 0, 0, 0],
        [1, 1, 1,  ..., 0, 0, 0],
        ...,
        [1, 1, 1,  ..., 1, 0, 0],
        [1, 1, 1,  ..., 1, 1, 0],
        [1, 1, 1,  ..., 1, 1, 1]], device='cuda:1', dtype=torch.uint8)

>>> target sentence mask shape:
torch.Size([128, 1, 40, 40])
>>> target sentence mask:
tensor([[[[1, 0, 0,  ..., 0, 0, 0],
          [1, 1, 0,  ..., 0, 0, 0],
          [1, 1, 1,  ..., 0, 0, 0],
          ...,
          [0, 0, 0,  ..., 0, 0, 0],
          [0, 0, 0,  ..., 0, 0, 0],
          [0, 0, 0,  ..., 0, 0, 0]]],


        [[[1, 0, 0,  ..., 0, 0, 0],
          [1, 1, 0,  ..., 0, 0, 0],
          [1, 1, 1,  ..., 0, 0, 0],
          ...,
          [0, 0, 0,  ..., 0, 0, 0],
          [0, 0, 0,  ..., 0, 0, 0],
          [0, 0, 0,  ..., 0, 0, 0]]],


        [[[1, 0, 0,  ..., 0, 0, 0],
          [1, 1, 0,  ..., 0, 0, 0],
          [1, 1, 1,  ..., 0, 0, 0],
          ...,
          ...,
          [0, 0, 0,  ..., 0, 0, 0],
          [0, 0, 0,  ..., 0, 0, 0],
          [0, 0, 0,  ..., 0, 0, 0]]]], device='cuda:1', dtype=torch.uint8)
```

+ Decoder Multi-Head Attention
解码器需要考虑两个序列，一是已经生成的序列（加掩码的目标序列），另一个是编码器提取的语义特征，这是为了进行两个序列的语义对齐，尤其是将 decoder attention 作为 query，encoder attention 作为 key、value。
    + **直观的理解**
    解码器向编码器提出一个查询请求，寻找下一个需要生成的 token，此时就需要比较解码器查询与编码器的特征表示的相似度，以此作为注意力分数，注意这个地方是不能在目标序列中得到下一个 token 的，因此 value 只能是编码器的 attention 输出，通过运算后这样会得到加权平均的语义特征，通过 projector 将这些语义特征投影到目标序列的词汇表中做一次分类，即可实现 token 的筛选。
    + **特征表示层面**
    通过自注意力机制，解码器提取出的特征表示为 $L_1\times D$，编码器提取出的语义特征为 $L_2\times D$, 其中$ L_1, L_2$ 表示目标序列以及源序列的 token 长度，而 $D$ 表示每一个 token 的特征长度。实际上计算应为：
    $$
    \begin{aligned}
    L_1\times D \cdot D\times L_2 &= L_1\times L_2\newline
    L_1\times L_2 \cdot L_2\times D &= L_1\times D
    \end{aligned}
    $$
    通过这种交叉注意力机制，解码器的每一个 token 都能够得到一个关于源序列各个 tokens 的表示关联程度的注意力权重，通过这个注意力权重与编码器提取出的语义特征，在 token 的没一个维度上进行加权求和，这样会得到相对于源序列的语义特征，这就是最后要生成的 tokens 序列。
    + **并行处理**
    一次性生成整个句子？
    其实深入地观察，可以发现，在解码器获取语义特征的过程中，施加了上面提到的掩码操作，这样就能够同时获得将要生成的 tokens 序列的位置关系，通过自注意力机制便一次性提取出所有 token 的语义特征，直接可以作为生成的 tokens 序列的特征。为了与源序列进行语义对齐，需要和编码器的语义特征计算相似度以获得源序列的注意力权重，再对源序列的语义特征进行加权平均。

```python Decoder
class DecoderLayer(nn.Module):
    "Decoder is made of self-attn, src-attn, and feed forward (defined below)"

    def __init__(self, n_head, d_model, d_ff, dropout):
        super(DecoderLayer, self).__init__()
        self.d_model = d_model
        self.self_attn = MultiHeadedAttention(d_model=d_model, n_head=n_head)
        self.cross_attn = MultiHeadedAttention(d_model=d_model, n_head=n_head)
        self.feed_forward = FeedForward(d_model, d_ff, dropout)
        # 3 add & norm sublayers one for self-attn, one for cross-attn and one for feed forward
        self.sublayer = clones(SublayerConnection(d_model, dropout), 3)

    def forward(self, dec, enc, src_mask, tgt_mask):
        "Compute self attention, cross attention, positionwise feed forward network.."

        dec = self.sublayer[0](dec, lambda dec: self.self_attn(dec, dec, dec, tgt_mask))
        dec = self.sublayer[1](dec, lambda dec: self.cross_attn(dec, enc, enc, src_mask))
        return self.sublayer[2](dec, self.feed_forward)


class Decoder(nn.Module):
    "Generic N layer decoder with masking."

    def __init__(self, dec_voc_size, max_len, n_layers, n_head, d_model, d_ff, dropout):
        super(Decoder, self).__init__()
        decoder_layer = DecoderLayer(n_head, d_model, d_ff, dropout)
        self.layers = clones(decoder_layer, n_layers)
        self.emb = Embedding(vocab_size=dec_voc_size,
                            d_model=d_model,
                            max_len=max_len,
                            dropout=dropout)

        self.norm = LayerNorm(decoder_layer.d_model)

    def forward(self, tgt, enc_src, src_mask, tgt_mask):
        tgt = self.emb(tgt)    # embedded the input_ids

        for layer in self.layers:
            tgt = layer(tgt, enc_src, src_mask, tgt_mask)
        return self.norm(tgt)
```
### Transformer

在完成上述各模块的设计后，可以得到完整的 Transformer 模型，其结构如下：
```python Transformer
"""
@author: Zhihao Li
@date: 2024-11-11
@homepage: https://zhihaoli.top/
"""
import torch
import torch.nn as nn
from torch.nn.functional import log_softmax

from model.encoder import Encoder
from model.decoder import Decoder

class Generator(nn.Module):
    "Define standard linear + softmax generation step."

    def __init__(self, d_model, vocab):
        super(Generator, self).__init__()
        self.proj = nn.Linear(d_model, vocab)

    def forward(self, x):
        return log_softmax(self.proj(x), dim=-1)


class Transformer(nn.Module):
    """
    A standard Transformer architecture. Base for this and many
    other models.
    """

    def __init__(self, src_pad_idx, tgt_pad_idx, tgt_bos_idx, enc_voc_size,
                 dec_voc_size, d_model, n_head, max_len, d_ff, n_layers, dropout, device):
        super().__init__()
        self.src_pad_idx = src_pad_idx
        self.tgt_pad_idx = tgt_pad_idx
        self.tgt_bos_idx = tgt_bos_idx
        self.device = device
        self.encoder = Encoder(enc_voc_size=enc_voc_size,
                            max_len=max_len,
                            n_layers=n_layers,
                            n_head=n_head,
                            d_model=d_model,
                            d_ff=d_ff,
                            dropout=dropout)
        self.decoder = Decoder(dec_voc_size=dec_voc_size,
                            max_len=max_len,
                            n_layers=n_layers,
                            n_head=n_head,
                            d_model=d_model,
                            d_ff=d_ff,
                            dropout=dropout)

        self.generator = Generator(d_model, dec_voc_size)

    def forward(self, src, tgt):
        "Take in and process masked src and target sequences."
        src_mask = self.make_src_mask(src)
        tgt_mask = self.make_tgt_mask(tgt)
        enc_src = self.encoder(src, src_mask)
        dec_tgt = self.decoder(tgt, enc_src, src_mask, tgt_mask)
        return self.generator(dec_tgt)

    def make_src_mask(self, src):
        """
        Mask the padding tokens int source sentence.
        """
        src_mask = (src != self.src_pad_idx).unsqueeze(1).unsqueeze(2)
        return src_mask

    def make_tgt_mask(self, tgt):
        """
        Mask the padding tokens int target sentence.
        """
        tgt_pad_mask = (tgt != self.tgt_pad_idx).unsqueeze(1).unsqueeze(3)
        tgt_len = tgt.shape[1]
        tgt_sub_mask = torch.tril(torch.ones(tgt_len, tgt_len)).type(torch.ByteTensor).to(self.device)
        tgt_mask = tgt_pad_mask & tgt_sub_mask
        return tgt_mask
```

## Exploration From Scratch

### Preparation

#### Clone Project

准备探索之前，需要将 <i class="iconfont icon-github7" style="font-size: 20px;"></i> [TransformerPractice](https://github.com/LZHMS/TransformerPractice) 项目克隆下来，可以使用如下命令克隆到本地：

```sh
git clone https://github.com/LZHMS/TransformerPractice.git
```

项目中已经集成好了所有必要的模型组件并通过不同的 Trainers 串联起来，以完成特定的下游任务。

#### Install Conda Environment

安装 conda 环境，tokenizer 使用最新的 spacy 库，其他库的版本也都是兼容下比较新的，可以通过以下命令进行环境配置：

```sh
conda env create -f environment.yml
```

#### Download the Dataset

本项目使用 [Multi30K Dataset](https://github.com/multi30k/dataset) 数据集训练和评估文本翻译模型，具体需要先在官网上下载数据集然后提取 `task1` 的所有文件，将其放置在目录 `data/multi30k` 下。详细目录结构可以见下文：

```python Category Structure
.
├─ data
│  ├─ multi30k
│  │  ├─ task1
│  │  │  ├─ ...
├─ dataset
├─ model 
├─ output
├─ trainer
└─ model
```

### Explore the Modules
对于 Transformer 处理流程的探索，可以在 [Jupyter Notebook](https://github.com/LZHMS/TransformerPractice/blob/main/main.ipynb) 中单步演示。

为了更好地体验，可以结合 _The Transformer Architecture: A Visual Guide_ [2] 对比分析。

### Training the Models
一次性训练文本翻译器，可以通过以下命令：
```python
python main.py --epochs 1000 > output/output.log
```

## Reference

+ \[1\] [Transformer: Concept and code from scratch](https://mina-ghashami.github.io/posts/2023-01-10-transformer/)
+ \[2\] [The Transformer Architecture: A Visual Guide](https://www.hendrik-erz.de/post/the-transformer-architecture-a-visual-guide-pdf-download)