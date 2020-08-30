---
title: Hadoop - What's the point?
tags:
  - Hadoop
id: '1785'
categories:
  - - Big Data
  - - Blog
  - - Business Intelligence
  - - Linux
comments: false
date: 2014-02-17 09:14:11
---

[![Small vs Large](http://edpflager.com/wp-content/uploads/2014/02/planes-300x110.jpg)](http://edpflager.com/wp-content/uploads/2014/02/planes.jpg)Its often said when you work closely with something, you sometimes lose track of the big picture (you can't see the forest for the trees). But I also find that a lot of times, people who aren't familiar with something focus too much on the details and miss the big picture. In discussing my Hadoop cluster project with people, they don't understand the point of it. "Why setup four PCs to work on something. Can't you just get one really powerful PC and do the work with that?" they ask. **DIVIDE THE WORK** The innovation  that Hadoop provides is in HOW it does the work. For many years, in IT as well as other areas, the solution when faced with bigger and bigger workloads has been to get a bigger, faster, stronger tool. Here are some examples:
<!-- more -->
*   Have to cut your home's lawn? Use a lawnmower. Have to cut the lawn for a large campus? Get a riding mower.
*   Want to make a pizza? Use your oven. Want to open a pizzeria? Better buy a large pizza oven.
*   Have to process payroll for a small store? You use a personal computer, and maybe use Excel or Quicken to cut the checks. Have to process payroll for manufacturing plant? Use a server and a dedicated application to handle it.

The downside to all of these approaches is two-fold: the cost involved with scaling up to larger tools is larger than the spending on the smaller one, and you don't always need the capacity of the larger tool.  Something else in common with these examples is that you can also do the work by dividing it up.

*   Cutting the lawn for a large campus could be done using several small lawnmowers with someone to push each of them.
*   You could make a large number of pizzas using several small ovens, rather than one large one.
*   Same thing with payroll for a large plant. Divide it up and spread it among several or more staff members to get it done.

**CONQUER THE CHALLENGE** The challenge when dealing with BIG DATA processing is dividing it and then assembling the workflows back into one complete one. Hadoop was designed to meet these challenges. Rather than try to process incredibly large data sets on a very large powerful computer system, it uses a cluster of smaller commodity systems, divides the workflow across them, and then reassembles the output into a complete result. Because you are using less powerful PCs (commodity ones), the hardware costs are less. In my case, I spent about $400 for the four PCs that make up my cluster. In order to get a single more powerful PC, I would have had to spend at least that, and probably more. A lot more. Another benefit of this approach is maintenance. If you have one powerful PC, its not any less likely to break down than any other PC. But if it does, you are effectively at a stand still for doing any work. But in the case of my cluster, if one of the machines breaks down, I still can work with the remaining three. Sure it will take longer, but that trade off is small when weighed against the absolute downtime of a bigger machine being down. Finally, lets think about our examples from above in terms of resource allocation:

*   Don't need all of the lawn movers on one job? Use some of them for another job.
*   Don't need to make all the pizzas you would normally on a given day? Use the spare ovens for something else, or shut them down to save on costs.
*   One shift of  your plant has been idled for a couple of weeks so you don't need all of your payroll people to cut checks? Assign them to do other tasks, like W2's.

In the same way, if you don't need to use all of your processing power in a Hadoop cluster for a specific job, use that excess capacity for something else or shut them down to save on costs. Its an innovative concept, although in hindsight its really pretty self-evident.