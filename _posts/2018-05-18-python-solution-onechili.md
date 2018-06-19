---
layout: post
title: "Python solutions: one chili"
date: 2018-05-17
excerpt: "Solution of Python exercises (in progress)"
tags: [python, solutions, exercises]
comments: true
project: true
---

Here are my proposed solutions to the excersies posted at [Practice Python](http://www.practicepython.org). In this post there will only be one chili exercises. 

Sometines I'll try to explain what I did, not because is too complex but because it helps me understand it better.

##  [Beginner Python exercises: E.1](http://www.practicepython.org/exercise/2014/01/29/01-character-input.html)

Create a program that asks the user to enter their name and their age. Print out a message addressed to them that tells them the year that they will turn 100 years old.

{% highlight python%} 

def age():
    name=input("What is your name? ")
    age=input("Hi {}, How old are you (in years)? ".format(name))
    try:
        age=float(age)
    except:
        while not type(age)==float:
            age=input("Please write a number. How old are you (in years)? ".format(name))
            try:
                age=float(age)
                break
            except:
                next
    diff_years=100-age
    print('{}, in {} you\'ll be 100 years old.'.format(name,diff_years))

{% endhighlight %}

Extras:
	*	Add on to the previous program by asking the user for another number and printing out that many copies of the previous message. (Hint: order of operations exists in Python)
    *	Print out that many copies of the previous message on separate lines. (Hint: the string "\n is the same as pressing the ENTER button)

	{% highlight python%} 

def age():
    number=input("Write a number ")
    name=input("What is your name? ")
    age=input("Hi {}, How old are you (in years)? ".format(name))
    number=int(number)
    try:
        age=float(age)
    except:
        while not type(age)==float:
            age=input("Please write a number. How old are you (in years)? ".format(name))
            try:
                age=float(age)
                break
            except:
                next
    diff_years=100-age
    text=name+", in "+str(diff_years)+"you\'ll be 100 years old."+"\n"
    print(text*number)
	
{% endhighlight %}