---
layout: post
title: DIP Exercises
subtitle: Some exercises from my university
cover-img: /assets/covers/cover_pdi.png
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

and the result is:

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/question2_result.png">
    <img src="/MyBlog/assets/img/pdi/question2_result.png" alt="example">
  </a>
</div>
<br />

# Question 3.1

This question asks to find and count the number of "bubbles" with and without hole, the image below is used as example.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/bolhas.png">
    <img src="/MyBlog/assets/img/pdi/bolhas.png" alt="example">
  </a>
</div>
<br />

We can see that there is a black background and some white bubbles, in this example tha background color is 0 and the bubble color is 255. Finally, all the bubbles that touches the edge of the image should be removed. So here is the final code.

To count the bubbles we search in the image for a white pixel, if we find we use the FloodFill algorithm to paint the object as black.
Here you can see more details about<a href="https://en.wikipedia.org/wiki/Flood_fill">FloodFill.</a>

```
#include <config_loader.h>
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace cv;
using namespace std;

void cleanEdges(Mat& image, int width, int height);

int main(int argc, char* argv[])
{
    Mat image, realce;
    int width, height;
    int nobjects, nobjects_with_holes;
    string image_file;
    Point p;

    if (argc != 2) {
        printf("Couldn't load all parameter, use example of use: ./3_floodfill config_file \n");
        return 0;
    }

    ConfigLoader param_loader(argv[1]);
    param_loader.checkAndGetString("q3_image", image_file);

    image = imread(image_file, IMREAD_GRAYSCALE);

    if (!image.data) {
        cout << "imagem nao carregou corretamente\n";
        return (-1);
    }

    width = image.cols;
    height = image.rows;
    cout << width << "x" << height << endl;

    p.x = 0;
    p.y = 0;

    // Removing bubles in the edges of image
    cleanEdges(image, width, height);
    imshow("image", image);
    waitKey();

    // Detecting all bubles in image and using floodfill to paint as gray
    nobjects = 0;
    for (int i = 0; i < height; i++) {
        for (int j = 0; j < width; j++) {
            if (image.at<uchar>(i, j) == 255) {
                // Found a bubble
                nobjects++;
                p.x = j;
                p.y = i;
                // Using floodFill
                floodFill(image, p, 150);
                imshow("image", image);
                waitKey();
            }
        }
    }

    // Inverting the color, now the black background will be white
    floodFill(image, Point(0, 0), 255);
    imshow("image", image);
    waitKey();

    // What is left as black are the bubles with holes, so lets find those
    for (int i = 0; i < height; i++) {
        for (int j = 0; j < width; j++) {
            if (image.at<uchar>(i, j) == 0) {
                imshow("image", image);
                waitKey();
                // Found a bubble
                nobjects_with_holes++;
                p.x = j;
                p.y = i;
                // Using floodFill
                floodFill(image, p, 255);
            }
        }
    }

    // Now I have the count of all bubles and the count of bubles with holes
    cout << "The image has " << nobjects_with_holes << " bubbles with holes\n";
    cout << "The image has " << nobjects - nobjects_with_holes << " bubbles without holes\n";

    return 0;
}

void cleanEdges(Mat& image, int width, int height)
{
    int i = 0;
    Point p;

    p.x = 0;
    p.y = 0;

    for (int j = 0; j < width; j++) {
        if (image.at<uchar>(i, j) == 255) {
            p.x = j;
            p.y = i;
            floodFill(image, p, 0);
        }
        if (j == width - 1) {
            if (i == height - 1) break;
            j = 0;
            i = height - 1;
        }
    }

    i = 0;
    for (int j = 0; j < height; j++) {
        if (image.at<uchar>(j, i) == 255) {
            p.x = i;
            p.y = j;
            floodFill(image, p, 0);
        }
        if (j == height - 1) {
            if (i == height - 1) break;
            j = 0;
            i = height - 1;
        }
    }
}
```
    The first thing I do is remove all bubbles that touches the edge. This sure is not the best implementation to do this but was the first I had in mind. So here I'm searching for white pixels in the borders and using floodfill to paint it with Black.

```
void cleanEdges(Mat& image, int width, int height)
{
    int i = 0;
    Point p;

    p.x = 0;
    p.y = 0;

    for (int j = 0; j < width; j++) {
        if (image.at<uchar>(i, j) == 255) {
            p.x = j;
            p.y = i;
            floodFill(image, p, 0);
        }
        if (j == width - 1) {
            if (i == height - 1) break;
            j = 0;
            i = height - 1;
        }
    }

    i = 0;
    for (int j = 0; j < height; j++) {
        if (image.at<uchar>(j, i) == 255) {
            p.x = i;
            p.y = j;
            floodFill(image, p, 0);
        }
        if (j == height - 1) {
            if (i == height - 1) break;
            j = 0;
            i = height - 1;
        }
    }
}
```

Then I detect the bubbles and I paint as gray(just for visualization), after this step I have all the bubbles with or without a hole in the image.

```
    // Detecting all bubles in image and using floodfill to paint as gray
    nobjects = 0;
    for (int i = 0; i < height; i++) {
        for (int j = 0; j < width; j++) {
            if (image.at<uchar>(i, j) == 255) {
                // Found a bubble
                nobjects++;
                p.x = j;
                p.y = i;
                // Using floodFill
                floodFill(image, p, 150);
                imshow("image", image);
                waitKey();
            }
        }
    }
```


Here is the output from the first step. After that I'm going to inverting the colors, the result of inverting the colors will make 
the image all white but all holes in the image will be paint as black, so now we can search for black pixels to count the holes.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/flood_fill.png">
    <img src="/MyBlog/assets/img/pdi/flood_fill.png" alt="example">
  </a>
</div>
<br />


```
  // Inverting the color, now the black background will be white
    floodFill(image, Point(0, 0), 255);
    imshow("image", image);
    waitKey();

    // What is left as black are the bubles with holes, so lets find those
    for (int i = 0; i < height; i++) {
        for (int j = 0; j < width; j++) {
            if (image.at<uchar>(i, j) == 0) {
                imshow("image", image);
                waitKey();
                // Found a bubble
                nobjects_with_holes++;
                p.x = j;
                p.y = i;
                // Using floodFill
                floodFill(image, p, 255);
            }
        }
    }
```
    
    Finally, I have the number of bubbles and the number of bubbles with hole, So I just need to subtract the number of total with the number of bubbles with hole to find the number of bubbles without hole.

```
   cout << "The image has " << nobjects_with_holes << " bubbles with holes\n";
   cout << "The image has " << nobjects - nobjects_with_holes << " bubbles without holes\n";
```

# Question 3.1
