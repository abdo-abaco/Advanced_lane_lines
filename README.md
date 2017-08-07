**Advanced Lane Finding Computer Vision Project**
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

**The series of steps are done on this project in order to acheive the goal of detecting and highlighting vehical traffic lanes:**


### Pre-step: Camera Calibration

#### Computer Vision neccesitates an accurate representation of the physical environment which requires a camera calibration to correct for lens distortion due to the lenses curvature and to account we are not assuming the 'pinhole model'. Calibration parameters K1 K2 P1 P2 K3 will be stored and used for detection, the K factors used for tangential distortion which corrects for the tilt effect whereas the P factors are used to undistort radial distortion which accounts for the warped effect.

A set of chessboard images is provided in the calibration_wide folder. We use OpenCV to compute the camera calibration matrix and distortion coefficients. First we use cv2.findChessboardCorners() to derive a set of image points to object points. We then use cv2.calibrateCamera() to find the distortion parameters.

'calibrate.py' computes and saves the distortion parameters and also displays a test example:  

![alt text][image1]

### Step:1 Undistorting Image

####  The camera matrix and distortion coefficients are now used on the first frame of the video.

'advancedlanes.py' line 64 applies the distortion correction.

![alt text][image2]

### Step:2 Obtaining Binary Image extracting lane lines
#### Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

 Apply color binary transforms and threshold to extract lane lines
* Use color transforms, gradients, etc., to create a thresholded binary image.


I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at lines # through # in `another_file.py`).  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![alt text][image3]

### Step:3 Performing perspective transform on the lane.
#### Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

Applying a perspective transform and effecting cropping outside areas to obtain a front view of the lanes.
* Apply a perspective transform to rectify binary image ("birds-eye view").

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `example.py` (output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[(img_size[0] / 2) - 55, img_size[1] / 2 + 100],
    [((img_size[0] / 6) - 10), img_size[1]],
    [(img_size[0] * 5 / 6) + 60, img_size[1]],
    [(img_size[0] / 2 + 55), img_size[1] / 2 + 100]])
dst = np.float32(
    [[(img_size[0] / 4), 0],
    [(img_size[0] / 4), img_size[1]],
    [(img_size[0] * 3 / 4), img_size[1]],
    [(img_size[0] * 3 / 4), 0]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

### Step:4 
#### Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]


5) Obtain histogram of pixels in the x-direction to detect the two lines.
* Detect lane pixels and fit to find the lane boundary.


### Step:5.
#### Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

* Determine the curvature of the lane and vehicle position with respect to center.


### Step:6.
#### Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]


* Warp the detected lane boundaries back onto the original image.

* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

The images for camera calibration are stored in the file called `calibration.p`.  The images in `test_images` are for testing your pipeline on single frames.  If you want to extract more test images from the videos, you can simply use an image writing method like `cv2.imwrite()`, i.e., you can read the video in frame by frame as usual, and for frames you want to save for later you can write to an image file.  


`ouput_images`
`project_video.mp4` 




#### Video. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

#### HOW WAS IT. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  


---

[//]: # (Image References)

[image1]: ./calibration_wide/undistorted.png "Undistorted"
[image2]: ./examples/Figure_2.png "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"


---
