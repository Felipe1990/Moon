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

##  [Beginner Python exercises: E.1](http://www.practicepython.org/exercise/2014/01/29/01-character-input.html)

Create a program that asks the user to enter their name and their age. Print out a message addressed to them that tells them the year that they will turn 100 years old.

{% highlight python %} 
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

{% highlight python %} 
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

##  [Beginner Python exercises: E.2](https://www.practicepython.org/exercise/2014/02/05/02-odd-or-even.html)

Ask the user for a number. Depending on whether the number is even or odd, print out an appropriate message to the user.

{% highlight python %}
def even():
    num=int(input("Please write ann integer: ",))
    if num % 2 == 0:
        print("That's a even number")
    else :
        print("That's an odd number")
{% endhighlight %}

Hopefully the user will follow the instructions.

Extras:

*	If the number is a multiple of 4, print out a different message
*	Ask the user for two numbers: one number to check (call it num) and one number to divide by (check). If check divides evenly into num, tell that to the user. If not, print a different appropriate message.
	
{% highlight python %}
def even_extra_1():
    num=int(input("Please write an integer: ",))
    if num % 4 == 0:
        print("Your number a multiple of 4")
    elif num % 2 ==0:
        print("That's a even number")
    else :
        print("That's an odd number")


def even_extra_2():
    num=int(input("Please write an integer to be verified: ",))
    check=int(input("Please write an integer (possible multiple): "))
    if num % check == 0:
        print("{} is multiple of {}".format(num, check))
    else :
        print("{} is not multiple of {}".format(num, check))
{% endhighlight %}

##  [Beginner Python exercises: E.12](https://www.practicepython.org/exercise/2014/04/25/12-list-ends.html)

Write a program that takes a list of numbers (for example, a = [5, 10, 15, 20, 25]) and makes a new list of only the first and last elements of the given list. For practice, write this code inside a function.

{% highlight python %}
def E_3(x):
    print([x[0], x[len(x)-1]])
{% endhighlight %}

A less efficient alternative could be:

{% highlight python %}
def E_3(x):
    element_1=x[0]
    x.reverse()
    print([element_1, x[0]])
{% endhighlight %}

##  [Beginner Python exercises: E.20](https://www.practicepython.org/exercise/2014/11/11/20-element-search.html)

Write a function that takes an ordered list of numbers (a list where the elements are in order from smallest to largest) and another number. The function decides whether or not the given number is inside the list and returns (then prints) an appropriate boolean.

{% highlight python %}
def search(a_list, x):
    return x in a_list
{% endhighlight %}

That felt a little like cheating since "in" is doing all the work.

Extra:

*	Use binary search.

Binary search is a search algorithm that finds the position of a target value within a sorted array.[4][5] Binary search compares the target value to the middle element of the array; if they are unequal, the half in which the target cannot lie is eliminated and the search continues on the remaining half until the target value is found. If the search ends with the remaining half being empty, the target is not in the array. [^1]

{% highlight python %}
def binary_search(a_list, x):
    
    inner_list=a_list
    
    while True:
    
        position=round(len(inner_list)/2+0.1)-1
    
        if len(inner_list)==1:
            return x==inner_list[0]
        elif len(inner_list)==2:
            return (x==inner_list[0] or x==inner_list[1])
        else:
            if x==inner_list[position]:
                return True
            elif x>inner_list[position]:
                inner_list=inner_list[position:]
        
            elif x<inner_list[position]:
                inner_list=inner_list[:position]
{% endhighlight %}

While the sintax suggests that the loop will run forever return stops the function (as well as the loop) so no break is necessary.

[^1]: [Wikipedia: binary search](https://en.wikipedia.org/wiki/Binary_search_algorithm)

## [Beginner Python exercises: E.33](https://www.practicepython.org/exercise/2017/01/24/33-birthday-dictionaries.html)

For this exercise, we will keep track of when our friend’s birthdays are, and be able to find that information based on their name. Create a dictionary (in your file) of names and birthdays. When you run your program it should ask the user to enter a name, and return the birthday of that person back to them.

{% highlight python %}
def query_birthday():
    # give back birthdate of selected people

    known_dates = {
        "Michelle": "07th August",
        "Felipe": "17th November",
        "Mario": "28th September",
        "Claudia": "10th February",
        "Manuel": "27th December",
        "Henry": "31th December"
    }
    print("Welcome to the birthday dictionary. We know the birthdays of: ")
    for k in iter(known_dates.keys()):
        print("- {0}".format(k))

    asked_name = input("Who's birthday do you want to know?")

    if asked_name in known_dates:
        print("{0}'s birthday is on {1}".format(asked_name, known_dates[asked_name]))
    else:
        print("Sorry we don't know {0}'s birthday".format(asked_name))
{% endhighlight %}