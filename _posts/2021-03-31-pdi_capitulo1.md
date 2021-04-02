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

# Question 4.1

In this question have to implement a histogram equalization in a video. Below there is two videos, the first one is the input video in gray scale and the second one is the output after histogram equalization. There isn't much to talk about this code, first there is a calculation of the histogram image and it is shown in image, and later the equalizeHist function of opencv equalizes the histogram of a grayscale image.

<iframe width="840" height="600"
src="https://www.youtube.com/embed/476-7_mJKEc">
</iframe>

<iframe width="840" height="600"
src="https://www.youtube.com/embed/L3xrARS64jE">
</iframe>

Here is the code:

```
#include <config_loader.h>
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace std;
using namespace cv;

int main(int argc, char* argv[])
{
    Mat image, hist;
    int width, height;
    vector<Mat> planes;
    float range[] = {0, 255};
    const float* histrange = {range};
    bool uniform = true, accumulate = false;
    string video_name;
    int channels[] = {0}, key, nbins = 64;

    if (argc != 2) {
        printf("Couldn't load all parameter, use example of use: ./4_histogram config_file\n");
        return 0;
    }

    ConfigLoader param_loader(argv[1]);
    param_loader.checkAndGetString("q4_video", video_name);

    VideoCapture cap(video_name);

    // Check if camera opened successfully
    if (!cap.isOpened()) {
        cout << "Error opening video stream or file" << endl;
        return -1;
    }
    cap.set(CAP_PROP_FRAME_WIDTH, 1280);
    cap.set(CAP_PROP_FRAME_HEIGHT, 720);
    width = cap.get(CAP_PROP_FRAME_WIDTH);
    height = cap.get(CAP_PROP_FRAME_HEIGHT);

    cout << "width = " << width << endl;
    cout << "height  = " << height << endl;

    int histw = nbins, histh = nbins / 2;
    Mat histImg(histh, histw, CV_8UC3, Scalar(0, 0, 0));

    while (1) {

        cap >> image;
        // If the frame is empty, break immediately
        if (image.empty()) break;

        // Converting image to gray
        cvtColor(image, image, COLOR_BGR2GRAY);

        calcHist(&image, 1, channels, Mat(), hist, 1, &nbins, &histrange, uniform, accumulate);
        normalize(hist, hist, 0, histImg.rows, NORM_MINMAX, -1, Mat());

        histImg.setTo(Scalar(0));

        for (int i = 0; i < nbins; i++) {
            line(histImg, Point(i, histh), Point(i, histh - cvRound(hist.at<float>(i))), Scalar(0, 0, 255), 1, 8, 0);
        }
        Mat equalized;
        equalizeHist(image, equalized);

        imshow("image_equalized", equalized);
        imshow("image", image);
        imshow("image_hist", histImg);

        // Press  ESC on keyboard to exit
        key = waitKey(30);
        if (key == 27) break;
    }

    // When everything done, release the video capture object
    cap.release();

    // Closes all the frames
    destroyAllWindows();

    return 0;
}
```

# Question 4.2

This question asks for a motionDetector based in the histogram of the image. Below is the full code.

```
#include <config_loader.h>
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace std;
using namespace cv;

void motionDetector(Mat hist, Mat& previouslyHist, int& motion_detection_count);

int main(int argc, char* argv[])
{
    Mat image, hist, previouslyHist;
    int width, height;
    float range[] = {0, 255};
    const float* histrange = {range};
    bool uniform = true, accumulate = false;
    string video_name;
    int channels[] = {0}, key, nbins = 64, i = 0;
    int motion_detection_count = 0;

    if (argc != 2) {
        printf("Couldn't load all parameter, use example of use: ./4_histogram config_file\n");
        return 0;
    }

    ConfigLoader param_loader(argv[1]);
    param_loader.checkAndGetString("q4_video", video_name);

    VideoCapture cap(video_name);

    // Check if camera opened successfully
    if (!cap.isOpened()) {
        cout << "Error opening video stream or file" << endl;
        return -1;
    }
    cap.set(CAP_PROP_FRAME_WIDTH, 1280);
    cap.set(CAP_PROP_FRAME_HEIGHT, 720);
    width = cap.get(CAP_PROP_FRAME_WIDTH);
    height = cap.get(CAP_PROP_FRAME_HEIGHT);

    cout << "width = " << width << endl;
    cout << "height  = " << height << endl;

    int histw = nbins, histh = nbins / 2;
    Mat histImg(histh, histw, CV_8UC3, Scalar(0, 0, 0));

    while (cap.isOpened()) {

        cap >> image;
        // If the frame is empty, break immediately
        if (image.empty()) break;

        // Converting image to gray
        cvtColor(image, image, COLOR_BGR2GRAY);

        // Calculate Hist
        calcHist(&image, 1, channels, Mat(), hist, 1, &nbins, &histrange, uniform, accumulate);
        normalize(hist, hist, 0, histImg.rows, NORM_MINMAX, -1, Mat());

        if (i == 0)
            previouslyHist = hist.clone();
        else
            motionDetector(hist, previouslyHist, motion_detection_count);

        histImg.setTo(Scalar(0));

        // Show image
        for (int i = 0; i < nbins; i++) {
            line(histImg, Point(i, histh), Point(i, histh - cvRound(hist.at<float>(i))), Scalar(0, 0, 255), 1, 8, 0);
        }
        Mat equalized;

        imshow("image", image);
        imshow("image_hist", histImg);

        // Press  ESC on keyboard to exit
        key = waitKey(30);
        if (key == 27) break;
        i++;
    }

    // When everything done, release the video capture object
    cap.release();

    // Closes all the frames
    destroyAllWindows();

    return 0;
}

void motionDetector(Mat hist, Mat& previouslyHist, int& motion_detection_count)
{
    float difference;
    int differenceBeans;
    for (int i = 0; i < 64; i++) {
        difference = abs(hist.at<float>(i) - previouslyHist.at<float>(i));
        if (difference >= 1) differenceBeans++; // Arbitrário
    }
    if (differenceBeans >= 16) {
        motion_detection_count++;
        cout << "motion Detector" << motion_detection_count << endl;
    } // 25% difference
    previouslyHist = hist.clone();
}
```

So let's go to what matters. the motion detector function.

```
void motionDetector(Mat hist, Mat& previouslyHist, int& motion_detection_count)
{
    float difference;
    int differenceBeans;
    for (int i = 0; i < 64; i++) {
        difference = abs(hist.at<float>(i) - previouslyHist.at<float>(i));
        if (difference >= 1) differenceBeans++; // Arbitrário
    }
    if (differenceBeans >= 16) {
        motion_detection_count++;
        cout << "motion Detector" << motion_detection_count << endl;
    } // 25% difference
    previouslyHist = hist.clone();
}
```

The histogram vector beans of the previously image is saved, and compared with the actual histogram image, for each bean I count if the difference is higher then 1, and if there is 16 beans(25%) that are different from the previously histogram I considere as a motion detection. Those parameters are totally arbitrary, below is the video showing the motion detection, and you can see that when the image is not moving, or moving slowly the motion detection is not triggered.


<iframe width="840" height="600"
src="https://www.youtube.com/embed/gK2wAXdiEvk">
</iframe>

You can see that when the image is not moving, or moving slowly the motion detection is not triggered.