---
layout: lab
num: lab02
ready: true
desc: "C++ Big-Three Review: Constructor, Destructor, Assignment Operator"
assigned: 2019-10-17 14:00:00.00-7
due: 2019-10-23 23:59:00.00-7
---

# Goals

By the end of this lab, given a description of a class containing data members that point to structures on the heap, you will be able to:

* write a correct copy constructor for the class
* write a correct destructor for the class
* write a correct assignment operator for the class

# Step by Step 

## Step 0: Getting Started

This lab may be done solo, or in pairs.

Before you begin working on the lab, please decide if you will work solo or with a partner.

As stated in previous labs, there are a few requirements you must follow if you decide to work with a partner. I will re-iterate them here:

* Your partner must be enrolled in the same lab section as you.
* You and your partner must agree to work together outside of lab section in case you do not finish the lab during your lab section. You must agree to reserve at least two hours outside of lab section to work together if needed (preferably during an open lab hour where you can work in Phelps 3525 and ask a mentor for help). You are responsible for exchanging contact information in case you need to reach your partner.
* If you choose to work with a partner for a future lab where pair programming is allowed, then you must choose a partner you have not worked with before.
* You MUST add your partner on Gradescope when submitting your work <strong>*<u>EACH TIME</u>*</strong> you submit a file(s). After uploading your file(s) on Gradescope, there is a "Group Members" link at the bottom (or "Add Group Member" under "Groups") where you can select the partner you are working with. Whoever uploaded the submission must make sure your partner is part of your Group. Click on "Group Members" -> "Add Member" and select your partner from the list.
* <b> You must</b> write your Name(s) and Perm number on each file submitted to Gradescope.

Once you and your partner are in agreement, choose an initial driver and navigator, and have the driver log into their account.

## Step by Step Instructions to getting setup with github <a name="git"></a>

In case you have not setup github yet, you can follow the steps to do so. For this lab and subsequent labs, you will be required to make Gradescope submissions via your github repo. If you already have your environment configured to use github, then you can skip these git configuration instructions.

## Do some initial ONE-TIME git configurations (this step has to be done individually)

* On separate machines, log onto your account.

* In your ~/cs32 directory, type the following commands, replacing Alex Triton with your name and atriton@umail.ucsb.edu with your email address.

```
   git config --global user.name "Alex Triton"

   git config --global user.email "atriton@umail.ucsb.edu"
```

