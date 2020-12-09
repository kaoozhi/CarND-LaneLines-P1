# **Finding Lane Lines on the Road**

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

<!-- [image1]: ./examples/grayscale.jpg "Grayscale" -->
[image1]: ./pipeline_images/gray.jpg "gray"
[image2]: ./pipeline_images/blur.jpg "blur"
[image3]: ./pipeline_images/edges.jpg "edges"
[image4]: ./pipeline_images/masked_edges.jpg "masked_edges"
[image5]: ./pipeline_images/image_drawn_with_lines.jpg "image_drawn_with_lines"
[image6]: ./shortcoming/challenge.png "challenge"

---

### Reflection

### 1. Description of the pipeline and modifications made on the draw_lines() function.

My pipeline consisted of 5 steps.
 - convert the image to grayscale

 ![alt text][image1]
 - smooth image by applying a Gaussian Noise kernel

 ![alt text][image2]
 - detect edges on image by Canny transform

 ![alt text][image3]

 - define a region of interest by using a four sided polygon to apply a mask on edges

 ![alt text][image4]

 - apply a Hough transform on image to find two single lane lines and draw them on original image

 ![alt text][image5]


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by the following:
- Filter potential horizontal lines if under a slope threshold

- Divide the filtered lines into two subgroups: positive slope group (left lines) and negative slope (right lines)
  - In each subgroup, iterate over all lines to cluster lines with close slope and intercept
  - the cluster with the most lines is selected as the most representative one of the lane line
  - Use linear regression by grouping all lines in the cluster to recompute slope and intercept of the lane line
  - extrapolate the lane line by the recomputed slope and intercept from the bottom of image to the top of region of interest

- Draw extrapolated single lane lines on image

<!-- ![alt text][image1] -->


### 2. Identify potential shortcomings with your current pipeline


One shortcoming is the slight fluctuations of drawn lines when testing on video,
Another shortcoming is testing the pipeline on the chanllenge.mp4, in the frame where the road surface has two segments with distinguished colours, pipeline detects edges between segments instead of left lane line, the detected right lane line is not accurate either:
 ![alt text][image6]


### 3. Suggest possible improvements to your pipeline

A possible improvement: lane lines' slopes and intercepts of the previous frame would be used in the pipeline to compute the lane lines' slopes and intercepts of the current frame to reduce fluctuations of drawn lines

Another potential improvement is to have adaptive canny or hough transform parameters in case of road surface segmentation, so as to avoid edge detection between road surface segments
