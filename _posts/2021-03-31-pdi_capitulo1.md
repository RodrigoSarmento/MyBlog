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

The image used as input in both programs is 

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/biel.png">
    <img src="/MyBlog/assets/img/pdi/biel.png" alt="example">
  </a>
</div>
<br />

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

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/quad.png">
    <img src="/MyBlog/assets/img/pdi/quad.png" alt="example">
  </a>
</div>
<br />


The code is show below:

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

    if (argc != 2) {
        printf("Couldn't load all parameter, use example of use: ./2_2regioes config_file \n");
        return 0;
    }

    ConfigLoader param_loader(argv[1]);

    param_loader.checkAndGetString("q2_image", image_file);

    image = imread(image_file, IMREAD_COLOR);
    if (!image.data) cout << "Couldn't open image" << image_file << endl;

    Mat image_quad_1(image, Range(0, image.rows / 2), Range(0, image.cols / 2));
    Mat image_quad_2(image, Range(0, image.rows / 2), Range(image.cols / 2, image.cols));
    Mat image_quad_3(image, Range(image.rows / 2, image.rows), Range(0, image.cols / 2));
    Mat image_quad_4(image, Range(image.rows / 2, image.rows), Range(image.cols / 2, image.cols));

    Mat3b final_image(image.rows, image.cols, Vec3b(0, 0, 0));

    image_quad_4.copyTo(final_image(Range(0, image.rows / 2), Range(0, image.cols / 2)));
    image_quad_3.copyTo(final_image(Range(0, image.rows / 2), Range(image.cols / 2, image.cols)));
    image_quad_2.copyTo(final_image(Range(image.rows / 2, image.rows), Range(0, image.cols / 2)));
    image_quad_1.copyTo(final_image(Range(image.rows / 2, image.rows), Range(image.cols / 2, image.cols)));

    imshow("image", final_image);
    cv::waitKey();

    return 0;
}
```
The main Idea here is pretty simple, I just divided the image in 4 quadrants and saved those images(First four lines). Then I created a new image to be the final output, that has the size of the input image, and I inserted the quadrants in the order desired.

```
    Mat image_quad_1(image, Range(0, image.rows / 2), Range(0, image.cols / 2));
    Mat image_quad_2(image, Range(0, image.rows / 2), Range(image.cols / 2, image.cols));
    Mat image_quad_3(image, Range(image.rows / 2, image.rows), Range(0, image.cols / 2));
    Mat image_quad_4(image, Range(image.rows / 2, image.rows), Range(image.cols / 2, image.cols));

    Mat3b final_image(image.rows, image.cols, Vec3b(0, 0, 0));

    image_quad_4.copyTo(final_image(Range(0, image.rows / 2), Range(0, image.cols / 2)));
    image_quad_3.copyTo(final_image(Range(0, image.rows / 2), Range(image.cols / 2, image.cols)));
    image_quad_2.copyTo(final_image(Range(image.rows / 2, image.rows), Range(0, image.cols / 2)));
    image_quad_1.copyTo(final_image(Range(image.rows / 2, image.rows), Range(image.cols / 2, image.cols)));
```
