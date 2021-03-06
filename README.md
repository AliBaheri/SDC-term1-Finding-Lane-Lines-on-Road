##### Self-Driving-Car-ND Term-1
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

Project: Finding lane lines on the road
* [Videos compilation](https://www.youtube.com/watch?v=PjG6rRI8ce0)
* [Report](https://github.com/ttungl/SDC-term1-Finding-Lane-Lines-on-Road/blob/master/Finding_Lane_Lines/writeup_report.md)
* [Code](https://github.com/ttungl/SDC-term1-Finding-Lane-Lines-on-Road/blob/master/Finding_Lane_Lines/Finding_lane_lines.ipynb)
* [Images output](https://github.com/ttungl/SDC-term1-Finding-Lane-Lines-on-Road/tree/master/Finding_Lane_Lines/test_images_out)
* [Videos output](https://github.com/ttungl/SDC-term1-Finding-Lane-Lines-on-Road/tree/master/Finding_Lane_Lines/test_videos_output)
---
# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

* The results are as below.

<img src="https://github.com/ttungl/SDC-term1-Finding-Lane-Lines-on-Road/blob/master/Finding_Lane_Lines/gifs/lane1.gif" height="149" width="270"> <img src="https://github.com/ttungl/SDC-term1-Finding-Lane-Lines-on-Road/blob/master/Finding_Lane_Lines/gifs/lane2.gif" height="149" width="270"> <img src="https://github.com/ttungl/SDC-term1-Finding-Lane-Lines-on-Road/blob/master/Finding_Lane_Lines/gifs/lane3.gif" height="149" width="270">

---
* Dependencies: OpenCV and Python3
* Techniques used for the pipeline implementation: Grayscale, Gaussian Smoothing, Canny edges detection, mask region of interest, and Hough transform.
---

### Reflection

### 1. Describe your pipeline.
In this part, we take the image below as the input, and the pipeline method is described in the following steps:

<img width="960" src="https://github.com/ttungl/Self-Driving-Car-ND-term-1/blob/master/Finding_Lane_Lines/test_images/solidYellowCurve.jpg">
Figure 1: Original image.

We convert the original image to the grayscale image as in Figure 2 below:
<img width="960" src="https://github.com/ttungl/Self-Driving-Car-ND-term-1/blob/master/Finding_Lane_Lines/test_images_out/blur_gray_lines.jpg">
Figure 2: Gaussian Smoothing.

We apply the Canny edges detection algorithm to extract the edges of the Gaussian Smoothing image, we still keep the low threshold and high threshold at 50 and 150 as recommended, respectively. Note, I have tried to change these values, but the videos have been crashed when running for some reasons. 
* Update: we have improved the code and the low threshold and high threshold are changed to 28 and 90, respectively.

<img width="960" src="https://github.com/ttungl/Self-Driving-Car-ND-term-1/blob/master/Finding_Lane_Lines/test_images_out/canny_edges_lines.jpg">
Figure 3: Canny edges detection.

We then mask the trapezoid shape (region of interest) to the image above to use for identifying the lane lines. I have found that the best combination for the hough transform parameters is as follows: rho = 1; theta = (2* np.pi / 180); threshold = 20; min_line_length = 5; and max_line_gap = 20; 
* Update: We have improved the code with the new updated parameters as follows: rho=2; threshold=15; min_line_length = 10; others are unchanged.
<img width="960" src="https://github.com/ttungl/Self-Driving-Car-ND-term-1/blob/master/Finding_Lane_Lines/test_images_out/masked_edges_region_interest.jpg">
Figure 4: Masked edges region of interest. 

For the draw lines function, we have added the calculation of the slope and the upper bound of the lines for both sides. Then we draw the lines based on the resulted points on left line and right line. 
<img width="960" src="https://github.com/ttungl/Self-Driving-Car-ND-term-1/blob/master/Finding_Lane_Lines/test_images_out/hough_lines.jpg">
Figure 5: Hough transform on the edges detection of the image.

Finally, we merge the output of the hough transform image with the original image, the result is as below.
<img width="960" src="https://github.com/ttungl/Self-Driving-Car-ND-term-1/blob/master/Finding_Lane_Lines/test_images_out/solidYellowCurve_modified.jpg">

The outputs of the pipeline method are good enough to keep the lines on the correct sides.

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be that the noise is too large for light-colored lane lines as in the "challenge.mp4". The current pipeline is not robust enough to detect the left lane line correctly. We ran the challenge.mp4 video, the right lane line is good but the left one is not. Then, we ran the raw detected edges without using drawing lines function, it showed that a lot of lines are poped-up when the car passes the white road. Note that the current pipeline method works fine with the dark road. I am still trying to figure out to fix this issue. 
* Update: I have fixed this issue.

### 3. Suggest possible improvements to your pipeline

A possible improvement for the current pipeline would be to modify the vertices. Instead of the trapezoid shape, we could split the shape into two seperated shapes for left lane line and right lane line. 
* Update: I have updated this issue.

