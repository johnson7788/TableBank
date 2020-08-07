# Model Zoo

## Introduction

下面列出了在我们的基准中训练的模型。 所有模型在各自的骨干网络下均经过了20个epoch的训练。 
使用[detectron](https://github.com/facebookresearch/Detectron/blob/master/MODEL_ZOO.md)

提供的预训练模型。

## License

All models available for download through this document are licensed under the [Attribution-NonCommercial-NoDerivs License](https://creativecommons.org/licenses/by-nc-nd/4.0/).

## Models

### Table Detection 
在TableBank上训练的模型以Detectron使用的格式提供。

- [Without Copyright Data X101](https://conversationhub.blob.core.windows.net/tablebank/model_zoo/Without_copyright/X101/model_final.pkl): Trained ResNeXt101 on latex and word mixed dataset without Copyright data.[[config](https://conversationhub.blob.core.windows.net/tablebank/model_zoo/Without_copyright/X101/config_X101.yaml)]
- [Without Copyright Data X152](https://conversationhub.blob.core.windows.net/tablebank/model_zoo/Without_copyright/X152/model_final.pkl): Trained ResNeXt152 on latex and word mixed dataset without Copyright data.[[config](https://conversationhub.blob.core.windows.net/tablebank/model_zoo/Without_copyright/X152/config_X152.yaml)]

### Table Recognition
在TableBank上训练的模型以OpenNMT使用的格式提供。

- [Without Copyright Data](https://conversationhub.blob.core.windows.net/tablebank/model_zoo/Recognition_all_without_copyright/model.pt): Trained image-to-markup model on latex and word mixed dataset without Copyright data.