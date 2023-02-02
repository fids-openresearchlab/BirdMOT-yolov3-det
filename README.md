
# BirdMOT-yolov3-det: Flying Birds Detection 
This repository contains information and files of the yolov3 BirdMOT flying birds detector, which leverages tiling in order to detect birds in high resolution footage (3840x2160 pixels). 

<!---
#ToDo: More Words on FIDS?
--->

<!---
#ToDo: gif?
--->

The two Darknet Tools [DarkMark](https://github.com/stephanecharette/DarkMark/) and [DarkHelp](https://github.com/stephanecharette/DarkHelp) by Stéphane Charette have been used to create the tiled training data and to run the detection. 
## Demo
![](demo_BirdMOT-det-yolov3-tiled.gif)


## Background
### Main Challenges
**Tiny Objects**
Birds which are flying on higher altitude are becoming tiny object in the video footage and hence become more difficult to detect compared to bigger objects. The dataset contains birds of different sizes.

**Resolution**
The video footage of the birds needs to have a high resolution in order to not lose tiny objects. The video footage used in this project was captured in 3840x2160 resolution. Feeding such high resolution images into object detection directly will cause massive network sized and hence require a massive amount of RAM. Using smaller networks would require to downsize images which will cause the network to perform worse when detection tiny birds.

**Blurring** 
Depending on factors like the bird species and altitude the birds will fly faster or slower through the image leading to blurring. The amount of blurring is also heavily influenced by the camera settings and light conditions. Blurred birds will be more difficult to detect as features are not clear anymore or might not be visible at all.

**Inference speed**
As this detector is used as part of a Multi Object Tracker running on videos the inference speed is important. However, the high resolution video footage 

### Approach
Initial tests have shown good results using an image tiling approach in combination with a darknet yolov3-tiny-3l  network which has 3 yolo layers. The images are sliced into tiles of size 1920x1080 pixels which still allowed a batch size of 64 while still allowing a subdivision of 32 while training on an Nvidia GeForce GTX  1080 Ti. Having bigger tiles allowed to reduce the errors caused by the joining of the tiles and bounding boxes.

## Dataset
The dataset contains 3811 images including ground truth annotations. 42 of those are negative samples as they do not contain birds but other objects which were previously falsely detected as birds. In total the images contain 10044 annotated birds of different sizes.

All images have a resolution of 3840x2160 pixels.

## Augmentation
Following augmentation methods have shown to improve performance:
- Mixup
- Cutmix
- Colour
  - Hue
  - Saturation
  - Exposure

## Train yourself
### 1. Install DarkMark and DarkHelp
Install [DarkMark](https://github.com/stephanecharette/DarkMark/) and [DarkHelp](https://github.com/stephanecharette/DarkHelp) following the installation instructions in the documentation.

### 2. Download our Dataset
- [BirdMOT-yolov3-det (color)](https://mega.nz/file/66IGRCZa#Mi7-cT1aZLQd3Qm3jdyWmwIweukTeOz0uRS4RVG9msk)
- [BirdMOT-yolov3-det (gray)](https://mega.nz/file/enYBASDI#_qCSjxHcrGQvfrf36JnirOLnO__JoCDbabBsqjToeXg)


### 3. Prepare data and create Augmentation Data
Decide if you want to train using the greyscale dataset or coloured dataset. Chose the DarkMark options as required in order to create the DarkMark folder, network configuration and the desired augmentation data. Here are some examples:
#### a. Coloured Dataset based Examples
  ```Shell
  name="yolov3-tiny-3l-1920-1080-mixup-true-cutmix-true"
  mkdir ./darkmark_folders
  mkdir ./darkmark_folders/$name
  cp -r DarkMark_09_22/* "./darkmark_folders/$name/"
  DarkMark add="./darkmark_folders/$name/" editor=gen-darknet mosaic=false mixup=true cutmix=false resize_images=false do_not_resize_images=false tile_images=true zoom_images=true limit_validation_images=false limit_neg_samples=false  flip=true width=1920 height=1080 template="/home/fids/darknet/cfg/yolov3-tiny_3l.cfg" darknet=run
  ```
#### b. Greyscale Dataset based Examples
```Shell
name="yolov3-tiny-3l-1920-1080-mixup-true-cutmix-true-gray_incl_negatives"
mkdir ./darkmark_folders
mkdir ./darkmark_folders/$name
cp -r DarkMark_09_22_gray/* "./darkmark_folders/$name/"
DarkMark add="./darkmark_folders/$name/" editor=gen-darknet mosaic=false mixup=true cutmix=false resize_images=false do_not_resize_images=false tile_images=true zoom_images=true limit_validation_images=false limit_neg_samples=false  flip=true width=1920 height=1080 template="/home/fids/darknet/cfg/yolov3-tiny_3l.cfg" darknet=run
```
```Shell
name="yolov3-tiny-3l-3840-2160-mixup-true-cutmix-true-gray_incl_negatives"
mkdir ./darkmark_folders
mkdir ./darkmark_folders/$name
cp -r DarkMark_09_22_gray/* "./darkmark_folders/$name/"
echo "bird" > ./darkmark_folders/$name/$name.names
DarkMark add="./darkmark_folders/$name/" editor=gen-darknet mosaic=false mixup=true cutmix=false resize_images=false do_not_resize_images=false tile_images=true zoom_images=true limit_validation_images=false limit_neg_samples=false  flip=true width=3840 height=2160 template="/home/fids/darknet/cfg/yolov3-tiny_3l.cfg" darknet=run
```
You can also use the DarkMark GUI in order to change data preparation and augmentation settings

### 4. Edit darknet config
In the darkmark folder a .cfg file was created. Edit the file to change the network configuration.

### 5. Run Training
```Shell
name="yolov3-tiny-3l-1920-2160-mixup-true-cutmix-true-gray_incl_negatives"
./$name/$(echo $name)_train.sh
```


## ToDo:
- [ ] Add low light images to dataset and train using increased exposure data augmentation

<!---
## Citation


--->


## Acknowledgement
This project depends on the work of Alexey Bochkovskiy on [Darknet](https://github.com/AlexeyAB/darknet) and is using the tools  [DarkMark](https://github.com/stephanecharette/DarkMark/) and [DarkHelp](https://github.com/stephanecharette/DarkHelp) by Stéphane Charette. 




<!---
The number of negative samples (empty images): 11007.

The number of new image tiles created: 15244.

The number of random crop & zoom images created: 7381.

--->

<!---

The number of negative samples (empty images): 10985.

The number of new image tiles created: 15244.

The number of random crop & zoom images created: 7330.


--->
<!---
The number of negative samples (empty images): 9249.

The number of new image tiles created: 15244.

IMPORTANT: 158 images were skipped because they have not yet been annotated.
--->

<!---

DarkHelp yolov3-tiny-3l-1920-1080-mixup-true-cutmix-true-gray_incl_negatives.cfg yolov3-tiny-3l-1920-1080-mixup-true-cutmix-true-gray_incl_negatives_best.weights yolov3-tiny-3l-1920-1080-mixup-true-cutmix-true-gray_incl_negatives.names -a 1920x1080 -s -g --keep --tiles true -t 0.15 --tile-edge 0.4 --tile-rect 2.7 --nms 0.2 -d false --autohide false /media/data/many_bird_export/MOT/MOT-1658226941_zoex_1544357_1544441/img1/*.jpg


--->
