# Terms

aperture: 孔，穴；（照相机，望远镜等的）光圈，孔径；缝隙

diffraction: （光，声等的）衍射，绕射

Orthographic： 正字法的；拼字正确的；直角的；正投影的

phenotype: 表型，表现型；显型

retina:  [解剖] 视网膜

morphology: （生物）形态学；（语言学中的）词法，形态学；结构，形态

equalisation:  均衡

bimodal:  双峰的；有两种统计方式的

semantic: 语义的；语义学的



---



# Introduction

+ **Image captioning**: Automatically generate natural language descriptions according to the content in an image
+ Deep learning (e.g. recurrent neural network)
+ Digital image processing    ->    Computer vision/Machine vision
  + recovers useful information about a scene from its two-dimensional projections **(Jain, 1995)**
  + an enterprise that uses *statistical* methods to disentangle data using models constructed with the aid of *geometry*, *physics*, and *learning* *theory* **(Forsyth, 2002)**.



---



# Image Formation 

+ An image is a 2D representation of 3D world

+ Digital image: ADC and sampling

+ Camera quantum efficiency

+ Image sensor

  A digital image is an array, or a matrix, of pixels arranged in columns and rows. The image is obtained from traditional film, scanner, **CCD (charge-coupled device )** or **CMOS (complementary metal-oxide semiconductor)** digital optical sensors directly. 

  **CCD**

  + Current driven
  + Pixels collect charge and shift charge on the imager surface to the output for sampling
  + Output: analogue pulse
  + Speed: lower

  **CMOS**

  + Voltage driven
  + Light strikes pixels and creates voltage. The voltage is sampled directly at the pixel, digitized on the imager
  + Output: digital signal
  + Layer stack up; lower sensitivity
  + Speed: Higher

+ Lenses

  + Aperture too small: diffraction effects blur the image(light seen as wave)
  + Aperture too big: many directions are averaged, blurring the image

+ Depth of field

  The distance between the nearest and the farthest objects that are in acceptably sharp focus in an image.
  
+ Projections

  + Perspective

    + distant objects appear smaller than nearer objects

    + lines parallel in nature appear to intersect in the projected image

  + Orthographic

    + projecting points and vectors in a perpendicular fashion onto a plane

    +  lengths are not foreshortened

    + e.g. in CAD

      

---



# Image Representation

Binary images, Greyscale images, Colour images

+ Binary images: Black and white image

+ Greyscale images: Greyscale, 8 bits (256 scale)

+ Colour image: True colour, 3 layers, 24bits, 16777216 scales

  + Colour separation - Bayer filter 

    Mimic the physiology of the human eye

    green photosensors *luminance-sensitive* elements

    red and blue ones *chrominance-sensitive elements*

  + Colour space - RGB     Red, Green Blue

  ![image-20230610161706828](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230610161706828.png)

  + Colour space - **HSI**
    + **Hue** - what humans implicitly understand as colour
    + **Saturation** - pastel colours (low saturation) and vibrant colours (high saturation)
    + **Intensity** - luminance information

![image-20230610161642217](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230610161642217.png)

+ RGB to Greyscale

  ![image-20230610162519105](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230610162519105.png)



---



# Applications

+ **AGRICULTURE**: **Plant phenotyping**

  ​	Surface gradients

+ **DIRECTED ADVERTISING**

+ **Future ticket**: The imaging system gate

+ **3D Face Recognition in Real-world Environments**

+ **Pig face recognition**



---



# **Human Vison    &    Machine Vision**

+ **The Human Eye**

  + **Lens** 

     Function: focuses light onto the retina

     Transparent tissue that bends light passing through the eye. 

     To focus light, the lens can change shape by bending. 

  + **Retina** 

     Function: converts images into neural impulses

     Multi-layered, light-sensitive lining of inner eye. 

     Formed from brain tissue.  

     Millions of specialized brain cells (neurons).

     Connected by the optic nerve to the brain.

  + **Brain**

     Function: analyses and interprets impulses to enable understanding / interaction.

+ Machine Vision

  ![image-20230610163453055](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230610163453055.png)



---



# **Image Thresholding and Binary Morphology**

