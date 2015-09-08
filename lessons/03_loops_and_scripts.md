--
layout: page
title: "The Shell"
comments: true
date: 2015-09-08
---

# The Shell: Loops & Scripts

Author: Bob Freeman

Approximate time: 30 minutes

## Objectives & Accomplishments

* Learn how to operate on multiple files 
* Capture previous commands into a script to re-run later
* Abstract your script for flexibility
* Write a series of scripts, that are increasingly more flexible, to automate your workflow

Now that you've been using quite a number of commands to interrogate your data, 
wouldn't it be great if you could generate a report for your data and the special
SRARunTable files as a part of the interrogation process? Even better, could we do this
for each set of data that comes in, without having to manually re-type the commands?
Welcome to the beauty and purpose of looping and shell scripts! Read on!

Looping

Right now we've used grep and redirecton to look at the bad reads that are present in
one file. And you've seen that you can use the grep pattern matting (glob) character
* to look at more than one file simultaneously. But using this option works on files
in batch, or all at once, without fine grain, one-by-one control. That's where
looping comes in.

Let's say we wanted to 

# plan

use for loop to iterate over each FASTQ file. 
dump out bad reads
get the count of bad reads.

Then when thats all done, generate our special SRARun Table files


If we wanted to 
