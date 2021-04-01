---
layout: post
title: DIP Exercises
subtitle: Some exercises from my university
cover-img: /assets/covvers/cover_pdi.png
thumbnail-img: /assets/thumbs/thumb_pdi.png
share-img: /assets/thumbs/thumb_pdi.png
tags: [C++, Computer Vision, Exercises, Tutorials]
readtime: true
---

# REPO

The code is available in this repository <a href="https://github.com/RodrigoSarmento/ListasPDI">(repository)</a>

# Question 2.1

Is asked to create a program that creates a negative region in Image given two points P1 and P2 that forms a rectangle.

Here is the code:

```
#include <config_loader.h>
#include <iostream>
#include <opencv2/opencv.hpp>
#include <string>

using namespace cv;
using namespace std;

int main(int argc, char* argv[])
{

    Mat image;
    Vec3b val;
    string image_file;

    int p1_x, p2_x, p1_y, p2_y;

    if (argc != 6) {
        printf("Couldn't load all parameter, use example of use: ./2_2regioes config_file p1_x p1_y p2_x p2_y\n");
        return 0;
    }

    ConfigLoader param_loader(argv[1]);

    p1_x = atoi(argv[2]);
    p1_y = atoi(argv[3]);
    p2_x = atoi(argv[4]);
    p2_y = atoi(argv[5]);
    param_loader.checkAndGetString("q2_image", image_file);

    image = imread(image_file, IMREAD_COLOR);
    if (!image.data) cout << "Couldn't open image" << image_file << endl;

    for (int i = p1_x; i < p2_x; i++) {
        for (int j = p1_y; j < p2_y; j++) {
            val[0] = 255 - image.at<Vec3b>(i, j)[0];
            val[1] = 255 - image.at<Vec3b>(i, j)[1];
            val[2] = 255 - image.at<Vec3b>(i, j)[2];
            image.at<Vec3b>(i, j) = val;
        }
    }

    imshow("image", image);
    waitKey();
    return 0;
}
```
Here we read the parameters from the user and load the image.

ConfigLoader is just a implementation I use to keep some parameters, what matters here is that ConfigLoader will
read a string from a file that is the path of our image.


```
    if (argc != 6) {
        printf("Couldn't load all parameter, use example of use: ./2_2regioes config_file p1_x p1_y p2_x p2_y\n");
        return 0;
    }

    ConfigLoader param_loader(argv[1]);

    p1_x = atoi(argv[2]);
    p1_y = atoi(argv[3]);
    p2_x = atoi(argv[4]);
    p2_y = atoi(argv[5]);
    param_loader.checkAndGetString("q2_image", image_file);

    image = imread(image_file, IMREAD_COLOR);
    if (!image.data) cout << "Couldn't open image" << image_file << endl;

```

The main thing is here, let's iterate over the rectangle region of the image and subtract 255 from every pixel to create the Negative effect.

```
for (int i = p1_x; i < p2_x; i++) {
        for (int j = p1_y; j < p2_y; j++) {
            val[0] = 255 - image.at<Vec3b>(i, j)[0];
            val[1] = 255 - image.at<Vec3b>(i, j)[1];
            val[2] = 255 - image.at<Vec3b>(i, j)[2];
            image.at<Vec3b>(i, j) = val;
        }
    }
```

Here is the result with the given Input for P1 and P2: 50 80 100 200


<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/negative_example.png">
    <img src="/MyBlog/assets/img/pdi/negative_example.png" alt="example">
  </a>
</div>
<br />

# Question 2.2

This question asks to switch quadrants of a image this way

1 2         Will Be         4 3
3 4                         2 1