+ Grey Level Image Analysis

  Most acquired images are grey level & contain too much data

  Often we wish to convert grey level images to binary (to reduce data)

+ Image Segmentation

  Involves isolating features of interest (e.g. an object) from other parts of the image in order to analyse individual features

  + **Thresholding**

    + histogram-based method

      ![image-20230610164453399](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230610164453399.png)

      Image histogram is invariant to rotations

      Histogram Equalisation:  Adjusting image intensities to enhance contrast

      ![image-20230610165408646](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230610165408646.png)

      ---

      **Bimodal histogram**:

      ![image-20230610165425486](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230610165425486.png)

      1. Plot the histogram

      2. Find a threshold

      3. Anything above the threshold is converted to 0s and anything below converted to 1s (or vice versa)

      **Grey Level to Binary Thresholding:**

      Possible methods for establishing *thresh* :

      + Human adjustment

      + “goodness measure” (e.g. empirical observations)

      + Use mean or median of intensity distribution

      + **Use a statistical method**

        #### **Otsu Thresholding**

        Iterate through every possible threshold value and minimise a combined weighted variance for foreground and background classes

        #### **Adaptive thresholding**

        If the value of the current pixel is *t* percent less than the average pixel value in a neighbourhood area, then set it to 0 (assuming 0 is background)

    + Clustering

    + Mixture model

  + **Using Boundaries**

  + **Learning based**

    + Semantic Segmentation

    + Instance Segmentation

  

+ **Morphological operations**

  + We must first segment the image into separate connected regions (blobs or objects), sometimes called clustering.

  + Then we can analyse each region in isolation

  + To find a connected region 

    + Find a seed pixel in the region.

    + Grow the region by locating all connected pixels

    + Repeat for all regions (i.e. until no more seed pixels are found)

      ![image-20230610171927779](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230610171927779.png)

    + The concept of 4 or 8 connected

#### **Erosion and Dilation**

**Erosion**: Remove pixels that are adjacent to the background  (4 or 8 adjacent)

**Dilation**: Add new pixels around the boundary of a region (4 or 8 adjacent)

**Progressive Erosion:**

1. First mark all pixels to be removed

2. Then alter pixel to background value

**Progressive Dilation:**

**Opening： Erosion followed by Dilation**

+ Border irregularity are removed by opening
+ The operation can smooth irregular borders, and fill in or remove isolated pixel noise and fine lines.
+ The difference between image A and image C is known as the residue and may be of interest.
+ Applications: Region separation

**Closing： Dilation followed by Erosion**

Closing and Opening: Principle of the Dilation – Erosion method for printed circuit board inspection

Structuring element

+ Fit: matches all

+ Hit: matches 1+

![image-20230610173003277](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230610173003277.png)![image-20230610173012818](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230610173012818.png)

Erosion with a structuring element:

+ If fit change to 1

+ else  change to 0

**Skeletonization (Thinning)**

Use successive boundary deletion

Rules:

1. Skeleton should be one pixel wide.

2. Skeleton should lie near the region centre.

3. Skeletal pixels must be connected and form the same number of regions as existed in the original figure.

Useful in text analysis when we are not interested in the ‘bulk’ of the shape

Robotic Weeding at the CMV： Meristem Detection 



---

# Image Convolution

## Features

A piece of information, commonly from an interesting part of an image, that can be used to solve a problem.

### Feature Extraction

+ Edges(contours)

+ Corners(interest points)

### Image noise

+ Salt and pepper noise: random white and black pixels

+ Gaussian noise: variations in intensity drawn from a Gaussian distribution

### Image filters

+ Emphasise certain features or remove other features

+ Smoothing, sharpening, edge enhancement

+ Applications: remove noise, detect edge

  + noise reduction:  Modify the pixels in an image based on some function of a local neighbourhood of each pixel

    #### Linear filtering: convolution

    kernel, mask, filter

    Padding: handling the boundary

### Edge detection

+ Gradient

  Sharp changes in brightness, first derivatives in 2D

  Gradient and edge are perpendicular

+ Convolution for edge detection

  + The Prewitt operator

    ![image-20230627144236150](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230627144236150.png)
    
  + The Sobel operator
  
    ![image-20230627144508890](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230627144508890.png)
  
  + The Laplacian operator
  
    ![image-20230627144531606](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230627144531606.png)
  
