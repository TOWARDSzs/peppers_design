---
title: 【bioinfo】-Q&A-23
date: 2020-02-18 15:32:16
tags:
- AQ100
- mapping
categories:
- bioinfo
- ngs
copyright:
---
## 第23题 转录组的比对与基因组的比对有何不同？
Hello大家好！我们今天又见面了！

我们通过前期的22个问题，从数据的简单质控，到测序数据的mapping，再到mapping后的SAM文件都有了一个比较清楚的认识。那么说了半天的mapping问题，一直都是在以DNA进行举例，RNA的比对我们都还没有谈。那么今天我们就来简单谈谈RNA序列的mapping，尤其是真核生物的RNA序列比对。

### 1. RNA与DNA结构的不同
一般来说，DNA的mapping比较容易，因为DNA在基因上是连续的，直接回贴到基因组就可以找到相应的定位。

就比如我们常用的**Whole Genome Sequence（WGS）即全基因组测序**；

或者是我们所说的**ChIP-Seq即染色体免疫共沉淀测序**都是直接对DNA进行建库测序，其测序结果都是FASTQ文件，直接用**bowtie2，bwa比对到基因组就可以拿到标准的SAM文件**。

但是RNA就不一样了，真核生物的RNA需要经过复杂的加工过程。在细胞中RNA层面的调控至少可以分成2个大的阶段

* co-transcription（转录的同时）

* post-transcription（转录以后）

其中的调控机制也有很多。

对我们mapping影响最大的因素是：**真核生物转录出来的初步的mRNA都是带有intron（内含子）的**，随后都需要在co-transcription（转录的同时） 或post-transcription（转录以后）阶段通过：

1. alternative splicing（可变剪切）剪切掉intron；

2. polyA尾巴；

3. 加5’的帽子结构。

这3个步骤，将不成熟的mRNA变为最终成熟的mRNA再转运出核，行使功能。

![](1.jpg)
图1 通过可变剪切同1个基因可以形成多种蛋白（https://en.wikipedia.org/wiki/Alternative_splicing）

而我们在进行RNA-Seq建库的时候，一般情况下都是使用**oligo dT针对带polyA尾巴的成熟mRNA进行富集，然后再进行反转录获得cDNA，之后再使用cDNA进行建库测序**。所以，我们最终得到的测序结果是与成熟mRNA序列保持一致的，只包含了**exon的序列**。

而exon在基因中间是有intron分割的，因此在**回贴回基因组的时候，回遇到跨越intron的reads回贴**。所以这个问题不能使用原来针对DNA的mapping策略进行mapping。

### 2. RNA比对的常用软件
目前大家最常用的转录组比对软件有下面几个：

1. tophat2，应用最广泛的比对软件，但是速度很慢，已经基本被淘汰了，大约需要4~5G内存就能运行；
2. hisat2，tophat2的原班人马搞得新一代转录组比对软件，**比对速度大大提高，我强烈推荐，大约需要4~5G内存就能运行；**
3. STAR，非常适合于**大量数据的并行计算，速度非常快，对于同时有参考基因组和参考转录组的物种，比对的准确率很高，不过index很大，至少需要30G以上内存才能运行。**

### 3. 提出问题
**问题1：**
如果你有一套标准的polyA捕获得到的RNA-Seq测序数据，对reads进行了前处理工作与质量控制工作，但是你的比对策略为：先尝试mapping，把能mapping到基因组上的reads都先mapping；然后把不能进行mapping的reads进行一定规则的拆分，再进行第二轮mapping，从而解决跨intron区域的问题（以上为tophat的mapping策略）。请问，这样mapping的最大问题是什么？（提示，需要知道一些假基因的概念！）
```
先解释一下假基因，假基因（Pseudogenes）是指一类染色体上的基因片段。假基因的序列通常与对应的基因相似，但至少是丧失了一部分功能，基因不能表达或其编码的蛋白质没有功能；

这种基因在基因组上的分布非常普遍，那么在假基因普遍存在的情况下上述比对策略就会受到假基因的干扰，会有很多基因比对到假基因上
```
**问题2：
在human中，是不是所有的蛋白基因（protein coding gene）都含有intron？**
```
并不是，SRY基因是人体Y染色体上的一段基因，该基因是决定男性睾丸发育的主要基因，存在于Y染色体的短臂末端上，该基因只有一个exon
```
![](2.png)

**问题3：
在human中，是不是所有的蛋白基因的成熟mRNA都有polyA尾巴？**

并不是，组蛋白mRNA末端就没有polyA尾巴
