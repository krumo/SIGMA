## [SIGMA: Semantic-complete Graph Matching For Domain Adaptive Object Detection (CVPR-22 ORAL)](https://arxiv.org/pdf/2203.06398.pdf)

[[Arxiv](https://arxiv.org/pdf/2203.06398.pdf)] [[知乎](https://zhuanlan.zhihu.com/p/492956292)]

By [Wuyang Li](https://wymancv.github.io/wuyang.github.io/)

Welcome to have a look at our previous work [SCAN](https://github.com/CityU-AIM-Group/SCAN) (AAAI'22 ORAL), which is the foundation of this work. 


## Installation

Check [INSTALL.md](https://github.com/CityU-AIM-Group/SIGMA/blob/main/INSTALL.md) for installation instructions.

If you have any problem in terms of installation, feel free to screenshot your issue for me. Thanks.

## Data preparation

Step 1: Format three benchmark datasets. (BDD100k is also available)

```
[DATASET_PATH]
└─ Cityscapes
   └─ cocoAnnotations
   └─ leftImg8bit
      └─ train
      └─ val
   └─ leftImg8bit_foggy
      └─ train
      └─ val
└─ KITTI
   └─ Annotations
   └─ ImageSets
   └─ JPEGImages
└─ Sim10k
   └─ Annotations
   └─ ImageSets
   └─ JPEGImages
```

Step 2: change the data root for your dataset at [paths_catalog.py](https://github.com/CityU-AIM-Group/SIGMA/blob/main/fcos_core/config/paths_catalog.py).

```
DATA_DIR = [$Your dataset root]
```
More detailed dataset preparation can be found at [EPM](https://github.com/chengchunhsu/EveryPixelMatters). 

## Tutorials for this project
1) We provide super detailed code comments in [sigma_vgg16_cityscapace_to_foggy.yaml](https://github.com/CityU-AIM-Group/SIGMA/blob/main/configs/SIGMA/sigma_vgg16_cityscapace_to_foggy.yaml). We strongly recommend you to have a look.
2) We modify the [trainer](https://github.com/CityU-AIM-Group/SIGMA/blob/main/fcos_core/engine/trainer.py) to meet the requirements of SIGMA.
3) GM is integrated in this "middle layer" [graph_matching_head](https://github.com/CityU-AIM-Group/SIGMA/blob/main/fcos_core/modeling/rpn/fcos/graph_matching_head.py).
4) Node sampling is conducted in [here](https://github.com/CityU-AIM-Group/SIGMA/blob/main/fcos_core/modeling/rpn/fcos/loss.py).
5) We preserve lots of APIs for many implementation choices in [here](https://github.com/CityU-AIM-Group/SIGMA/blob/main/fcos_core/config/defaults.py)
6) We hope this work can inspire lots of good ideas

## Well-trained models
We have provided lots of well-trained models at ([onedrive](https://portland-my.sharepoint.com/:f:/g/personal/wuyangli2-c_my_cityu_edu_hk/Eh94jXa1NSxAilUAE68-T0MBckTxK3Tm-ggmzZRJTHNHww?e=B30DNw)).
1) Kindly note that it is easy to get higher results than the reported ones with tailor-tuned hyperparameters.
2) We didn't tune the hyperparameters for ResNet-50, and it could be further improved.
3) We have tested on C2F and S2F with end-to-end training, finding it can also achieve SOTA results, as mentioned in our appendix.
4) After correcting a default hyper-parameter, our S2C gives four mAP gains compared with the reported one, as explained in the config file.
<!-- 4) As mentioned in the paper, we tried to obtain the best results with two-stage training, which will be provided in the future.  -->

| dataset | backbone |   mAP	 | mAP@50 |  mAP@75 |	 file-name |	
| :-----| :----: | :----: |:-----:| :----: | :----: | 
| Cityscapes -> Foggy Cityscapes | VGG16 | 24.0 |43.6|23.8| city_to_foggy_vgg16_43.58_mAP.pth|
| Cityscapes -> Foggy Cityscapes | VGG16 | 24.3 |43.9|22.6| city_to_foggy_vgg16_43.90_mAP.pth|
| Cityscapes -> Foggy Cityscapes | Res50 | 22.7 |44.3|21.2| city_to_foggy_res50_44.26_mAP.pth|
| Cityscapes -> BDD100k| VGG16 | - |32.7|- |city_to_bdd100k_vgg16_32.65_mAP.pth|
| Sim10k -> Cityscapes | VGG16 | 33.4 |57.1 |33.8 |sim10k_to_city_vgg16_53.73_mAP.pth|
| KITTI -> Cityscapes | VGG16 | 22.6 |46.6 |20.0 |kitti_to_city_vgg16_46.45_mAP.pth|

## Get start

Train the model from the scratch
```
python tools/train_net_da.py \
        --config-file configs/SIGMA/xxx.yaml \

```
Test the well-trained model
```
python tools/test_net.py \
        --config-file configs/SIGMA/xxx.yaml \
        WEIGHT 'well_trained_models/xxx.pth'

```


## Citation 

If you think this work is helpful for your project, please give it a star and citation:
```

@inproceedings{li2022sigma,
  title={SIGMA: Semantic-complete Graph Matching for Domain Adaptive Object Detection},
  author={Li, Wuyang and Liu, Xinyu and Yuan, Yixuan},
  booktitle={IEEE Conference on Computer Vision and Pattern Recognition (CVPR)},
  year={2022}
}
```


## Contact

E-mail: wuyangli2-c@my.cityu.edu.hk 
<!-- Wechat: 17720031102 -->

## Acknowledgements 

This work is based on [SCAN (AAAI'22 ORAL)](https://github.com/CityU-AIM-Group/SCAN) and [EPM (ECCV20)](https://github.com/chengchunhsu/EveryPixelMatters). 

The implementation of our anchor-free detector is from [FCOS](https://github.com/tianzhi0549/FCOS/tree/f0a9731dac1346788cc30d5751177f2695caaa1f).


## Abstract

Domain Adaptive Object Detection (DAOD) leverages a labeled source domain to learn an object detector generalizing to a novel target domain free of annotations. Recent advances align class-conditional distributions through narrowing down cross-domain prototypes (class centers). Though great success, these works ignore the significant within-class variance and the domain-mismatched semantics within the training batch, leading to a sub-optimal adaptation. To overcome these challenges, we propose a novel SemantIc-complete Graph MAtching (SIGMA) framework for DAOD, which completes mismatched semantics and reformulates the adaptation with graph matching. Specifically, we design a Graph-embedded Semantic Completion module (GSC) that completes mismatched semantics through generating hallucination graph nodes in missing categories. Then, we establish cross-image graphs to model class-conditional distributions and learn a graph-guided memory bank for better semantic completion in turn. After representing the source and target data as graphs, we reformulate the adaptation as a graph matching problem, i.e., finding well-matched node pairs across graphs to reduce the domain gap, which is solved with a novel Bipartite Graph Matching adaptor (BGM). In a nutshell, we utilize graph nodes to establish semantic-aware node affinity and leverage graph edges as quadratic constraints in a structure-aware matching loss, achieving fine-grained adaptation with a node-to-node graph matching. Extensive experiments demonstrate that our method outperforms existing works significantly.

![image](https://github.com/CityU-AIM-Group/SIGMA/blob/main/overall.png)
