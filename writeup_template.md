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

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 major steps and one additional step of weighing to overlay line segment with the original image. 
Step 1, I converted the images to grayscale and reduce the colorspace to one channel. This helps for canny edge generation.
Step 2, In order to reduce noice in terms of small sharp edges, Gaussian Blurring is called next. I played with the kernel size, but decided to go with 3 similar to earlier examples. Increasing this value caused the image to be blurred too much.
Step 3, Use the blurred image and run through canny edge generation method. Here the low and high threshold values play important role, selecting appropriate values makes a big difference in the edges generated. I followed values from previous quiz and it worked out ok.
Step 4, As we are interested in finding lane edges on road, here I defined a polygon somewhat matching with the road shape. The size of the polygon is defined based on image proportion. This polygon shape will work fine on straight road, but will have trouble limiting the region of interest on a curved road.
Step 5, Finally I called the hough transform line creation method which stiches the line segment.

I implemented a single line generation for identified line segments. The draw_lines method is enhanced to generate single line for the line segments. As suggested in the guidelines, first slope and center point for all segments is calculated. Then they are averaged to get single value for the slope and center. Finally the line is extended to middle and bottom to generate single line.

![alt text][image1]
[image2] ./test_images/processed_solidWhiteCurve.jpg "Solid right lane"

### 2. Identify potential shortcomings with your current pipeline

As mentioned earlier, the region of interest is defined by a hard-coded sized polygon. This approach will work ok for straight road with road portion is about 60% of the bottom half of the image. For curvy road this polygon will endup trimming the top segment of the road on one or other side. 
Also, the current implementation doesn't look for slopes on the lane segments. For challenge video where few shadows appears on the road and they get identified as lane markings as they fall into the region. To prevent that some limiting slope values could be used.


### 3. Suggest possible improvements to your pipeline

The draw_lines method can be further improved by adding invalid slope range.

Apart from that if any conditions to filter out low slope values are added to draw_lines method then the horizontal lanes can be eliminated. I hope to implement these two improvements. 

Also, additional testing with the road image from night time will identify improvement to the current logic.  