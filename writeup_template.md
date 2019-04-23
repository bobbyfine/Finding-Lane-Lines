# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on my work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Detect lanes pipeline

With the help of helper function, the pipeline consist with 6 steps.

First, I converted the images to grayscale
Second, I apply gaussian blur with kernel size 5 to remove noises from images
Third, I apply canny edge detection to extract edges from processed images
Forth, I focused images with the area I want (assume the camera is in front of the car) by applying region_of_interest function
Fifth, I extract line segment by using hough_lines
Final, I draw lines on original images

Example:
Original:
[image1]: /home/workspace/CarND-LaneLines-P1/test_images/solidWhiteCurve.jpg
Final:
[image2]: /home/workspace/CarND-LaneLines-P1/test_images_output/result_solidWhiteCurve.jpg

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by seperate right and left lines, average the slopes, positions and calculate the final top and bottom x positions.



### 2. Identify potential shortcomings with your current pipeline

The first big shortcoming would be the pipeline can only draw straight lines. When the self-driving car moves along the straight line, our detection method works very well. However, when the car makes turns or move up or down, the lane line becomes curvy or even disappears, our approach cannot find the straight line in those scenario and may have problem to accurately detect lanes.
Second, as we discussed above, I set fixed parameters and assuming the camera is in front of the car. This assumption does not hold in reality. In reality we will incorporate many cameras to have better views.


### 3. Suggest possible improvements to your pipeline

*Straight line issue: the simple improvement is to draw non-straight curves that best match the changing lane line. Straight line uses Y=a*X+b to model the lane, while curves can use Y=a*X² + b*X+c or even higher order polynomials to build the model. The coefficients “a”, “b”, and “c” should be computed in real time according to captured images. It is noticed that high order polynomial include more terms and needs more computation resource, so the self-driving car system cannot use very high order models. We need to find the balance between the accuracy level and computation requirement.
*Fixed parameter issue: to improve the flexibility, we can train a neural network model with previous lane detection results (similar to supervised training using marked training set). In this way, the model can extract the lane line information from the marked region inside images or frames of videos. Later on, when we test this model in self-driving road test, the model can automatically move the region of interest towards the position that lane line is more likely to be detected. The difficulty here is how to guarantee the robustness and reliability of our neural network model. I believe it needs huge amount data to train it and test it off line.