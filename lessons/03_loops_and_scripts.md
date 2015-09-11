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
Welcome to the beauty and purpose of looping and automating with shell scripts! Read on!

Looping

Right now we've used `grep` and redirecton (`>`) to capture all the bad reads that are present in
one FASTQ file. And you've seen that you can use the grep pattern matching (glob) character
`*` to look at more than one file simultaneously. But using this option works on files
in batch, or all at once, without fine grain, one-by-one control. That's where
looping comes in.

Looping in bash is very similar to other languages. let's dive right in:

for filename in SRR098023.fastq SRR098023.fastq
do
echo $filename
done

So what does this do? Well, first we specify a list of files: SR** and SR**. Then, we execute a series of command between the do and done. But we execute these commands for each item in this list, and we store in a placeholder, filename, each item every we do the set of commands. In essesnce, we loop through the list, executing the commands inside the do / done, with the value of filename changing each time.

Now also notice, in the 'for' statement, we refernece, or set 'filename'. but in the loop, we explicitly use $filename. Why? Well, in the former, we're setting the value. In the latter, we're retrieving the vavlue.

Of course, filename is a great varialbe name. But it doesn't matter mcuh what we use:

for x in SRR098023.fastq SRR098023.fastq
do
echo $x
done
 
The only potential problem is that 'x' really doesn't mean much. In the long run, it's best to use a name that will help point out function, so your future self will understand what you are thinking now.

Looping over 2 files is great, but its rather inflexible. What can we do to grab a whole directory ofi files? Use the '*' notation:

for filename in SRR*.fastq
do
echo $filename
done

Now we've made our looping list more flexible, and it will work on any umber of files. So let's put that to work:

for filename in SRR*.fastq; do 
  echo $filename; 

  # grab all the bad read records
  grep -B1 -A2 NNNNNNNNNN $filename > $filename-badreads.fastq
done

In addition to the echo statemnt, we've included a comment -- lines that start with # -- and our grep statement that grabs bad reads and puts them in a new file. And we've specified a new filename with the variable $filename. So each iteration of the loop will grep a particular file and then output the bad reads to a new file that has that partiulcar filename as part of the new name.

pretty simple and cool, huh?

## Automating with Scripts

Now that we've learned how to use loops and variables, let's put this processing power to work. Imagine, if you will, a series of commands that would do the following for us each time we get a new data set:

use for loop to iterate over each FASTQ file. 
dump out bad reads
get the count of bad reads.
After all the FASTQ files are process, we generate one summary file of bad read coutns file.

You might not realize this, but this is something that you now know how to do. Let's get started... Move to our sample data directory and use nano to create our new script file...

cd cd ~/dc_sample_data
nano generate_bad_reads_summary.sh

And now we enter the command we want to execute. First we want to move into our untrimmed_fastq direcotry

cd untrimmed_fastq

And now we loop over all the FASTQs

for filename in SRR*.fastq

and we execute the commands for each loop:

do
  # tell us what file we're working on
  echo $filename
  
  # grab all the bad reads records
  grep -B1 -A2 NNNNNNNNNN $file > $file-badreads.fastq
  
We'll also get the counts of these reads and put that in a new file, using the grep -c 

  # grab the # of bad reads from our badreads file
  grep -cH NNNNNNNNNN $file-badreads.fastq > $file-badreads.counts
done

If you've noticed, we slipped a new grep flag -H in there. This flag will report when its found a match what the name of the filename is that we're curerntly woring on. This won't matter within each file, but it will matter when we generate the summary:

# grab all our bad read info to a summary file
cat *.counts > bad-reads.count.summary

And now, per our good form of caputing all of our work into a running summary log...

# and add this summary to our run log
cat bad-reads.count.summary >> ../runlog.txt

And in good form, at the end of the script, let's return to the directory we started from

# go back from whence we came
cd ..

Exit out of nano, and voila! You now have a sciptr you can use to assess the quality of all your new data asets. Your finished script should look liek this (complete with comments)


# work on untrimmed FASTQs
cd untrimmed_fastq

# work on each FASTQ file in our directory
for file in SRR09802*.fastq; do 
  echo $file; 

  # grab all the bad read records
  grep -B1 -A2 NNNNNNNNNN $file > $file-badreads.fastq

  # grab the # of bad reads from our bad reads file
  grep -cH NNNNNNNNNN $file-badreads.fastq > $file-badreads.counts
done

# grab all our bad read info to a summary file
cat *.counts > bad-reads.count.summary

# and add this summary to our run log
cat bad-reads.count.summary >> ../runlog.txt

# go back from whence we came
cd ..

To run this script, we simply enter the following command:

bash generate_bad_reads_summary.sh

Exercises:
* what would you need to do it you wanted...
* add to your script a section to generate the special SRARunTable files that we did towards the end of teh Searching Files lesson.



*** add cd command to start of searching and finding

