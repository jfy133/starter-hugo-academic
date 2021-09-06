---
title: "How-to: cancel a MrBayes run early"
summary: "How to cancel MrBayes run early"
date: 2017-05-28
tags:
  - mrbayes
  - phylogenetics
---

During my M.Sc. thesis I started to use the Bayesian phylogenetic MrBayes on clusters that do not make it feasible to keep open  a terminal window. This meant I had to use ‘[batch mode](http://mrbayes.sourceforge.net/wiki/index.php/FAQ_3.2)’, meaning I could only follow the status of the MrBayes run through the log files.

One issue I found when using MrBayes is that it is quite slow, so when I found my MCMC trace reached stability very early on in the chain, I wanted to stop the chain early, requiring me to force cancel the run. This meant that the data was not loaded in the program and I could not run the ‘sump’ or ‘sumt’ commands in MrBayes.

I googled how to do cancel a MrBayes run early and found a couple of links (such as [here](https://www.biostars.org/p/139267/) and [here](https://sourceforge.net/p/mrbayes/mailman/mrbayes-users/thread/b392349a0811081442j4480854fqc462424a6381dd@mail.gmail.com/)). Although easy to do, neither explanations were particularly explicit in how finish the chain early and continue the analysis.

This post will give a quick step by step guide with additional notes on how to cancel a run early and to finish the analyses.

1) Submit your batch script to your cluster

2) Check your log file, and once you are at a stage you wish to stop the run, cancel the run.

3) To the end of every ‘.t’ file, in a text editor or with the command line, append to a new line without an indentation the following:

![Finishing the block](/images/2017-05-28_1.png)

**Note**: If you want to ‘round off’ your number of steps in your chain, make sure you have the same numbers of entries in both .t and .p files. E.g. if you had already reached stability by 40,000,000 steps but you finished 40,000,125 - in all files delete lines below 40,000,000 and add ‘end;’ in the tree files.

4) Now open MrBayes again (typically with mb) and re-load the original nexus file you selected in your batch file.

Note: If you selected a different name for the output of the run in your batch file, you may have to re-name the input nexus file to that of the output files (so it is the same as the .mcmc, .t, .p files etc.)

5) Now, as you would normally, run the ‘sump’ and ‘sumt’ commands to summarise your run and generate your consensus trees.

Hopefully this will allow you to get your results much faster rather letting the run to finish (in my case saving around 3 weeks)!
