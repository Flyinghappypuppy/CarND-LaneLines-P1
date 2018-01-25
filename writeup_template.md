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
2. Apply Gaussian Blur to the gray scale image
3. Apply Canny edge detection algorithm on the image
	
	<img title="After first 3 steps" src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/pipeline/a.gif" width="600" >

4. Select a quadrilateral region to select just the road region 
	
	<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/pipeline/b1.gif" width="300" > <img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/pipeline/b2.gif" width="300" >

5. Apply Hough Transformation to obtain lines

	<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/pipeline/c.gif" width="600" >

6. Modified the draw_lines() to do the following:
	
	1. Filter lines that dont fall within the acceptable slope and y-intercept ranges.
	
	2. Filter outliers that dont fall within a factor of Standard Deviation (Standard Deviation is calculated on upto last 12 values - greedily selected). 
	
		Before using filters:

		<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/draw_line/no-filter.gif" width="600" >

		After using filters:

		<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/draw_line/filter.gif" width="600" >
	
	3. To smoothen the line draw, I used a weighted average of the parameters of the current line and that of the last 'N' lines. When frames didnt have a line, used the mean of last 'N' lines' parameters as a substitute. 

		No Smoothening:
			
		<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/draw_line/non-smoothened.gif" width="600" >
			
		After Smoothening:
	
		<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/draw_line/smoothened.gif" width="600" >

	4. Use the above calculated weighted average to extrapolate to appropriate length in every frame.


Final result after the pipeline:

<img src="https://github.com/timeperceptron/Lane-Detection-CarND-P1/blob/master/demo_gifs/final.gif" width="600" >


### 2. Identify potential shortcomings with your current pipeline

Some shortcomings:

1. There can be some noise/bad lines very similar to road lanes, with similar angles and y-intercepts,  which can pass thought the thresholds of my filters and adversely affect the mean slope and intercept causing it to deviate. Examples: Duplicate lanes, shadows, merging lanes, deformations/objects on road which resemble the lane etc. Static thresholds tuned for particular samples cannot adapt to varying conditions like the ones mentioned above. 

2. Constantly winding roads can affect the mean, the current window size of 'N' to calculate last 'N' lines' mean is static value and it is tuned to better perform for roads that dont wind as often. A dynamically adjusting 'N' value is needed to account for rapid changes in the road direction.

3. Change in lighting and weather conditions will make the current pipeline perform subpar as the static parameters wont be able to adapt to the challenging conditions.


### 3. Suggest possible improvements to your pipeline

1. Color conversion or RGB transformation of the image to a form which will help supress noises like shadows and hightlight lanes more.

2. Dynamically adjusting or self-tuning parameters which adapt to varying conditions, specially the following parameters: Region selection, angle and intercept filters,  outlier filters and line smoothing.
