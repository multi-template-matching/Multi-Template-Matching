---
layout: default
title: Parameters
permalink: doc/parameters
---

## Input images

Parameters | Explanations
---------- | ------------
__Template(s)__ | This is a small image representing the object to search. Multiple templates can be provided, or a given template can be transformed by rotation and flipping to generate additional templates. It can be of any bit depth. RGB images are automatically converted to 32-bit grayscale.
__Image(s)__ | This is the larger image in which the object-search is performed. Like for the template image any bit depth is possible.
            


## Template transformations 
The more transformations the longer will be the computation time, but the higher the probability to find the object.  
Performing additional searches with the transformed template allows to maximize the probability to find the object, if the object is expected to have different orientations in the image.  
The reason is that template matching only looks for translated version of the templates provided.  
Possible transformations include __flipping__ (also called mirroring) and __rotation__.  
Scaling is not proposed in the graphical interfaces but several templates at different scale can be provided in the multiple template version of the plugin.  

Parameters | Explanations
---------- | ------------
__Flipping__ | Generate additional templates by flipping (mirroring) the provided templates (vertical, horizontal, or both. If both, 2 templates are generated, one for each flipping axis).
__Rotations__ | List of clockwise rotations in degrees separated by commas,e.g.: 45,90,180. If flipping is selected, both the original and flipped versions of the template will be rotated. 

__NB__: For rotation with angles not corresponding to "square rotations" (not a multiple of 90°) the rotated template will have some background area which are filled either with the modal gray value of the template (Fiji) or with the pixel at the border in the initial template (KNIME). For higher performance,  the non square rotations can be manually generated before calling the plugin and saved as templates.

## Template matching 
__Score Type__ : This is the formula used to compute the probability map (see [opencv documentation](https://www.docs.opencv.org/2.4/doc/tutorials/imgproc/histograms/template_matching/template_matching.html)).  
The choice is limited to normalised scores to be able to compare different correlation maps when multiple templates are used.  
Default score is 0-mean Normalized Cross-Correlation, which usually offers the best results if light-intensity varies between images. 

Score-types | Explanations
----------- | ------------
__Normalised Square Difference (NSD)__ | The pixels in the probability map are computed as the sum of difference between the gray level of pixels from the image patch and from the template normalised by the square root of the sum of the squared pixel values. __Therefore a high probability to find the object corresponds to a low score value__ (not as good as the correlation scores usually).  
__Normalised Cross-Correlation (NCC)__ | The pixels in the probability map are computed as the sum of the pixel/pixel product between the template and current image patch, also normalized for the difference. __a high probability to find the object corresponds to a high score value.__
__0-mean Normalized Cross-Correlation (0-mean NCC)__ | The mean value of the template and the image patch is substracted to each pixel value before computing the cross-correlation as above. __Like the correlation method, a high probability to find the object corresponds to a high score value.__ (usually this method is most robust to change of illumination)


## Maxima detection and Non-Maxima Supression (NMS)

Parameters| Explanations
----------|-------------
__Expected number of objects (N)__ | This is the expected number of object expected in each image. If provided, the algorithm returns N (or less) possible object-locations. __If the number of object is not known__, set this value to a large number in Fiji (in python default to infinity) and use the score-threshold to keep the best predictions.

### If N>1
If several objects are expected in the image, detections are filtered to prevent overlaping detections of the same-object.

Parameters| Explanations
----------|-------------
__Score Threshold__ (range 0-1, default 0.5) | Used for the extrema detection on the score map(s).  

__If the difference-score is used__, only minima below this threshold are collected before NMS (i.e. increase to evaluate more hits).  
__If a correlation-score is used__, only maxima above this threshold are collected before NMS (i.e. decrease to evaluate more hits).


Parameters| Explanations
----------|-------------
__Maximal overlap__ (range 0-1, default 0.3) | This is the maximal value allowed for the ratio of the Intersection Over Union (IoU) area between overlapping bounding boxes. Typically in the range 0.1-0.5.

The IoU is used for the Non-Maxima Supression.  
Overlapping bounding-boxes can either predict the locations of distinct neighboring objects (lower overlap), or the location of the same object at slightly shifted positions (e.g by 2 different templates, higher overlap).  
If 2 candidate bounding-boxes are overlapping above this overlap threshold, then the lower score bounding-box is discarded.  
The value of this parameter depends on object-size and density: high object-density usually means more overlap between bounding boxes.
