---
layout: post
title: "Python solutions: three chili"
date: 2018-07-10
excerpt: "Solution of Python exercises (in progress)"
tags: [python, solutions, exercises]
comments: true
project: true
---

Here are my proposed solutions to the excersies posted at [Practice Python](http://www.practicepython.org). In this post there will only be three chili exercises. 

##  [Beginner Python exercises: E.8](https://www.practicepython.org/exercise/2014/03/26/08-rock-paper-scissors.html)

Make a two-player Rock-Paper-Scissors game. (Hint: Ask for player plays (using input), compare them, print out a message of congratulations to the winner, and ask if the players want to start a new game)

{% highlight python %}
import getpass as gp

def rps():
    while True:
        P1=str.lower(gp.getpass("Player 1, rock (r), paper (p), scissors (s)? "))
        while P1 not in ["r", "p", "s"]:
            P1=str.lower(gp.getpass("Player 1, rock (r), paper (p), scissors (s)? "))
        P2=str.lower(gp.getpass("Player 2, rock (r), paper (p), scissors (s)? "))
        while P2 not in ["r", "p", "s"]:
            P2=str.lower(gp.getpass("Player 2, rock (r), paper (p), scissors (s)? "))
        if P1==P2:
            print("Draw")
        elif P1=="r" and P2=="s":
            print("Player 1 wins")
        elif P1=="p" and P2=="r":
            print("Player 1 wins")
        elif P1=="s" and P2=="p":
            print("Player 1 wins")
        else: 
            print("Player 2 wins")
        new_game=input("Want to play again? (y/n)")
        while new_game not in ["y", "n"]:
            new_game=input("Want to play again? (y/n)")
        if new_game=="n":
            break
{% endhighlight %}

gp.getpass allows the player to write their choice without letting the other player know what they chose. Not supported by Spyder IDE, though.

##  [Beginner Python exercises: E.9](https://www.practicepython.org/exercise/2014/04/02/09-guessing-game-one.html)

Generate a random number between 1 and 9 (including 1 and 9). Ask the user to guess the number, then tell them whether they guessed too low, too high, or exactly right

{% highlight python %}
from random import randint

def guessing_game():
	target_number=randint(1,9)
	while True:
		guess=int(input("Guess the number between 1 and 9: "))
		if target_number==guess:
			return "Congrats! you guessed the number"
		elif target_number>guess:
			print("too low, try again: ")
		else:
			print("too high, try again: ")
{% endhighlight %}

##  [Beginner Python exercises: E.11](https://www.practicepython.org/exercise/2014/04/16/11-check-primality-functions.html)

Ask the user for a number and determine whether the number is prime or not. (For those who have forgotten, a prime number is a number that has no divisors.). You can (and should!) use your answer to Exercise 4 to help you.

{% highlight python %}
def divisors(input_1):
    a=range(1, round((input_1)/2+1))
    divisors=[x for x in a if input_1 % x==0]
    divisors.append(input_1)
    return divisors

def check_prime():
	divid=divisors(int(input("Write natural number: ")))
	if len(divid)>2 or len(divid)==1:
		return "Number is not prime"
	else:
		return "Number is prime"
{% endhighlight %}

