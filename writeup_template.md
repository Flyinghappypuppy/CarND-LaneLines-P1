# **Finding Lane Lines on the Road** 

## Writeup by Ashwin R

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Original Video:
![alt text][./demo_gifs/orig.gif]

My pipeline consisted of 5 steps:

1. Convert the image to Gray Scale.
2. Apply Gausian Blur to the gray scale image
3. Apply Canny edge detection algorithm on the image
![alt text][./demo_gifs/pipeline/a.gif]
4. Select a quadrilateral region to select just the road region 
![alt text][./demo_gifs/pipeline/b1.gif]
![alt text][./demo_gifs/pipeline/b2.gif]
5. Apply Hough Transformation to obtain lines
![alt text][./demo_gifs/pipeline/c.gif]
6. Modified the draw_lines() to do the following:
	1. Filter lines that dont fall within the acceptable angle and y-intercept ranges.
	2. Filter outliers that dont fall within a factor of Standard Deviation (Standard Deviation is calculated on upto last 12 values - greedily selected)
	Before Filters:
	![alt text][./demo_gifs/draw_line/challenge.gif]
	After Filters:
	![alt text][./demo_gifs/draw_line/with_angle-intercept_filter.gif]
	3. To smoothen the line draw - used a weighted average of current line with mean of Last N lines.Fill in frames with no lines with mean of last N
Use the weighted average to extrapolate 
	No Smoothening:
	![alt text][./demo_gifs/draw_line/non-smoothened.gif]
	After Filters:
	![alt text][./demo_gifs/draw_line/solidYellowLeft.gif]

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]	
<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/challenge.gif" width="650" >



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 
There can be some types of noise/bad lines which can pass thought the thresholds of my fliters and adversely affect the mean slope and intercept causing it to deviate. Examples: Duplicate lanes, merging lanes, shadows, line like defarmations/objects on road. Static filters tuned for particular samples cannot adapt to varying conditions like the ones mentioned above. 

Another shortcoming could be ...
Constantly Winding Roads will affect the mean, the current last N mean size is static value and it tuned to better perform for roads that dont bean as often. A dynamically adjusting N value is needed to account for rapid changes in road direction.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...
Color conversion or RGB transformation of the image to a form which supresses noises like shadow and which hightlight the lanes more in the images

Another potential improvement could be to ...
Dyanmically adjusting or Self tuning of parameters based on the type of road and video. Specially parameters of: Region selection, angle and intercept filters,  Outlier filters and Line smoothing.
