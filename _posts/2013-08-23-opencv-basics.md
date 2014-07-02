---
layout: post
title: 'OpenCV: Basics'
categories: opencv
status: published
date: 2013-08-23 09:00:16
description: "OpenCV as you all know is an open-source Computer Vision library for C, C++ and also Python. This post will guide you for how to use and how to implement any problem on OpenCV."
display: true
---

OpenCV as you all know is an open-source Computer Vision library for C, C++ and also Python. This post will guide you for how to use and how to implement any problem on OpenCV. First of all an apology for not writing any post till now. From last week I am thinking about the topic to write for the blog and I came up with this. Choose any one from the given two Integrated Development Environments(IDEs) its needed for developing a project then even executable file at the end and it also helps a lot in debugging.

<h2><a href="http://www.eclipse.org" target="_blank">Eclipse IDE</a>:</h2>
As we are dealing with C and C++  we need an environment for developing the program so we use Eclipse (here). Precisely Eclipse's CDT plugin. You need to download it from <a href="http://www.eclipse.org/downloads/packages/eclipse-ide-cc-developers-includes-incubating-components/indigosr2">here</a>. Its around 100 MBs.

<h2><a href="http://www.microsoft.com/visualstudio" target="_blank">Visual Studio</a>:</h2>
For detailed instructions on configuring OpenCV on Visual Studio and Eclipse Please visit <a href="http://docs.opencv.org/doc/tutorials/introduction/windows_visual_studio_Opencv/windows_visual_studio_Opencv.html" target="_tab">OpenCV on VS</a> and <a href="http://docs.opencv.org/doc/tutorials/introduction/linux_eclipse/linux_eclipse.html" target="_tab">OpenCV on Eclipse</a> respectively.

<h2>Basics:</h2>
Basics of OpenCV also starts with reading an image and showing it in a window. Well its not as simple as it was in Matlab. So before reading an image lets start with the basic structure of OpenCV C language program.

<pre><code>#include "stdafx.h" //if you are using Visual Studio
#include &lt;cv.h&gt;
int main()
{
declaration of variables;
\...
program;
\...
return 0;
}</code></pre>

<b>For C++:</b>

<pre><code>#include &lt;cv.h&gt;using namespace cv;int main()
{
\...
return 0;
}</code></pre>

<b>For Python:</b>

<pre><code>from cv2.cv import *
\...
\...</code></pre>

The function in OpenCV header file <code>cv.h</code> for reading an image is <code>cvLoadImage()</code> in C and <code>imread()</code> in C++. From that you may get an idea C++ is better to code in rather than C (later you will think its better to code in Python<a title="For those who already know how to code in Python">*</a> rather than C++) Following is the code for reading an image and displaying on a window named "Display"

<pre><code>#include&lt;cv.h&gt;
#include&lt;highgui.h&gt;
int main()
{
IplImage* img1 = cvLoadImage("C:\image\path", CV_LOAD_IMAGE_COLOR);
	cvNamedWindow("Display", CV_WINDOW_AUTOSIZE);
	cvShowImage("Display",img1);
	cvWaitKey(0);
	cvReleaseImage(&amp;img1);
	return 0;
}</code></pre>

<h2>Explanation</h2>

Starting from the beginning "The Header Files"

<ul>
	<li><code>cv.h</code> contains all the necessary functions like reading an image filtering creating shapes, etc.</li>
	<li>Header file <code>highgui.h</code> on the other hand contains functions for Graphical User Interfaces like showing image in window having adjustment bars, etc.</li>
</ul>

Now "The Code"

<ul>
	<li>The first part like <code>int main()</code> <a title="I often wonder why the write int as return type when they don't want to return anything">*</a> which says main function starts now.</li>
	<li>Next is quite difficult to understand its the structure that is used to store images i.e. <code> IplImage* img1 </code> assigns the starting address of memory to <code>img1</code>.</li>
	<li><code>img1 = cvLoadImage("Path to Image",CV_LOAD_IMAGE_COLOR)</code> stores the image defined in path to address pointed by img1 and the macro <code>CV_LOAD_IMAGE_COLOR</code> is for specifying that the fetched image should be in form of "color" not "gray-scaled" if we write GRAYSCALE in place of COLOR it will read/load image in form of gray-scaled image.</li>
	<li>Now we use <code>highgui.h</code> for creating a window for showing the image using <code>cvNamedWindow("WindowName",size)</code> here again macro has been used named <code>CV_WIDOW_AUTOSIZE</code> which resizes the window with its content.</li>
	<li>Now as we defined the window we need to add the content in it using the function <code>cvShowImage("WindowName",image-var)</code></li>
	<li>That's it the image will be displayed now to hold it to view and not to just 'return 0' and exit we need to add one more line as <code>cvWaitKey(time)</code> this will wait.</li>
	<li>We know that we need to clear the memory been used by image variable so we use <code>cvReleaseImage()</code> function to release the image memory.</li>
</ul>

Well that ended the C part now almost the same remains in both C++ and Python. The codes are given with comments in the and they are self-explanatory .

<h2>For C++:</h2>

<pre><code>#include &lt;cv.h&gt;
#include &lt;highgui.h&gt;
using namespace cv;
int main()
{
	Mat image;  //Image is in form of matrix (here)
	image = imread("/home/darshil/images/hello.jpg", 1 );
	if(!image.data ) //Check if image has data
	{
		printf( "No image data \n" );
		return -1;
	}
	namedWindow( "Display Image", CV_WINDOW_AUTOSIZE );
	imshow( "Display Image", image );
	waitKey(0);
	return 0;
}
</code></pre>

<h2>For Python:</h2>

<pre><code>from cv2.cv import * # importing the opencv module
img=LoadImageM("path/to/image.jpg") # loading image in form of matrix 
NamedWindow("Display",CV_WINDOW_AUTOSIZE) # Creating a window
ShowImage("Display",img)  #Displaying the image
WaitKey(0) #waitkey to wait till the output comes
</code></pre>

This is it.

End of Post.
