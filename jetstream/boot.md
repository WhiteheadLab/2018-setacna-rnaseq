# Booting a Jetstream Computer Instance for your use!

What we're going to do here is walk through starting up a computer (an "instance") on the Jetstream service. 
* Jetstream is run by [XSEDE, an NSF-funded program](https://www.xsede.org/about/what-we-do), and provides elastic cloud computing services. 
* "Cloud" computing is a fancy word for being allowed to temporarily use someone else's computer somewhere else with full administrative privaleges to install whatever software we want. 
* Sometimes we need to use computing resources that are larger than our personal computers. Cloud computing lets us decide how much capacity we want to be using. 

If you would like to read more about cloud computing, see this [Carpentry Cloud Computing lesson](http://www.datacarpentry.org/cloud-genomics/01-why-cloud-computing/).

Below, we've provided screenshots of the whole process. The important areas to fill in are circled
in red or pointed out with arrows.

Some of the details may vary -- for example, if you have your own XSEDE account, you may want to log in with that in the future -- and the name of the operating system or "Image" may also vary from "Ubuntu 18.04" or "DIBSI 2018" depending on the workshop.

### Resources

If you are interested in applying for XSEDE-Jetstream allocation credits, that is great! These are free compute resources provided by [XSEDE](https://www.xsede.org/about/what-we-do), and available to all investigators at US academic, government and non-profit institutions. [Here is our proposal that we submitted for this workshop](https://github.com/ljcohen/jetstream-xsede-illo/blob/master/xsede_applications/setac.md). There are several types of allocations: 1) [education](https://portal.xsede.org/allocations/education) (which we applied for), 2) [start-up](https://portal.xsede.org/allocations/startup) and 3) [research](https://portal.xsede.org/allocations/research). [Here is a collection of successful proposals for all three types](https://github.com/ljcohen/jetstream-xsede-illo/tree/master/xsede_applications). [Start-up allocations requests for ~50,000 SU](https://portal.xsede.org/allocations/startup) are the first step before applying for full research allocations. We have found the process to be very easy and highly recommend [contacting the XSEDE help desk if you have any questions](https://portal.xsede.org/help-desk). They are very helpful and usually quick to respond. 

-----

First, go to the Jetstream application at [https://use.jetstream-cloud.org/application](https://use.jetstream-cloud.org/application).

Now:

## Request to log in to the Jetstream Portal

Click the login link in the upper right.

![login](images/login-1.thumb.png)
## Use "XSEDE"

Choose "XSEDE" as your account provider (it should be the default) and click
on "Continue".
           
![foo](images/login-2.thumb.png)
## Fill in the username and password and click "Sign in"

Fill in the username and then the password (which we will tell you in class).

![foo](images/login-3.thumb.png)

## Select Projects and "Create New Project"

Now, this is something you only need to once if you have your own
account - but if you're using a shared account like 'dibbears', you will
need a way to keep your computers separate from everyone else's.

We'll do this with Projects, which give you a bit of a workspace in which
to keep things that belong to "you".

Click on "Projects" up along the top.

![foo](images/login-5.thumb.png)
           
## Name the project for yourself, click "create"

Enter your name into the Project Name, and something simple like "SETAC"
into the description. Then click 'create'.

![foo](images/login-6.thumb.png)

## Boot an instance with a pre-built image 

Select 

![foo](images/Images.thumb.png)

## Find the "RNASeq_1DayWorkshop"  base image (Oct 15, 2018 by ljcohen), click on it

Go to "Show All" tab and enter "RNASeq_1DayWorkshop" into the search bar - make sure it's from
Oct 15st, 2018 by ljcohen. This images is based on Ubuntu 18.04 devel and docker, with Rstudio and [bioconda](https://bioconda.github.io/) package manager added, in addition to other workshop software. Instructions for how Lisa set this up are [here](https://github.com/WhiteheadLab/2018-setacna-rnaseq/blob/master/RNASeq_1DayWorkshop_image.md).

Here, "image" refers to the resources that are pre-loaded into your computing workspace on your instance. Think of it like apps that come with your phone before you add new ones on your own. Loading this image built before the workshop prevents us from having to choose our operating distrubution and download frequently-used packages on our own, and makes sure that everyone at the workshop has the same basic computing environment. That ensures that the commands we tell you to use will work, and makes it easier for us to figure out what's wrong if you run into error messages.

![foo](https://i.imgur.com/WPhJ6Vx.png)
           
Launch

## Name it something simple

Change the name after what we're doing - "RNAseq SETAC", for example, but it doesn't matter. Pull down the drop-down menu under 'Project' to select your name. Then make sure the appropriate resources are selected. You probably won't have to change these. The 'Allocation Source' will already be selected. (This is our XSEDE allocation grant ID.) Choose the 'm1.large' instance size. This is the minimum instance size. A larger instance can be selected, depending on what we will be doing. The 'Provider' will be randomly chosen as either 'Jetstream - Indiana University' or 'Jetstream - TACC'.

![foo](https://i.imgur.com/W2oi7iv.png)

## Wait for it to become active

It will now be booting up! This will take 2-15 minutes, depending.
Just wait! Don't reload or anything. When it is ready, the colored dot under "Status" will turn green and look like this: 

![foo](images/login-11.thumb.png)
           
## Click on your new instance to get more information!

Now, you can login to the instance! Note that you'll need to use the private key file located in the #general channel in slack. The username will be `dibbears`. Use these log-in [instructions](https://setac-omics.readthedocs.io/en/latest/jetstream/login.html) for using a private-key.

If you cannot access the terminal using the private key, a web shell is available:

![foo](images/login-12.thumb.png)

## Miscellany

There's a possibility that you'll be confronted with this when you log in to jetstream:

![foo](images/possible_instance_problem.thumb.png)

A refresh of the page should get you past it. Please try not to actually move any instances to
a new project; it's probably someone else's and it could confuse them :)

## Suspend your instance

You can save your workspace so you can return to your instance at a later time without losing any of your files or information stored in memory, similiar to putting your physical computer to sleep. At the Instance Details screen, select the "Suspend" button. 

![foo](images/suspend-1.png)

This will open up a dialogue window. Select the "Yes, suspend this instance" button.

![foo](images/suspend-2.png)

It may take Jetstream a few minutes to process, so wait until the progress bar says "Suspended."

### Resuming your instance

To *wake-up* your instance, select the "Resume" button.

![foo](images/resume-1.png)

This will open up a dialogue window. Select the "Yes, resume this instance" button. 

![foo](images/resume-2.png)

It may take Jetstream a few minutes to process, so wait until the progress bar says "Active." 

![foo](images/resume-3.png)

## Shutting down your instance

You can shut down your workspace so you can return to your instance another day without losing any of your files, similiar to shutting down your physical computer. You will retain your files, but you will lose any information stored in memory, such as your history on the command line. At the Instance Details screen, select the "Stop" button. 

![foo](images/stop-1.png)

This will open up a dialogue window. Select the "Yes, stop this instance" button.

![foo](images/stop-2.png)

It may take Jetstream a few minutes to process, so wait until the progress bar says "Shutoff."

![foo](images/stop-3.png)

![foo](images/stop-4.png)

### Restarting your instance

To start your instance again, select the "Start" button.

![foo](images/start-1.png)

This will open up a dialogue window. Select the "Yes, start this instance" button. 

![foo](images/start-2.png)

It may take Jetstream a few minutes to process, so wait until the progress bar says "Active." 

![foo](images/start-3.png)

## Deleting your instance

To completely remove your instance, you can select the "delete" buttom from the instance details page. 

![foo](images/delete-1.png)

This will open up a dialogue window. Select the "Yes, delete this instance" button.

![foo](images/delete-2.png)

It may take Jetstream a few minutes to process your request. The instance should disappear from the project when it has been successfully deleted. 

![foo](images/delete-3.png)

![foo](images/delete-4.png)
