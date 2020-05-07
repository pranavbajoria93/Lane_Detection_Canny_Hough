# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[grayscale]: ./test_images/gray_scale.jpg "gray_scale"
[blurred]: ./test_images/blurred.jpg "blurred"
[canny_edges]: ./test_images/canny_edges.jpg "canny_edges"
[region_of_interest]: ./test_images/region_of_interest.jpg "region_of_interest"
[with_lines_after_hough]: ./test_images/with_lines_after_hough.jpg "with_lines_after_hough"
[overlayed_weighted]: ./test_images/overlayed_weighted.jpg "overlayed_weighted"
---

### Reflection

### 1. Pipeline

My pipeline consisted of 5 steps. 
1. converting the images to grayscale
![alt text][grayscale]

2. Blurring the grayscale image using opencv's gaussian blur with a kernel size of 5x5
![alt_text][blurred]

3. using the opencv canny edges filter, using low threshold = 40 and high threshold = 120
![alt_text][canny_edges]

4. applying a region of interest function by giving a polygon with 4 relavent vertices that appropriately selected the region of interest.
![alt_text][region_of_interest]

5. using hough lines to find small linesegments. This needed a lot of hyperparameter tuning to get it to work well on all the images.

6. modifying the draw_lines() function to draw single optimum line for each left and right boundary of the lane that best fits the smaller lines that were detected by the hough.
  To do this, I followed the following series of steps:
  * computed slope and y intercept for each line in the hough lines
  * performed some sanity checks by disregarding all the slopes that were either too steep or too flat, i.e. m<abs(0.3) or m>abs(1)
  * sorted and stored left and right slopes and intercepts in separate list based on a sign check on the slope
  * took a mean of both slopes and intercepts to average the line so that we get the optimum parameters of the line that fits best
  * calculated the extreme coordinates based on the image's shape that satisfied the left line and the right line equation
  * drew lines using these coordinates and updated the cache
  * for any empty list errors, which means there were no sane lines found by the hough lines function, used the cached coordinates to draw and retain the previously computed lines (useful on videos).
  * used the ROI again to limit the left and right lanes to the actual lanes.
![alt_text][with_lines_after_hough]

7. overlaying the lines on to the original image by performing a weighted addition
![alt_text][overlayed_weighted]

### 2. Identify potential shortcomings with your current pipeline
One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
