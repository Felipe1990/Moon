---
layout: post
title: "Python solutions: two chili"
date: 2018-07-09
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
        P1=str.lower(gp.getpass("Player 1, ¿rock, paper, scissors? "))
        while P1 not in ["rock", "paper", "scissors"]:
            P1=str.lower(gp.getpass("Player 1, ¿rock, paper, scissors? "))
        P2=str.lower(gp.getpass("Player 2, ¿rock, paper, scissors? "))
        while P2 not in ["rock", "paper", "scissors"]:
            P2=str.lower(gp.getpass("Player 2, ¿rock, paper, scissors? "))
        if P1==P2:
            print("Draw")
        elif P1=="rock" and P2=="scissors":
            print("Player 1 wins")
        elif P1=="paper" and P2=="rock":
            print("Player 1 wins")
        elif P1=="scissors" and P2=="paper":
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