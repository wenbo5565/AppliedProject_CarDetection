## Project - Vehicle Detection
---

**Vehicle Detection Project**

The goals of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction, possibly combined with color histogram and bin spatial features  on a labeled training set of images and train a classifier to detect if an image is a car.
* Implement a sliding-window technique and use trained classifier to search for vehicles in images.
* Develop an pipeline to detect vehicles on a video stream and create a heat map of recurring detections frame by frame to reject outliers.
* Estimate a bounding box for vehicles detected.

---
### Writeup

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the code cell [5,6] of the [IPython notebook](https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/Object%20Detection.ipynb)

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

<img src="https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/Images/example.png" height="60%" width="60%">

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `RGB` color space and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:

<img src="https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/Images/hog.png" height="60%" width="60%">



#### 2. Explain how you settled on your final choice of HOG parameters.

Whether HOG features are good or not depends on its performance in the classification stage. So I put different combinations of HOG parameters in the classifiers and find the following parameters achieve 99%-100% accuracy on training set. This indicates the selected HOG features lead to a low-bias model.

| Parameters        | Value   | 
|:-------------:|:-------------:| 
| orientations      | 9        | 
| pixels_per_cell   | (8,8)      |
| cells_per_block   | (2,2)      |




#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features

I trained a linear-kernel SVM using HOG features and color histogram features (RGB channel with 32bins). The original data is split into training and testing set. The training set takes 70% of the data and the testing set takes the remaining 30%. As both training accuracy (~99%) and testing accuracy (>98%) are alreday satisfactory, I didn't spend much time on tuning hyper-parameters of the linear SVM. All hyper-parameters are set at default values. 

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

Since roads only appera at bottom half in the current camera, my entire search area is the bottom half frame (y:[400-700])

Since the further vehicles are, the smaller they appear, I implement two scales window search:
  - The first scale is 64*64 for area covered by (y:[400-550])
  - The second scale is 96*96 for area covered by (y:[450-700])
  
<img src="https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/Images/windowsearch.png" height="80%" width="80%">

The code for this step is contained in the code cell [30] of the [IPython notebook](https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/Object%20Detection.ipynb)

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

I searched on two scales using RGB 3-channel HOG features plus histograms of color in the feature vector, which provided a nice result.  Here are some example images:

<img src="https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/Images/testimages.png" height="80%" width="80%">

---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

**Output Video on Youtube - Click to View**

[![Video on Youtube](http://img.youtube.com/vi/YfJCT0hr6yk/0.jpg)](https://youtu.be/YfJCT0hr6yk)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I use heatmap method to filter false positive detections and to combine overlapping bounding boxes. The steps are
  - Create an array of zeros. The shape is same as one-channel frame
  - Iterate over each positve search window. Add one to each pixel within the search window
  - Iterate over each pixel in the array. Apply thresholding by setting pixel equal to 0 for those having values less than the pre-specified threshold.
  - Apply `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap. We assum each blob corresponded to a vehicle. We construct bounding boxes to cover the area of each blob detected.


Here's an example result showing the heatmap from a frame <!--a series of frames-->, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the frame

<img src="https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/Images/heatmap.png" height="80%" width="80%">

<!--### Here are six frames and their corresponding heatmaps:-->



<!--### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:-->

<!--### Here the resulting bounding boxes are drawn onto the last frame in the series:-->
 



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I found bounding boxes are not smooth from frame to frame. For detecting any bounding box for current frame, I haven't leverage any information from previous frames. If I can think of some way to use the information, bounding box would become more smooth from frame to frame.

