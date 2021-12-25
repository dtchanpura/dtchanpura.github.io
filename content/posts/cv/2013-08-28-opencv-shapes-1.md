---
layout: post
title: "OpenCV: Shapes - Circles"
categories: opencv
status: published
date: 2013-08-28 07:25:43
description: "This post will guide you to, as you can see in the title, draw shapes on image or in a blank image."
draft: false
aliases: [/opencv/2013/08/28/opencv-shapes-1.html]
---

Well here we are after a week a new post. This post will guide you to, as you can see in the title, draw shapes on image or in a blank image. I am going use both a blank image and an image from my hard drive and implement it on both C++ and Python.

Following are the few shapes which you can draw:

<ul>
	<li>Circle</li>
	<li>Rectangle</li>
	<li>Ellipse</li>
	<li>Polygon (Tough)</li>
</ul>

Before understand drawing this shape we need to understand the use of it. That is where we can use it. Like if you are making an application on <a class="zem_slink" title="Face detection" href="http://en.wikipedia.org/wiki/Face_detection" target="_blank" rel="wikipedia">Face Detection</a> we can make a use of circle and drag it all over wherever the face is detected.

<h1>Circle:</h1>

This objects are found in <a href="http://docs.opencv.org/modules/core/doc/drawing_functions.html" target="_blank">Drawing Functions of OpenCV</a> (go to the link if you need to get details about each and every drawing function). Following is the syntax of function circle

<pre><code>circle(img,center,radius,color,thickness=1,lineType=8,shift=0)
cv2.circle(img, center, radius, color[, thickness[, lineType[, shift]]])
cv.Circle(img, center, radius, color[, thickness[, lineType[, shift]]])</code></pre>

<b>Note:</b> These functions are not returning anything i.e. <code>void</code> functions. So it edits the image in the function itself. First is C++ and other two are for python.

Following is the code for drawing a circle.

<b>For C++:</b>

<pre><code>#include &lt;cv.h&gt;
#include &lt;highgui.h&gt;

using namespace cv;
int main()
{
Mat img1= Mat::zeros(500,500,CV_8UC3);
circle(img1,Point(img1.rows/2,img1.cols/2),100,Scalar(255,255,0),1,8);
namedWindow("Window",CV_WINDOW_AUTOSIZE);
imshow("Window",img1);
return 0;
}</code></pre>

<ul>
	<li>I think we should start with the declaration of <code>img1</code> we have used a constructor of <code>Mat</code> to initialize the object <code>img1</code> as by filling it with black color or in numerals with '0'.</li>
	<li>Now comes the main part of drawing the circle. <code> circle() </code> has many arguments like '100' is value of <code>radius</code> then '1' is for <code>thickness</code> again '8' is for <code>lineType</code> as discussed in the syntax portion. Rest is the inline functions used such as <code>Point()</code> to avoid declaring a variable for <code>center</code>. Similarly for <code>Scaler(B,G,R)</code> we have directly used it.</li>
</ul>

Now it will show circle of color 'Cyan' in a black background. You can make your own color using the function <code>Scalar(B,G,R)</code> and be sure values are between 0-255 :-P. Also at any place i.e. by changing the <code>Point()</code> which is defined for <code>center</code>.

<b>For Python:</b>

<pre><code>from cv2.cv import * # importing all the opencv libraries
img=CreateMat(400,400,CV_8UC3) # creating blank image of 400x400 pixels
Circle(img,(img.rows/2,img.cols/2),50,(255,255,255), 1)
ShowImage("Display",img) # Displaying the image  
WaitKey(0)</code></pre>

I hope you all would have got an idea about everything and here in place of <code>zeros()</code> we used <code>CreateMat()</code>. Also as you can see in the syntax we have '[]' brackets which shows the arguments inside are optional.

As I wrote earlier that we can draw shape in an existing image also so here you can use <code>img1=imread("path/to/image")</code> or <code>img1=LoadImageM("path/to/image")</code> instead of <codE>zeros()</code> or <code>CreateMat()</code>

That's it for circles next post we will see the ellipses... you can get the files from my github repository <a href="https://github.com/OpenDCLabs/opencv-programs.git" target="_blank">opencv-programs</a>

End Of Post