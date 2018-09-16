### Computer Vision Application Project - Vehicle Tracking

by Wenbo Ma

#### Overview

Vehicle tracking is an important component function for a self-driving car system. It enables a car to perceive surrounding environment and provide input data for subsequent path-planning and motion control component. In this project, we build a software pipeline to track nearby vehicles via a front-facing camera. We leverage classical computer vision techniques such as histogram-of-gradients (HOG) transformation to extract effective features from images. We use linear support vector machines (SVM) to predict whether a sub-region of an image is a car. We test our pipeline in a recorded video and it performs well.

Here is a sub-clip of our test result

<p align='center'>
  <img src = "https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/Images/upload.gif" height="70%" width="55%">
</p>

#### Technical summary

The steps to build the pipeline can be summarized as follows:

* Perform a HOG transoformation, possibly combined with color histogram and bin spatial extraction to obtain effective features of cars and non-cars images.
* Build a SVM model to predict whether an image is car or not.
* Search a single frame with the trained SVM to identify all the cars
* Use heatmap method to reject false positive detections and to refine overlapping detections
* Put all the steps into a pipeline and run it on a video.


#### Technical details link

Techinical details consisting of the python notebook, the technical writeup and the video output can be found at

[Python code](https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/Object%20Detection.ipynb)

[Technical writeUp](https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/ProjectWriteUp.md)

[Output video](https://github.com/wenbo5565/AppliedProject_CarDetection/blob/master/project_output_video_2.mp4)
