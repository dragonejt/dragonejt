---
title: "Attending CVPR 2024: Interesting Talks and What I Learned"
datePublished: Fri Jun 28 2024 02:16:39 GMT+0000 (Coordinated Universal Time)
cuid: clxy2bb2y000109l81jyx3krf
slug: attending-cvpr-2024-interesting-talks-and-what-i-learned
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1719540113007/1951f7c2-168f-4c95-ba91-016a4e45f236.jpeg
tags: ai, artificial-intelligence, conference, machine-learning, research, computer-vision, deep-learning, ml, space, geospatial, space-exploration, cvpr

---

Last week, I attended [CVPR 2024](https://cvpr.thecvf.com/Conferences/2024), the annual IEEE/CVF Computer Vision and Pattern Recognition conference. More specifically, I attended the [AI4Space Workshop](https://aiforspace.github.io/2024/) and a few tutorials at the conference, as the main conference sessions didn't capture my interest as much. It was a fun and insightful experience, where I got to listen to many interesting talks, check out multiple interesting posters, and overall learn a lot from the conference. I wanted to go over a few papers and tutorials I found interesting from what I attended.

## [Mitigating Challenges of the Space Environment for Onboard Artificial Intelligence: Design Overview for a recently launched payload](https://openaccess.thecvf.com/content/CVPR2024W/AI4Space/papers/Del_Castillo_Mitigating_Challenges_of_the_Space_Environment_for_Onboard_Artificial_Intelligence_CVPRW_2024_paper.pdf)

I am interested in the development and deployment of onboard machine learning for satellites, so this paper was particularly insightful for me. This paper explained well the challenges of "managing thermal constraints, ensuring radiation resilience, \[and\] overcoming the limited communication bandwidth" when running machine learning models onboard satellites, as well as steps taken to mitigate those risks. The actual hardware used for machine learning was the NVIDIA Jetson Nano, an edge AI hardware device that includes a CUDA-compatible GPU.

Temperature control is crucial to computing in space, as extreme cold will shut off the computers, while extreme heat can cause overheating and damage. There is no natural convection in space, so other actions had to be taken to spread the heat. To mitigate this problem, the team added a "carrier frame, a singular aluminium frame designed to closely follow the contours of the module". The frame acts as a heat sink and conducts heat away from the CPU, GPU, and power management chip. Radiation poses another risk, as it can flip bits in the board's memory, leading to data corruption. The team mitigated the radiation risk by storing backups of all files and computing MD5 hashes of the files. If the hash of a file was different, then the file would be replaced with its backup, and if all backups were also corrupted, the satellite would wait to uplink a good version of the file. The final challenge considered was the amount of bandwidth that the satellite could use when downlinking data to ground stations, which slows transfers and limits the number of files that can be sent daily. The mitigation that the team took was to use the "JPEG-XL algorithm for efficient on-orbit compression of image data". This allowed the team to downlink more pictures in a day.

I found the paper particularly insightful because it used a backup and hashing mechanism to ensure the integrity of files on disk rather than only relying on hardware radiation shields to protect from radiation and bit flips. The team also explained their reasoning for choosing the JPEG-XL image compression algorithm well, discussing the options when using JPEG-XL and the tradeoffs compared to other algorithms. Their software approaches to fixing many "hardware" or "physical" problems were very insightful. This paper also won Honourable Mention at the AI4Space workshop, and I believe they did an excellent job!

## [Transformers for Orbit Determination Anomaly Detection and Classification](https://openaccess.thecvf.com/content/CVPR2024W/AI4Space/papers/Re_Transformers_for_Orbit_Determination_Anomaly_Detection_and_Classification_CVPRW_2024_paper.pdf)

This was an interesting application to a common set of space machine learning problems. Transformers are generally known in the world of natural language processing and large language models as models that accept sequential input in the form of tokens and find relationships between tokens. Seeing them applied to the world of space machine learning on time series data was definitely intriguing. The paper itself goes through parts of spacecraft navigation that fit the model of time series data, and applies transformers to orbit determination, anomaly detection, and anomaly classification.

The team tested three transformer models on tokenized time series data. The Measurement Transformer (MT) "builds off the BERT architecture, adapted for time series data by using a time encoding instead of position encoding". The Tracking Pass Transformer (TPT) uses nested Transformer encoders to extract information from short periods of measurements and combine them for anomaly classification. The Vision Transformer (ViT) "divides an image into a set of 2-dimensional patches, then treats this set of image patches as a sequence that can be processed with a Transformer encoder model". All three transformer models were trained and tested on five classes:

* Drag – erroneous estimate of the spacecraft’s coefficient of drag.
    
* Gravity – reduction in spherical harmonics degree and order in the estimation filter’s dynamical model.
    
* Maneuver – erroneous finite thrust maneuver direction and magnitude estimate.
    
* Nominal – no dynamical or measurement mismodel present.
    
* SRP – erroneous estimate of the solar radiation pressure scale factor
    

Overall, all 3 models "achieved over 80% validation accuracy, with the best performing Tracking Pass Transformer models achieving 93% validation accuracy". This signifies the effectiveness of using transformers for time series data in spacecraft navigation.

Personally, I found this paper very interesting because of the novel approach of something that is generally used on NLP being applied in space machine learning on time series data. It will be interesting to see if, in the future, more models or methods in other fields are applied to space machine learning.

## [Geospatial Computer Vision and Machine Learning for Large-Scale Earth Observation Data](https://cvpr.thecvf.com/virtual/2024/tutorial/23727)

After attending the AI4Space workshop, I then attended this tutorial on geospatial computer vision at CVPR because it was in the same domain as the current [Street Level Imagery project that I am contributing to for the American Red Cross](https://github.com/AmericanRedCross/street-view-green-view). This tutorial mainly covered background knowledge and recent advances in geospatial computer vision on satellite and aerial imagery, and covered standards and methods, such as GeoTIFF for storing geospatial data inside rasterized images, shapefiles for storing geospatial data as well as several current machine learning models being used for geospatial computer vision. There were also discussions about how to participate in the field, such as competitions like [SpaceNet](https://spacenet.ai/).

Overall, while the discussion of geospatial computer vision was very interesting, it didn't align too much with my Street Level Imagery project, as the tutorial focused more on satellite and aerial images and the challenges and advances there, whereas we are focusing more on street level imagery as it is much more accessible and much more frequently updated. For example, while shapefiles are a common geospatial data format, GeoTIFFs are most commonly seen in satellite and aerial imagery. I will be publishing a post about my experience with the Street Level Imagery project and the American Red Cross soon, so be on the lookout for that!

## Conclusion: What's Next?

Attending CVPR 2024 was definitely an experience that allowed me to learn more about two new fields that I have become interested in. The AI4Space Workshop allowed me to gain a better understanding of where machine learning is being applied in space exploration and its current challenges. Meanwhile, the geospatial computer vision tutorial gave a great overview of the current datasets, methods, and community surrounding geospatial CV. These sessions not only broadened my knowledge of the field but also inspired new ideas for my ongoing projects. If you are following my blog, you'll hear from me more as I delve deeper into the papers presented at AI4Space, and when I talk about my experience working on the Street Level Imagery project with the American Red Cross and Civic Tech DC.