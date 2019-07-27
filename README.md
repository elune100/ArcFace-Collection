# Angular-space-loss-function
Angle Space Loss Function for Face Recognition.
# InsightFace

PyTorch implementation of Additive Angular Margin Loss for Deep Face Recognition.
[paper](https://arxiv.org/pdf/1801.07698.pdf).

## Dataset

Function|Dataset|
|---|---|
|Train|MS-Celeb-1M|
|Test-1|LFW|
|Test-2|MegaFace|

### Introduction

MS-Celeb-1M dataset for training, 3,804,846 faces over 85,164 identities.


## Dependencies
- Python 3.6.7
- PyTorch 1.0.0

## Usage

### Data wrangling
Extract images, scan them, to get bounding boxes and landmarks:
```bash
$ python extract.py
$ python pre_process.py
```

Image alignment:
1. Face detection(MTCNN).
2. Face alignment(similar transformation).
3. Central face selection.
4. Resize -> 112x112. 


### Train
```bash
$ python train.py
```

To visualize the training process：
```bash
$ tensorboard --logdir=runs
```

## Performance evaluation

### LFW

#### Introduction
Use Labeled Faces in the Wild (LFW) dataset for performance evaluation:

- 13233 faces
- 5749 identities
- 1680 identities with >=2 photo

#### Download
Download LFW database put it under data folder:
```bash
$ wget http://vis-www.cs.umass.edu/lfw/lfw-funneled.tgz
$ wget http://vis-www.cs.umass.edu/lfw/pairs.txt
$ wget http://vis-www.cs.umass.edu/lfw/people.txt
```

#### Start evaluation
```bash
$ python lfw_eval.py
```

### MegaFace
 
#### Introduction
 
MegaFace dataset includes 1,027,060 faces, 690,572 identities. [Link](http://megaface.cs.washington.edu/)
 
Challenge 1 is taken to test our model with 1 million distractors. 


 
#### Download

1. Download MegaFace and FaceScrub Images
2. Download Linux DevKit from [MagaFace WebSite](http://megaface.cs.washington.edu/) then extract to megaface folder:

```bash
$ tar -vxf linux-devkit.tar.gz
```
 
#### Generate features

1. Crop MegaFace.
2. Generate features for FaceScrub and MegaFace.
3. Remove noises. 
Note: we used the noises list proposed by InsightFace, at https://github.com/deepinsight/insightface.

```bash
$ python3 megaface.py --action crop_megaface

$ find megaface/facescrub_images -name "*.bin" -type f -delete
$ find megaface/MegaFace_aligned/FlickrFinal2 -name "*.bin" -type f -delete

$ python3 megaface.py --action gen_features
```

#### Evaluation

Start MegaFace evaluation through devkit: 

```bash
$ cd megaface/devkit/experiments
$ python run_experiment.py -p /dev/code/mnt/InsightFace-v2/megaface/devkit/templatelists/facescrub_uncropped_features_list.json /dev/code/mnt/InsightFace-v2/megaface/MegaFace_aligned/FlickrFinal2 /dev/code/mnt/InsightFace-v2/megaface/facescrub_images _0.bin results -s 1000000
```

#### Results

##### Curves

Draw curves with matlab script @ megaface/draw_curve.m. 