### 3D Convolution



![image-20230627144806601](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230627144806601.png)

# Feature Extraction

## Feature invariance

+ Greyscale invariance

  **Texture spectrum**:  A texture image can be decomposed into a set of essential small units, called Texture Units

  ![image-20230627150513364](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230627150513364.png)

  ​    **LBP: Local binary patterns**

  ​	**2**^8 =256 units  instead of **3^**8=6561 compared to Texture spectrum

  ![image-20230627151018424](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230627151018424.png)

  **texture primitives**

  ![image-20230627151410023](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230627151410023.png)

  **Uniformity patterns**

  Uniformity measure U (“pattern”) is the number of bitwise transitions from 0 to 1 or vice versa

  Its uniformity measure is at most 2

  ![image-20230627152510944](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230627152510944.png)

+ Rotation invariance

  + Utilize magnitude of difference to find dominant direction in neighbourhood
  + Dominant direction is the maximum difference of neighbouring pixels from central pixel 
  + Dominant direction is set as reference 
  + Use this reference to rotate the “weight”

+ Illumination invariance

  + Divide image into cells
  + Compute LBP histogram pre cell
  + Concatenate all histograms(with or with out normalisation)

+ Scale invariance

  + **SIFT**: Scale Invariant Feature Transform 

    1. Image subsequently blurred using a Gaussian convolution

    2. Scale space constructed to simulate different scales of observation

       Down-sampling and repeat this process    -->     First Second Third Octave

    3. Candidates for the *key points* 

       + **LoG:  Laplacian of Gaussian**
    
       + **DoG:  Difference of Gaussian** 
    
         ![image-20230627153524293](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230627153524293.png)
    
    4. confirm *key points –* feature detection achieved
    
    5. identify key point orientation
    
    6. compute feature descriptor
    
       ![image-20230627153750998](C:\Users\18217\AppData\Roaming\Typora\typora-user-images\image-20230627153750998.png)
    
    **Appications**
    
    + Object recognition
    + SLAM
    + Panorama stiching
    + Tracking



---

# Machine learning in machine vision

## CNN

+ Convolutional layers
+ Activation layers
+ Pooling layers
+ Fully connected layers









---

# Object detection

+ **Image classification**

  Classify an image according to its visual content, outputting a class or a probability that the input is a particular class

+ **Object detection**

  Classifying and localising multiple objects in an image

## Object detection with a CNN

+ Weights updating  -->  Output with additional 4 units: [x, y, width, height]

+ Multiple objects

  + Divide image into a ***n*** by ***m*** grid

  + Slide a CNN across all i by i regions (e.g. 3 by 3 regions)

  + Problem A: missing out large pumpkins (larger than the 3 by 3 window)

    Solution to A: increase window size and repeat

  + Problem B: detecting the same object multiple times, at slightly different locations
	  Solution to B: non-max suppression:

    + Add an extra *objectness* output to the CNN to estimate the probability that a pumpkin is in the bounding box
    + Remove all bounding boxes with a low objectness score
    + Find the bounding box with the highest objectness score
    + Remove all the other bounding boxes that largely overlap with it (using IoU)
  
  + Viola & Jones 
  
+ Supervised learning - AdaBoost

  + The output of 'weak learners' is combined into a weighted sum that represents the final output of the boosted classifier.
  + AdaBoost operates in a sequential manner where subsequent weak learners are tweaked in favour of those instances misclassified by previous classifiers
  + The weak learning algorithm is designed to select the single rectangle feature which best separates the positive and negative examples

+ Evaluation

  + Intersection over union (IoU)
    + The area of the intersection divided by the area of the union of a predicted bounding box and a ground-truth box

  + Precision and Recall
  + Mean average precision (mAP)
    + Average Precision (AP) is the area under the curve
    + Mean Average Precision (mAP) is the mean of AP across all classes

+ Object detection with deep learning

  + R-CNN (Fast R-CNN, Faster R-CNN)
  + The YOLO family


## YOLO

Unified Detection

+ uses features from the entire image to predict each bounding box
+ predicts all bounding boxes across all classes for an image simultaneously
+ end-to-end training and real-time speeds with a high AP







