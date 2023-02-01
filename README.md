# BirdMOT-det-yolov3-tiled: Flying Birds Detection 
This repository contains information and files of the yolov3 BirdMOT flying birds detector, which leverages tiling in order to detect birds in high resolution footage (3840x2160 pixels). 

<!---
#ToDo: More Words on FIDS?
--->

<!---
#ToDo: gif?
--->

The two Darknet Tools [DarkMark](https://github.com/stephanecharette/DarkMark/) and [DarkHelp](https://github.com/stephanecharette/DarkHelp) by Stéphane Charette have been used to create the tiled training data and to run the detection. 

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

## Results

## Train yourself
### 1. Install DarkMark and DarkHelp
Install [DarkMark](https://github.com/stephanecharette/DarkMark/) and [DarkHelp](https://github.com/stephanecharette/DarkHelp) following the installation instructions in the documentation.

### 2. Download our Dataset
- [BirdMOT-yolov-det (color)]()
- [BirdMOT-yolov-det (gray)]()


## ToDo:
- [ ] Add low light images to dataset and train using increased exposure data augmentation

<!---
## Citation


--->


## Acknowledgement
This project depends on the work of Alexey Bochkovskiy on [Darknet](https://github.com/AlexeyAB/darknet) and is using the tools  [DarkMark](https://github.com/stephanecharette/DarkMark/) and [DarkHelp](https://github.com/stephanecharette/DarkHelp) by Stéphane Charette. 