* Next, generate a private/public key pair and upload your public key to your github account. To do this refer to this tutorial: [https://ucsb-cs56-pconrad.github.io/topics/github_ssh_keys/](https://ucsb-cs56-pconrad.github.io/topics/github_ssh_keys/) In the process of setting up your key pair, when asked for a passphrase just press enter. By doing this step you will avoid having to enter a password or passphrase everytime you push your code to git.

## Create a new private repo on the github organization, add your partner as collaborator (if applicable)

* Create a repo for this lab on the pilot's github accoun: To do this, open a browser and navigate to [www.github.com](www.github.com). Log into the pilot's github account. From the drop down menu on the left, select our class organization: ucsb-cs32-f19 and proceed to create a new private repo. Follow this naming convention: if your github username is jgaucho and your partner's is alily, your should name your repo lab02_agaucho_alily (usernames appear in alphabetical order). Also remember that you must set the visibity of your repo to be 'PRIVATE' when creating it.

* If applicable, the pilot should add the navigator as a collaborator on github. To do this navigate to the git repo you just created. Choose the settings tab. Then click on the 'Collaborators and teams' option on the left. Scroll all the way down and add the navigator's github account. Then press on the 'Add collaborator' button. Now you and the navigator share the ownership of your git repo.

## clone the git repo to your local computer

* On the terminal, change to your cs32 directory:

```
cd ~/cs32
```

* Using the web-browser, navigate to your newly created repo on github. Find the address of your git repo. Click on the green "clone or download button". If your git repo was named lab02_alily_jgaucho, then the git address should something like: "git@github.com:ucsb-cs32-f19/lab02_alily_jgaucho.git". Now clone your repo into your CSIL account by typing the following on the terminal, replacing the last argument with the address of your git repo.

```
git clone git@github.com:ucsb-cs32-f19/lab02_alily_jgaucho.git
```

* Type ls to see your new git repo directory and change into that directory

```
cd lab02_alily_jgaucho
```

## Step 1: Copying some files from my directory 

Visit the following web link—you may want to use "right click" (or "control-click" on Mac) to bring up a window where you can open this in a new window or tab:

<http://cs.ucsb.edu/~richert/cs32/misc/lab02/>

You should see a listing of several C++ files. We are going to copy those into your local lab02 github repo directory all at once with the following command:

<div>
<tt>cp ~richert/public_html/cs32/misc/lab02/* ~/cs32/[YOUR_LAB02_GITHUB_DIRECTORY]</tt>
</div>

You should now see several files in your directory&mdash;the same ones that you see if you visit the link <http://cs.ucsb.edu/~richert/cs32/misc/lab02/>

If so, you are ready to move on.

If you don't see those files, go back through the instructions and make sure you didn't miss a step. If you still have trouble, ask your TA / tutors for assistance.

Its now time to use the git-command line tools to perform version control for the files in your git repo. The four essential commands we will be using are:

```
git pull
git add .
git commit -m "Initial version of lab02 files"
git push origin master
```

Go ahead and type them out on a terminal in your git repo directory. The above commands save a snapshot of your code on github. To check that this was done sucessfully open a web-browser and navigate to your repo on github. Then check to see that the starter code appears in your repo.

Note: Every time you add a new piece of logic to your code, you should save a snapshot of the latest version of your code by issuing the commands: *git add ...* , *git commit ...* and *git push ...*. All the previous versions will be available to you as well and you have the option of reverting to older versions (We will see how in later labs). As you go through the rest of this lab you will essentially need to use these commands to keep track of the different versions of your code. Note that you should only keep relevant files in your repo - avoid uploading non-important LARGE files in your repo since this may cause errors when making your submission to Gradescope.

Congratulations on integrating git into your workflow!

## Step 2 Getting the code to pass the tests

In this week's lab, you have the following files:

* Makefile
* student.h, student.cpp
* studentRoll.h, studentRoll.cpp
* tddFuncs.h, tddFuncs.cpp
* testStudent00.cpp, etc.
* testStudentRoll00.cpp, etc.

Your job is, as usual, get all the test cases to pass. This involves implementing the "Big Three": Copy Constructor, Overloaded Assignment Operator, and Destructor.

In addition to the regular test cases, there are also "leakTests". This involves running a utility called valgrind on your code to see whether there are any memory leaks, or other problems involving memory management. You will only pass the tests if your code has proper memory management.

You will submit only the `student.cpp` and `studentRoll.cpp` files. As a result, there are two quite annoying things that you'll just have to put up with:

* In the Student class, the `name` attribute is implemented with a C-string that is allocated with dynamic memory on the heap. This is annoying. You might prefer to use the `std::string` class. Of course you would. But, that's not the point of this assignment. The point of this assignment is to know whether you can manage memory properly.
* In the StudentRoll class, the list of students is a linked list of structs rather than an `std::list<Student>` or `std::vector<Student>` or something. This is indeed annoying. Tough. We are training you for the situation where you don't have any choice, but have to work with the data structures you are given.

In certain later assignments, you will be given the freedom to choose whatever data structure or implementation is appropriate. You'll be able to decide whether to use `std::string`, or C-strings, whether to use array or `std::vectors`, etc. This is not one of those assignments.

## Suggested way to proceed
I suggest proceeding in the following steps:

1. Work on each test file for student, getting those tests to pass, i.e. testStudent00.cpp, testStudent01.cpp, etc.
* To get these to pass, you need to implement, possibly among other things, the Copy Constructor and Overloaded Assignment Operator for Student.

2. Then, try to get the leak tests to pass as they pertain to Student, i.e.
* `make lts00`
* `make lts01`
* `make lts02`
* `make lts03`
* This will require implementing the destructor for `Student`

3. Work on each test file for StudentRoll, getting those tests to pass, i.e. `testStudentRoll00.cpp`, `testStudentRoll01.cpp`, etc.
* To get these to pass, you need to implement, possibly among other things, the Copy Constructor and Overloaded Assignment Operator for StudentRoll
4. Then, try to get the leak tests for StudentRoll to pass, i.e.
* `make ltsr00`
* `make ltsr01`
* `make ltsr02`
* This will require implementing the destructor for `StudentRoll`

## How do I know if I'm done?
When you are done, you should be able to do both of the following, and see no error messages:

* `make tests`
* `make leaktests`

## Step 3: Submitting via Gradescope

The lab assignment "Lab02" should appear in your Gradescope dashboard in CMPSC 32. If you haven't submitted anything for this assignment yet, Gradescope will prompt you to upload your files.

For this lab, you will need to upload your modified files (i.e. `student.cpp` and `studentRoll.cpp`). For this lab and subsequent labs, you are <b>required</b> to submit your files with your github repo (if you haven't configured github yet, please refer to the instructions at the beginning of this lab).

If you already submitted something on Gradescope, it will take you to their "Autograder Results" page. There is a "Resubmit" button on the bottom right that will allow you to update the files for your submission.

For this lab, if everything is correct, you'll see a successful submission passing all of the autograder tests.

**Remember to add your partner to Groups Members for this submission on Gradescope if applicable. At this point, if you worked in a pair, it is a good idea for both partners to log into Gradescope and check if you can see the uploaded files for Lab02.**
