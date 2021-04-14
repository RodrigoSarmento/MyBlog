---
layout: post
title: Fixing wrong pose optimization
subtitle: A new tracker
cover-img: /assets/img/slam_solver_test/slam_solver_aruco_optimization_final.png
thumbnail-img: /assets/img/slam_solver_test/slam_solver_aruco_optimization_final_wrong.png
share-img: /assets/img/slam_solver_test/slam_solver_aruco_optimization_final_wrong.png
tags: [C++, Computer Vision, Aruco]
readtime: true
---

# Update

Following the previously posts <a href="https://rodrigosarmento.github.io/MyBlog/2021-03-14-slam_solver_test/">First</a> 
<a href="https://rodrigosarmento.github.io/MyBlog/2021-03-14-slam_solver_test_update/">Second</a>

I have a new update about this project, doing some tests I noticed that the pose optimization was not exactly good, so I'm going to show what was wrong and what I had to do to fix it.

# Finding the problem

While trying to add some new features I noticed that the camera pose optimization was misaligning comparing with the camera pose history, the left image shows the camera path before the optimization and the right image shows the camera path after the optimization, of course, that the optimization shouldn't have as result the same path, this would make no sense at all, but it's clear as well that the camera path optimization is wrong, so I had to go to a further investigation about what was causing those wrong pose estimations. 

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/slam_solver_aruco_wrong_optimization_example.png">
    <img src="/MyBlog/assets/img/slam_solver_test/slam_solver_aruco_wrong_optimization_example.png" alt="example">
  </a>
</div>
<br />

I had a lot of guesses about what was creating those behaviors if it was just a problem with the visualizer but the actual graph was right, or if it was in the optimization function, the program params, the tracker, and so on, then I decided to see the pose Aruco was given, and I could see a wrong behavior from Aruco that I already have seen in others projects but I thought was fixed in the lasts releases, the problem is a wrong orientation estimation sometimes.

The images below show two poses at the center, the one name "first" is the first Aruco pose found, and the "newone" is the Aruco pose given in the actual frame. The position does not matter here since I just want to compare the orientation of the poses, the left image shows how the new Aruco pose was supposed to be found, with the same orientation as the first one(Z-axis), and the image in the right shows the pose being found with another orientation(Z-axis rotated 180º), and this was adding wrong poses estimations to my graph of poses, and thus creating wrong pose optimizations.


<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/slam_solver_aruco_wrong_right_orientation.png">
    <img src="/MyBlog/assets/img/slam_solver_test/slam_solver_aruco_wrong_right_orientation.png" alt="example">
  </a>
</div>
<br />

I also uploaded a video showing this wrong process, you can see that this just happens in some frames, but it is enough to create a wrong optimization.

<iframe width="840" height="600"
src="https://www.youtube.com/embed/WFmsaDNnImc">
</iframe>

### How to fix it

The idea of the fix is simple, and you probably already thought of the same solution I did, I'm just creating a validation when a pose is found, and comparing the difference of the angle between the new pose and the first one found(origin) if the angle is below a minimum value(I decided 10º) then I validate this pose if it's not I just ignore. Here is the code.


```
bool isOrientationCorrect(Eigen::Affine3f& first, Eigen::Affine3f newone)
{
    double angle = acos((first(0, 2) * newone(0, 2)) + (first(1, 2) * newone(1, 2)) + (first(2, 2) * newone(2, 2))) *
                   180.0 / 3.1315;

    return angle > 10 ? false : true;
}
```

And here is the result after using this validation.

<iframe width="840" height="600"
src="https://www.youtube.com/embed/25on6jDa1yI">
</iframe>

Below there is a comparison between the optimizations, in the left without validation and in the right with the validation, there is a region I selected where there is the biggest difference, this region is also the same region(not a coincidence) where there is more wrong Aruco orientation, you can see this in the previous video from 15 seconds until 21 seconds.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/slam_solver_test/slam_solver_aruco_optimization_final.png">
    <img src="/MyBlog/assets/img/slam_solver_test/slam_solver_aruco_optimization_final.png" alt="example">
  </a>
</div>
<br />

This is what I had to show for today, thanks for reading.

