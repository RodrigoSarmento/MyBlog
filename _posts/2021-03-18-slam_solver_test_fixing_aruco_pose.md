---
layout: post
title: Update of Slam Solver Test
subtitle: A new tracker
cover-img: /assets/img/slam_solver_test/slam_solver_update_3.png
thumbnail-img: /assets/img/slam_solver_test/slam_solver_update_1.png
share-img: /assets/img/slam_solver_test/slam_solver_update_1.png
tags: [C++, Computer Vision, Aruco]
readtime: true
---

# Last Post

In the previous post <a href="https://rodrigosarmento.github.io/MyBlog/2021-03-14-slam_solver_test/">Here</a> 
where I introduced the slam solver project I make it clear that we could improve a lot, and here is the first improvement. If you are new to SLAM I recommend you to see the last post where I introduce some concepts.

# What have Changed

Two things have changed that improved the optimization and detection of KeyFrames.
The main thing was changing the algorithm tracker, now I'm using an implementation of a friend of mine made <a href="https://www.linkedin.com/in/marcos-henrique-fernandes-marcone/">(His Linkedin)</a> , using ORB-SLAM as a base, this tracker has more accurate features and thus allows me to produce more KeyFrames with less amount of motion.
The second thing was a little change in how the algorithm adds a KeyFrame, previously I had only one restriction to add a KeyFrame to the graph, the distance between the last KeyFrame, but this can leads to add a lot of KeyFrame of only one class(odometry or loop closure), so now the restriction is based in the distance between the last KeyFrame of the same "class".

### Results and discussion

Below there is a video showing the changes, and I'm going to split this into three things.

<iframe width="840" height="600"
src="https://www.youtube.com/embed/476-7_mJKEc">
</iframe>

In the below image you can see in the left that some features are "floating" in the image and there are some features in places without texture, such as the white wall, those are some older features that have not been tracked right or have not been thrown away when they should, the image in the right shows the new features that are better placed.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/slam_solver_update_1.png">
    <img src="/MyBlog/assets/img/slam_solver_test/slam_solver_update_1.png" alt="example">
  </a>
</div>
<br />

Here you can see the difference between the amount of KeyFrames the older tracker created when it was not seeing the Aruco Marker(only one), and with the new tracker, we have a lot of Odometry poses(the poses that don't have a red arrow connecting to the origin). This decreases the amount of accumulated error between poses.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/slam_solver_update_2.png">
    <img src="/MyBlog/assets/img/slam_solver_test/slam_solver_update_2.png" alt="example">
  </a>
</div>
<br />

And finally, the optimization, one of the things I talked about in the last post was the wrong optimization of poses when the algorithm was not seeing the Aruco Marker, this created a lot of error since there was a huge gap between the two poses. Now when the algorithm is not seeing the Aruco Marker the new tracker can still handle the calculation of those poses by itself, given more data to the optimizer algorithm and resulting in better optimization.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/slam_solver_update_3.png">
    <img src="/MyBlog/assets/img/slam_solver_test/slam_solver_update_3.png" alt="example">
  </a>
</div>
<br />

# Next Steps

The result for this example is good for now and I'm going to change my focus to another problem. The next problem I'm going to try to solve is the error accumulation in a known environment, my approach is to make a local optimization when the camera is seeing multiple markers, hopefully, this will calculate a new Camera pose every time this optimization is made reducing the error accumulation during the process. 