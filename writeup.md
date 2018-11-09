# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/SolidYellowCurve.jpg "SolidYellowCurve"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:

First, I converted the images to grayscale, then I applied a Gaussian blur for smoothing. The edges were extracted using Canny edge detection, forming an image with the edges outlined. A polygon mask was then applied to isolate the road with the left and right lanes. The HoughLines function was used to detect straight lines from the masked image. The extracted lines are finally overlaid over the original frame. An example of the processed image is shown below:

[!alt text][image1]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by using numpy's _polyfit_ function to fit a single line over the extracted lines. Before fitting, the lines were first filtered according to their angles - the intuition is that the orientation of the road lanes must appear at a certain range of angles only. Only lines with angles between 25 and 40 degrees were kept. This step eliminates many lines coming from other objects. Next, the lines belonging to the left and right lanes were grouped together according to their slope. Finally, the endpoints of each line were used in the _polyfit_ function to generate a linear fit for the left and right lanes. 



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the road is curvy and sloped. This scenario may lead to the road lanes going out of the static region of interest. Additionally, since the lane detection pipeline looks for straight lines only, curved lines would possibly confuse the system. 

Another shortcoming could be when there are vehicles in front, within the region of interest, might also confuse the system. This is especially possible in cases where the vehicle is composed of straight lines, such as a bus. 

Another possible shortcoming could be in low contrast situations, such as fog, or in low light conditions such as night. These condition may make the lane detection task considerably difficult. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to choose the objects according to color (i.e. yellow, white), which may reduce the likelihood of undesired objects being fed into the Canny edge detector. 

Another potential improvement could be to create an adaptive region of interest which would change according to the shape of the road. this would ensure that the left and right lanes are isolated as much as possible. 

Finally, another potential improvement would be to modify the lane detection to accomodate curved lanes. A possible approach would be to treat the lines as segments which can be connected together. However this approach relies very much on the accuracy of the edge detection algorithm and the appropriateness of the region of interest. 
