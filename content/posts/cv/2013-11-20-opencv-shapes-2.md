---
layout: post
title: "OpenCV: Shapes - Rectangles and Ellipses"
categories: opencv
status: published
type: post
date: '2013-11-20 15:00:50'
description: "OpenCV Stuff just continuing with the shapes. This will include Rectangles and Ellipses. This can be used for most of the image processing needs."
draft: false
aliases: [/opencv/2013/11/20/opencv-shapes-2.html]
---

A new post! Sorry if I am taking long long very long gaps in between two posts but there are many different academic activities like attending lectures, preparing for exams and finally giving those exams. Enough from my life we should better start with the Rectangle and Ellipse

For drawing these shapes one should know what they are actually. Rectangle is a square with two pairs of different lengths. As shown :-P.

<div class="container">
<a href="http://www.theknockoffeconomy.com/wp-content/uploads/2012/10/rectangle.gif"><imgs src="http://www.theknockoffeconomy.com/wp-content/uploads/2012/10/rectangle.gif" width="420" height="259" /></a>
</div>

So how can we draw it? This is the big question. For that Drawing Functions used are <code>rectangle();</code> OpenCV allows us to draw a rectangle with just two points here any two opposite points. This will draw the rectangle in these form as shown above. 

<pre><code>void rectangle(img,pt1,pt2,color,thickness=1,lineType=8,shift=0)
cv2.rectangle(img, pt1, pt2, color[, thickness[, lineType[, shift]]])</code></pre>

Here this <code>pt1</code> and <code>pt2</code> are <code>Point</code> the following code explains all with comments.

<pre><code>void main()
{
  Mat image = Mat::zeros(400, 400, CV_8UC3); 
  //Creating an Image of size 400x400 8bit x 3layers
  rectangle(image,Point(100,100),Point(300,200),Scalar(255,255,0),1,8,0);
  //two points as (100,100) and (300,200) with color Cyan (255,255,0)
  namedWindow("Display Image", CV_WINDOW_AUTOSIZE);
  imshow("Display Image", image);
  //Displaying the image in a Window named Display Image
  waitKey(0);
  //#Required!!
}</code></pre>

Now explanation goes this way that we are passing the argument as an <code>Mat image</code> and it gets edited according to the draw a rectangle of size as given by two points. These points are of class <code>Point</code> which has two values x and y which can be defined as <code>Point(x,y)</code> its constructor. We have not used any type of variable to store it but we can do it if we want.

Taking an example of object detection I wanted to draw a rectangle of the size of that object I need atleast two points. These points are coming from the processing of static background and the real-time frame this is quite different and tough though so will discuss it sometime later and coming back to shapes we have made a rectangle in this <code>image</code>.

Now I have one question for you all. Why is this (255,255,0) is cyan and not yellow?? If you can answer this its a big accomplishment for me!

This is the description of various arguments used here (from <a href="http://docs.opencv.org/modules/core/doc/drawing_functions.html#rectangle">OpenCV Documentation</a>)

<pre><code>img – Image.
pt1 – Vertex of the rectangle.
pt2 – Vertex of the rectangle opposite to pt1 .
rec – Alternative specification of the drawn rectangle.
color – Rectangle color or brightness (grayscale image).
thickness – Thickness of lines that make up the rectangle. Negative values, like CV_FILLED , mean that the function has to draw a filled rectangle.
lineType – Type of the line. See the line() description.
shift – Number of fractional bits in the point coordinates.</code></pre>

Following is the python code if some on wants to work on it. Its almost the same.

<pre><code>from cv2.cv import * # importing all the opencv libraries
img=CreateMat(600,600,CV_8UC3) # creating blank image of 400x400 pixels
Rectangle(img,(img.rows/2,img.cols/3),(img.rows/3,img.cols/2),(255,255,255), 3) 
ShowImage("Display",img) # Displaying the image ... 
WaitKey(0)</code></pre>

Now we come to the second part of the Post 

<h2>Ellipse:</h2>

<pre><code>void ellipse(Mat img, center, Size axes, double angle, double startAngle, double endAngle, color, thickness=1, lineType=8, shift=0)
cv2.ellipse(img, center, axes, angle, startAngle, endAngle, color[, thickness[, lineType[, shift]]]) → None</code></pre>

This is just simple if you saw the earlier example of rectangle here we need to understand the arguments

<ol>
<li><code>Mat img</code>: it's the image on which we are desired to draw ellipse.
<li><code>Point center</code>: it is the center of ellipse i.e. the intersection of two axes (Major and Minor)
<li><code>Size axes</code>: this is the size of the ellipse i.e. the major and minor axes length.
<li><code>double angle</code>: its the angle of ellipse with image's X axis.
<li><code>double startAngle, double endAngle </code>: its the starting and ending position of the ellipse I.e. at which angle to start and at what angle to end.
</ol>

<div class="container">
<a href="http://dtchanpura.files.wordpress.com/2013/11/ellipse.png"><img src="http://dtchanpura.files.wordpress.com/2013/11/ellipse.png" width="435" height="259"  /></a>
</div>

From the given image everything will be clear that which axis are primary the angle represents the change of angle in the major axis the start and end angle represents the starting and ending points of the ellipse as shown.

Code is almost the same as just have to put values in place of variables and which will make an Ellipse of your choice.

End of Post