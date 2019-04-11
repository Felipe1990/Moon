---
layout: post
title: "Let's order lunch"
date: 2019-04-08
excerpt: "Set-up to order loncheo using python"
tags: [Python, Selenium, webscraping, automation]
comments: true
---

[Loncheo](https://loncheo.co/) is a relatively new start-up in Bogotá, Colombia, which basically bring you lunch every day for an affordable price. It works like this: you review the available dishes for next day and then order the one you want by filling a Google form. I found this latter really inefficient since you end up repeting the same information every time you order(name, address, time, etc.). So, what if we use Python to do the boring stuff for us, without going full automatic.

What we will do:

1.	Create a conda enviroment with the required modules for this and other "web scraping" projects
2.	Write the function that will take care of ordering the food
3.	Highlight potential bugs

It is assumed that:

-	Conda is installed and available in your computer. I'm using Ubuntu 16.04LTS but every step should work just fine in other systems.
- You will configure [chromedriver](http://chromedriver.chromium.org/) if you want to use Chrome or [geckodriver](https://github.com/mozilla/geckodriver/releases) if you want to use Firefox, to correctly use Selenium. Documentation for them is broadly available for each system you may be working on; but if it just doesn't want to work, let me know with a comment I'll do my best to help you.

**Let's begin**

## 1. Create a conda enviroment

Creating a conda environment is fairly straighforward. It can be done by passing all the instruction in the command line, but I'll recommend you to create them using a .yml file. Why? Simply because that way it's easier to document the characteristics of your environments.

First you have to create a .yml file which will have all the requiered instructions; here is the one I used to create the enviroment I used to order my lunch:

{% highlight yml%}

name: web_scraping
dependencies:
  - python=3.6
  - scikit-learn
  - scipy
  - numpy
  - pandas
  - matplotlib
  - seaborn
  - plotly
  - statsmodels
  - beautifulsoup4
  - selenium
channels:
  - conda-forge

{% endhighlight %}

After you created it, go to the folder where the file is and run the following command. You could also use the complete address of the file, but by opening the terminal o command line in the folder where the file is you, just have to run the following as it is:

{% highlight bash%}
conda env create -f environment.yml
{% endhighlight %}

And you're done. For more information about conda environments and useful commands visit the [documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html). Environments are saved in the anaconda/envs folder.

Once you have your environment ready to go you have to activate it, for that you have run:

{% highlight bash%}
conda activate web_scraping
{% endhighlight %}

if you're using Windows, or 

{% highlight bash%}
source activate web_scraping
{% endhighlight %}

If you're using linux

## 2. Write the function that will take care of ordering the food

The logic behind the code (what it does) is the following:

- Open a session in Firefox and enter the Google form URL
- Ask the user (me) what he wants for lunch, there are either two or three options
- Find the places where the information should be and send the information to those places
- Ask the user if everythin is OK
- If everything is OK close the send the form and close the window; if something is not OK, leave the page open and wait for manual instructions

One challenge here is that sometimes there are two options and sometimes there are three, so the Xpath to the form boxes are not the same. To work around this I included two different methods to order my lunch and a wrapper -fill- that would try one (considering the structure when there are only two options in the menu - fill_out_of_two -) and if it fails try the one for where there are three options in the menu -fill_out_of_three-.

This is the complete code that define the class where the methods live.

{% highlight python %}
class fill_loncheo:
        
    def __init__(self):
        self.browser = webdriver.Firefox()
        self.browser.get('https://docs.google.com/forms/d/e/1FAIpQLSfE1D_wCsn_65Ic2WrQ1Rxp_PxqOWx56_cYkDZ-u7woEKLHaA/viewform')
        
    def fill_out_of_two(self, opcion):
        info_keys = [
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[2]/div/div[2]/div/div[1]/div/div[1]/input', 'My name'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[3]/div/div[2]/div/div[1]/div/div[1]/input', 'My cellphone'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[5]/div/div[2]/div/div[1]/div/div[1]/input', 'My address'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[11]/div/div[2]/div[1]/div[2]/textarea', 'Additional comments')
            ] 

        for location, key in info_keys:
            forma = self.browser.find_element_by_xpath(location)
            forma.clear()
            forma.send_keys(key)
        # zona norte
        self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[4]/div/div[2]/div/content/div/div[3]/label/div/div[1]/div[3]').click()

        # hora preferida de llegada (antes de las 12)
        self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[7]/div/div[2]/div/content/div/div[1]/label/div/div[1]/div[3]').click()

        # opcion 1
        if opcion == 1:
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[2]/content/div[3]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[3]/content/div[2]/div/div[3]').click()
        else:
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[2]/content/div[2]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[3]/content/div[3]/div/div[3]').click()

        review = input("¿Todo en orden? (Y/N) ")

        if review == "Y":
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[3]/div[1]/div/div/content').click()
            self.browser.close()
        else:
            print("Esperando cambios para envio manual")
            
    def fill_out_of_three(self, opcion):
        info_keys = [
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[2]/div/div[2]/div/div[1]/div/div[1]/input', 'My name'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[3]/div/div[2]/div/div[1]/div/div[1]/input', 'My cellphone'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[5]/div/div[2]/div/div[1]/div/div[1]/input', 'My address'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[11]/div/div[2]/div[1]/div[2]/textarea', 'Additional comments')
            ] 

        for location, key in info_keys:
            forma = self.browser.find_element_by_xpath(location)
            forma.clear()
            forma.send_keys(key)
        # zona norte
        self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[4]/div/div[2]/div/content/div/div[3]/label/div/div[1]/div[3]').click()

        # hora preferida de llegada (antes de las 12)
        self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[7]/div/div[2]/div/content/div/div[1]/label/div/div[1]/div[3]').click()

        # opcion 1
        if opcion == 1:
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[2]/content/div[3]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[3]/content/div[2]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[4]/content/div[2]/div/div[3]').click()
        elif opcion == 2:
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[2]/content/div[2]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[3]/content/div[3]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[4]/content/div[2]/div/div[3]').click()
        else:
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[2]/content/div[2]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[3]/content/div[2]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[4]/content/div[3]/div/div[3]').click()
                        
        review = input("¿Todo en orden? (Y/N) ")

        if review == "Y":
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[3]/div[1]/div/div/content').click()
            self.browser.close()
        else:
            print("Esperando cambios para envio manual")


    def fill(self, opcion=1):
        """llena el cuestionario de loncheo"""
        try:
            self.fill_out_of_two(opcion)
        except:
            self.fill_out_of_three(opcion) 

