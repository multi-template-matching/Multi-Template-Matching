---
layout: page
title: Parameters
permalink: doc/parameters
---

## Input images

* (1) _Template image_ : This is the image searched in the target image. It must be smaller than the target image. It can be of any bit depth. RGB images are automatically converted to 32-bit grayscale.

* (2) _Target image_ : This is the image in which the template image is searched (and must thus be larger than the template). Like for the template image any bit depth is possible.

## Template transformations 
__NB : The more transformations the longer will be the computation time, but the higher the probability to find the object.__  

* (3) _Flip template vertically/horizontally_: __Performing additional searches with the transformed template allows to maximize the probability to find the object__, if the object is expected to have different orientations in the image.  
This is due to the fact that the template matching only looks for translated version of the templates provided.  
Possible transformations include flipping (also called mirroring), rotation (below). Scaling is not proposed in the interface but several templates at different scale can be provided in the multiple template version of the plugin.  
__If vertical and horizontal flipping are selected__, then the plugin generates 2 additional templates for the corresponding tansformation.

* (4) _Additional rotations_ : it is possible to provide a list of clockwise rotations in degrees separated by commas,e.g.: 45,90,180.  
AS with flipping, performing searches with rotated version of the template increases the probability to find the object if it is expected to be rotated.  
__If flipping is selected, both the original and flipped versions of the template will be rotated.__    
__NOTE:__ The template must be of rectangular shape, i.e. for angles not corresponding to "square rotations" (not a multiple of 90°) the rotated template will have some background area which are filled either with the modal gray value of the template (Fiji) or with the pixel at the border in the initial template (KNIME). For higher performance,  the non square rotations can be manually generated before calling the plugin and saved as templates.

* (5) _Score Type_ (default : 0-mean Normalized Cross-Correlation) : This is the formula used to compute the probability map (see [opencv documentation](https://www.docs.opencv.org/2.4/doc/tutorials/imgproc/histograms/template_matching/template_matching.html)). The choice is limited to normalised scores to be able to compare different correlation maps when multiple templates are used.  
  - __Normalised Square Difference (NSD)__: The pixels in the probability map are computed as the sum of difference between the gray level of pixels from the image patch and from the template normalised by the square root of the sum of the squared pixel values. __Therefore a high probability to find the object corresponds to a low score value__ (not as good as the correlation scores usually).  
  - __Normalised Cross-Correlation (NCC)__ : The pixels in the probability map are computed as the sum of the pixel/pixel product between the template and current image patch, also normalized for the difference. __a high probability to find the object corresponds to a high score value.__
  - __0-mean Normalized Cross-Correlation (0-mean NCC)__ : The mean value of the template and the image patch is substracted to each pixel value before computing the cross-correlation as above. __Like the correlation method, a high probability to find the object corresponds to a high score value.__ (usually this method is most robust to change of illumination)

* (6) _Expected number of objects (N)_: This is the expected number of object expected in each image. The plugin will return N or less predicted locations of the object.

## If N>1
Parameters 7 and 8 are only used when several object are expected in each image
* (7) _Score Threshold_ (range 0-1, default 0.5): Used for the extrema detection on the score map(s).  
__If the difference-score is used__, only minima below this threshold are collected before NMS (i.e. increase to evaluate more hits).  
__If a correlation-score is used__, only maxima above this threshold are collected before NMS (i.e. decrease to evaluate more hits).

* (8) _Maximal overlap_ (range 0-1, default 0.3): __Typically in the range 0.1-0.5__.  
This parameter is for the Non-Maxima Suppression (NMS). It must be adjusted to prevent overlapping detections while keeping detections of close objects. This is the maximal value allowed for the ratio of the Intersection Over Union (IoU) area between overlapping bounding boxes.  
If 2 bounding boxes are overlapping above this threshold, then the lower score one is discarded.


## Output
* (9) _Add detected ROI to ROI Manager_ : The predicted location will be added as a rectangular bounding box to the ROI manager and overlaid on the image (if not tick the "Display all" in the ROI Manager)

* (10) _Show result table_ : Returns a result table at the end of the execution with the name of the image and template for that match, the coordinates of the bounding box (center and top left corner) as well as the score of the detection.