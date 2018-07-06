---
layout: post
title: "How to use Selenium in R (in Windows 10)"
date: 2018-07-06
excerpt: "Installation, set-up and brief example of RSelenium in Windows 10"
tags: [RSelenium, R, webscraping]
comments: true
---

Selenium is a "suite of tools to automate web browsers"[^1], which can be used from R using the package RSelenium and it's a great tool for webscrapping. In contrast to the using it from Python, to make it work in R some additional "tunning" is requiered. In essence this post follows the idea of [this](http://rpubs.com/johndharrison/RSelenium-Docker) RSelenium vignette but showing the process a bit clearer (I hope).

What we will do:

1.	Install Docker Toolkit
2.	Install RSelenium
3.	Install VNC viewer
4.	Set all up
5.	A brief example

It is assumed that:

1.	Virtual Machine is enable in your computer. To check whether virtual machine is enable you can open the task manager and review that virtualization is enable. It should look like this:

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_rselenium/task_manager.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_rselenium/task_manager.jpg" width="400" height="300" align="middle"></a>
</figure>

2.	R and RStudio are already installed.

Let's begin

## 1. Installing Docker Toolkit

Docker is used to run software packages called "containers". In a typical example use case, one container runs a web server and web application, while a second container runs a database server that is used by the web application.[^2] It would basically help us to perform the navigation in a virtual machine using a "container" with an specific web browser (firefox in our case).

