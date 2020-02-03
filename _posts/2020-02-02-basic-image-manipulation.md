---
layout: post
current: post
class: post-template
subclass: post
navigation: True
title: Basic Image Manipulation
slug: basic-image-manipulation
cover: /assets/img/posts/abhishek_manipulated.jpg
date: 2020-02-02 9:50:00 -05:00
tags:
---

This post is a part of assignment of my CS682 (Computer Vision) course taught by Prof. Zoran Duric in Spring 2020. I demonstrate a few basic image manipulation techniques using my 1000x1000 color image and its grayscale version as examples.

![Color image](/assets/img/posts/abhishek.jpg){:width="50%"}
*Color image*

![Grayscale image from color image](/assets/img/posts/abhishek_gray.jpg){:width="50%"}
*Grayscale image from color image*

# Swapping color channels

The green channel in the color image was swapped with blue channel to
produce the following result.

![RGB to RBG](/assets/img/posts/abhishek_rbg.jpg){:width="50%"}
*RGB to RBG*

# Gaussian blur

A Gausian filter of size 13x13 was applied to both color and grayscale
images.

![Gaussian blur (Color)](/assets/img/posts/abhishek_color_blur.jpg){:width="50%"}
*Gaussian blur (Color)*

![Gaussian blur (Grayscale)](/assets/img/posts/abhishek_gray_blur.jpg){:width="50%"}
*Gaussian blur (Grayscale)*

# Cropping

The images were cropped from coordinates (200,700) to (250,650).

![Cropped image (Color)](/assets/img/posts/abhishek_color_crop.jpg){:width="50%"}
*Cropped image (Color)*

![Cropped image (Grayscale)](/assets/img/posts/abhishek_gray_crop.jpg){:width="50%"}
*Cropped image (Grayscale)*

# Thresholding

The pixel values above 100 were set to 0, and the rest were set to 255.
For color image, this was done across the three color channels producing
weird colors as shown.

![Thresholded image (Color)](/assets/img/posts/abhishek_color_threshold.jpg){:width="50%"}
*Thresholded image (Color)*

![Thresholded image (Grayscale)](/assets/img/posts/abhishek_gray_threshold.jpg){:width="50%"}
*Thresholded image (Grayscale)*

# Inverting

All the pixel values were subtracted from 255 to produce the inverted
images.

![Inverted image (Color)](/assets/img/posts/abhishek_color_invert.jpg){:width="50%"}
*Inverted image (Color)*

![Inverted image (Grayscale)](/assets/img/posts/abhishek_gray_invert.jpg){:width="50%"}
*Inverted image (Grayscale)*

# Changing contrast

All the pixel values in color image were scaled by 0.5 to decrease
contrast. Similarly, for grayscale image, all pixel values were scaled
by 1.8 to increase contrast. Pixel values exceeding the range of
\[0,255\] were thresholded to lie at the boundary, and the resulting
floating values were truncated to integers of type *uint8*.

![Decreased contrast (Color)](/assets/img/posts/abhishek_color_contrast_dec.jpg){:width="50%"}
*Decreased contrast (Color)*

![Increased contrast (Grayscale)](/assets/img/posts/abhishek_gray_contrast_inc.jpg){:width="50%"}
*Increased contrast (Grayscale)*

# Changing brightness

All the pixel values in color image were increased by 50 to increase
brightness. Similarly, for grayscale image, all pixel values were
decreased by 100 to decrease brightness. Pixel values exceeding the
range of \[0,255\] were thresholded to lie in the boundary.

![Increased brightness (Color)](/assets/img/posts/abhishek_color_brightness_inc.jpg){:width="50%"}
*Increased brightness (Color)*

![Decreased brightness (Grayscale)](/assets/img/posts/abhishek_gray_brightness_dec.jpg){:width="50%"}
*Decreased brightness (Grayscale)*

# Adding Gaussian noise

Gaussian noise with mean 0 and standard deviation 50 was added to the
images. The strategy I applied to add noise in the image is as follows.

1.  Generate normally distributed noise of random floating numbers as an
    array with the same shape as the image.

2.  Add the noise to image.

3.  Threshold the pixel values exceeding the range of \[0,255\] to lie
    at the boundary.

4.  Truncate floating pixel values to integers of type *uint8*.

![Adding Gaussian noise (Color)](/assets/img/posts/abhishek_color_noisy.jpg){:width="50%"}
*Adding Gaussian noise (Color)*

![Adding Gaussian noise (Grayscale)](/assets/img/posts/abhishek_gray_noisy.jpg){:width="50%"}
*Adding Gaussian noise (Grayscale)*

# Image gradients

Sobel filters of size 5x5 was applied on grayscale image to produce
image gradients along horizontal and vertical direction of image. The
magnitude of gradients were also computed to produce image that
contained gradients along both directions.

![Image gradient along x-axis](/assets/img/posts/abhishek_sobel_x.jpg){:width="50%"}
*Image gradient along x-axis*

![Image gradient along y-axis](/assets/img/posts/abhishek_sobel_y.jpg){:width="50%"}
*Image gradient along y-axis*

![Image gradient magnitude along both axis](/assets/img/posts/abhishek_sobel_mag.jpg){:width="50%"}
*Image gradient magnitude along both axes*

# Gaussian pyramid

The following figure shows the Gaussian pyramid of depth 9. At each
level, the image from the earlier level is smoothed using Gaussian
filter and is scaled down by half. Images generated at each level of the
pyramid were combined to form one image.

![Gaussian pyramid](/assets/img/posts/abhishek_gaussian_pyramid_alter.jpg){:width="70%"}
*Gaussian pyramid*

The algorithm I implemented to
combine images from multiple levels of pyramid into one large image is
as follows.

1.  Generate an ordered list of *n* images from all levels of pyramid
    in descending order of size.

2.  Assign the smallest image at level *n* as the *result_image*.

3.  Stack the *result_image* along the smaller lengthed axis of image
    at level *n-1*, and fill remaining locations in the image as
    black.

4.  Assign the image generated in step 3 as the *result_image*.

5.  *n ← n-1*

6.  Repeat step 3 to 5 until *n=0*.

Since images are stacked along the smaller of the two axes in step 3,
this resulting image always fitted in the smallest possible rectangle.
For example, since my original image size is 1000x1000, the size of
smallest possible rectangle is 1500x1000, and so is the size of the
final combined image. The space requirement for the combined image of
pyramid is 1500\*1000\*3 = 4500000 bytes ≈ 4.29 MB.

A slightly different stacking strategy produces the following results.

![Gaussian pyramid](/assets/img/posts/abhishek_gaussian_pyramid_layer.jpg){:width="70%"}
*Gaussian pyramid with slightly different stacking strategy*