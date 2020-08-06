# Table Detection Task

这提供了一个pipeline来测试预训练的模型并可视化“Table Detection task”任务的性能。

表格检测旨在使用文档中的边框来定位表格。
给定图像格式的文档页面，将生成几个边界框，这些边界框表示表在此页面中的位置。

作者使用Detectron库对两个模型（ResNeXt-101和ResNeXt-152）进行了预训练。 因此，我们需要先安装Detectron。

正式介绍Detectron的安装 [此处]（https://github.com/facebookresearch/Detectron/blob/master/INSTALL.md）。
Detectron基于Caffe2，现在已集成到Pytorch库中。 但请注意**detectron中的操作员没有CPU版本，因此需要GPU系统**。
 另外，当我通过`pip install -r $ DETECTRON / requirements.txt`根据https://github.com/facebookresearch/Detectron/blob/master/INSTALL.md#detectron安装依赖项时，我
注意，我必须使用`pip install pyyaml == 3.12`来安装旧版本的pyyaml而不是最新版本，
否则我将遇到错误`BBOX_XFORM_CLIP：!! python / object / apply：numpy.core`。 
此问题发布在[here]（https://github.com/facebookresearch/Detectron/issues/840）。

完整的命令是
```
# DETECTRON=/path/to/clone/detectron
git clone https://github.com/facebookresearch/detectron $DETECTRON
pip install pyyaml==3.12
pip install -r $DETECTRON/requirements.txt
cd $DETECTRON && make
python $DETECTRON/detectron/tests/test_spatial_narrow_as_op.py
```
我将在下面的ResNeXt101模型中进行实验，而ResNeXt152的pipline则类似。

您需要首先在[此处](https://github.com/doc-analysis/TableBank/blob/master/MODEL_ZOO.md#table-detection）
下载配置文件和模型的权重。 在下文中，我们使用$ MODEL_PATH来表示下载的权重，使用$ CONFIG_MODEL来表示配置文件。

Then simply run the commands 

```
python tools/infer_simple.py --cfg $CONFIG_MODEL --output-dir /tmp/detectron-tablebank --image-ext jpg \
    --wts $MODEL_PATH /home/shr/TableBank/data/Sampled_Detection_data/Latex/images
```

where `/home/shr/TableBank/data/Sampled_Detection_data/Latex/images` is the input directory and `jpg` is the input format.

检测到的表将显示在目录“/tmp/detectron-tablebank”中。 
默认情况下，如果无法为输入图像检测到对象，则目标目录中将没有相应的输出文件。 
您可以添加`---- always-out`来生成带注释的pdf，即使未检测到任何东西。
