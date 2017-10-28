## Project: Search and Sample Return
---


**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook). 
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands. 
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./misc/color_thresh.png
[image2]: ./misc/results.png

## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it!

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.

I captured my own test images and then followed the steps from the lesson to populate the notebook. I experimented with color thresholding until I was able to segment navigable terrain, obstacles, and rocks samples. 

![alt text][image1]

#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 

I added a black and white mask to the output of my perspective transform function to get all visible pixels. In process\_image, I take this mask and subtract all pixels that represent obstacles. Then I converted the image, which originated from the rover's camera, into rover coordinates before finally converting them into world coordinates to overlay on the image of the map. test\_mapping1.mp4 contains the output from the process\_image pipeline. 

### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.

I added my code from the jupyter notebook into perception_step(), with minor adjustments to the input and output so that they refer to the Rover. Adding this code allowed the Rover to differentiate between obstacles, navigable pixels, and rocks. 

I left the decision tree in decision.py as is, as it passed the requirements of the project. 

#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

Resolution: 1024 x 768

Graphics Quality: Good

Frames per second: 36

My rover achieved satisfactory results on these settings, see image below. From the screenshot, you can see that I navigated 46% of the terrain at 68% fidelity. 

In order to reduce false positives in world mapping, I made sure that the roll and pitch of the rover were both within 1 degree of zero before updating the Rover's worldmap. This way, pixels in the distance wouldn't get mislabeled when the Rover image was transformed into locations on the world map because the Rover wasn't level. 

To improve the performance of the rover, I would try to improve the navigation by being smarter about getting stuck. If the robot was in forward mode with velocity close to zero for a duration of time, I would put it into stop mode to try and get it to back out. 

![alt text][image2]

I referenced the project walkthrough and the robotics nanodegree slack to complete this project.
