# CarND_LaneLines
## _Finding Lane Lines :: Udacity - Self-Driving Car Engineer Nanodegree_

[![Binder](http://mybinder.org/badge.svg)](https://beta.mybinder.org/v2/gh/bitsurgeon/CarND_LaneLines/master)
[![Udacity - Self-Driving Car Engineer Nanodegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)  

### The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

#### _Binder_
_Binder serves interactive notebooks online, so you can run the code and change the code within your browser without downloading the repository or installing Jupyter. Test drive online now by click the binder badge above._

---

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

There are 6 steps in my lane finding pipeline. I will use the following camera image as an example to illustrate how the pipeline works.  
_An example of a camera image is shown in below:_  
<img src="./writeup_images_output/extrapolate/0_camera_image.jpg" alt="Camera" width="350">  

1. Converted the camera images to grayscale.  
<img src="./writeup_images_output/extrapolate/1_grayscale_image.jpg" alt="Grayscale" width="350">  

2. Apply Gaussian blur to reduce noise.  
<img src="./writeup_images_output/extrapolate/2_noise_reduced_image.jpg" alt="Gaussian" width="350">  

3. Use Canny Edge Detector to extract edge information.  
<img src="./writeup_images_output/extrapolate/3_edge_detected_image.jpg" alt="Canny" width="350">  

4. Select region of interest for lane finding.  
<img src="./writeup_images_output/extrapolate/4_region_selected_image.jpg" alt="Region" width="350">  

5. Perform Hough Transform to find lane line segments on the road.  
<img src="./writeup_images_output/segment/5_lane_line_image.jpg" alt="Hough_s" width="350">  

6. Mark the camera image with the lane line segments detected.  
<img src="./writeup_images_output/segment/6_lane_marked_image.jpg" alt="Marked_s" width="350">  

7. Perform filtering/averaging/extrapolation on line segments to generate one lane line on each side of the road.  
<img src="./writeup_images_output/extrapolate/5_lane_line_image.jpg" alt="Hough" width="350">  

8. Mark the camera image with the full extent lane lines, one on each side.  
<img src="./writeup_images_output/extrapolate/6_lane_marked_image.jpg" alt="Marked" width="350">  

In Step 7 and 8 above, in order to draw a single line on the left and right lane lines, I modified the **_draw_lines()_** function to perform the following operations:
* **filtering** - select sensible lines by checking its slope and the difference to the current averaged slope
* **weighted averaging** - only valid line slopes and intercepts are weight averaged, historical data was given more weight
* **extrapolation** - estimate the full extent of lane lines using the averaged slopes and intercepts with in the region of interest

### 2. Identify potential shortcomings with your current pipeline

* the region of interest used is a fixed one, this may not work well when the camera mounting point changed, or when the road ahead is not flat
* the slope calculation may not work well when the lane line on the image is vertical
* linear extropolation on the lane lines may not work well on the curve lanes
* simple grayscale processing may not work well when there are shades on the road or under certain lighting conditions

### 3. Suggest possible improvements to your pipeline

* explore other color space in pre-processing to extract lane information when there are shades on the road
* update line extrapolation code to make it work better with curve lane lines
