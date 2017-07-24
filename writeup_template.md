# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: test_images_output/solidWhiteCurve.jpg

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

First, I converted the RGB image to grayscale. Then I applied a gaussian blur with kernel size = 5 to remove small noises in between. After this, I used canny edge detector to get the edges in my region of interest using a polygon mask. The detected edges were taken to hough transform to get the major lines of interest. Small lines less than the threshold value were ignored, while broken lines with a gap less than max_line_gap were joined together. This gave me a list of lines including my lane lines. After this from the list of all lines, I picked up only lines which if projected downwards would intersect the image boundary only within a certain range on the lower side. All other lines were rejected. Now I separated the list of lines based on their slope is positive or negative. Endpoints of all lines with positive slope were collected together, while endpoints of all lines with negative slope were collected together. Now a line is fitted through all these two set of points seperately. These lines are extended downwards to the bottom boundary of the image and upto the region of interest upwards. These are our lane lines which is drawn in the image with red color.


![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the road is not of uniform texture/ road is dirty. There would be noise edges detected on the road. This is easily seen in the last optional video, where the algorithm gets comletely broken.  

Another shortcoming could be during turns when the lane lines are actually lines but curves (during turns), then the algorithm would not be able to fit lines properly. I was distinguishing the left and the right lane by the sign of the slope of the individual lines, which might not hold true there.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to look for white/ yellow color through the white/ yellow color values. Edges not belonging to white/ yellow variant of color range could be rejected as noise. Also confidence might be gained by the number of lines aligned in one direction, also weighted by their length maybe.

Another potential improvement could be to use the data of the previous frame to process the current frame. We know that each frame is related to its last frame as the change in positions of lane lines would be only within a very small range. So once a lane is detected, we could focus to find lane in the next frame somewhere near to the previously detected lane position, and reject lines which are way too far from those previous positions.
