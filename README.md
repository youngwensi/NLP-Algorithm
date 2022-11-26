# NLP-Algorithm
### 关键词提取

关键词是快速了解文档内容以及把握文档主题的重要方式，文档的关键词通常是几个词或者词组，代表该文档的主要内容的提要
在我们的业务中，我们采用过三种方法提取文本关键词：

- 1）RAKE

RAKE(Rapid Automatic Keyword Extraction) ,中文称为快速自动关键字提取。其亮点在于Rapid,快。
其算法特点其一是简单而高效；其二是提取的关键词并不是单一的单词，也有可能是短语，能够提取一些较长的专业术语。

算法步骤：

(1)【分词】算法首先对句子进行分词，分词后去除停用词，根据停用词划分短语; 

(2)【共现举证】计算每一个词在短语的共现词数,并构建 词共现矩阵; 

(3)【特征提取】共现矩阵的每一列的值即为该词的度deg（是一个网络中的概念，每与一个单词共现在一个短语中，度就加1，考虑该单词本身）,每个词在文本中出现的次数即为频率freq; 

(4)【score定义】score=deg/feq,score越大则该词权重更重 ;

(5)【降序输出】最后按照得分的大小值降序， 输出1/3文档词汇量的关键词



- 2)textrank

基于图的排名方法在无监督关键词提取算法中非常流行。在开创性工作 TextRank 中引入了基于图形的关键短语提取方法。它为给定的文本构建一个加权图，其中节点由单词表示，并且只有当单词类型在指定大小的窗口内同时出现时，边缘才连接单词类型。这里的基本思想是每个节点都为其链接的节点贡献一个“投票”。一个节点所链接的多个节点对它的投票是聚合在一起的，这个聚合得分可以看作是一个节点的重要性。此外，节点投“票”的重要性由节点投票的重要性决定。因此，通过使用 Google 的个性化 PageRank 算法迭代地聚合每个节点的“投票”，可以获得每个节点的分数。该算法一直运行，直到为节点获得的分数在后续迭代之间不发生变化。一旦达到这个收敛标准，每个节点获得的分数将用于以相反的顺序对顶点进行排序。然后从排序列表中选择前 k 个顶点，其中 k 是用户指定的参数。前 k 个顶点对应于给定文本中的前 k 个关键字，可用于索引等下游任务。一些关键字也组合在一起形成关键短语。尽管 TextRank 给出的结果很有希望，但它存在一个问题。由 TextRank 形成的图中的边被统一加权。但这是不可取的，因为它可能导致不重要的词仅仅因为它们在给定文本中频繁出现而被分配更高的分数。此外，可以为连接两个频繁同时出现的词或罕见词的某些边分配更高的权重，以确保它出现在前 k 个结果中。

TextRank是一种基于随机游走的关键词提取算法，考虑到不同词对可能有不同的共现（co-occurrence），TextRank将共现作为无向图边的权值。

其实现包括以下步骤：

1)把给定的文本T按照完整句子进行分割；

2)对于每个句子，进行分词和词性标注处理，并过滤掉停用词，只保留指定词性的单词，如名词、动词、形容词，即，其中 ti,j 是保留后的候选关键词；

3)构建候选关键词图G = (V,E)，其中V为节点集，由2)生成的候选关键词组成，然后采用共现关系（co-occurrence）构造任两点之间的边，两个节点之间存在边仅当它们对应的词汇在长度为K的窗口中共现，K表示窗口大小，即最多共现K个单词；

4)根据上面公式，迭代传播各节点的权重，直至收敛；

5)对节点权重进行倒序排序，从而得到最重要的T个单词，作为候选关键词；

6)由5得到最重要的T个单词，在原始文本中进行标记，若形成相邻词组，则组合成多词关键词；

- 3)singlerank

SingleRank 算法是 TextRank 算法的扩展，其中为边缘分配的权重等于词汇表中不同单词的共现计数。《Single Document Keyphrase Extraction Using Neighborhood Knowledge》 一文中，通过在可变大小w≥2的窗口中共同出现的单词之间加上加权边缘，将TextRank扩展为SingleRank。
与TextRank不同的是，SingleRank保留所有的unigrams词，然后类似TextRank方法，滑动窗口方式计算更高的n-grams词，两个分值较低的unigram，有可能产生较高分值的bi-gram。作者表明，与 TextRank 算法相比，它可以带来更好的单词排名，在 TextRank 算法中，节点之间的所有边都被分配了统一的权重。另外，在SingleRank算法中，在使用PageRank排序机制为图中的节点分配分数后，并没有像TextRank算法那样过滤掉低分节点。相反，关键短语是通过对匹配的关键字进行分组来创建的，并且每个关键短语的分数被计算为组成关键字分数的总和。然后使用它们的分数对关键短语进行排名，并获得前 k 个关键短语作为输出。

- 4)端到端到实体识别方法

文本标注+bert
实体识别指识别文本中具有特定意义的实体，包括地名，组织，产品等。
对文档进行实体识别，也就是识别文本中的实体指称的边界和类别。帮助更好地理解文档内容，同时有利于文档从非结构化的数据向结构化的数据转化

### 文本分类：
1）svm

2)knn

3)bert




