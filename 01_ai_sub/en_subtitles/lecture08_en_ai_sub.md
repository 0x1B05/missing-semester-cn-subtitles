All right everyone let's get started with the next lecture. 
So, today we're gonna tackle the topic of meta programming, 
and this title is a little weird. 
It's not entirely clear what we mean by meta programming. 
We couldn't really come up with a better name for this lecture because this lecture is about the processes that surround the work that you do when working with software. 
It is not about programming itself necessarily but about the process. 
This can be things like how your system is built, how it's tested, how you add dependencies to your software, 
that sort of stuff that becomes really relevant when you build larger pieces of software, 
but they're not really programming in and of themselves.
So, the first thing we're going to talk about in this lecture is the notion of build systems. 
So, how many of you have used a build system before or know what it is? Okay, 
so about 1/2 of you. 
So, for the rest of you, the central idea behind a build system is that you're writing a paper, 
you're writing software, you're working on a class, whatever it might be, 
and you have a bunch of commands that you like either written down in your shell history 
or you wrote them down in a document somewhere that you know you have to run if you want to do a particular thing. 
So, for example, there are a sequence of commands so you need to run in order to build your paper or build your thesis 
or just to run the tests for whatever class you're currently in, 
and a build system sort of idea is that you want to encode these rules for what commands to run in order to build particular targets into a tool that can do it for you.
And in particular, you're going to teach this tool about the dependencies between those different artifacts that you might build. 
There are a lot of different types of tools of this kind, 
and many of them are built for particular purposes, particularly languages. 
Some of them are built for building papers, 
some of them are built for building software, 
some of them are built for particularly programming languages like Java or some. 
Some tools even have built-in tools for builds. 
So, NPM, for example, you might be aware if you've done Node.js development, has a bunch of built-in tools for doing tracking of dependencies and building them 
and building all of the dependent stuff of your software.
But more generally, these are known as build systems, 
and at their core, they all function in a very similar way. 
And that is, you have a number of targets. 
These are the things that you want to build. 
These are things like paper dot PDF, 
but they can also be more abstract things like run the test suite or build the binary for this program. 
Then you have a bunch of dependencies, 
and dependencies are things that need to be built in order for this thing to be built. 
And then you have rules that define how do you go from a complete list of dependencies to the given target. 
So, an example of this might be, in order to build my paper dot PDF, I need a bunch of like plot images. 
They're gonna go into the paper, 
so they need to be built. 
But then once they have been built, how do I construct the paper given those files?
But then, once they have been built, how do I construct the paper given those files? 
So that is what a rule is. 
It's a sequence of command, 
so you run to get from one to the other. 
How you encode these rules differs between different tools. 
In this particular class, we're gonna focus on a tool called make. 
Make is a tool that you will find on almost any system that you log in today. 
Like, it'll be on Mac OS, it'll be on basically every Linux and BSD system, 
and you can pretty easily get it on Windows. 
It's not great for very complex software, 
but it works really well for anything that's sort of simple to medium complexity.
Now, when you run make to make user command, you can run on the command line, 
and when you type make and this is an empty directory, if I type make, it just has no target specified and no make file found, stop. 
And so it helpfully tells you that it stopped running, 
but also tells you that no make file was found. 
Make will look for a file literally called Makefile in the current directory, 
and that is where you encode these targets, dependencies, 
and rules. 
So, let's try to write one. 
Let's imagine that I'm writing this hypothetical paper, 
and so I'm gonna make a Makefile, 
and then in this Makefile, I'm going to say that my paper dog PDF, the hands-on, that's what the colon here indicates. 
Paper dot text was going to be a little attack file, 
and plot data dot PNG, 
and the command in order to build this is going to be PDF latex of paper dot Tex.
So, for those of you who are not familiar with this particular way of building documents, the tech is a really handy programming language for documents. 
It's a really ugly language, 
and it's a pain to work with, 
but it produces pretty nice documents. 
And the tool you use to go from a tech file to PDF is PDF latex. 
And here, I'm saying that I also depend on this plot plot data PNG that's gonna be included in my document. 
And what I'm really saying here is if either of those two dependencies change, I want you to build paper PDF. 
They both need to be present, 
and should they ever change, I wanted to rebuild it. 
But I haven't really told it how to generate this plot data PNG, 
so I might want a rule for that as well.
So, I'm gonna define another target here, 
and it's gonna look like this: plot - % & % means, 
and make is any string, 
sort of a wildcard pattern. 
But the cool thing is as you go and repeat this pattern in the dependencies, 
so I can say that plot - % dot PNG is going to depend on % dot data or debt, that is a common sort of suffix for data files. 
And it's also going to depend on some script that's gonna actually plot this for me, 
and the rules for to go from one to the other, these can be multiple lines, 
but in my particular case, they're just one line. 
I'm gonna explain what this is in a little second. 
Alright, 
so here we're gonna say that in order to go from a wildcard dot dot file that matches the wildcard in the target 
and a plot dot python file, run the python file with - i which is going to be like the way we take the mark what the input is in our python file. 
I'll show it to you later. 
$* dollarz star is a special variable that is defined for you and make file rules that matches whatever the percentile was. 
So, if I do plot to PNG, then it's going to look for foo.dot that and it dollars stars can expand to foo. 
So, this will produce the same file name as the one we matched here and dollar act as a special variable that means the name of the target, right? 
So, the output file, and hopefully what plotter py will do is that it will take whatever the data is here, it will produce a PNG somehow, 
and it will write it into the file indicated by the dollar at all, right? 
So now we have a make file. 
Let's see what happens if the only file in this directory is the make file, 
and we run make. 
One says no rule to make target paper dot tex needed by paper dot PDF, stop. 
So, what it's saying here is, first of all, it looked at the first rule of our file, the first target, 
and when you give make no arguments, it tries to build whatever the first target is. 
This is known as the default goal. 
So, in this case, it tried to helpfully build paper dot PDF for us, 
and then it looked up the dependencies, 
and it said, well, in order to build paper dot PDF, I need paper Tex, 
and I need this PNG file. 
And I can't find paper dot X, 
and I don't have a rule for generating paper dot X, 
and therefore I'm gonna exit. 
This isn't nothing more I can do. 
So, let's try to make some files here. 
Let's just make like an empty paper dot X and then type make. 
So now it says no rule to make target plot data dot PNG needed by paper to PDF, right? 
So now it knows that it has one dependency, 
but it doesn't know how to get the other one. 
It knows it as a target that matches, 
but it can't actually find its dependencies, 
and so it ends up doing nothing at all. 
It still needs us to generate this PNG for the input for the PNG. 
So, let's actually put some useful stuff into these files. 
Let's say that luckily I have one from earlier plot da py to here, 
so let's so good. 
Well, this text file is what text looks like. 
It's not very pretty, 
but see, I'm defining an empty document. 
I'm going to include graphics, which is the way you include an image file. 
I'm going to include plot data dot PNG, 
and this is of course why we want a dependency of paper dot PDF to be the PNG file. 
Plot the py is also not very interesting. 
It just imports a bunch libraries. 
It parses the - ion - Oh arguments. 
It loads data from the I argument. 
It uses library called matplotlib, which is very handy for just quickly plotting data. 
And it's going to plot the first column of the data as X's and the second column of the data as Y's. 
So, we're just going to have a data file that's two columns, X and Y on every line, 
and then it saves that as a figure into whatever the given - oh value is. 
So we need a data file that's going to be data.dot because we want plot_data.dot.png, 
and our rules said that the way you go from that pattern to the dot file, the dot that file is just by whatever follows plot. 
So if we want plot-data, then we want data.dot that. 
And then this file, we're just gonna put in some linear coordinates because why not? That's not linear. 
All right. 
And now, what happens if we're gonna make, well, mmm, okay, 
so what just happened? Well, make first ran plot.py with the correct files to generate the PNG file, 
and then it ran PDF latex paper.tex, 
and all the stuff we see below is the output from that tool. 
If you wanted to, we silence the effort from this tool, 
so we don't have to like have it mess with all our output, 
but in general, you notice that it ran the two commands, then it wrote the random, perhaps unsurprisingly, in the right order. 
And if we now do LS in the current directory, we see that we have a bunch of files that were generated by PDF latex, 
but in particular, we have the PNG file which was generated, 
and we have the paper.pdf. 
And if we open the paper.pdf file, we see that it has one image which has the straight line, perhaps in and of itself not a very surprising or interesting result, 
but where this gets really handy is I can do things like if I type make again, make just says paper.pdf is up to date. 
It does no work. 
Whenever you run make, it tries to do the minimal amount of work in order to produce whatever you ask it to produce. 
In this case, none of the dependencies have changed, 
so there's no reason to rebuild the paper or to rebuild the plot. 
If I now let's say I'm gonna edit paper.tex, I'm gonna add hello here and now make, then if we scroll up, we'll see it didn't run plot.py again because I didn't need to. 
None of the dependencies changed, 
but it did run PDF latex again. 
And indeed, if we open the paper, analysis hello over there. 
On the other hand, if I were to change say the data file and make this point 8 an hour and make, then now it plots again because the data changed, 
and it regenerates the PDF because the plot changed. 
And indeed, the paper turns out the way we expected it to. 
So that's not to say that this particular pipeline is very interesting because it's not. 
It's only true very, very straightforward targets and rules. 
But this can come in really handy when you start building larger pieces of software or there might be dependencies. 
You might even imagine that if you're writing a paper, one of your targets might be producing this data file in the first place, right? 
So one of the makefile targets might be run my experiment, right? Run my benchmark and stick the data points that come out into this file and then plot the results 
and then and then and then and then all the way until you end up with a final paper. 
And what's nice about this is, first of all, you don't have to remember all the commands to run. 
You don't have to write them down anywhere, 
but also the tool takes care of doing the minimal amount of work needed. 
Often you'll find things like they'll be too. 
There'll be sub-commands to make, like make tests, right? Which is going to compile your entire piece of software and also run the tests. 
There might be things like make release, which builds it with optimizations turned on and creates a tarball and uploads that somewhere, right? 
So it's gonna do the whole pipeline for you. 
The idea is to reduce the effort that you have to do as any part of your build process.
Now what we saw here was a very straightforward example of dependencies, right? 
So we saw here that you could declare files as dependencies, 
but you can also declare sort of transitive dependencies. 
Very often when you work with dependencies in the larger area of software, you'll find that your system ends up having many different types of dependencies. 
Some of these are files like we saw here, 
some of them are programs, like this sort of implicitly depends on Python being installed on my machine. 
Some of it might be libraries, like you might depend on something like matplotlib, which we depend on here. 
Some of them might be system libraries like OpenSSL or OpenSSH or like low-level crypto libraries, 
and you don't necessarily declare all of them.
Very often, there are tools for managing those dependencies for you. 
And very often, these systems you might depend on are stored in what are known as repositories. 
So a repository is just a collection of things usually related that you can install. 
That's basically all a repository is, 
and you might be familiar with some of them already, right? 
So some examples of repositories are PyPI, which is a well-known repository for Python packages, RubyGems, which is similar for Ruby, crates.io for Rust, NPM for Node.js, 
but other things have repositories too, right? Like there are repositories for cryptographic keys like Keybase, 
there are repositories for system-installed packages like if you ever use the apt tool in Ubuntu or in Debian, 
you are interacting with a package repository where people who have written programs and libraries upload them so that you can then install them. 
Similarly, you might have repositories that are entirely open, 
so the Ubuntu repositories, for example, are usually provided by the Ubuntu developers, 
but in Arch Linux, there might be something called the Arch User Repository where users can just share their own libraries and their own packages themselves.
Very often, repositories are either sort of managed or they are just entirely open, 
and you should often be aware of this because if you're using an entirely open repository, 
maybe the security guarantees you get from that are less than what you get in a controlled repository. 
One thing you'll notice if you start using repositories is that very often software is versioned. 
And what I mean by version, well, you might have seen this for stuff like browsers, right? 
Where there might be something like starting like Chrome version 64 dot zero dot two zero one 903 24, right? This is a version number. 
It might, there's a dot here, this is one kind of version number. 
But sometimes, if you start, I don't know, like Photoshop, or you start any other tool, there might be other kinds of versions that are, like, 8.1 dot seven, right? 
These version numbers are usually numerical, 
but not always. 
Sometimes they have hashes in them, for example, to refer to Git commits. 
But you might wonder, why do we have these? Why is it even important that you add a number to software that you release? The primary reason for this is because it enables me to know whether my software would break. 
Imagine that I have a dependency on a library that Jose has written, right? 
And Jose is constantly doing changes to his library because he wants to make it better. 
And he decides that one of the functions that his library exposes has a bad name, 
so he renames it. 
My software suddenly stops working, right? Because my library calls a function on Jose's library, 
but that function no longer exists. 
Depending on which version people have installed of Jose's library, versions help because I can say I depend on this version of Jose's library, 
and there have to be some rules around what is Jose allowed to do within a given version. 
If he makes a change that I can no longer rely on, his version has to change in some way. 
There are many thoughts on exactly how this should work, like what are the rules for publishing new versions, how do they change the version numbers. 
Some of them are just dictated by time. 
So for example, if you look at browsers, they very often have time versions that look like this. 
They have a version number on the far left that's just like which release, 
and then they have sort of an incremental number that is usually zero, 
and then they have a date at the end, right? 
So this is March 24th, 2019, for some reason. 
And usually, that will indicate that this is version 64 of Firefox from this date. 
And then, if they release sort of patches or hot fixes for security bugs, they might increment the date but keep the version of the left the same. 
And people have strong, strong opinions on exactly what the scheme should be, 
and you sort of depend on knowing what schemes other people use, right? 
If I don't know what scheme Jose is using for changing his versions, maybe I just have to say, you have to run like eight one seven of Jose's software, 
otherwise, I cannot build my software. 
But this is a problem, too, right? Imagine that Jose, as a responsible developer of his library, 
and he finds the security bug and he fixes it, 
but it doesn't change the external interfaces library. 
No functions change, no types change. 
Then I want people to be building my software with his new version,
and it just so happens that building mine works just fine with his new version because that particular version didn't change anything I depended on. 
So one attempted solution to this is something called semantic versioning. 
In semantic versioning, we give each of the numbers separated by dots in a version number a particular meaning. 
And we give a contract for when you have to increment the different numbers. 
In particular, in semantic versioning, we call this the major version, we call this the minor version, 
and we call this the patch version. 
The rules around this are as follows: if you make a change to your software that is entirely backwards compatible, 
like it does not add anything, it does not remove anything, it does not rename anything externally, 
it is as if nothing changed, then you only increment the patch number, nothing else. 
Usually, security fixes, for example, will increment the patch number. 
If you add something to your library (I'm just gonna call them libraries because usually libraries are the things where this matters), 
you increment the minor version and you set the patch to zero. 
In this case, if we were to do a minor release, the next minor release version number would be eight to zero. 
And the reason we do this is because I might have a dependency on a feature that José added in a 2-0, which means you can't build my software with 8-1-7. 
That would not be okay, even though if you had written it towards 8-1-7, you could run it with a 2-0. 
The reverse is not true because it might not have been added yet. 
And then, finally, the major version you increment if you make a backwards incompatible change. 
Where if my software used to work with whatever version you had, 
and then you make a change that means that my software might no longer work, such as removing a function or renaming it, 
then you increment the major version and set minor and patch to zero. 
So, the next major version here would be 9-0-0. 
Taken together, these allow us to do really nice things when setting what our dependencies are. 
In particular, if I depend on a particular version of someone's library, rather than saying it has to be exactly this version, 
what I'm really saying is it has to be the same major version and at least the same minor version, 
and the patch can be whatever. 
This means that if I have a dependency on José's software, then any later release that is still within the same major is fine. 
That includes, keep in mind, an earlier version, assuming that the minor is the same.
Imagine that you are on some older computer that has version 8.1.3. 
In theory, my software should work just fine with 8.1.3 as well. 
It might have whatever bug that José fixed in between, like whatever security issue. 
But this has the nice property that now you can share dependencies between many different pieces of software on your machine. 
If you have version 8.3.0 installed and there are bunch of different software that one requires 8.1.7, one requires 8.2.4, 
and one requires 8.0.1, all of them can use the same version of that dependency. 
You only need to install it once.
One of the most common or most familiar, perhaps, examples of this kind of semantic versioning is if you look at the Python versioning. 
Many of you may have come across this, where Python 3 and Python 2 are not compatible with one another. 
They're not backwards compatible. 
If you write code in Python 2 and try to run it in Python 3, it might not work. 
There are some cases where it will, 
but that is more accidental than anything else. 
Python actually follows semantic versioning, at least mostly. 
If you write software that runs on Python 3.5, then it should also work in 3.6, 3.7, 
and 3.8. 
It will not necessarily work in Python 4, although that will hopefully be a long time away. 
But if you write code for Python 3.5, it will possibly not run on Python 3.4.
So, one thing you will see many software projects do is they try to bring the version requirements they have as low as possible. 
If you can depend on major and then minor in patch 0.0, 
that is the best possible dependency you can have because it is completely liberal as to which version of that major you're depending on. 
Sometimes this is hard. 
Sometimes you genuinely need a feature that was added, 
but the lower you can get, the better it is for those who want to depend on your software.
In turn, when working with these sort of dependency management systems or in with versioning in general, you'll often come across this notion of lock files. 
You might have seen this where you try to do something and it says, "Cannot reconcile versions" or you get an error like, 
"Lock file already exists." These are often somewhat different topics, 
but in general, the notion of a lock file is to make sure that you don't accidentally update something. 
The lock file, at its core, is really just a list of your dependencies and which version of them you are currently using. 
My version string might be 8.1.7, 
and the latest version, like on the internet somewhere, might be 3.0, 
but whatever is installed on my system is not necessarily one of those two. It might be like 8.2.4 or something like that, 
and the lock file will then say, "Dependency José version 8.2.4." The reason you want a lock file, there can be many. 
One of them is that you might want your builds to be fast.
if every single time you try to build your project, whatever tool you were using download the latest version and then compile it and then compile your thing, 
you might wait for a really long time each time, depending on the release cycle of your dependencies. 
If you use a lock file, then unless the version, unless you've updated the version in your lock file, 
it'll just use whatever it built previously for that dependency and your sort of development cycle can be a lot faster. 
Another reason to use lock files is to get reproducible builds. 
Imagine that I produce some kind of security-related software, 
and I very carefully audited my dependencies, 
and I produce, like, a signed binary of, like, 'Here is, thus, like a sworn statement for me that this version is secure.' 
If I didn't include a lock file, then by the time someone else installs my program, they might get a later version of their pendency, 
and maybe that later version as I've been hacked somehow or just has some other security vulnerability that I haven't had a chance to look at yet, right? 
And a lock file basically allows me to freeze the ecosystem as of this version that I have checked. 
The extreme version of this is something called 'vendoring.' When you vendor your dependencies, it really just means you copy/paste of them. 
Vendoring means take whatever dependency you care about and copy it into your project because that way, 
you are entirely sure that you will get that version of that dependency. 
It also means that you can, like, make modifications to it on your own, 
but it has the downsides that now you no longer get these benefits of versioning, right? 
You no longer have the advantage that if there are newer releases of that software, your users might get them automatically, like, 
for example, when Hosea fixes his security issues, not that he has any of course. 
One thing you'll notice is that when talking about this, I've been talking about sort of bigger processes around your systems. 
These are things like testing. 
They're things like checking your dependency versions. 
They're also things that are just setting up build systems, 
and often you don't just want a local build system. 
You want to build process that includes other types of systems, or you want them to run even when your computer is not necessarily on, 
and this is why, as you start working a larger and larger project, you will see people use this idea of continuous integration, 
and continuous integration systems are essentially a cloud build system. 
The idea is that you have your project stored on the internet somewhere, 
and you have set it up with some kind of service that is running an ongoing thing for your project, whatever it might be, 
and continuous integration can be all sorts of stuff. 
It can be stuff like releasing your library to PyPI automatically whenever you push to a particular branch. 
It could be things like run your test suite whenever someone submits a pull request, or it could be check your code style every time you commit. 
There all sorts of things you could do with continuous integration, 
and the easiest way to think about them is that they're sort of event-triggered actions. 
So, whenever a particular event happens for your, possibly for your project, a particular action takes place, where the action is usually some kind of script, 
some sequence of programs that are going to be invoked, 
and they're going to do something. 
This is really an umbrella term that encapsulates a lot of different types of services.
Some continuous integration services are very general things like Travis CI or Azure pipelines, or GitHub actions are all very broad CI platforms. 
They're built to let you write what you want to happen whenever any event that you define happens, very broad systems. 
There are some more specialized systems that deal with things like continuous integration coverage testing. 
So, like annotate your code and show you have no tests that test this piece of code, 
and they're built only for that purpose, or they're built only for testing browser-based libraries or something like that. 
And so, often you can find CI tools that are built for the particular project you're working on, or you can use one of these broader providers. 
And one thing that's nice is that many of them are actually free, especially for open source software, or if you're a student, you can often get them for free as well.
In general, the way you use the CI system is that you add a file to your repository, 
and this file is often known as a recipe. 
And what the recipe specifies is this sort of dependency cycle again, 
sort of what we saw with make files, it's not quite the same. 
The events, instead of being files, might be something like when someone pushes a commit 
or when a commit contains a particular message or when someone submits a pull request or continuously write. 
One example of a continuous integration service that's not tied to any particular change to your code is something called the Dependabot. 
You can find this on GitHub, 
and the Dependabot is something that you hook up to your repository, 
and it will just scan whether there are newer versions available of your dependencies that you're not using.
So, for example, if I was depending on 8.1.7 and I had a lock file that locked it to 8.2.4, 
and then 8.3.0 is released, the Dependabot will go, "You should update your lock file," and then submit the pull request to your repository with that update. 
This is a continuous integration service. 
It's not tied to me changing anything but to the ecosystem at large changing. 
Often these CI systems integrate back into your project as well. 
So, very often, these CI services will provide things like little badges.
Let me give an example. 
So, for example, here's a project I've worked on recently that has continuous integration set up. 
So, this project, you'll notice it's README. 
If I can zoom in here with that Chrome bean, nope, nope, that's much larger than I wanted. 
Here, you'll see that at the top of the repositories page, there are a bunch of these badges, 
and they display various types of information. 
"You'll notice that I have dependable running, right? 
So the dependencies are currently up to date. 
It tells me about whether the test suite is currently passing on the master branch. 
It tells me how much of the code is covered by tests, 
and it tells me what is the latest version of this library and what is the latest version of the documentation of the library that's available online. 
And all of these are managed by various continuous integration services. 
Another example that some of you might find useful or might even be familiar with is the notion of GitHub Pages. 
So GitHub Pages is a really nice service the GitHub provides which lets you set up a CI action that builds your repository as a blog essentially. 
It runs a static site generator called Jekyll, 
and Jekyll just takes a bunch of markdown files and then produces a complete website. 
And that, as a part of GitHub Pages, they will also upload that to GitHub servers and make it available at a particular domain. 
And this is actually how the class website works. 
The class website is not a bunch of like HTML pages that we manage. 
Instead, there's a repository 'Missing Semester.' So if you look at the Missing Semester repository, you will see, if I zoom out a little here, 
that this just has a bunch of markdown files. 
Right? It has 'Saket 20/20 - Metaprogramming.md,' so this is the raw markdown for today's lecture. 
So this is the way that I write the lecture notes, 
and then I commit that to the repository we have, 
and I push it. 
And whenever a push happens, the GitHub Pages CI is gonna run the build script for GitHub Pages 
and produces the website for our class without me having to do any additional steps to make that happen. 
And so, yeah, 
sorry, good, yeah. 
So Jekyll, it's using a tool called Jekyll, which is a tool that takes a directory structure that contains markdown files and produces a website. 
It produces like HTML files, 
and then as a part of the action, it takes those files and uploads them to GitHub servers at a particular domain, 
and usually, that's the domain under like 'github.io' that they control. 
And then I have set Missing Semester to point to the GitHub domain. 
I want to give you one aside on testing because it's something that many of you may be familiar with from before, right? You have a rough idea of what testing is. 
You run the test before. 
You've seen a test fail. 
You know, like the basics of it, or maybe you've never seen a test fail. 
In case congratulations, 
but as you get to more advanced projects, though, you'll find that people have a lot of terminology around testing.
"and testing is a pretty deep subject that you could spend many many hours trying to understand the ins and outs of, 
and I'm not going to go through it in excruciating detail, 
but there are a couple of words that I think it's useful to know what mean. 
And the first of these is a test suite. 
So a test suite is a very straightforward name for all of the tests in a program. 
It's really just a suite of tests, it's a large collection of tests that usually are run as a unit, 
and there are different types of tests that often make up a test suite. 
The first of these is what's known as a unit test. 
A unit test is a, often usually fairly small, test of self-contained tests that tests a single feature. 
What exactly a feature might mean is a little bit up to the project, 
but the idea is that should be sort of a micro test that only tests a very particular thing. 
Then you have the larger tests that are known as integration tests. 
Integration tests try to test the interaction between different subsystems of a program. 
So this might be something like, an example of a unit test might be if you're writing an HTML parser, the unit test might be test that it can parse an HTML tag. 
An integration test might be, here's an HTML document, parse it. 
So that is going to be the integration of multiple of the subsystems of the parser. 
You also have a notion of regression tests. 
Regression tests are tests that test things that were broken in the past. 
So imagine that someone submits some kind of issue to you and says your library breaks if I give it a marquee tag, 
and that makes you sad, 
so you want to fix it. 
So you fix your parser to now support my key tags, 
but then you want to add a test to your test suite that checks that you can parse marquee tags. 
The reason for this is so that in the future, you don't accidentally reintroduce that bug. 
So that is what regression tests are for, 
and over time your project is gonna build up more and more of these, 
and they're nice because they prevent your project from regressing to earlier bugs. 
The last one I want to mention is a concept called mocking. 
So mocking is the idea of being able to replace parts of your system with a sort of dummy version of itself that behaves in a way that you control. 
A common example of this is you're writing something that does, oh I don't know, file copying over SSH. 
Right? This is a tool that you've written that does file copying over SSH. 
There are many things you might want to mock here. 
For example, when running your test suite, you probably don't actually care that there's a network there. 
Right? You don't need to have to like set up TCP ports and stuff, 
so instead you're gonna mock the network. 
The way this usually works is that, 
somewhere in your library, you have something that like opens a connection, or reads from the connection, or writes to the connection, 
and you're gonna overwrite those functions internally in your library with functions that you've written just for the purposes of testing, 
where the read function just like returns the data, 
and the write function just drops the data on the floor, or something like that. 
Similarly, you can write a mocking function for the SSH functionality. 
You could write something that does not actually do encryption, 
it doesn't talk to the network: it just like takes bytes in here and just magically they pop out the other side, 
and you can ignore everything that's between, because for the purpose of copying a file, if you just wanted to test that functionality, 
the stuff below doesn't matter for that test, 
and you might mock it away. 
Usually, in any given language, there are tools that let you build these kind of mocking abstractions pretty easily. 
That is the end of what I wanted to talk about metaprogramming, 
but this is a very, very broad subject. 
Things like continuous integration, build systems, there are so many out there that can let you do so many interesting things with your projects, 
so I highly recommend that you start looking into it a little. 
The exercises are sort of all over the place, 
and I mean that in a good way. 
They're intended to try to just show you the kind of possibilities that exist for building and working with these kind of processes. 
So, for example, the last exercise has you write one of these continuous integration actions yourself where you decide what the event be and you decide what the action be, 
but try to actually build one. 
And this can be something that you might find useful in your project. 
The example I gave in the exercises is to try to build an action that runs a linter for the English language on your repository, such as RightGood or Proselint. 
And if you do like, we could enable that for the class repository so that our lecture notes are actually well written. 
Right, 
and this is one other thing that's nice about this kind of continuous integration testing, is that you can collaborate between projects. 
If you write one, I can use it in my project, 
and there's a really handy feature where you can build this ecosystem of improving everything. 
Any questions about any of the stuff we recorded today? Yeah, 
so the question is, why do we have both make and CMake? What do they do, 
and is there a reason for them to talk together? 
So, CMake, I don't actually know what the tagline for CMake is anymore, 
but it's sort of like a better make for C. 
As the name implies, CMake generally understands the layout of C projects a little bit better than make files do. 
They're sort of built to try to parse out what the structure of your dependencies are, what the rules are from going to one to the other. 
It also integrates a little bit nicer with things like system libraries, 
so CMake can do things like detect given libraries available on your computer or if it's available at multiple different paths, 
it tries to find which of those paths it's present on on this system and then link it appropriately. 
So CMake is a little bit smarter than make. 
Make will only do whatever you put in the make file. 
Not entirely true, there are things called implicit rules that are like built-in rules in make, 
but they're pretty dumb, whereas CMake tries to be able to be a larger build system that is opinionated by default to work for C projects. 
Similarly, there's a tool called Maven and Ant, which is another project. 
They are both built for Java projects. 
They understand how Java code interacts with one another, how you structure Java programs, 
and they're built for that task. 
Very often, at least when I use make, I use make sort of at the top and then make my call other tools that build whatever subsystem they know how to build. 
For me, make is sort of the glue at the top that I might write. 
Usually, if your make file gets very large, there's a better tool. 
What you'll find at big companies, for example, is they often have one build system that manages all of their software. 
So if you look at Google, for example, they have this open-source system called Basil, 
and I don't think Google literally uses Basil inside of Google, 
but it's sort of based on what they use internally. 
And it's really just intended to manage the entire build of everything Google has. 
And Basil, in particular, is built to be, I think they call it, like a polyglot build framework. 
So the idea is that it works for many different languages. 
There's like an implement, there's a module for Basil for this language and that language in that language, 
but they all integrate with the same Basil framework, which then knows how to integrate dependencies between different libraries and different languages.
Got a question? Sure.
So when you say expressions, you mean the things in this file? Or yeah, 
so these are so make files are their own language. 
They are. 
It's a pretty weird language. 
Like, it has a lot of weird exceptions. 
In many ways, it's weird just like bash is weird, 
but in different ways, which is even worse. 
Like, when you're writing a make file, you sort of, you can sort of think like you're writing bash, 
but you're not because it's broken in different ways. 
But, 
but it is its own language. 
And the way that make files are generally structured is that you have a sequence of, I think they call them directives. 
So, every like, this thing (oops) this thing is a directive, 
and this is a directive. 
And every directive has a colon somewhere, 
and everything to the left of the colon is a target, 
and everything to the right of the colon is right of the colon is a dependency. 
And then all of the lines below that line are the sequence of operations known as the rules for once you have the dependencies. 
How do you build these targets? Notice that make is very particular that you must use a tab to indent the rules. 
If you do not, make will not work. 
If they must be tabs, they cannot be four eight spaces. 
Must be tabs. 
And like, you can have multiple operations here. 
I like I can do echo hello or whatever, 
and then they would first run this and then run this. 
There's a there's an exercise for today's lecture that has you try to extend this make file with a couple of other targets that you might find interesting that goes into a little bit more detail. 
There's also some ability to execute external commands to like determine what the dependencies might be if your dependencies are not like a static list of files, 
but it's a little limited. 
Usually, once you've started needing that sort of stuff, you might want to move to a more advanced build system. 
Yeah, 
so the question is what happens if I have, let's say that I have library a and library B, 
and they both depend on library C, 
but library A depends on like 4.0.1, 
and library B depends on 3.4.7. 
So they both depend on C, 
and so ideally, we'd like to reuse C, 
but they depend on different major versions of C. 
What do we do? Well, what happens in this case depends entirely on the system that you're using, the language that you're using. 
In some cases, the tool would just be like, well, I'll just pick for which sort of implies that they're not really using semantic versioning. 
In some cases, the tool will say, this is not possible. 
Like, if you do this, it's an error, 
and the tool will tell you you either need to upgrade B, like have B use a newer version of C, or you need to downgrade A. 
You do not get to do this, 
and compilation will fail. 
Some tools are gonna build two versions of C, 
and then like when it builds A, it will use the major four version of C, 
and when it builds B, it will use the major three version of C. 
One thing you end up with is really weird conditions here were like if C has dependencies, then now you have to build all of C's dependencies twice to 1 for 3 and 1 for 4, 
and maybe they share and maybe they don't. 
You can end up in particularly weird situations. 
If imagine that the library C, like imagine that library C, like rights to a file, like rights to some like file on disk, 
some cache stuff.If you run your application now and like A does something to call like C.dot save and B to something like C.adult load, 
then suddenly your application of the bottom is not going to work because the format is different, right? 
So, these situations are often very problematic, 
and most tools that support semantic versioning will reject this kind of configuration for exactly that reason. 
But it's so easy to shoot yourself in the foot. 
All right, we will see you again tomorrow for security. 
Keep in mind, again, if you haven't done the survey, the question I care the most about in the survey is what you would like us to cover in the last two lectures. 
So, the last two lectures are for you to choose what you want us to talk about and to give any questions you want us to answer. 
So please, like, add that if you can. 
And that's it. 
See you tomorrow.