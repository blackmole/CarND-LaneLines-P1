#**Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[gray]: ./writeup_images/gray.jpg
[blured]: ./writeup_images/blured.jpg
[canny]: ./writeup_images/edges.jpg
[roi]: ./writeup_images/roi.jpg
[result]: ./writeup_images/result.jpg
---

### Reflection

###1. Pipeline description

####My pipeline steps:

1.  Convert the image to grayscale: I only work on the edge information, so no color is needed.<br />
![gray image][gray]

2.  Blur the image: In order to reduce the noise, the image is blured.<br />
![blured image][blured]

3.  Canny edge detection: Now I want to find the edges, so I use the canny algorithm.<br />
![canny image][canny]

4.  ROI cutout: I dicard all edges that are not in my region of interest, so only the edges of most non-lanemarker-objects are gone<br />
![roi image][roi]

5.  Hough base line inter- / extrapolation: See below

6.  Combine original image and line image<br />
![result image][result]

####Details on step 5:

To extrapolate and interpolate the right and left line, the hough lines are
seperated into two groups, one with a slope between -0.8 and -0.5, the other
between 0.5 and 0.8. These are the slopes that are common when driving in the
middle of the lane, outliers (e.g., horizontal lines) are discarded.

For each group, the x and y means are determined, then the y intersect and the
mean slope are calculated. From this information, I can calculate the endpoints
of each of the lines and draw the line.

###2. Potential shortcomings with the current pipeline

There are several shortcomings:
* The line is quite unstable, because 
    * there is no tracking
    * there are outliers that are not discarded by the current algorithm
* The ROI is quite narrow, so in for example in curves, no line can be detected
* The model (a line) does not allow a good fit for curved lane markers
* The detection probably only works for good weather and good light conditions
* Worn out lane markers will not be recognized

###3. Possible improvements

There should be more plausibility checks whether the edges belong to a lane
marker or not. Tracking would make the line more stable. The model for the
lanes should be splines.
