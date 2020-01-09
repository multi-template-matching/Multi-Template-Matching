---
layout: default
title: Multi-channel and 3D images
permalink: doc/multichannel
---

# Multi-Channel images
The plugin can be used with RGB images directly.
There are internally converted to grayscale for the detection.  

More generally, there are 2 options for template matching with multi-channel images : 
1) either projecting the channels to a grayscale image (typically average projection for RGB images) 
2) running the detection on exclusively one channel. 

The choice depends on what should be found: if a particular channel distribution is expected, or if a single channel offers enough discriminative power for the detection. 

# Template matching in 3D
For 3D images there are also 2 options:
- the volume can be projected to a 2D image (e.g maximum intensity projection) before running the detection with a 2D template. 

- the detection with the 2D template can be run on every Z-slice or on a subset of Z-slices and the detections with the highest score can be retained.  
3D bounding boxes can even be reconstructed by combining overlapping bounding boxes between Z-slices, similar to 
D. Waithe, J. M. Brown, K. Reglinski, I. Diez-Sevilla, D. Roberts, and C. Eggeling, ‘Object Detection Networks and Augmented Reality for Cellular Detection in Fluorescence Microscopy Acquisition and Analysis.’, bioRxiv, Feb. 2019.

# What about using a 3D template ?
Performing the search with a 3D template is technically possible, but probably computationally expensive, as this would mean comparing pixel intensities between 3D volumes for every position of the sliding window and performing the search along the 3 dimensions of the images.  
More dramatically, the number of template rotations to perform to capture a maximum of object can possibly be very high, as one can rotate along the 3 axis, so in the end it is not a method of choice. 