---
layout: post
title: "The Chronicles of Django"
categories: django
date: 2014-07-08 18:00:00
status: beta
description: "Django is a Web-Framework. It is mostly used for designing the back-end for any website but is also useful for making good websites. Some of the websites are Disqus, Instagram, Pinterest,..."
display: true
---
For those who don't know what frameworks are I can explain you it as a template. Web-Frameworks are like templates where the bare bone of a website is given you need to append functionalities required and some basic modifications in the framework code. An example of this kind of frameworks is <a href="http://getbootstrap.com/">Bootstrap</a>

Frameworks are a collection of components you can use to build a website. You don't have to write all code from scratch. You can just take already existing code and reuse it. For more details about frameworks I recommend the post by Satyajit Sahoo in the References .

##Django:

<div id="container"><img src="/images/django.jpg" /></div>

Apart from those like Bootstrap, Skeleton, etc. there are some different kind of Web-Frameworks which are programmable and not static as just for design purposes. As you may see in most of these frameworks you can have a great design but static. So for dynamic behavior we can either use it on our computer or rather on the server itself.

Django is one of the programmable framework. Now it can be used by installing a package using <code>python-pip</code> and django is ready to use.

To start using it just create a new project by running a python script with a argument <code>startproject</code> as <code>django-admin.py startproject</code>.

The file structure is quite complex so to explain it I have this tree..

<pre><code>
|-- db.sqlite3
|-- manage.py 			<b>Managing most of the project</b>
|-- mysite #Project Folder
|   |-- __init__.py
|   |-- __init__.pyc
|   |-- settings.py		<b>Project Settings containing</b>
|   |-- settings.pyc 		<b>diff. dirs and apps</b>
|   |-- urls.py
|   |-- urls.pyc
|   |-- wsgi.py
|   `-- wsgi.pyc
`-- polls			<b>app folder for an app named polls</b>
    |-- admin.py
    |-- admin.pyc
    |-- __init__.py
    |-- __init__.pyc
    |-- models.py		<b>models file defining most of the</b>
    |-- models.pyc 		<b>functions used for db</b>
    |-- templates #templates for HTML
    |   |-- index.html
    |   `-- polls
    |       |-- detail.html
    |       `-- index.html
    |-- tests.py
    |-- urls.py 		<b>urls and its redirection and things to</b>
    |-- urls.pyc		<b>do when this type of regex is found</b>
    |-- views.py 		<b>view functions been called by url.py</b>
    `-- views.pyc
</code></pre>

Most of the things are shown above and that is the basic structure of a "django project". Now the thing to be noted here is its based on Python and updates may migrate it to Python ver.3 also. The other thing is Security. While going through the documentation I saw many procedures for authentication and other database related algorithms and methods.

Django has a great first impressions like it has been designed in form of layers. Model Layer, View Layer and Template Layer.

End of Post

References:
<a href="http://wibblystuff.blogspot.in/2014/05/why-i-like-frameworks-and-why-i-dont.html">"Why I like frameworks, and why I don't" by Satyajit Sahoo</a>
<a href="https://docs.djangoproject.com/en/1.6/">Django Documentation</a>