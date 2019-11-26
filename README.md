# Image Forgery Localization


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

## Data Used

Number of Images in Spliced Dataset =  13470

Number of Images in Copymove Dataset =  13470

Total Number of Images in combined Dataset =  26940

Generate 64 patches per image. So the total number of Patches will be :  1724160

Storing these Many number of patches is an issue. So Randomly Sample 2000 Images each from both the 2 Datasets for both Part1 and Part 2.

Used Random Sampling because both the Dataset contains same Forgery only Copymove(a part of the image is copied and pasted in another part of the same image to conceal an object or to duplicate certain image elements) and Splicing(a process of making a composite picture by cutting some object from image and adding it to the some other image.)

## Evaluation Metric Used

Patch Classification : F1 Score was used as a metric. Because the dataset is imbalanced and f1 would give a balance between precision and recall. Because in an imbalanced dataset, precision and recall plays an important role in determining the performance of the model.

Manipulation Localization : IOU(Intersection Over Union) which is an evaluation metric used to measure the accuracy of an object detector on a particular dataset.

## Methodology

The paper implements patch classification and Localization using a single model, however due to memory constrains, I split the task into two parts : <br/>
Part 1 : Patch Classification <br/>
Part 2 : Manipulation Localization

#### Part1 Steps:
1) Divide the images and coresponding masks into non-overlapping 64 patches of size 32x32.<br/>
2) For each patch, label a patch as manipulated if the IOU(Intersection Over Union) between the mask and the image is >=12.5%.<br/>
3) Feed the image patches and corresponding patches to the following network:
  
  ![](/patch_classification.png)

  The input is of shape 32X32X3.<br/>
  The output has a shape of 2.<br/>
  Softmax is used for Classification.<br/>
  
#### Part2 Steps :
1) Divide the images and corresponding masks into 64 non-overlapping patches of size 32x32 <br/>
2) Feed both the mask patches as well as the image patches to the following network:

  ![](/mask_prediction.png)

  The input is image patches of size 32x32.<br/>
  The Output is mask patches of size 32x32 <br/>
  The model takes in patches of an image as input and predicts the corresponding masks.<br/>

#### Results : 
###### Part 1:
  The model was trained using the Synthetic Dataset. 2k images from Set1 zip(Spliced Data) and 2k images from Set2 zip(Copy Move). The data was splitted into train and test set with 80% of data for training and 20% for testing. The model was evaluated using F1-score as a metric. The model gave test F1 score of 0.75. 
  
###### Part 2 : 
  The model was trained using the Synthetic Dataset. 2k images from both Set1(Spliced Data) and 2k images from Set 2(Copy Move) was used for training the model. The model takes images as input and output masks for the images. Following are some of the masks generated by the model:
  For The Synthetic Dataset :
    <img src = 'https://github.com/bitmask93/Image_Forgery_Localization/blob/master/output_mask./Synt1.png'>
    </br>
    ![](/Synt2.png)
    </br>
    ![](/Synt3.png)
    </br>
    For IEEE Dataset : 
    ![](https://github.com/bitmask93/Image_Forgery_Localization/blob/master/output_mask./IEEE1.png)
    </br>
    ![](https://github.com/bitmask93/Image_Forgery_Localization/blob/master/output_mask./IEEE2.png)
    </br>
    ![](https://github.com/bitmask93/Image_Forgery_Localization/blob/master/output_mask./IEEE3.png)
    </br>
    For The NIST Dataset :
    </br>
    ![](https://github.com/bitmask93/Image_Forgery_Localization/blob/master/output_mask./NIST1.png)
    </br>
    ![](https://github.com/bitmask93/Image_Forgery_Localization/blob/master/output_mask./NIST2.png)
    </br>
    ![](https://github.com/bitmask93/Image_Forgery_Localization/blob/master/output_mask./NIST3.png)
    </br>
    ![](https://github.com/bitmask93/Image_Forgery_Localization/blob/master/output_mask./NIST4.png)
    </br>

### Results :

For Part 1: Patch Classification 
    
    Train f1 = 0.9933
    Test f1 = 0.756
    
    Using IEEE Dataset
    
    Test f1 = 0.5010
    
For Part2 : Localization

    Train IOU = 0.9183

### Conclusion and Future :
1) The model does a decent job in localizing forgeries in Digital images. The model was tested using both IEEE forensic dataset and the NIST Dataset. 

2) For IEEE data model does an ok job only mainly cos IEEE dataset contains complex levels of forgeries other than copy move and splicing. However for NIST it does really good job. It can identify true and fake images clearly as it can be seen that in one of the images the mask is empty cos model knows it's a genuine image.

3) The results could be further improved by considering more data for training.

4) For solving the IEEE problem, creating a new dataset with more tamperings(other than copy move and splice) could improve the results.


