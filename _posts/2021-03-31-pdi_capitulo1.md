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

Is asked to create a program that creates a negative region in the Image given two points P1 and P2 that form a rectangle.
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

ConfigLoader is just an implementation I use to keep some parameters, what matters here is that ConfigLoader will
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

This question asks to switch quadrants of an image this way.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/quad.png">
    <img src="/MyBlog/assets/img/pdi/quad.png" alt="example">
  </a>
</div>
<br />


The code is shown below:

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
The main idea here is pretty simple, I just divided the image into 4 quadrants and saved those images(The first four lines). Then I created a new image to be the final output, that has the size of the input image, and I inserted the quadrants in the order desired.

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

# Question 3

This question asks to find and count the number of "bubbles" with and without holes, the image below is used as an example.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/bolhas.png">
    <img src="/MyBlog/assets/img/pdi/bolhas.png" alt="example">
  </a>
</div>
<br />

We can see that there are a black background and some white bubbles, in this example, the background color is 0 and the bubble color is 255. Finally, all the bubbles that touch the edge of the image should be removed. So here is the final code.

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
    The first thing I do is remove all bubbles that touch the edge. This sure is not the best implementation to do this but was the first I had in mind. So here I'm searching for white pixels in the borders and using flood fill to paint it with Black.

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

Then I detect the bubbles and I paint them as gray(just for visualization), after this step I have all the bubbles with or without a hole in the image.

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
the image all white but all holes in the image will be painted as black, so now we can search for black pixels to count the holes.

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
    Finally, I have the number of bubbles and the number of bubbles with hole, So I just need to subtract the number of the total with the number of bubbles with a hole to find the number of bubbles without a hole.

```
   cout << "The image has " << nobjects_with_holes << " bubbles with holes\n";
   cout << "The image has " << nobjects - nobjects_with_holes << " bubbles without holes\n";
```

# Question 4.1

In this question have to implement a histogram equalization in a video. Below there are two videos, the first one is the input video in grayscale and the second one is the output after histogram equalization. There isn't much to talk about in this code, first, there is a calculation of the histogram image and it is shown in the image, and later the equalizeHist function of OpenCV equalizes the histogram of a grayscale image.

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

This question asks for a motionDetector based on the histogram of the image. Below is the full code.

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

The histogram vector beans of the previous image are saved, and compared with the actual histogram image, for each bean, I count if the difference is higher than 1, and if there are 16 beans(25%) that are different from the previously histogram I consider as motion detection. Those parameters are arbitrary, below is the video showing the motion detection, and you can see that when the image is not moving or moving slowly the motion detection is not triggered.


<iframe width="840" height="600"
src="https://www.youtube.com/embed/gK2wAXdiEvk">
</iframe>

# Question 5

This question asks for a Gauss Laplacian Filter, the program below was used as base.

```

#include <config_loader.h>
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace cv;
using namespace std;

void printmask(Mat& m)
{
    for (int i = 0; i < m.size().height; i++) {
        for (int j = 0; j < m.size().width; j++) { cout << m.at<float>(i, j) << ","; }
        cout << "\n";
    }
}

int main(int argc, char* argv[])
{
    float media[] = {0.1111, 0.1111, 0.1111, 0.1111, 0.1111, 0.1111, 0.1111, 0.1111, 0.1111};
    float gauss[] = {0.0625, 0.125, 0.0625, 0.125, 0.25, 0.125, 0.0625, 0.125, 0.0625};
    float horizontal[] = {-1, 0, 1, -2, 0, 2, -1, 0, 1};
    float vertical[] = {-1, -2, -1, 0, 0, 0, 1, 2, 1};
    float laplacian[] = {0, -1, 0, -1, 4, -1, 0, -1, 0};
    float boost[] = {0, -1, 0, -1, 5.2, -1, 0, -1, 0};

    Mat frame, framegray, frame32f, frameFiltered, frameTemp;
    Mat mask(3, 3, CV_32F);
    Mat result;
    double width, height;
    int absolute;
    char key;
    string video_name;

    if (argc != 2) {
        printf("Couldn't load all parameter, use example of use: ./4_histogram config_file\n");
        return 0;
    }

    ConfigLoader param_loader(argv[1]);
    param_loader.checkAndGetString("q4_video", video_name);

    VideoCapture cap(video_name);
    cap.set(CAP_PROP_FRAME_WIDTH, 640);
    cap.set(CAP_PROP_FRAME_HEIGHT, 480);
    width = cap.get(CAP_PROP_FRAME_WIDTH);
    height = cap.get(CAP_PROP_FRAME_HEIGHT);
    cout << "Width  =" << width << "\n";
    cout << "Height =" << height << "\n";
    cout << "fps    =" << cap.get(CAP_PROP_FPS) << "\n";
    cout << "format =" << cap.get(CAP_PROP_FORMAT) << "\n";

    namedWindow("filtroespacial", WINDOW_NORMAL);
    namedWindow("original", WINDOW_NORMAL);

    mask = Mat(3, 3, CV_32F, media);

    absolute = 1; // calcs abs of the image

    while (cap.isOpened()) {
        cap >> frame; // get a new frame from camera
        cvtColor(frame, framegray, COLOR_BGR2GRAY);
        flip(framegray, framegray, 1);
        imshow("original", framegray);
        framegray.convertTo(frame32f, CV_32F);
        filter2D(frame32f, frameTemp, frame32f.depth(), Mat(3, 3, CV_32F, gauss), Point(1, 1), 0);
        if (absolute) { frameTemp = abs(frameTemp); }

        filter2D(frameTemp, frameFiltered, frameTemp.depth(), mask, Point(1, 1), 0);
        if (absolute) { frameFiltered = abs(frameFiltered); }

        frameFiltered.convertTo(result, CV_8U);

        imshow("filtroespacial", result);

        key = (char)waitKey(10);
        if (key == 27) break; // Esc pressed!
        switch (key) {
            case 'a': absolute = !absolute; break;
            case 'm':
                mask = Mat(3, 3, CV_32F, media);
                printmask(mask);
                break;
            case 'g':
                mask = Mat(3, 3, CV_32F, gauss);
                printmask(mask);
                break;
            case 'h':
                mask = Mat(3, 3, CV_32F, horizontal);
                printmask(mask);
                break;
            case 'v':
                mask = Mat(3, 3, CV_32F, vertical);
                printmask(mask);
                break;
            case 'l':
                mask = Mat(3, 3, CV_32F, laplacian);
                printmask(mask);
                break;
            case 'b': mask = Mat(3, 3, CV_32F, boost); break;
            default: break;
        }
    }
    return 0;
}
```