To install it your should just open this [link](https://docs.docker.com/toolbox/overview/#ready-to-get-started), click on "Get Docker Toolbox for Windows" and follow the instructions (your shouldn't need to change any installation options).

After installing it, in the main menu the program "Docker Quickstart Terminal" should be available, click on it and wait. The sign that everything worked properly is the presence of a whale... like this:

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_rselenium/whale.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_rselenium/whale.jpg" width="400" height="300" align="middle"></a>
</figure>

It could take a while, the first time you open the program it will "configure itself". The most likely error you could encounter at this point is due to not having the virtual machine enable in your computer.

Once we are here, close the progam. We will come back here later.

[^2]: [Wikipedia](https://en.wikipedia.org/wiki/Docker_(software))

## 2. Installing RSelenium

Although RSelenium has been (removed from CRAN)[https://github.com/ropensci/RSelenium/issues/172], previous working versions can be installed from the archive.

Running the following code in R should install all what is needed (in R).

{% highlight r%} 
install.packages("devtools")
library(devtools)

install.packages("semver")
install.packages("subprocess")
install.packages("https://cran.r-project.org/src/contrib/Archive/wdman/wdman_0.2.2.tar.gz",
                 repos = NULL, type = "source")
install.packages("https://cran.r-project.org/src/contrib/Archive/binman/binman_0.1.0.tar.gz",
                 repos = NULL, type = "source")
install.packages("https://cran.r-project.org/src/contrib/Archive/RSelenium/RSelenium_1.7.1.tar.gz",
                 repos = NULL, type = "source")
{% endhighlight %}

To check if everything is ok try loading RSelenium using *library()*.

## 3. Installing VNC viewer

VNC viewer will let you view and "access" the virtual machine you will be running using Docker. In the VNC you can find the server application and the viewer application. For our purpose you will just need the viewer, which can be downloaded following this [link](https://www.realvnc.com/en/connect/download/viewer/).

Again, there is no need to change any of the preset configurations. Once you have installed it don't open it yet.

## 4. Setting all up

At this point you should have almost everything needed and now we will set it up. I said almost everything because we still need the Docker containers.

The point of what we are about to do is download the firefox docker, initiate the virtual machine with the corresponding container, then conect it to the VNC viewer using the correct IP and finally use the settings we used to initiate the virtual machine to connect everything to RStudio.

First things first... open "Docker Quickstart Terminal" when the whale appears run the following in the command line (just below the whale).

{% highlight docker%} 
docker pull selenium/standalone-firefox:2.53.0
docker pull selenium/standalone-firefox-debug:2.53.0
{% endhighlight %}

After installing the containers run the following line

{% highlight docker%} 
docker run -d -p 4445:4444 -p 5901:5900 selenium/standalone-firefox-debug:2.53.0
{% endhighlight %}

Theese lines define the ports to be use by R and VNC. They will appear again later.

If all is going well you should see something like this:

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_rselenium/run_docker.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_rselenium/run_docker.jpg" width="400" height="300" align="middle"></a>
</figure>

Do not close "Docker Quickstart Terminal" and now open VNC viewer and as VNC server address copy and paste the following: *192.168.99.100:5901* just before pressing enter it should look like this:

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_rselenium/VNC_viewer.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_rselenium/VNC_viewer.jpg" width="400" height="300" align="middle"></a>
</figure>

It is going to say that you have to type a password. The password is secret... is "secret" [^3] and voila now you are looking at the virtual machine you initiate using Docker.

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_rselenium/virtual.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_rselenium/virtual.jpg" width="400" height="300" align="middle"></a>
</figure>

[^3]: Literally type "secret" and the press enter.

Now, let's link all to R.

Open RStudio (if you closed it before) and run the following code:

{% highlight r%} 
library(RSelenium)
remDr <- remoteDriver(remoteServerAddr = "192.168.99.100",
                      port = 4445L)
remDr$open(silent = T)
{% endhighlight %}

That will link your session to the virtual machine you initiate, which you can check using VNC viewer. Note the numbers (IP and port), the same we defined before.

From this point on RSelenium [help](https://mran.revolutionanalytics.com/snapshot/2014-09-12/web/packages/RSelenium/RSelenium.pdf) and tutorials like [this](https://www.computerworld.com/article/2971265/application-development/how-to-drive-a-web-browser-with-r-and-rselenium.html) will help a lot. Basically what you'll be doing is sending, from R to the web browser, the directions you usually send using your mouse and keyboard.

After you are done using RSelenium I consider good practice the following steps.

1.	Close the web browser connection from R.

{% highlight r%} 
remDr$close()
{% endhighlight %}

2.	Close the VNC Viewer application.

3.	Stop the docker an virtual machine in "Quickstart Docker Terminal".

Run the following lines:

{% highlight docker%} 
docker stop $(docker ps -q)
docker-machine stop default
{% endhighlight %}

And close the "Quickstart Docker Terminal" application.

**Now a brief example**

## 5. Brief example

The following code opens the directory of state employees in Colombia and look for all who match with the search term "Juan", then click on "Siguiente" (next) and then closes the web browser.

For this code to run properly the Quickstart Docker Terminal should be open and the firefox container running. The VNC viewer is needed only if you want to see what the code is doing in real time.

{% highlight r%} 
library(RSelenium) # load the package

remDr <- remoteDriver(remoteServerAddr = "192.168.99.100",
                      port = 4445L) # create the connection
					  
remDr$open(silent = T) # open the web browser

remDr$navigate("http://busquedas.dafp.gov.co/search?q=&btnG=Buscar&client=Hojas_de_vida&output=xml_no_dtd&proxystylesheet=Hojas_de_vida&sort=date%3AD%3AL%3Ad1&oe=UTF-8&ie=UTF-8&ud=1&getfields=*&site=Hojas_de_Vida&filter=0&") # tell the web browser to go to an specific URL

a<-remDr$findElement(using = "css", value = "#GSA_GSASearch > font:nth-child(1) > input:nth-child(1)") # find the blank space where the search term is expected
a$clearElement() # delete whatever is in there
a$sendKeysToElement(list("Juan", key="enter")) # "write" Juan and press "Enter"

remDr$findElement("partial link text", "Siguiente")$clickElement() # click on "Siguiente" (next).

remDr$close() # close the web browser

Happy webscrapping

{% endhighlight %}


