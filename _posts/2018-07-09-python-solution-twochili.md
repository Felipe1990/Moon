---
layout: post
title: "Python solutions: two chili"
date: 2018-07-09
excerpt: "Solution of Python exercises (in progress)"
tags: [python, solutions, exercises]
comments: true
project: true
---

Here are my proposed solutions to the excersies posted at [Practice Python](http://www.practicepython.org). In this post there will only be two chili exercises. 

##  [Beginner Python exercises: E.3](http://www.practicepython.org/exercise/2014/02/15/03-list-less-than-ten.html)

Take a list, say for example this one:

{% highlight python %} 
a = [1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
{% endhighlight %}

and write a program that prints out all the elements of the list that are less than 5.

{% highlight python %}
[x for x in a if x<5]
{% endhighlight %}

An additional solution using lambdas:

{% highlight python %}
list(filter(lambda x: x<5, a))
{% endhighlight %}

Extras:

*	Ask the user for a number and return a list that contains only elements from the original list a that are smaller than that number given by the user.

{% highlight python %}
def ask_number():
    def ask_number(list_a):
    limit=int(input("Please write an integer: "))
    return [x for x in a if x<limit]
{% endhighlight %}

##  [Beginner Python exercises: E.4](https://www.practicepython.org/exercise/2014/02/26/04-divisors.html)

Create a program that asks the user for a number and then prints out a list of all the divisors of that number. (If you don’t know what a divisor is, it is a number that divides evenly into another number. For example, 13 is a divisor of 26 because 26 / 13 has no remainder.)

{% highlight python %}
def divisors():
    input_1=int(input("Write an integer: "))
    a=range(1, round((input_1+1)/2))
    divisors=[x for x in a if input_1 % x==0]
    divisors.append(input_1)
    print("Divisor of {} are: {}".format(input_1, divisors))
{% endhighlight %}

##  [Beginner Python exercises: E.5](https://www.practicepython.org/exercise/2014/03/05/05-list-overlap.html)

Take two lists, say for example these two:

{% highlight python %}
  a = [1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
  b = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
{% endhighlight %}

and write a program that returns a list that contains only the elements that are common between the lists (without duplicates). Make sure your program works on two lists of different sizes.

An initial approach could be to write a loop that runs through the a list and checks if the element tested is part of the list b. One problem it arises is that one could result with a list that has two identical elements:

{% highlight python %}
solution_1=[]
for i in a:
    if i in b:
        solution_1.append(i)

[1, 1, 2, 3, 5, 8, 13]
{% endhighlight %}

One way to solve it is to convert the list into a set and then back a to a list (it could be inefficient).

{% highlight python %}
c1=list(set(c1))
[1, 2, 3, 5, 8, 13] 
{% endhighlight %}
 
Another way could be adding additional conditions to the loop:

{% highlight python %}
c2=[]
for i in a:
    if i in b and i not in c2:
        c2.append(i)
{% endhighlight %}

**One liners:**

{% highlight python %}
c3=list(set(a).intersection(b))
c4=list(set(a) & set(b))
{% endhighlight %}

Extras:

*   Randomly generate two lists to test this
*   Write this in one line of Python

{% highlight python %}
import random as rdm

max1=rdm.randint(15,60)
max2=rdm.randint(15,60)
n1=rdm.randint(0,15)
n2=rdm.randint(0,15)

a=rdm.sample(range(0, max1), n1)
b=rdm.sample(range(0, max2), n2)

c=list(set(a).intersection(b))

print(c)
{% endhighlight %}

##  [Beginner Python exercises: E.6](https://www.practicepython.org/exercise/2014/03/12/06-string-lists.html)

Ask the user for a string and print out whether this string is a palindrome or not. (A palindrome is a string that reads the same forwards and backwards.)

My initial (and inneficient) answer:

{% highlight python %}
def palindrome():
    word=input("Enter a word: ")
    lenght=len(word)
    wordlower=str.lower(word)
    check=[]
    if lenght % 2==0:
        for i in range(0,int(lenght/2)) :
            check.append(wordlower[i]==wordlower[(lenght-i-1)])
        if all(check):
            print("The word \"{}\" is a palindrome ".format(word))
        else:
            print("The word \"{}\" is not a palindrome ".format(word))
    elif lenght % 2==1:
        for i in range(0,int(lenght/2-0.5)) :
            check.append(wordlower[i]==wordlower[(lenght-i-1)])
        if all(check):
            print("The word \"{}\" is a palindrome ".format(word))
        else:
            print("The word \"{}\" is not a palindrome ".format(word))
{% endhighlight %}

But I find the author's proposed solution more elegant. Note the use of [::-1] to reverse the string thus taking advantage that in Python letters in string chains can be access as they were lists.

{% highlight python %}
wrd=input("Please enter a word: ")
rvs=wrd[::-1]
if wrd == rvs:
    print("This word is a palindrome")
else:
    print("This word is not a palindrome")
{% endhighlight %}

I would just add:

{% highlight python %}
wrd=str.lower(input("Please enter a word: "))
{% endhighlight %}

in the first line to avoid problems with capitlized words.

{% highlight python %}
wrd=str.lower(input("Please enter a word: "))
rvs=wrd[::-1]
if wrd == rvs:
    print("This word is a palindrome")
else:
    print("This word is not a palindrome")
{% endhighlight %}

##  [Beginner Python exercises: E.7](https://www.practicepython.org/exercise/2014/03/19/07-list-comprehensions.html)

Let’s say I give you a list saved in a variable: *a = [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]*. Write one line of Python that takes this list a and makes a new list that has only the even elements of this list in it.

{% highlight python %}
new_list=[x for x in a if x%2=0]
{% endhighlight %}

##  [Beginner Python exercises: E.13](https://www.practicepython.org/exercise/2014/04/30/13-fibonacci.html)

Write a program that asks the user how many Fibonnaci numbers to generate and then generates them. Take this opportunity to think about how you can use functions. Make sure to ask the user to enter the number of numbers in the sequence to generate.(Hint: The Fibonnaci seqence is a sequence of numbers where the next number in the sequence is the sum of the previous two numbers in the sequence. The sequence looks like this: 1, 1, 2, 3, 5, 8, 13, …)

{% highlight python %}
def fibonacci():
    a=input("Write an integer greater than 2: ")
    if a.isnumeric() and int(a)>2:
        b=list(range(1,int(a)+1-2))
        fibon=[1,1]
        for i in b:
            fibon.append(fibon[i-1]+fibon[i])
        return fibon
    else:
        print("{} is not a integer or is not greater than 2".format(a))
{% endhighlight %}

##  [Beginner Python exercises: E.34](https://www.practicepython.org/exercise/2017/02/06/34-birthday-json.html)

In the previous exercise we created a dictionary of famous scientists’ birthdays. In this exercise, modify your program from Part 1 to load the birthday dictionary from a JSON file on disk, rather than having the dictionary defined in the program.

Bonus: Ask the user for another scientist’s name and birthday to add to the dictionary, and update the JSON file you have on disk with the scientist’s name. If you run the program multiple times and keep adding new names, your JSON file should keep getting bigger and bigger.

Let's create the initial json file with the names and dates of some people.

{% highlight python %}
import json

known_dates = [
    {"name": "Michelle",
     "birthday":  "07th August"},
    {"name": "Felipe",
     "birthday":  "17th November"},
    {"name": "Mario",
     "birthday":  "28th September"},
    {"name": "Claudia",
     "birthday":  "10th February"},
    {"name": "Manuel",
     "birthday": "27th December"},
    {"name": "Henry",
     "birthday": "31th December"}
]

with open("info.json", "w") as f:
    json.dump(known_dates, f)
{% endhighlight %}

Now the function that allows the user to add new member to the list. Notice that no checking is included to control whether the name is already in the json file.

{% highlight python %}

def add_new_people():
    with open("info.json", "r") as f:
        birthdays = json.load(f)

    name = input("What's the name of the person which birthday you want to add?")
    birthday = input("what's her/his birthday? (dayth Month)")

    birthdays.append({"name": name, "birthday": birthday})

    with open("info.json", "w") as f:
        json.dump(birthdays, f)

{% endhighlight %}

Finally, the function that load the json file and looks for the birthday.

{% highlight python %}
def json_query_birthday():
    # give back birthday of selected people

    with open("info.json", "r") as f:
        known_dates = json.load(f)

    print("Welcome to the birthday dictionary. We know the birthdays of: ")
    for k in [x["name"] for x in known_dates]:
        print("- {0}".format(k))

    asked_name = input("Who's birthday do you want to know?")

    if asked_name in [x["name"] for x in known_dates]:
        print("{0}'s birthday is on {1}".format(asked_name,
                                                [x["birthday"] for x in known_dates if x["name"] == asked_name][0]))
    else:
        print("Sorry we don't know {0}'s birthday".format(asked_name))
{% endhighlight %}

##  [Beginner Python exercises: E.34](https://www.practicepython.org/exercise/2017/02/28/35-birthday-months.html)

In the previous exercise we saved information about famous scientists’ names and birthdays to disk. In this exercise, load that JSON file from disk, extract the months of all the birthdays, and count how many scientists have a birthday in each month.

First step is to load the json file and extract the list of all the months (no matter duplicates)

{% highlight python %}
# load data
with open("info.json", "r") as f:
    known_dates = json.load(f)

months = [x["birthday"].split(" ")[1] for x in known_dates]

{% endhighlight %}

Firts solution using a loop:

{% highlight python %}
count_months_manual = {}
all_months = ["January", "February", "March", "April",
              "May", "June", "July", "August",
              "September", "October", "November",
              "December"]

for month_i in all_months:
    count_months_manual[month_i] = sum([x == month_i for x in months])
{% endhighlight %}

Second solution using the collections module

{% highlight python %}
count_months_collections = dict(coll.Counter(months))
{% endhighlight %}
