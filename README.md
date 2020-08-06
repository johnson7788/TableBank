# TableBank

**\*\*\*\*\* A new benchmark dataset DocBank ([Paper](https://arxiv.org/abs/2006.01038), [Repo](https://github.com/doc-analysis/DocBank)) is now available for document layout analysis \*\*\*\*\***

**\*\*\*\*\* Our data can only be used for research purpose \*\*\*\*\***

**\*\*\*\*\* Our paper has been accepted in [LREC 2020](https://lrec2020.lrec-conf.org/en/conference-programme/accepted-papers/) \*\*\*\*\***

TableBank是一个新的基于图像的table检测和识别的数据集。
Word和Latex文档的新型的弱监督方法， 在互联网上，包含417K高质量有标签的table。

## Introduction
为了满足对标准开放领域的table基准数据集的需求，
我们提出了一种新颖的弱监督方法来自动创建TableBank，
该TableBank比现有人工标记table数据集大几个数量级。 
与传统的弱监督训练集不同，我们的方法不仅可以获得大规模的训练数据，而且可以获得高质量的训练数据。

如今，网络上有大量电子文档，例如Microsoft Word（.docx）和Latex（.tex）文件。 
这些在线文档本质上在其源代码中包含表的标记标签。
直观地，我们可以通过在每个文档中使用标记语言添加边界框来操纵这些源代码。
对于Word文档，可以在标识每个表边界的地方修改内部Office XML代码。
对于Latex文档，还可以在识别表的边界框的地方修改tex代码。 通过这种方式，可以为各种领域创建高质量的标签数据
例如商业文件，官方装填，研究论文等，这对于大规模表格分析任务非常有用。


TableBank数据集总共包含417,234个高质量标记的表以及它们在各个领域中的原始文档。


### Statistics of TableBank
| Task                        | Word    | Latex   | Word+Latex |
|-----------------------------|---------|---------|------------|
| Table detection             | 163,417 | 253,817 | 417,234    |
| Table structure recognition | 56,866  | 88,597  | 145,463    |


## License
TableBank is released under the [Attribution-NonCommercial-NoDerivs License](https://creativecommons.org/licenses/by-nc-nd/4.0/). You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may not use the material for commercial purposes. If you remix, transform, or build upon the material, you may not distribute the modified material.



## Task Definition

### Table Detection
表格检测旨在使用文档中的边框来定位表格。
给定图像格式的文档页面，将生成几个边界框，这些边界框表示表在此页面中的位置。


### Table Structure Recognition
表格结构识别旨在识别表格的行和列布局结构，尤其是在非数字文档格式（如扫描图像）中。
给定图像格式的表，将生成HTML标记序列，该序列表示行和列的排列以及表单元格的类型。

## Baselines
为了验证Table-Bank的有效性，我们使用了具有端到端深度神经网络的最新模型来建立几个强大的基线。 
表格检测模型基于具有不同设置的Faster R-CNN [Ren et al。2015]体系结构。 
表结构识别模型基于图像到文本的编码器-解码器框架。

### Data and Metrics
为了评估表格检测，我们分别从Word和Latex文档中采样了2,000个文档图像，
其中1,000个图像用于验证，而1,000个图像用于测试。 
每个采样的图像至少包含一个表。 
同时，我们还在ICDAR 2013数据集上评估了我们的模型，
以验证TableBank的有效性。 为了评估表结构的识别性，
我们分别从Word文档和Latex文档中抽取500个表进行验证和测试。 
整个训练和测试数据将很快向公众公开。 
对于表格检测，我们以与[Gilani等，2017]中相同的方式precision, recall 和 F1 ，
其中所有文档的指标都是通过对重叠区域，预测区域和基本事实的求和来计算的。
对于表结构识别，我们使用4-gram BLEU得分作为具有单个参考的评估指标。

### Table Detection
我们使用开源框架Detectron [Girshick et al。，2018]在TableBank上训练模型。
Detectron是用于对象检测研究的高质量和高性能代码库，它支持许多最新算法。
在此任务中，我们使用具有ResNeXt [Xie et al。，2016]的Faster R-CNN算法作为骨干网络架构，
其中参数在ImageNet数据集上进行了预训练。 使用4个P100 NVIDIA GPU使用数据并行同步SGD（最小批量为16张图像）训练所有基线。
对于其他参数，我们使用Detectron中的默认值。 在测试过程中，生成bounding boxes的置信度阈值设置为90％。

| Models                   | Word      |        |        | Latex     |        |        | Word+Latex |        |        |
|--------------------------|-----------|--------|--------|-----------|--------|--------|------------|--------|--------|
|                          | Precision | Recall | F1     | Precision | Recall | F1     | Precision  | Recall | F1     |
| ResNeXt-101 (Word)       | 0.9496    | 0.8388 | 0.8908 | 0.9902    | 0.5948 | 0.7432 | 0.9594     | 0.7607 | 0.8486 |
| ResNeXt-152 (Word)       | 0.9530    | 0.8829 | **0.9166** | 0.9808    | 0.6890 | 0.8094 | 0.9603     | 0.8209 | 0.8851 |
| ResNeXt-101 (Latex)      | 0.8288    | 0.9395 | 0.8807 | 0.9854    | 0.9760 | 0.9807 | 0.8744     | 0.9512 | 0.9112 |
| ResNeXt-152 (Latex)      | 0.8259    | 0.9562 | 0.8863 | 0.9867    | 0.9754 | **0.9810** | 0.8720     | 0.9624 | 0.9149 |
| ResNeXt-101 (Word+Latex) | 0.9557    | 0.8403 | 0.8943 | 0.9886    | 0.9694 | 0.9789 | 0.9670     | 0.8817 | 0.9224 |
| ResNeXt-152 (Word+Latex) | 0.9540    | 0.8639 | 0.9067 | 0.9885    | 0.9732 | 0.9808 | 0.9657     | 0.8989 | **0.9311** |

### 表结构识别
对于表结构识别，我们使用开源框架OpenNMT [Klein et al。，2017]来训练图像到文本模型。
OpenNMT主要设计用于神经机器翻译，它支持许多编码器-解码器框架。
在此任务中，我们在OpenNMT中使用图像到文本的方法训练模型。
该模型还使用4个P100 NVIDIA GPU进行了训练，学习速率为0.1，批处理大小为24。
对于其他参数，我们使用OpenNMT中的默认值。

| Models                     | Word   | Latex  | Word+Latex |
|----------------------------|--------|--------|------------|
| Image-to-Text (Word)       | **0.7507** | 0.6733 | 0.7138     |
| Image-to-Text (Latex)      | 0.4048 | **0.7653** | 0.5818     |
| Image-to-Text (Word+Latex) | 0.7121 | 0.7647 | **0.7382**     |

## Model Zoo

训练有素的模型可从 [TableBank Model Zoo](MODEL_ZOO.md)下载

## Quick Start
这是测试预训练模型并可视化“Table Detection”任务的性能的pipeline。 [表格检测]（TestPretrainedModel.md）。

## Get Data and Leaderboard
由于某些数据存在版权问题，不应发布，因此我们过滤了所有数据并将其排除在外。 
我们还将在更改后的数据集上重新训练所有基准模型，并将其在排行榜网站上列出。

Please fill this [form](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRw1hSTX2waZIoerSk1J6CyNUMTRCUEZCR0lVOVZaTVhLUFVJTjhJUkdXSi4u). 
如果审核通过，下载链接将发送到您的电子邮件地址。

该链接将在“申请后的下周一”进行审核并发送**
排行榜网站是[https://doc-analysis.github.io/](https://doc-analysis.github.io/）。 
如果您想添加一篇报告该数字等于或高于当前水平的论文，请发送电子邮件至[Minghao Li]（mailto：liminghao1630@buaa.edu.cn）。

### Statistics of TableBank (Removing copyright protection data)
| Task                        | Word    | Latex   | Word+Latex |
|-----------------------------|---------|---------|------------|
| Table detection             | 101,889 | 253,817 | 355,706    |
| Table structure recognition | 56,866  | 88,597  | 145,463    |

## Paper and Citing
https://arxiv.org/abs/1903.01949
```
@misc{li2019tablebank,
    title={TableBank: A Benchmark Dataset for Table Detection and Recognition},
    author={Minghao Li and Lei Cui and Shaohan Huang and Furu Wei and Ming Zhou and Zhoujun Li},
    year={2019},
    eprint={1903.01949},
    archivePrefix={arXiv},
    primaryClass={cs.CV}
}
```

## References

- [Ren et al., 2015] Shaoqing Ren, Kaiming He, Ross B. Girshick,
    and Jian Sun. Faster R-CNN: towards real-time
    object detection with region proposal networks. CoRR,
    abs/1506.01497, 2015.
- [Gilani et al., 2017] A. Gilani, S. R. Qasim, I. Malik, and
    F. Shafait. Table detection using deep learning. In Proc. of
    ICDAR 2017, volume 01, pages 771–776, Nov 2017.
- [Girshick et al., 2018] Ross Girshick, Ilija Radosavovic,
    Georgia Gkioxari, Piotr Doll´ar, and Kaiming He. [Detectron](
    https://github.com/facebookresearch/detectron).
    2018.
- [Xie et al., 2016] Saining Xie, Ross B. Girshick, Piotr
    Doll´ar, Zhuowen Tu, and Kaiming He. Aggregated residual
    transformations for deep neural networks. CoRR,
    abs/1611.05431, 2016.
- [Klein et al., 2017] Guillaume Klein, Yoon Kim, Yuntian
    Deng, Jean Senellart, and Alexander M. Rush. Open-NMT:
    Open-source toolkit for neural machine translation.
    In Proc. of ACL, 2017.]
