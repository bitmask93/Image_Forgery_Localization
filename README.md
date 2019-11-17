# Image_Forgery_Localization


## Problem Statement
To Localize forgeries in digital images.

This is an implementation of the paper titled "Exploiting Spatial Structure for Localizing Manipulated Image Regions" by Jawadul H. Bappy et.al. [Paper](https://vcg.engr.ucr.edu/sites/g/files/rcwecm2661/files/2019-02/iccv_jawad.pdf)

## Introduction 

Due to the advancements in technologies, it has become difficult to find manipulations in digital images. Oftem times it is extremely difficult to detect any tamperings on the images with naked eye because of the strong resemblence to the original image. This paper discuss about a simple Convolutional-LSTM model for localizing the tamered regions of a digital image.

## Dataset

The data used is a synthetic dataset that was generated by the authors of this paper. The dataset is created by splicing different objects obtained from MS-COCO dataset into the images of DRESDEN benchmark. The Dataset can be downloaded from [here](https://www.dropbox.com/sh/palus3sq4zvdky0/AACu3s7KA5Fhr_BJUeDOxnTLa?dl=0). 

The dataset is split between three different zip files.
a) spliced_nist.zip - ~27K, Pristine images from the MFC18, spliced with a single object from the MS_COCO dataset.
b) copymove_nist.zip - ~27K, Pristine images from the MFC18, with a single object from MS_COCO spliced onto it multiple times.
c) dresden_spliced_mscoco.zip - ~71K, Pristine images cropped from the Dresden Dataset, spliced with objects fromthe MS_COCO    dataset.

For this work, I have considerd only the First two zip files.

## Methodology

The paper implements patch classification and Localization using a single model, however due to memory constrains, I split the task into two parts : 
Part 1 : Patch Classification
Part 2 : Manipulation Localization

#### Part1 Steps:
1) Divide the images and coresponding masks into non-overlapping 64 patches of size 32x32.
2) For each patch, label a patch as manipulated if the IOU(Intersection Over Union) between the mask and the image is >=12.5%.
3) Feed the image patches and corresponding patches to the following network:
  
  ![](https://github.com/bitmask93/Image_Forgery_Localization/blob/master/patch_classification.png)

  The input is of shape 32X32X3.
  The output has a shape of 2.
  Softmax is used for Classification.
  
#### Part2 Steps :
1) Divide the images and corresponding masks into 64 non-overlapping patches of size 32x32



