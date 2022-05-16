# Multi-Template Matching
Multi-Template-Matching is an accessible method to perform object-detection in images using one or several template images for the search.  
The strength of the method compared to previously available single-template matching, is that by combining the detections from multiple templates,
one can improve the range of detectable patterns. This helps if you expect variability of the object-perspective in your images, such as rotation, flipping...  
The detections from the different templates are not simply combined, they are filtered using Non-Maxima Supression (NMS) to prevent overlapping detections.  

# Implementations
We currently have implemented Multi-Template-Matching (MTM) in:

- [Fiji](https://github.com/multi-template-matching/MultiTemplateMatching-Fiji)  
Activate the _IJ-OpenCV_ and _Multi-Template Matching_ update site.  

- [Original Python implementation](https://github.com/multi-template-matching/MultiTemplateMatching-Python) relying on OpenCV matchTemplate  
_pip install Multi-Template-Matching_ (case sensitive and mind the - )

- [python-oop](https://github.com/multi-template-matching/mtm-python-oop) : a more object-oriented programming version, with a cleaner syntax.
This one relies on scikit-image and shapely (for the BoudingBox)  
Maybe a bit slower but more interoperable and easier to extend.  

- [KNIME](https://github.com/multi-template-matching/MultiTemplateMatching-KNIME) (relying on the python implementation)  
This repository also contains the workflow for classification using multiple templates.  
Download the [workflow](https://kni.me/w/9i0_HPPQlbNzW598) from the KNIME Hub.

# Documentation
Refer to the wiki sections of the respective GitHub repository for the implementation-specific documentation.  
In particular, the Fiji and KNIME implementation have dedicated __youtube tutorials__, while the python implementation comes with example notebooks that can be executed in a browser.  
Below some generic documentation pages:  

- [Introduction](https://multi-template-matching.github.io/Multi-Template-Matching/doc/explanations)
- [Parameters](https://multi-template-matching.github.io/Multi-Template-Matching/doc/parameters)
- [Flowchart](https://multi-template-matching.github.io/Multi-Template-Matching/doc/Flowchart) of the implementation
- [Multi-Channel and 3D image](https://multi-template-matching.github.io/Multi-Template-Matching/doc/multichannel)

Additional resources: 
- the [open-access publication](https://doi.org/10.1186/s12859-020-3363-7)
- [Slides](https://doi.org/10.5281/zenodo.6554166)
- [YouTube playlist](https://www.youtube.com/playlist?list=PLHZOgc1s26MJ8QjYau7NcG5k0zh9SjHpo)
- [Recorded talk](https://youtu.be/xL4uNloai9Q) about single vs template matching, and other outcomes of my PhD
- [Poster](http://doi.org/10.5281/zenodo.3698982) of the project available on Zenodo
- my [PhD thesis](https://doi.org/10.11588/heidok.00030804) on object-detection and image-annotation solutions for microscopy

# Citation
If you use these implementations, please cite:
  
Thomas, L.S.V., Gehrig, J.  
_Multi-template matching: a versatile tool for object-localization in microscopy images_  
BMC Bioinformatics 21, 44 (2020). [https://doi.org/10.1186/s12859-020-3363-7](https://doi.org/10.1186/s12859-020-3363-7)  
Download the citation as a [ris file](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-020-3363-7.ris).

# Funding
This project has received funding from the European Union’s Horizon 2020 research and innovation programme under the Marie Sklodowska-Curie grant agreement No 721537 “ImageInLife”.
