#**Finding Lane Lines on the Road** 
This is the first project in Udacity Self-driving Car Nanodegree program. The goals of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report
In this project, I applied the knowledge I learned from Udacity such as OpenCV, Hough Transform to detect lane lines and draw lines. The project was challenging because I did not have experience with OpenCV.

![Udacity Self-driving Car](https://github.com/baocongchen/carnd-finding-lane-lines/blob/master/examples/Udacity-Self-Driving-Car.jpg "Udacity self-driving car")

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps:

First, I use thresholds to filter image such that yellow lines and white lines are kept, and other areas are converted to black color. 

<p align="center">
<img src="https://github.com/baocongchen/carnd-finding-lane-lines/blob/master/pipeline_images/1colorfilter.png" width="450" alt="Filter color">
</p>

Next I convert the images to grayscale with only one color channel. 

<p align="center">
<img src="https://github.com/baocongchen/carnd-finding-lane-lines/blob/master/pipeline_images/2grayscale.png" width="450" alt="Grayscale">
</p>

I define a kernel size and apply Gaussian smoothing to suppress noise and spurious gradients, thereby smoothing the edges. 

<p align="center">
<img src="https://github.com/baocongchen/carnd-finding-lane-lines/blob/master/pipeline_images/3smoothed_image.png" width="450" alt="Smooth image">
</p>

Then, I apply canny function to find the edges of the lane lines. 

<p align="center">
<img src="https://github.com/baocongchen/carnd-finding-lane-lines/blob/master/pipeline_images/4edges.png" width="450" alt="Create edges">
</p>

I create a polygon mask to choose my region of interest, thereby filtering out area that does not contribute to the detection of lane lines. 

<p align="center">
<img src="https://github.com/baocongchen/carnd-finding-lane-lines/blob/master/pipeline_images/5masked_edges_img.png" width="450" alt="Mask edges">
</p>

I define parameters and apply Hough Transform to identify lane  from canny edges. 

<p align="center">
<img src="https://github.com/baocongchen/carnd-finding-lane-lines/blob/master/pipeline_images/6lines.png" width="450" alt="Draw lines">
</p>

Finally, I make the lines semi-transparent by applying the addWeighted function.

<p align="center">
<img src="https://github.com/baocongchen/carnd-finding-lane-lines/blob/master/pipeline_images/7lines_edges.png" width="450" alt="Add weight">
</p>

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by doing the following steps:
- Divide lines into 2 groups: negative group whose slopes are smaller than -0.4 and positive group whose slopes are larger than 0.4
- Calculate the distance of each point in one group, choose 2 points that make the longest distance in one group.
- Using the function y = mx + b, calculate the slope (m) and b value based on the 2 points chosen
- Define y values of the start point and end point, then use m and b to get the x values the start point and the end point.


###2. Identify potential shortcomings with your current pipeline
My pipeline does a fairly good job in detecting lane lines. However, the algorithm does not take into consideration the points in the middle but the 2 points that make the longest distance for a detected lane line. As a result, the algorithm may fail to create a smooth line if there is a sharp curve ahead. 


###3. Suggest possible improvements to your pipeline
I need to consider all lines detected by using each line length as weight, then calculate the average slopes for the positive lines and the negative lines. I also need to calculate the b values (y = mx + b) for the negatives slopes and the positive slopes, but I do not know how to get the y and x value so that I can calculate the b value.
