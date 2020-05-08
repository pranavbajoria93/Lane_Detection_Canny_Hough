# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

## Write-up Report

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

### 2. potential shortcomings with the current pipeline
Following are some drawbacks of this pipeline
* The region of interest has to be manually set, hence not robust
* This will not work for curved lanes
* A straight line equation is used to fit the detected edges, due to which curvy lanes can not be fit well
* Edge detection might fail in poor weather conditions or bad lighting as all the hyperparameters are tuned and kept static

### 3. Suggest possible improvements to your pipeline
Following are the possible improvements to this pipeline
* Use more sophisticated algorithms like RANSAC to interpolate and find the best fit for the lane
* Use higher degree polynomial functions to properly fit curves.
* Use night vision camera to keep detecting lanes even in poor lighting
* use sensor fusion from multiple cameras 

## Steps to run the project
---

## If you have already installed the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) you should be good to go!   If not, you should install the starter kit to set up the environment for this project. ##

**Step 1:** Set up the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) if you haven't already.

**Step 2:** Open the code in a Jupyter Notebook

To start Jupyter in your browser, use terminal to navigate to your project directory and then run the following command at the terminal prompt (be sure you've activated your Python 3 carnd-term1 environment as described in the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) installation instructions!):

`> jupyter notebook`

A browser window will appear showing the contents of the current directory.  Click on the file called "P1.ipynb".  Another browser window will appear displaying the notebook.  

**Step 3:** Use shift enter to run all the cells one by one or use the 'run all' option under 'cells'.

## Files
* The main code can be found in P1.ipynb
* The writeup report can be found in 


