---
layout: post
title: 'Image Processing: From the ‘Sixth Sense’ view.'
categories: "Image Processing"
date: 2012-07-10 22:00:09
status: published
description: "Image processing is just another phrase to make any computer or a device not just to see objects but also to understand what it represents."
display: true
---
From last few years the demands of image processing applications have increased considerably. Especially the one concept given by Pranav Mistry about his application ‘Sixth Sense’. Yes, in this post I will try to give you the brief idea about that and also you can build your own sixth sense device*. He interfaced a camera and projector with one of the best processors

Image processing is just another phrase to make any computer or a device not just to see objects but also to understand what it represents. For example to find a color proportion in an image, for finding any cracks or flaws on a wall or even finding a particular color from the image, etc.

<!--more-->

There are mainly two types divided on the basis of their use. The first is just the simple image from a camera or any other source. It is also called static image. The other is a video or an array of images (the one which we are going to use). We all know this type of format and a video is the array of images called frame. This kind of array is used by dividing them into many images and processing one frame at a time.

So here also we are going to do the same firstly we will process one image than doing it continuously for upcoming frames from a camera. For doing that we need some components as follows:
<ol>
	<li>PC camera / webcam (built-in)</li>
	<li>The MathWorks™ Matlab®</li>
	<li>Any output device (here I use my mouse pointer, moving according to the object in camera.)</li>
</ol>
Now let’s start with some basic steps to make our own sixth sense:
<ol>
	<li>Get an image.</li>
	<li>Process the taken image.</li>
	<li>Generate output with respect to the image processed.</li>
</ol>
<strong>Firstly we will do this all processes for a simple one image then by making a while(1) loop we can do continuously take a image, process it, and generate desired output.</strong>

<strong>Step 1:</strong>Getting an image.

This step has its own sub steps (Write this codes in command window)
<ul>
	<li>Getting information about image acquisition hardware available. The function used here is
<pre>Code:            imaqhwinfo()</pre>
</li>
	<li>This gives output as</li>
</ul>
<pre>ans =
InstalledAdaptors: {'coreco'  'winvideo'}
MATLABVersion: '7.8 (R2009a)'
ToolboxName: 'Image Acquisition Toolbox'
ToolboxVersion: '3.3 (R2009a)'</pre>
<ul>
	<li>Now we have to select ‘winvideo’ from it by writing simply imaqhwinfo('winvideo') This gives information about which tools are available for winvideo e.g. as I have two of my cameras I get two matrices [1] and [2] which shows the number of image devices present output will be:</li>
</ul>
<pre>ans =
AdaptorDllName: 'C:\Program Files\MATLAB\R2009a\toolbox\imaq\imaqadaptors\win32\mwwinvideoimaq.dll'
AdaptorDllVersion: '3.3 (R2009a)'
AdaptorName: 'winvideo'
DeviceIDs: {[1]  [2]}
DeviceInfo: [1x2 struct]</pre>
<ul>
	<li>Select any one and write the command to declare a variable v(say) as
<pre>Code:            v=videoinput('winvideo',1,'YUY2_640x480');</pre>
</li>
	<li>‘videoinput’ function has its syntax as (Adapter name, device id, Format of image captured). This will generate any output and just will declare the variable but if you want to have details about that device or input object.</li>
	<li>Now as we have declared an object we need to use that. We will use function called preview(). This has a simple syntax and have no return type so just write preview(v); this will open a window which shows the preview of the camera.
<pre>Code:            preview(v);</pre>
</li>
	<li>Till this point we have just initialized our Camera or the input device to our Matlab® now we will see how to capture an image from that camera.</li>
	<li>Capturing an image is very simple you just have to select the video input object and specify some variable your variable will store the image. Just write
<pre>Code:            q=getsnapshot(v);</pre>
</li>
	<li>There are many functions for images e.g.
<ul>
	<li>imread to read image from file stored on harddisk</li>
	<li>imshow to show the image</li>
	<li>imview to show image and give the general specifications of image at particular point by mouse pointer i.e. X and Y co-ordinates value or intensity of color red, green, blue, etc.</li>
</ul>
</li>
	<li>The image we captured is of the form luminance(y):chrominance(CbCr) but for image processing we need it in RGB form so we convert it using the function ycbcr2rgb(q); and store it in other variable i(say).
<pre>Code:            i=ycbcr2rgb(q);</pre>
</li>
	<li>So now this is the end of step 1 and finally we get an image of the form RGB.</li>
</ul>


<b>Step 2:</b>Process the image

This step also has its own sub steps (Write this codes in command window)

You should have an image as we have here from the camera.
<div id="container" width=392><a href="http://dtchanpura.files.wordpress.com/2012/08/rgbimage.png"><img class=" wp-image-92 " title="rgbimage" alt="Image of variable 'i'" src="http://dtchanpura.files.wordpress.com/2012/08/rgbimage.png" width="392" height="294" /></a><br /> Image of variable 'i'</div>

<ul>
	<li>Now we have an rgb image lets say we have a ball or a small object which is of different color i.e. red, green, orange or pink this will be the pointer. So when we move that ball our mouse pointer will also move.</li>
</ul>
<ul>
	<li>The main step here is to filter the colors in the image that is to take the intensities of red color in the rgb image. So now for that we need to write a command as follows which will give the output as only red color intensities to variable 'fr'(say filtered red)</li>
</ul>
<pre >fr=i(:,:,1);</pre>
<ul>
	<li>Here by writing i(:,:,1) ':' represents all the values i.e. first all the values in x dimension and other gives all the values in y dimension and 1 represents the color 'red' similarly we write 2 for green and 3 for blue</li>
</ul>
<pre>fg=i(:,:,2);
fb=i(:,:,3);</pre>
<ul>
	<li>Now we will see another function which is imview() it is similar to imshow but here it also returns the values of that particular pixel with its (x,y) co-ordinates use it for all of the above three. So the code is as follows</li>
</ul>
<pre>
imview(fr);
imview(fg);
imview(fb);
</pre>
<div id="container">
<a href="http://dtchanpura.files.wordpress.com/2012/08/frfgfb.png"><img class="size-full wp-image-93" title="frfgfb" alt="" src="http://dtchanpura.files.wordpress.com/2012/08/frfgfb.png" width="560" height="138" /></a><br /> Variables 'fr' 'fg' 'fb' respectively</div>
<ul>
	<li>By writing this you will get three windows with three gray-scale images first one will show the image with red intensity and then so on. Now you can see we have got three images with different frequency of visibility. Now we have to find the region of interest that is the object (here the ball). <em>.</em></li>
</ul>