So I only had to added a new mask for laplacian_gauss, the mask is:

```
float laplacian_gauss[] = {0, 0, -1, 0, 0, 
                            0, -1, -2, -1, 0, 
                            -1, -2, 16, -2, -1, 
                            0, -1, -2, -1, 0, 
                            0, 0, -1, 0, 0};

```

And here is the result using laplacian_gauss:

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/laplacian_gauss.png">
    <img src="/MyBlog/assets/img/pdi/laplacian_gauss.png" alt="example">
  </a>
</div>
<br />

# Question 6.1

This question asks for a focus simulation. We use this function to create the filter.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/decay.png">
    <img src="/MyBlog/assets/img/pdi/decay.png" alt="example">
  </a>
</div>
<br />

Below is the code and the input image.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/image_frasqueirao.png">
    <img src="/MyBlog/assets/img/pdi/image_frasqueirao.png" alt="example">
  </a>
</div>
<br />

```
#include <config_loader.h>
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace cv;
using namespace std;

Mat mask(3, 3, CV_32F), mask1;
Mat image1, image2, imageFiltered;

double alpha, l1, l2;
int y_max;
int height = 0, center = 0, decayValue = 1;
int barRegion = 0, decaySlider = 0, barCenter = 0;

/**
 * Blur Image
 */
void blurImage();

/**
 * Set decayValue
 */
void setDecayFocus(int, void*);
/**
 * Set center height of region focus
 */
void setCenterHeight(int, void*);
/**
 * Set Center position of center focus region
 */
void setCenterPosition(int, void*);

/**
 * Calculate and show focus
 */
void calculateFocus();

int main(int argc, char* argv[])
{
    char TrackbarName[50];
    string image_name;

    if (argc != 2) {
        printf("Couldn't load all parameter, use example of use: ./6_tilt config_file\n");
        return 0;
    }

    ConfigLoader param_loader(argv[1]);
    param_loader.checkAndGetString("q6_exercise", image_name);

    image1 = imread(image_name);  // Open Image
    y_max = image1.size().height; // Get height

    image2 = image1.clone();

    // Blur Image
    for (int i = 1; i < 10; i = i + 2) { GaussianBlur(image2, imageFiltered, Size(i, i), 0, 0); }

    namedWindow("Focus", 1);

    sprintf(TrackbarName, "Decay: ");
    createTrackbar(TrackbarName, "Focus", &decaySlider, 100, setDecayFocus);
    setDecayFocus(decaySlider, 0);

    sprintf(TrackbarName, "Center Position");
    createTrackbar(TrackbarName, "Focus", &barCenter, 100, setCenterPosition);
    setCenterPosition(barCenter, 0);

    sprintf(TrackbarName, "Height");
    createTrackbar(TrackbarName, "Focus", &barRegion, 100, setCenterHeight);
    setCenterHeight(barRegion, 0);

    waitKey();
    return 0;
}

void setDecayFocus(int, void*)
{
    // Min decay is 1
    decayValue = (double)decaySlider + 1;
    calculateFocus();
}

void setCenterHeight(int, void*)
{
    height = (double)(barRegion * y_max) / 100;
    calculateFocus();
}

void setCenterPosition(int, void*)
{
    center = (double)(barCenter * y_max) / 100;
    calculateFocus();
}

void calculateFocus()
{
    // Calculate the l1 and l2 of function.
    l1 = center - height / 2;
    l2 = center + height / 2;

    for (int i = 0; i < imageFiltered.size().height; i++) {
        // Alpha function
        alpha = 0.5 * (tanh((i - l1) / decayValue) - tanh((i - l2) / decayValue));
        addWeighted(image1.row(i), alpha, imageFiltered.row(i), 1.0 - alpha, 0.0, image2.row(i));
    }

    imshow("Focus", image2);
    imwrite("focusResult.png", image2);
}

```

