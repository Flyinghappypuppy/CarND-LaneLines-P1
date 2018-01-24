# **Finding Lane Lines on the Road** 

## Writeup by Ashwin R

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Original Video:

<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/orig.gif" width="600" >

My pipeline consisted of 5 steps:

1. Convert the image to Gray Scale.
2. Apply Gausian Blur to the gray scale image
3. Apply Canny edge detection algorithm on the image
	After first 3 steps:
	<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/pipeline/a.gif" width="600" >
4. Select a quadrilateral region to select just the road region 
<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/pipeline/b1.gif" width="600" >
<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/pipeline/b2.gif" width="600" >
5. Apply Hough Transformation to obtain lines
<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/pipeline/c.gif" width="600" >
6. Modified the draw_lines() to do the following:
	1. Filter lines that dont fall within the acceptable angle and y-intercept ranges.
	2. Filter outliers that dont fall within a factor of Standard Deviation (Standard Deviation is calculated on upto last 12 values - greedily selected)
	Before Filters:
	<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/draw_line/no-filter.gif" width="600" >
	After Filters:
	<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/draw_line/filter.gif" width="600" >
	3. To smoothen the line draw - used a weighted average of current line with mean of Last N lines.Fill in frames with no lines with mean of last N
Use the weighted average to extrapolate 
	No Smoothening:
	<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/draw_line/non-smoothened.gif" width="600" >
	After Smoothening:
	<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/draw_line/smoothened.gif" width="600" >

If you'd like to include images to show how the pipeline works, here is how to include an image: 

Finally:
<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/final.gif" width="600" >


### 2. Identify potential shortcomings with your current pipeline

Some shortcomings:
1. There can be some noise/bad lines very similar to road lanes, with similar angle and y-intercept,  that can pass thought the thresholds of my fliters and adversely affect the mean slope and intercept causing it to deviate. Examples: Duplicate lanes, shadows, merging lanes, line like defarmations/objects on road etc. Static thresholds tuned for particular samples cannot adapt to varying conditions like the ones mentioned above. 

2. Another shortcoming could be ...
Constantly Winding Roads will affect the mean, the current last N mean size is static value and it tuned to better perform for roads that dont bean as often. A dynamically adjusting N value is needed to account for rapid changes in road direction.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...
Color conversion or RGB transformation of the image to a form which supresses noises like shadow and which hightlight the lanes more in the images

Another potential improvement could be to ...
Dyanmically adjusting or Self tuning of parameters based on the type of road and video. Specially parameters of: Region selection, angle and intercept filters,  Outlier filters and Line smoothing.
