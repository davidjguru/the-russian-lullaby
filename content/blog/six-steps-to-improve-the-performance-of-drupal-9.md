---
title: "Six Steps to Improve the Performance of Drupal 9"
date: 2020-11-25
draft: true

# post thumb
image: "images/post/davidjguru_drupal_9_improve_performance_main.jpg"

# meta description
description: "Improve the performance of your Drupal Installation"

# taxonomies
categories: 
  - "Development"  
tags:
  - "Backend"
  - "Contrib Modules"
  - "Environments"
  - "PHP"

# post type
type: "post"
---


It is said that a small change or the sum of many of them can end up generating great transformations. This is valid as a legend to address improvements or to analyze the meaning of chaos, but now I just want to use it to frame the meaning of today's article. Well, I was speaking about perfomance with some of my colleagues and then I thought of some cases from diverse projects I had worked on and decided to put together some notes about perfomance always in a PHP / Drupal. I have gathered various experiences, conclusions and notes and tried to articulate them in an orderly way. You might be interested if you are concerned about the slowness of your Drupal installation. One part to find out how some issues work at the performance level, other parts to improve it, so yes...today I'll write about this topic. There we go!  

--------------------------------------------------------------------------------------
**Picture from Unsplash, user [Benjamin Child](https://unsplash.com/@bchild311).**

  
---------------------------------------------------------------------------------

**Table of Contents**  
<!-- TOC -->  
[Introduction](#introduction)  
[Step 1 - Select your Tooling](#step-1-select-your-tooling)  
* [Prompt](#prompt)  
* [Browser Extensions](#browser-extensions)  
* [Drupal Modules](#drupal-modules)  

[Step 2 - Take an updated photo](#step-2-take-an-update-photo)  
* [Make your own plan](#make-your-own-plan)  
* [Get a snapshot](#get-a-snapshot)  
* [Goals and Benchmarking ](#goals-and-benchmarking)  

[Step 3 - Check out PHP](#step-3-check-out-php)  
* [Understanging PHP](#understanding-php)
* [OPCache](#opcache)  
* [Some More Optimizations](#some-more-optimizations)  
  
[Step 4 - Review your Nginx webserver](#step-4-review-your-nginx-webserver)  
* [FPM: Processes, Workers and Threads](#fpm-processes-workers-and-threads)  
* [Enabling some key params](#enabling-some-key-params)  
* [Compression and functions](#compression-and-functions)  
  
[Step 5 - Have a little checklist for your stack](#step-5---have-a-little-checklist-for-your-stack)
* [Review your Drupal installation](#review-your-drupal-installation)  
* [Tuning MySQL (MariaDB)](#tuning-mysql-mariadb)  
* [How Docker Works](#how-docker-works)  

[Step 6 - Read more and more, and more](#step-6---read-more-and-more-and-more)  
* [About Drupal Performance](#about-drupal-performance)  
* [About Nginx Performance](#about-nginx-performance)  
* [About PHP Performance](#about-php-performance)  
* [About MySQL Performance](#about-mysql-performance)  
* [About Docker Performance](#about-docker-performance)  

[:wq!](#wq)  
<!-- /TOC -->

-------------------------------------------------------------------------------

## Introduction

Managing Performance and the related improvements is a complex and arduos process consisting of a multitude of records, testing and interventions, in a continuous cycle of test-measure-review-intervention, and this can be quite a tedious task. Due to this, therefore, in order to better define the baselines, I'll describe the intervention escenarios as layers within the tech stack of our project, in order to address increasingly specific tasks and technological differences based on their own context.  

### Acknowledgements

The first assumption that we must make is that we have a long, complex and abstract path ahead of us, so to conjure up these challenges we will take as a premise the idea that _**"Knowing where to start and which issues are of high priority can be one of the most difficult parts of optimizing a site"**_ , one of the first (and main idea) present in the fundamental work _**"High Performance Drupal"(2014)**_ , written by Sheltren, Newton and Catchpole: [oreilly.com/high-performance-drupal](https://www.oreilly.com/library/view/high-performance-drupal/9781449358013/) and which has turned out to be an old basic pillar of our training as part of the Drupal community.  

This journey will take place together with them and always with a hand on their essential work about performance issues in the context of Drupal-based projects. Only if we stading on the shoulders of giants can we better see the horizon. Don't you think so?.  

### The Heuristic Approach 

Heuristics as we know it today - but also as we intuit it - remains a combined issue of divergent thinking and creative tensions, a result of which these two keys to try to find concrete solutions to specific problems are intertwined by treating these two lines as true tools of reasoned thought or at least as resources for rational mechanisms of analysis.  

Basically we could interpret it as a discipline of human rational activity that can have two meanings: on the one hand, it can be a method of enquiry based on experience to understand specific problems.  On the other hand, it can be something more related to arts, tactics and strategies, ready to be oriented to the generation of particular solutions. The first meaning would be more linked to the Hungarian mathematician [Imre Lakatos (1983)](https://es.wikipedia.org/wiki/Imre_Lakatos). The second one can be directly related to another illustrious Hungarian mathematician called [George Pólya (1945)](https://en.wikipedia.org/wiki/George_P%C3%B3lya), so if we assume that a child can owe his upbringing to two people of the same sex, we would have to converge assuming that Heuristics as a small paradigm has two parents called Pólya and Lakatos.  

The case of Pólya is important in this case, since with his seminal work _**["How to solve it"](https://www.amazon.es/How-Solve-Mathematical-Penguin-Science/dp/0140124993)**_ he lays the foundations of this type of lateralized approaches to consistent problems, providing us besides a strong conceptual framework, with an elementary process to approach our works under this prism:  

* **Polya's First Principle:** Understanding the problem.
* **Polya's Second Principle:** Create a plan.
* **Polya's Third Principle:** Carry out the plan.
* **Polya's Fourth Principle:** Look back, Review and interpret the result.

**Read more about the four principles of Pólya**: [math.berkeley/polya.pdf](https://math.berkeley.edu/~gmelvin/polya.pdf).  

As with any major problem that remains (for the moment) in the form of the enormous and the abstract, our situation invites a lateral approach, intuitive interpretation, thoughtful creativity: we have some macro figures, some "general" assessments regarding the performance of our Drupal installation and we must reduce it: that is our mission, the only important thing now, Mr Anderson. 

But as it's still an undetermined scenario (thinking about the first time), we require and propose a lateral, partial, iterative and incremental approach. We need perhaps to induce more than to deduct and take the parts until we reach the category and settle the whole. For all these reasons, we have chosen a heuristic approach that allows us to work with items, activities and strategies that facilitate the lateral approach of that great vessel that is itself the performance of an online platform of these characteristics. 


### Scenario   

* For Debian Ubuntu   

* Stack (Envinronment)  

### Reading Materials  

This whole process of experience and iterative learning, has been produced - _for me_ - progressively through practice and theory.  



## Step 1 - Select your Tooling

The first step in our way is oriented to looking for and select a set of tools to be used in our scenarios. I've gathered some of the most recurrent resources.  


### Prompt 

#### Apache Benchmarking 

```bash
sudo apt install apache2-utils
```

#### Parallel

```bash
sudo apt install parallel
```

#### GNU-Plot

```bash
sudo apt install gnu-plot
```

#### Others

**TMUX** 

```bash
sudo apt install tmux
```

**Atop**

```bash
sudo apt install atop
```


**Siege**

```bash
sudo apt install siege
```

```bash
sudo apt -y update
sudo apt install apache2-utils \
parallel \
gnu-plot \ 
tmux \
atop \
siege
```

Or you can download from this repository and execute it just like a bash script for Ubuntu / Debian:  

```bash
$ wget https://gitlab.com/davidjguru/scripting_for_drupal/-/raw/main/clinic/installing_tooling_for_performance
$ chmod +x installing_tooling_for_performance
$ ./installing_tooling_for_performance
```
But remember that: the script will executes update and upgrade of your installed packages.  


### Browser Extensions

#### Diver

#### PLT

#### RTM


### Drupal Modules

#### Performance Monitor


#### Monitoring
 

#### Remote Dashboard 


## Step 2 - Take an updated photo

### Make your own plan

From the book "High Performance Drupal":  

1. Measure and record the current site performance.  
2. Define goals and requirements for the site.  
3. Actually perform your review.  
4. Define a list of potential improvements based on the site goals and requirements,
using the information gathered in the performance baseline and your review. 

### Get a snapshot 


### Goals and benchmarking



## Step 3 - Check out PHP 



### Understanding PHP 

### OPCache 

**Memory**

**Times**

**Files**

### Some more optimizations



## Step 4 - Review your Nginx webserver

### FPM: processes, workers and threads


### Enabling some key params

#### Compression and Functions







## Step 5 - Have a little checklist for your Stack


### Review your Drupal installation



### Tuning MySQL (MariaDB)



### How Docker Works


## Step 6 - Read more and more, and more

### About Drupal performance

https://www.kelltontech.com/kellton-tech-blog/how-optimize-drupal-website-performance
https://www.siteground.com/kb/how_to_optimize_drupal_for_better_performance/
https://www.monitis.com/blog/7-best-practices-for-improving-your-drupal-performance/
https://www.keycdn.com/blog/speed-up-drupal
https://www.valuebound.com/resources/blog/a-beginners-guide-to-performance-optimization-drupal-8
https://2bits.com/articles/drupal-performance-tuning-and-optimization-for-large-web-sites.html
https://www.o8.agency/blog/case-study-drupal-performance-testing





### About Nginx Performance 

https://docs.bluehosting.cl/tutoriales/servidores/como-configurar-nginx-para-obtener-un-rendimiento-optimo.html
https://dzone.com/articles/maximizing-drupal-8-performance-with-nginx
https://dzone.com/articles/maximizing-drupal-8-performance-with-nginx-part-ii
https://www.nginx.com/blog/maximizing-drupal-8-performance-nginx-part-ii-caching-load-balancing/
https://www.ashnik.com/nginx-drupal-help-boost-site-performance/




### About PHP Performance

https://www.cloudways.com/blog/php-performance/
https://stackify.com/php-performance-optimization-guide/
https://stackify.com/php-performance-tuning/
https://codeburst.io/php-performance-optimization-992acaa78817
https://tideways.com/profiler/blog/5-ways-to-increase-php-performance

https://www.yourhowto.net/best-practices-to-optimize-code-performance-in-php/

### About MySQL Performance

https://codeburst.io/database-performance-optimization-8d8407808b5b
https://www.infoworld.com/article/3210905/10-essential-performance-tips-for-mysql.html
https://www.linode.com/docs/databases/mysql/how-to-optimize-mysql-performance-using-mysqltuner/
https://severalnines.com/database-blog/mysql-performance-cheat-sheet
https://www.percona.com/blog/2014/01/28/10-mysql-performance-tuning-settings-after-installation/


### About Docker Performance

https://rancher.com/5-tips-making-containers-faster/
https://containerjournal.com/features/make-docker-containers-faster-improve-performance/
https://hackernoon.com/another-reason-why-your-docker-containers-may-be-slow-d37207dec27f
https://jobandtalent.engineering/optimizing-docker-images-for-a-faster-development-workflow-591dc3ac4de0
https://crate.io/a/analyzing-docker-container-performance-native-tools/
https://medium.com/@beld_pro/how-does-docker-play-with-cgroups-when-i-create-an-instance-6ce24daccfb2

https://shekhargulati.com/2019/01/03/how-docker-uses-cgroups-to-set-resource-limits/


## :wq! 

##### Recommended song

{{< youtube e5Z56ZXvAy4 >}}