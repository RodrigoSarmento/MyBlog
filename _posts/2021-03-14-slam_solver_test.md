---
layout: post
title: First Version of SLAM Solver Test
subtitle: A description of one of the project I'm working on
cover-img: /assets/img/slam_solver_end.jpg
thumbnail-img: /assets/img/slam_solver_end.jpg
share-img: /assets/thumbs/love_to_code.png
tags: [C++, Computer Vision, Aruco]
readtime: true
---

# What is SLAM

First of all, what is SLAM? SLAM is an acronym to Simultaneous Localization and Mapping. It is the computational problem of creating a mapping of an environment at the same time of location itself in the map. There are a lot of ways to solve the SLAM problem, in this project we use a RGB-D camera (Kinect) and Aruco markers.

# How does it work?

There are a lot of steps here and they are not that easy to explain but I'm going to give my best.

## Detect Features in the image: 

  First of all, we have to detect some features in the image and track it's position, we call features, pixels in the image that contrast with the rest of the image, there is an example below showing those features(marked as blue dots), so we have a lot of features in the objects above the black table, because they contrast with the black environment around them.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/features.png">
    <img src="/MyBlog/assets/img/slam_solver_test/features.png" alt="example">
  </a>
</div>
<br />

## Tracking features: 

  Every pixel in the image has a 3d depth position, a 2d pixel position and a color, with this we can keep track of a feature and calculate the amount of motion of our camera by the amount of motion of the features in scene. Let's say that in the ```sequence of image 1``` we found a feature and in the ```sequence of image 2``` we found this same feature again, but know the position of the feature has changed, with this we know that our camera has moved in the scene(we are assuming that the features are not moving) and we can calculate the camera motion between two images. 
  Of course that there a lot more involved, we have to keep tracking a lot of features and matching with previously features found, there is calibration of camera involve, optimization and a lote more, this is only a scratch of the whole. 

  The image below show the motion of the KeyFrames, the really small blue dot is the previously position, the big blue dot is the actual position and the blue line is the motion.+

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/motion_estimation.png">
    <img src="/MyBlog/assets/img/slam_solver_test/motion_estimation.png" alt="example">
  </a>
</div>
<br />

## Aruco Marker:
  The principal is the same with the features in image, expect that the Aruco markers is an already known feature. We need to say to the algorithm to look forward Aruco markers in the image, and we say as well the size of this markers, this make much easier to make the distances calculations since we have a identifier in the image that we are sure it's size.   <a href="https://docs.opencv.org/master/d5/dae/tutorial_aruco_detection.html">Here is everything you need to know if you want to start using Aruco(highly recomended)</a> 

## Keyframes:

  So, our main goal here is to create a graph of camera poses that we can optimize, but it is not smart to just add all camera poses, because there will be a lot of redundance, we just want to add poses that gives a lot of good and new information, those poses we call KeyFrame.
  We have two different KeyFrames, the first one is when we are seeing a lot of new features in the scene, this says to us that we are moving to a new place, the second one is when We are sure that we are seeing something that we already know what is, we have two situations when this can happen, the first is when the camera goes back to some place it already was before, the second one is when we find a Aruco maker, since the aruco marker can give to us with a high precision the pose of the camera. 
  With this Graph of KeyFrames poses we can make a live optimization of the camera pose.
  

# 2º Example running

Now to the action, in the video below you can see how it works. I'm going to explain some steps that are happen.
<iframe width="420" height="315"
src="https://www.youtube.com/watch?v=esQundkMeLU&ab_channel=SarmentoScience">
</iframe>

In the left, we have the KeyFrames added with the 3d Axis, the blue arrows are the edges that goes from Keyframe N-1 to N, the red arrows are the edges that goes from the actual Keyframe to the first one(This one we call a loop closure edge).

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/slam_solver_start.png">
    <img src="/MyBlog/assets/img/slam_solver_test/slam_solver_start.png" alt="example">
  </a>
</div>
<br />

Here we have sometime that the camera has not seeing the Aruco marker, so the new KeyFrame we added is a KeyFrame given by the motion estimation and does not have a RedArrow.


<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/slam_solver_not_seeing_aruco.png">
    <img src="/MyBlog/assets/img/slam_solver_test/slam_solver_not_seeing_aruco.png" alt="example">
  </a>
</div>
<br />


Here we have a live optimization, the pink arrows are the blue arrows optimized, this is, the new pose of those KeyFrames. To be fair, there are some problems with the KeyFrames optimization after the time the algorithm was not seeing the Aruco Marker. We still need further examination to know what actually is making those wrong optimized poses. 

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/slam_solver_local_optimization.png">
    <img src="/MyBlog/assets/img/slam_solver_test/slam_solver_local_optimization.png" alt="example">
  </a>
</div>
<br />


And this is the final result.


<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/slam_solver_end.png">
    <img src="/MyBlog/assets/img/slam_solver_test/slam_solver_end.png" alt="example">
  </a>
</div>
<br />

# Next Steps

There are still a lot to do here, we are not updating the 3d point cloud after optimization, the motion estimation algorithm can do better, and the local optimization does not perform well if the Aruco Marker is gone for some time. 

The new feature I'm doing right know is to add a subGraph optimization anytime the camera sees multiple markers if I have a previously Aruco Marker map. Let's say that the camera is seeing 5 Aruco markers(and I have already previously mapped then, so I know their position). I'm going to add to a subGraph, th two pose for each marker, the first one is the pose calculated by the camera(live) and the second one is the pose given by the map I have calculated before, and of course, I'm going to add the camera pose that will be optimized, so the graph will have N_markers*2 + 1 poses. 