{% endhighlight %}

Once the class is defined I run create an instance of the class and run the method fill

{% highlight python %}

pedido = fill_loncheo()
pedido.fill(opcion = int(input("¿Cuál menu quieres hoy?: ")))

{% endhighlight %}

The complete .py file that I run to order my lunch just integrate both pieces of code in one file and include the call to the selenium module:

{% highlight python %}
from selenium import webdriver

class fill_loncheo:
        
    def __init__(self):
        self.browser = webdriver.Firefox()
        self.browser.get('https://docs.google.com/forms/d/e/1FAIpQLSfE1D_wCsn_65Ic2WrQ1Rxp_PxqOWx56_cYkDZ-u7woEKLHaA/viewform')
        
    def fill_out_of_two(self, opcion):
        info_keys = [
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[2]/div/div[2]/div/div[1]/div/div[1]/input', 'My name'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[3]/div/div[2]/div/div[1]/div/div[1]/input', 'My cellphone'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[5]/div/div[2]/div/div[1]/div/div[1]/input', 'My address'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[11]/div/div[2]/div[1]/div[2]/textarea', 'Additional comments')
            ] 

        for location, key in info_keys:
            forma = self.browser.find_element_by_xpath(location)
            forma.clear()
            forma.send_keys(key)
        # zona norte
        self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[4]/div/div[2]/div/content/div/div[3]/label/div/div[1]/div[3]').click()

        # hora preferida de llegada (antes de las 12)
        self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[7]/div/div[2]/div/content/div/div[1]/label/div/div[1]/div[3]').click()

        # opcion 1
        if opcion == 1:
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[2]/content/div[3]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[3]/content/div[2]/div/div[3]').click()
        else:
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[2]/content/div[2]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[3]/content/div[3]/div/div[3]').click()

        review = input("¿Todo en orden? (Y/N) ")

        if review == "Y":
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[3]/div[1]/div/div/content').click()
            self.browser.close()
        else:
            print("Esperando cambios para envio manual")
            
    def fill_out_of_three(self, opcion):
        info_keys = [
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[2]/div/div[2]/div/div[1]/div/div[1]/input', 'My name'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[3]/div/div[2]/div/div[1]/div/div[1]/input', 'My cellphone'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[5]/div/div[2]/div/div[1]/div/div[1]/input', 'My address'),
            ('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[11]/div/div[2]/div[1]/div[2]/textarea', 'Additional comments')
            ] 

        for location, key in info_keys:
            forma = self.browser.find_element_by_xpath(location)
            forma.clear()
            forma.send_keys(key)
        # zona norte
        self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[4]/div/div[2]/div/content/div/div[3]/label/div/div[1]/div[3]').click()

        # hora preferida de llegada (antes de las 12)
        self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[7]/div/div[2]/div/content/div/div[1]/label/div/div[1]/div[3]').click()

        # opcion 1
        if opcion == 1:
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[2]/content/div[3]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[3]/content/div[2]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[4]/content/div[2]/div/div[3]').click()
        elif opcion == 2:
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[2]/content/div[2]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[3]/content/div[3]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[4]/content/div[2]/div/div[3]').click()
        else:
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[2]/content/div[2]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[3]/content/div[2]/div/div[3]').click()
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[2]/div[8]/div/div[2]/div/div[1]/div/div[4]/content/div[3]/div/div[3]').click()
                        
        review = input("¿Todo en orden? (Y/N) ")

        if review == "Y":
            self.browser.find_element_by_xpath('//*[@id="mG61Hd"]/div/div[2]/div[3]/div[1]/div/div/content').click()
            self.browser.close()
        else:
            print("Esperando cambios para envio manual")


    def fill(self, opcion=1):
        """llena el cuestionario de loncheo"""
        try:
            self.fill_out_of_two(opcion)
        except:
            self.fill_out_of_three(opcion) 

pedido = fill_loncheo()
pedido.fill(opcion = int(input("¿Cuál menu quieres hoy?: ")))
{% endhighlight %}


And we you run it, it looks like this:


<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_fill_loncheo/photo_1.png"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_fill_loncheo/photo_1.png" width="400" height="300" align="middle"></a>
</figure>


<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_fill_loncheo/photo_2.png"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_fill_loncheo/photo_2.png" width="400" height="300" align="middle"></a>
</figure>


<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_fill_loncheo/photo_3.png"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_fill_loncheo/photo_3.png" width="400" height="300" align="middle"></a>
</figure>

## 3. Highlight potential bugs

- Dynamic references to the HTML tree maybe form efficient or at least more flexible than fixed Xpaths. If the page changes the code won't work
- There is no control to select a non-existing option, e.g. if you select the menu 4 or 999 it will still get the second option if there are two or the third one if there are three


Happy lunching