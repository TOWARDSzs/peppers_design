---
title: 【bioinfo】-Q&A-81
date: 2020-02-24 21:01:31
tags:
- AQ100
- Molecular replacement
- structure
categories:
- bioinfo
- tech
copyright:
---
## molecular replacement是什么？

分子置换（MR）是一种定相方法，它使用与晶体中的成分相关或同源的已知结构形式的先验信息。 由于MR不需要其他实验程序或数据，并且可以简化模型的构建，因此，当有合适的搜索模型时，MR通常是确定结构的首选方法。

### Steps in structure solution with molecular replacement
#### Model selection
合适的模型的结构通常是通过同源性搜索找到的（例如对于合理的近亲使用NCBI Blast或对于更远的亲属使用HHpred），并且可以通过与靶序列的序列同一性来表征。

使用分子替代品的主要限制是需要适当相似的搜索模型。 尽管没有确切的规则，但是序列同一性和MR成功之间的关系大致如下：
```
Better than 40% identity: usually easy, unless large conformational changes are involved.
30-40%: MR usually possible, but sometimes more difficult.
20-30%: MR usually difficult if at all possible, careful model search and preparation required.
Less than 20%: MR unlikely to work, but MR-Rosetta can help in marginal cases.
In terms of RMSD, above 2.5A is very unlikely to work, while 1.5A or less is preferable.
```
经历大的构象变化的结构可能需要分成单独的域以进行搜索，而与序列同一性无关。 如果有多个类似的搜索模型可用，将它们组合到一个集成中通常会提高成功的可能性，尤其是如果使用Ensembler中的修整选项将该集成修整到其保守的核心时。

在Phenix中，可以使用Sculptor和Ensembler实用程序来准备搜索模型。 Sculptor可根据模型结构与目标序列之间的比对，通过修剪残基和侧链来改善模型，也可将B因子权重应用于模型，以减少不可靠的零件。 Ensembler可用于自动叠加同源模型。 推荐的顺序是先运行Sculptor，然后运行Ensembler，这是MR FAQ中解释的原因。
#### Molecular replacement
放置搜索模型的过程大致分为两个步骤，一个旋转函数(RF)确定其方向，一个平移函数(TF)确定其在单元格中的绝对位置。可以将多个组件按顺序放置以解决在非对称单元中包含多个副本的复杂晶体或晶体的结构。
##### Input files and mandatory parameters
Phenix中的所有MR程序都需要一个包含实验数据（带有sigma）的单个反射文件。 Phaser-MR GUI将接受任何文件格式或数据类型，包括强度。 传统上，该过程使用所有反射，因此不需要R-free标志。 （与精化不同，这不会对最终的无R值产生明显的偏差，因为自由度非常低：每个分子旋转和平移六个自由度，而总B因子和估计的RMS误差则每个自由度。）

至少需要一个搜索模型； 在Phaser的上下文中，搜索模型通常称为“整体”。 在许多情况下，这将是包含一个结构的单个PDB文件。 对于更远的模型，可以改为使用实际的集成模型-具有多个MODEL记录的PDB文件或多个类似的PDB文件。 当使用多模型合奏时，所有模型都必须以相同的方向叠加。 Ensembler实用程序用于此目的。 搜索模型的大小，复杂性或数量没有限制。 对于复合物或多聚体，如果各个亚基的相对位置不变，则可以将整个组件（例如核糖体亚基）用作搜索模型，而不是分别放置每个组件。

另一种选择是使用电子密度（或者更确切地说，是包含预加权映射系数的MTZ文件）进行搜索，该密度通常以低分辨率求解或从cryoEM图像重建中获得。 这需要有关要搜索的地图部分的中心和范围的其他信息。 最好使用phenix.cut_out_density工具准备此MTZ文件，并确保将密度放置在至少是密度的x，y和z范围的2.5倍的单位单元中。

Phaser中使用的最大似然定相方法需要事先了解搜索模型与实际结构的偏差（或方差），以及预期的ASU含量或晶体的散射质量。 为了指定模型方差，可以使用RMSD值或百分比序列同一性（将使用从测试计算数据库确定的关系在内部进行转换）。 重要的是要尽可能减小方差（请参阅“模型选择”以获取指南），这通常需要消除原子或修改B因子。 给定PDB文件和序列比对，Sculptor实用程序将执行此步骤。 （这对于与目标分子具有高度序列同一性的搜索模型通常是不必要的，但是，特别是在较低序列同一性的情况下，强烈建议使用Sculptor实用程序处理模型。）此外，Ensembler实用程序可以修剪在成员之间有偏差的环。 合奏，仅保留保守的核心。

对于ASU内容物，您可以提供序列文件（蛋白质或核酸），或直接输入分子量。 如果知道，Phaser的独立版本还接受每个搜索集合的分数ASU内容。 请注意，ASU内容数据不一定与搜索集合具有1：1的对应关系（有关详细信息，请参阅MR FAQ）。 如果您希望在搜索多个亚基时仅指定复合物的分子量，则可以将复合物的质量作为单个组分输入。 （您也可以使用多记录FASTA格式的序列文件。）即使您仅从多个（例如，蛋白质-DNA复合物中的蛋白质）中搜索单个集合，您仍必须提供预期的ASU内容。 因为Phaser需要知道每个搜索模型包含不对称单元的哪一部分，所以整个晶体都是如此。
##### Outline of MR procedure
```
各向异性校正：按比例缩放反射以克服各向异性（特定方向上的弱数据）。

平移非晶体对称性（tNCS）校正：检查是否存在tNCS。 如果存在，确定描述副本之间平移和小的取向差异的参数，并将其用于计算校正因子。

旋转功能：识别模型的可能方向。

平移功能：给定来自RF的方向，找到晶胞中的绝对位置。

堆积分析：给定截止值，根据原子之间的碰撞数过滤TF结果。

精炼和定相：对放置的分子进行简单的刚体精炼，并根据最终溶液计算相。

对数似然增益计算：确定最终的LLG，可用于评估MR的成功性。
```
##### Limitations
由于模型偏差和/或多余的残渣而导致的包装冲突会导致抛出其他有效的解决方案。 相位器将尝试通过修剪精简到低占用率的链段来解决冲突。 如果这没有成功，则手动清除有害的残留物或降低填料的临界值可以解决该问题。
### Automated Molecular replacement
MRage框架将模型处理步骤和分子替换集成到一个应用程序中。 它提供了具有并行化选项的高度可定制的界面，并能够连接到NCBI Blast和wwPDB网站以执行同源性搜索并获取潜在模型。 Phaser主页上提供了单独的MRage教程。
