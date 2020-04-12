# In Defense of Grid Features for Visual Question Answering
**Grid Feature Pre-Training Code**

<p align="center">
  <img src="http://xinleic.xyz/images/grid-vqa.png" width="500" />
</p>

This is a feature pre-training code release of the [paper](https://arxiv.org/abs/2001.03615):
```
@InProceedings{jiang2020defense,
  title={In Defense of Grid Features for Visual Question Answering},
  author={Jiang, Huaizu and Misra, Ishan and Rohrbach, Marcus and Learned-Miller, Erik and Chen, Xinlei},
  journal={IEEE Conference on Computer Vision and Pattern Recognition (CVPR)},
  year={2020}
}
```
For more sustained maintenance, we release code using [Detectron2](https://github.com/facebookresearch/detectron2) instead of [mask-rcnn-benchmark](https://github.com/facebookresearch/maskrcnn-benchmark) which the original code is based on. The current repository should reproduce the results reported in the paper, *e.g.*, produce **~72.5** single model VQA score for a X-101 backbone paired with [MCAN](https://github.com/MILVLG/mcan-vqa)-large.

## Installation
Install Detectron 2 following [INSTALL.md](https://github.com/facebookresearch/detectron2/blob/master/INSTALL.md). Then clone this repository:
```bash
git clone git@github.com:facebookresearch/grid-feats-vqa.git
```

## Data
[Visual Genome](http://visualgenome.org/) split released from the bottom-up-attention [code](https://github.com/peteanderson80/bottom-up-attention) are used for pre-training. They are prepared in [COCO](http://cocodataset.org/) format and include an additional field for `attribute` prediction. We provide the `.json` files [here](https://dl.fbaipublicfiles.com/grid-feats-vqa/json/visual_genome.tgz). Same as in Detectron2, the expected dataset structure under the `DETECTRON2_DATASETS` (default is `./datasets` relative to your current working directory) folder should be:
```
visual_genome/
  annotations/
    visual_genome_{train,val,test}.json
  images/
    # visual genome images (~108K)
```

## Training
Once the dataset is setup, to train a model, run (by default we use 8 GPUs):
```bash
python grid-feats-vqa/train_net.py --num-gpus 8 --config-file <config.yaml>
```
For example, to launch grid-feature pre-training with ResNet-50 backbone on 8 GPUs, one should execute:
```bash
python grid-feats-vqa/train_net.py --num-gpus 8 --config-file configs/R-50-grid.yaml
```
The final model by default should be saved under `./outputs` of your current working directory once it is done training. We also provide the region-feature pre-training configuration `configs/R-50-updn.yaml` for reference. Note that we use `0.2` attribute loss (`MODEL.ROI_ATTRIBUTE_HEAD.LOSS_WEIGHT = 0.2`), so the object detection/VQA performance is expected to be higher than reported in the paper.

# Pre-Trained Models
We release two models 
| Backbone | AP<sub>50:95</sub> | download |
| -------- | ------- | -------- |
| R50      | 3.2 | <a href="https://dl.fbaipublicfiles.com/grid-feats-vqa/R-50/model_final.pth">model</a>&nbsp;\| &nbsp;<a href="https://dl.fbaipublicfiles.com/grid-feats-vqa/R-50/metrics.json">metrics</a> |
| X-101    | ?? | <a href="https://dl.fbaipublicfiles.com/grid-feats-vqa/X-101/model_final.pth">model</a>&nbsp;\| &nbsp;<a href="https://dl.fbaipublicfiles.com/grid-feats-vqa/X-101/metrics.json">metrics</a> |
