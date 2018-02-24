# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the work


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve "Result"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps. 

#### step 1
I filtered out colors based on threshold for the RGB colors. This enabled me to better seperate the white and yellow lane lines from the prictures

#### step 2
A small blur is added. This filters out small, low gradient spots

#### step 3
Image is converted to grayscale to make it easier to locate edges

#### step 4
Edges are located with the canny edge detector

#### step 5
The region of the image I want to look at is limited to a triange capturing mostly the driving lane

#### step 6
Left and right line lines are detected with the hough lines function. One important part in this step was adjusting the draw_lines() function

### draw_lines() function
First a loop is started to loop over all edge detected lines.   
I seperate left and right lane slopes and centers into seperate arrays.  
When deciding if a lane should be added to the slopes and centers arrays I check for a reasonable slope. All others will be ignored.  
After looping over all lines, the averages for the slopes and centers are calucated.  
The center coordinates together with the slopes are used to calculate the x coordinates for the lines that start at 60 percent of the image height and end at the image height.  
The calculated coordinates are used to draw the lane line onto the image.

#### step 7
Overlay left lane and right lane image on original image

#### final image output
The output of a image with lane line drawn on it looks like this:

---

![result of drawing lane lines][image1]


### 2. Potential shortcomings with the current pipeline

* When calculating line center averages, every line gets the same importance. Therfor, if edges are detected that belong to objects at the side of the road, the drawed lines will be pushed next to the lane lines.
* The drawed lines in the video shake quit a bit. It seems like a good option to calculate a moving average to prevent this from happening.
* Lines that have light shining on them are not well detected, because the gradient is lower then the gradient used in the canny edge detection. Using a lower value in the edge detection function added to much noise, because it results in more edges that are detected


### 3. Possible improvements to the pipeline

* Give more importance to lines that are longer and closer to the center of the lane. I think this might enable me to use a lower gradient value in the canny edge detection function. More objects will be detected alongside of the road, but those will be ignored if we can lower there importance.
* Implement a moving average of the line centers while playing a video. This should prevent the drawed line from shaking to much