Here is the program working without a region define, so the image is all blur.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/blur.png">
    <img src="/MyBlog/assets/img/pdi/blur.png" alt="example">
  </a>
</div>
<br />

And here is the image when focusing in the stadium with 0 decay, you can see that there is a line separating the focused image and the blurred image right above and below the stadium, creating a bad transition.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/blur_0decay.png">
    <img src="/MyBlog/assets/img/pdi/blur_0decay.png" alt="example">
  </a>
</div>
<br />

And here we can see the same region but using 100 decay, creating a smooth transition between focused and blur.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/pdi/blur_100decay.png">
    <img src="/MyBlog/assets/img/pdi/blur_100decay.png" alt="example">
  </a>
</div>
<br />

# Question 6.2

The Goal in this question is to use the previous program with a video, producing a tilt-shift in the frames and dropping some frames to produce miniaturization of a scene. This effect will create a scene that looks like a stop motion.

Here is the full code, you can notice how similar is to the previous one except by a wait of 5ms between friends(and the video except for an image of course).

```
#include <config_loader.h>
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace cv;
using namespace std;

Mat mask(3, 3, CV_32F), mask1;
Mat image1, image2, imageFiltered;

double alpha, l1, l2;
int y_max;
int height = 0, center = 0, decayValue = 1;
int barRegion = 0, decaySlider = 0, barCenter = 0;

/**
 * Blur Image
 */
void blurImage();

/**
 * Set decayValue
 */
void setDecayFocus(int, void*);
/**
 * Set center height of region focus
 */
void setCenterHeight(int, void*);
/**
 * Set Center position of center focus region
 */
void setCenterPosition(int, void*);

/**
 * Calculate and show focus
 */
void calculateFocus();

int main(int argc, char* argv[])
{
    char TrackbarName[50], key;
    string video_name;
    double width, height;

    if (argc != 2) {
        printf("Couldn't load all parameter, use example of use: ./6_tilt_video config_file\n");
        return 0;
    }

    ConfigLoader param_loader(argv[1]);
    param_loader.checkAndGetString("q6_tilt_video", video_name);

    VideoCapture cap(video_name);
    cap.set(CAP_PROP_FRAME_WIDTH, 640);
    cap.set(CAP_PROP_FRAME_HEIGHT, 480);
    width = cap.get(CAP_PROP_FRAME_WIDTH);
    height = cap.get(CAP_PROP_FRAME_HEIGHT);

    while (cap.isOpened()) {
        cap >> image1; // get a new frame from camera

        y_max = image1.size().height; // Get height
        image2 = image1.clone();

        // Blur Image
        for (int i = 1; i < 10; i = i + 2) { GaussianBlur(image2, imageFiltered, Size(i, i), 0, 0); }

        namedWindow("Focus", 1);

        sprintf(TrackbarName, "Decay: ");
        createTrackbar(TrackbarName, "Focus", &decaySlider, 100, setDecayFocus);
        setDecayFocus(decaySlider, 0);

        sprintf(TrackbarName, "Center Position");
        createTrackbar(TrackbarName, "Focus", &barCenter, 100, setCenterPosition);
        setCenterPosition(barCenter, 0);

        sprintf(TrackbarName, "Height");
        createTrackbar(TrackbarName, "Focus", &barRegion, 100, setCenterHeight);
        setCenterHeight(barRegion, 0);

        // Wait 5ms to the next frame, this will make the video looks a little more like a stop motion
        key = (char)waitKey(5);
        if (key == 27) break; // esc pressed!
    }
    waitKey();
    return 0;
}

void setDecayFocus(int, void*)
{
    // Min decay is 1
    decayValue = (double)decaySlider + 1;
    calculateFocus();
}

void setCenterHeight(int, void*)
{
    height = (double)(barRegion * y_max) / 100;
    calculateFocus();
}

void setCenterPosition(int, void*)
{
    center = (double)(barCenter * y_max) / 100;
    calculateFocus();
}

void calculateFocus()
{
    // Calculate the l1 and l2 of function.
    l1 = center - height / 2;
    l2 = center + height / 2;

    for (int i = 0; i < imageFiltered.size().height; i++) {
        // alpha function
        alpha = 0.5 * (tanh((i - l1) / decayValue) - tanh((i - l2) / decayValue));
        addWeighted(image1.row(i), alpha, imageFiltered.row(i), 1.0 - alpha, 0.0, image2.row(i));
    }

    imshow("Focus", image2);
    imwrite("focusResult.png", image2);
}

void blurImage() {}

```

Below is the video showing the process.



<iframe width="840" height="600"
src="https://www.youtube.com/embed/yMU6DuOi6t8">
</iframe>