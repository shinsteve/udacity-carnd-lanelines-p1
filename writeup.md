# **Finding Lane Lines on the Road** 

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

[diagram1]: ./T1-P1.png "Pipeline"

My pipeline is illustrated in below diagram.

![alt text][diagram1]

1. Convert src image into grayscale.
1. Mask the luma value into certain range in order to stabilize the black color of road surface.
1. Apply gaussian blur to reduce the noise so that edge detection become stable.
1. Detect edge by Canny's algo.
1. Mask the trapezoid region where lane is supposed to exist.
1. Detect lines from the edges by Hough transform.
1. Extrapolate the left and right lane by averaging the lines.
1. Draw the lane image and blend it with src image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by separating line segments into left line and right line. This is done by:
- Their slope ((y2-y1)/(x2-x1)). 
    - At the same time, the parameter "slope_thr" is used to remove the line other than lane.
- The position (right half or left half) of line segments.

After that, the position of each of the lines are averaged into one line segment.
By using linear equation corresponding to that segment, we can extrapolate the top and bottom of the lane.



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the input image is not so simple to detect lanes. For instance:

- Any line like edges other lane exist on the road. (Ex, shadow of tree, crack of surface etc)
- The segment of lane is very short or broken.

This is because only the current image is used to judge lane and the previous result is not used. 

Another shortcoming could be when the curvature of the lane is large. This is because there's some assuption about the slope value of lane in current algo.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use the result of previous, which is 1 or 2 frames before, so that the result become more stable and can be extrapolated even in difficult situation. In detail, the previous result can be used to narrow down the candidate of segments or average the result as per IIR filter.
