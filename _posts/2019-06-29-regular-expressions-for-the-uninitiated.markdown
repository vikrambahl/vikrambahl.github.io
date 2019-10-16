---
layout: post
title:  "Regular Expression for the uninitiated"
date:   2019-06-29 9:30:00 +1000
categories: programming
excerpt_separator: <!--more-->
---

Even though I had been using Regular Expressions since my early programming days, it was largely relegated to googling for the right expressions and then using it. It wasn't until I wrote my first scraping program that I had to really start designing and defining my own regular expressions. These are my notes as I learnt the fabulous art of designing regular expressions.

<!--more-->

## Why Regular Expressions

If you are here, I am quite sure you'd have figured out why you need regular expressions. I mean just the fact that you can understand the term Regular Expression means somebody (or Google) pointed you to here. 

However, for the few of you who may have landed here without the awareness of what Regular Expressions are, just follow on from the problem-statement specified below and you'll be able to grasp their purpose.

### What problem are we trying to solve

I scrape through home listings on real-estate websites. I need to extract the total land area of the house from the description provided in the listing. It could be there in the description or it may not be there. The land area could have been described in the following ways

- some digits followed by the words square-metre 
- some digits followed by the words sqm
- some digits followed by the words m<sup>2</sup>

So our requirement is we need to extract this information from the text-description. 


## The syntax and vocabulary 

If I can put a finger on why I never really cared to learn Regular Expressions was because it required me to learn the entire syntax before I could get started. Or atleast that's what I thought I needed. I realised later that I could have just picked up one metacharacter at a time and that would have still worked out all right. I shall therefore take you through the set of characters one by one and as we progress.


### ^ - Starts with 

The `^` symbol denotes the starts-with configuration. If we need to find a text that starts with 'A' we will simply create the regular expression as `^A`. 

### $ - end-of-line

The `$` denotes the end-of-line. 

### [] - Any Single Character

The `[]` denotes a single character from the characters defined within the `[]` brackets. 

As is required in our example, we need to find out the square meter. This means our selection should start with a digit. In order to achieve that we use the single character brackets `[1-9]`. We could also use the `[AEIOU]` to denote the vowels. 

### * - Repeated Zero or more times

The `*` denotes a repetition of the character. 

`[A]*` would translate into text starting with A and having one or more As after that initial A. Similarly `[1-9]*` would repeated set of digits (but not having zero in them anywhere). 

### . - Any character

The `.` matches any character including both whitespace and non-whitespace characters. 

So if I wish to find out whether we have digits in our description it would be `^.*[1-9][0-9]*.*`. This suggests the text can start with any number of repeated characters including whitespaces but must have a number and then followed by any number of characters. 

### \s - Whitespace character

The `\s` matches a whitespace character. 

If we are looking for the text 'sqm' we need to define the ensure that we find sqm followed by a 'blank'. The way to achieve that would be through `^sqm\s*`.

### \S - Non Whitespace character

The `\S` matches a non-whitespace character. 

## Regular Expressions in Python

Python uses the `re` library to work with Regular Expressions. Therefore if we intend to use Regular Expressions, we'll need to `import re` into our application

### re.search() Vs re.findall()

The `search()` function is used to find for patterns that match a certain regex. It simply matches and lets you know whether the string exists in the text provided. The `findall()` function actually extracts that piece of text as well. So `findall()` would extract these texts and provide them to you to work with. 





