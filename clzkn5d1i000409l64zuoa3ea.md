---
title: "Contributing to the Street Level Imagery project for the American Red Cross"
datePublished: Thu Aug 08 2024 02:10:32 GMT+0000 (Coordinated Universal Time)
cuid: clzkn5d1i000409l64zuoa3ea
slug: contributing-to-the-street-level-imagery-project-for-the-american-red-cross
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1722808635259/a15187cb-2682-4d56-8bc0-19965b38c576.png
tags: image-processing, python, pandas, computer-vision, climate-change, geospatial

---

A while ago, I posted about joining [Civic Tech DC](https://www.civictechdc.org/), an organization that collaborates with local nonprofits and governments on tech projects. Since then, a lot has happened. We've gained more projects and partners, become a registered 501(c)(3) nonprofit, and I am now a co-organizer and core contributor on one of the projects. Since January, I have been contributing to the [Street Level Imagery](https://github.com/AmericanRedCross/street-view-green-view) project with the [American Red Cross](https://www.redcross.org/) and Civic Tech DC. This project, started by the American Red Cross, uses geospatial data to download images from [Mapillary](https://www.mapillary.com/) and processes them to detect the amount of vegetation, known as the Green View Index (GVI). In this post, I'll discuss my work on this project, its use case, and some technical aspects of the project.

## Background

The Red Cross Red Crescent network deals with a lot of natural disasters, and data can help them respond more effectively. One type of disaster is extreme heat, and the Red Cross will set up cooling centers in extreme heat disasters. However, they need to know where to place a cooling center to have the greatest impact. Placing a cooling center in an area more vulnerable to extreme heat is more effective, so they need data about which areas are most at risk.

We can use urban vegetation as one piece of data for assessing extreme heat risk. When an area has more trees and bushes, it will likely be cooler because there is more cooling evapotranspiration and more shade. With data about urban vegetation, we can determine if placing a cooling center there will be effective or not. Conveniently, urban vegetation data can be relatively easily extracted from street level imagery, using a variety of methods like pixel counting and image segmentation. This forms the basis of our project: sourcing street level imagery for an area and then calculating the amount of vegetation there as a proxy for extreme heat risk.

There are some alternatives to using street level imagery and sourcing our own images. As you may know, Google Street View is a large existing repository of street level imagery. However, it costs money to access and the images are licensed restrictively. This does not suit our use case as sourcing thousands of images for an area adds up, and we can't use the images freely. We instead decided to use Mapillary, which is a free and Creative Commons-licensed repository of street level imagery. Satellite imagery is another option and is very popular for working with geospatial data, but it is expensive, and the level of detail for calculating vegetation is just not there for commercial satellites.

## A Tour of Our Repo

The street level imagery project currently consists of three Python scripts:

1. `create_points.py` for taking in an OpenStreetMap roads file and calculating latitude and longitude points along the roads every 20 meters for getting images. Essentially, we get a roads file for a particular area we want to analyze vegetation data for, and `create_points` will filter for specific wanted highway types and interpolate along each highway with a 20m distance and save the latitude and longitude of each point to a GeoDataFrame, which is a pandas DataFrame with one column for geospatial data.
    
2. `assign_images.py` for sourcing images and assigning them to points. These images can be sourced from Mapillary or from local images on disk. For the Mapillary image source, we call the Mapillary API to get the data of all images within a 20m bounding box of a point, and then get the closest one to the point and download that. For the local image source, each image has GPS data of where it was taken in the image's EXIF metadata. We search through all local images in a folder and get the one with the closest GPS location.
    
3. `assign_gvi_to_points.py`, which calculates a Green View Index for each image and point. We currently use two methods to calculate a Green View Index for an image. The Pixel Counting method takes the green channel of an RGB image and calculates the percentage of pixels in the image that exceed a certain value of green. The Segmentation method, on the other hand, uses [Facebook's Mask2Former](https://huggingface.co/facebook/mask2former-swin-large-cityscapes-semantic) segmentation model to segment out the vegetation in an image and calculates the percentage of pixels identified as vegetation out of the total pixels in the image.
    

## What I've Worked On

I have mainly worked on the image sourcing and green view index calculation parts of the project. When I started contributing in January, I implemented the Mapillary image downloader to get open source street level imagery from Mapillary. There were several challenges with that, including deduplicating images where one image could be the closest image for two points and handling timeouts that bubbled up as HTTP 400 Bad Requests from the API. I also implemented the local image source, which matches local images to points using GPS EXIF metadata. It was a challenge to explore the EXIF data in an image, and search for where the GPS metadata was. Overall, I've really enjoyed my time working on this project so far, and have learned a lot about geospatial data and image processing.