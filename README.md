# Online Attention Accumulation
This work is finished by [Peng-Tao Jiang](pengtaojiang.github.io), [Qibin Hou](http://mmcheng.net/qbhou/), [Yang Cao](https://mmcheng.net/ycao), [Ming-Ming Cheng](https://mmcheng.net/cmm/), [Yunchao Wei](https://weiyc.github.io/), [Hongkai Xiong](http://min.sjtu.edu.cn/xhk.htm).
This repository contains the original code and the links for data and pretrained models. If you have any questions about our paper "Integral Object Mining via Online Attention Accumulation", please feel free to contact me (pt.jiang AT mail DOT nankai.edu.cn).

### Table of Contents
1. [Citation](#citation)
2. [Installation](#installation)
3. [Results](#results)
4. [Pytorch re-implementations](#pytorch-re-implementations)
### Citation
If you use these codes and models in your research, please cite:

### Installation
#### 1. Dependence
  ubuntu 16.04  
  python 2.7  
  [caffe dependence](https://caffe.berkeleyvision.org/install_apt.html)

#### 2. Compilie caffe
```
git clone https://github.com/PengtaoJiang/OAA.git
cd OAA/
make all -j4 && make pycaffe
```
#### 3. Download VOC 2012 dataset
Please refer to https://drive.google.com/open?id=1uh5bWXvLOpE-WZUUtO77uwCB4Qnh6d7X.  
Download the `VOCdevkit.tar.gz` file and extract the voc data into `data/` folder.

#### 4. Train the classification network for accumulating attention
```
cd examples/oaa/
./train.sh exp1 0
```
After the process of OAA is finished, you can resize the cumulative attention maps to the size of original images by
```
cd exp1/
python res.py
```
(optional) After OAA, you can train a integral attention model to further improve the quality of OAA. You need to perform serveal steps:
First, construct the pixel-level supervision from cumulative attention maps.
```
cd exp1/
python res1.py
python eval.py 30000 0
```
Once you generate the pixel-level supervision, train the integral attention model 
```
cd examples/oaa/
./train.sh exp2 0
```
The attention maps can be obtained from the integral attention model by
```
python eval.py 30000 0
```
#### 5. Train the Deeplab-LargeFOV network
We provide two Deeplab-LargeFOV versions, which are based on VGG16(`examples/seg/exp1`) and ResNet101(`examples/seg/exp2`), respectively. After generating proxy segmentation labels, put them into `data/VOCdevkit/VOC2012/`.  
```
cd examples/seg/exp1/
```
Adjust the training list `train_ins.txt` for matching your path of proxy segmentation labels, then train the segmentation network.
```
cd examples/seg/
./train.sh exp1 0
```
#### 6. Inference from the Deeplab-LargeFOV network
```
python eval.py 15000 0 exp1
```
If you want to use crf to smooth the segmentation results, you can download the crf code from [this link](https://github.com/Andrew-Qibin/dss_crf).
Move the code the `examples/seg/`, then uncomment line `175` and line `176` in `examples/seg/eval.py`.



