I guess we should do an intro to this as well, 
so this is just sort of a free-form Q&A lecture where you as in the two people sitting here, 
but also everyone at home who did not come here in person, get to ask questions. 
And we have a bunch of questions people asked in advance, 
but you can also ask additional questions during, 
for the two of you who are here, you can do it either by raising your hand or you can submit it on the forum and be anonymous. 
It's up to you. 
Regardless though, what we're gonna do is just go through some of the questions that have been asked 
and try to give as helpful answers as we can, although they are unprepared on our side. 
And yeah, that's the plan. 
I guess we go from popular to least popular. 
Fire away!
Alright, so for our first question, any recommendations on learning operating system related topics like processes, virtual memory, interrupts, memory management, etc.? 
So I think, this is an interesting question because these are really low-level concepts that often do not matter 
unless you have to deal with this in some capacity, right? 
So one instance where this matters is if you're writing really low-level code like you're implementing a kernel or something like that, 
or you want to just hack on the Linux kernel. 
It's rare otherwise that you need to work with, especially like virtual memory and interrupts and stuff yourself. 
Processes, I think, are a more general concept that we've talked a little bit about in this class as well, 
and tools like `htop`, `pgrep`, `kill`, and signals and that sort of stuff.
In terms of learning it, maybe one of the best ways is to try to take either an introductory class on the topic. 
So for example, MIT has a class called 6.828, which is where you essentially build and develop your own operating system based on some code that you're given, 
and all of those labs are publicly available and all the resources for the class are publicly available, 
and so that is a good way to really learn them is by doing them yourself. 
There are also various tutorials online that basically guide you through how do you write a kernel from scratch. 
Not necessarily a very elaborate one, not one you would want to run any real software on, 
but just to teach you the basics. 
And so that would be another thing to look up, like how do I write a kernel in and then your language of choice. 
You will probably not find one that lets you do it in Python, 
but in like C, C++, Rust, there are a bunch of topics like this.
One other note on operating systems, 
so like Jon mentioned, MIT has a 6.828 class, 
but if you're looking for a more high-level overview, not necessarily programming or an operating system, 
but just learning about the concepts, another good resource is a book called "Modern Operating Systems" by Andy Tannenbaum. 
There's also actually a book called "The FreeBSD Operating System" which is really good. 
It doesn't go through Linux, 
but it goes through FreeBSD, 
and the BSD kernel is arguably better organized than the Linux one and better documented, 
and so it might be a gentler introduction to some of those topics than trying to understand Linux.
For our next question, what are some of the tools you'd prioritize learning first? Maybe we can all go through and give our opinion on this? Yeah.
Tools to prioritize learning first? I think learning your editor well, 
just serves you in all capacities like being efficient at editing files, is just like a majority of what you're going to spend your time doing. 
And in general, just using your keyboard more and your mouse less. 
It means that you get to spend more of your time doing useful things and less of your time moving. 
I think that would be my top priority, 
so I would say that for what tool to prioritize will depend on what exactly you're doing. 
I think the core idea is you should try to find the types of tasks that you are doing repetitively and, 
so if you are doing some sort of like machine learning workload and you find yourself using Jupyter notebooks, like the one we presented yesterday, a lot. 
Then again, using a mouse for that might not be the best idea and you want to familiarize with the keyboard shortcuts. 
And pretty much with anything, you will end up figuring out that there are some repetitive tasks, 
and you're running a computer, 
and just trying to figure out, "oh, there's probably a better way to do this," be it a terminal, be it an editor. 
And it might be really interesting to learn to use some of the topics that we have covered, 
but if they're not extremely useful on an everyday basis, then it might not be worth prioritizing them.
Out of the topics covered in this class, in my opinion, two of the most useful things are version control and text editors. 
And I think they're a little bit different from each other, in the sense that text editors I think are really useful to learn well, 
but it was probably the case that before we started using Vim 
and all its fancy keyboard shortcuts, you had some other text editor you were using before and you could edit text just fine, maybe a little bit inefficiently. 
Whereas I think version control is another really useful skill, 
and that's one where if you don't really know the tool properly, 
it can actually lead to some problems like loss of data or just inability to collaborate properly with people. 
So I think version control is one of the first things that's worth learning well.
Yeah, I agree with that. 
I think learning a tool like Git is just gonna save you so much heartache down the line. 
It also, to add on to that, really helps you collaborate with others, 
and Anish touched a little bit on GitHub in the last lecture, 
and just learning to use that tool well in order to work on larger software projects that other people are working on is an invaluable skill.
For our next question, "When do I use Python versus a Bash script versus some other language?" 
This is tough because I think this comes down to what Jose was saying earlier too, that it really depends on what you're trying to do. 
For me, I think for Bash scripts in particular, Bash scripts are for automating running a bunch of commands. 
You don't want to write any other, like, business logic in Bash. 
Like, it is just for, 'I want to run these commands, in this order... 
maybe with arguments?' But - but, like, even that, it's unclear that you want a Bash script once you start taking arguments. 
Similarly, like, once you start doing any kind of, like, text processing, or configuration, all that, reach for a language that is... 
a more serious programming language than Bash is. 
Bash is really for short, one-off scripts, or ones that have a very well-defined use case, on the terminal, in the shell, probably.
For a slightly more concrete guideline, you might say, 'Write a Bash script if it's less than a hundred lines of code or so', 
but once it gets beyond that point, Bash is kind of unwieldy, 
and it's probably worth switching to a more serious programming language, like Python. 
And, to add to that, I would say, like, I found myself writing, 
sometimes, scripts in Python, because if I have already solved some subproblem that covers part of the problem in Python, 
I find it much easier to compose the previous solution that I found out in Python than just try to reuse Bash code, that I don't find as reusable as Python.
And in the same way it's kind of nice that a lot of people have written something like Python libraries or like Ruby libraries to do a lot of these things, whereas, in Bash, 
it's kind of hard to have, like, code reuse. 
And, in fact, I think to add to that, usually, if you find a library, in some language that helps with the task you're trying to do, use that language for the job. 
And in Bash, there are no libraries. 
There are only the programs on your computer.
Bash is really hard to get right. 
It's very easy to get it right for the particular use case you're trying to solve right now, 
but, like, things like, "What if one of the filenames has a space in it?" It has caused so many bugs, 
and so many problems in Bash scripts. 
And, if you use a real programming language, then those problems just go away. 
Yes! Checked it.
For our next question, what is the difference between sourcing a script, 
and executing that script? Ooh. 
So, this, actually, we got in office hours a while back, as well, which is, 'Aren't they the same? Like, aren't they both just running the Bash script?' 
And, it is true both of these will end up executing the lines of code that are in the script.
The ways in which they differ is that sourcing a script is telling your current Bash script, your current Bash session, 
to execute that program, whereas the other one is, 'Start up a new instance of Bash, 
and run the program there, instead.' And, this matters for things like... 
Imagine that "script.sh" tries to change directories. 
If you are running the script, as in the second invocation, "./script.sh", then the new process is going to change directories. 
But, by the time that script exits, 
and returns to your shell, your shell still remains in the same place. 
However, if you do "cd" in a script, 
and you "source" it, your current instance of Bash is the one that ends up running it, 
and so, it ends up "cd"-ing where you are.
This is also why, if you define functions, for example, that you may want to execute in your shell session, 
you need to source the script, not run it, because if you run it, that function will be defined in the instance of Bash, in the Bash process that gets launched, 
but it will not be defined in your current shell. 
I think those are two of the biggest differences between the two.
Next question... 
"What are the places where various packages and tools are stored and how does referencing them work? What even is /bin or /lib?" 
So, as we covered in the first lecture, there is this PATH environment variable, 
which is like a semicolon-separated- string of all the places where your shell is gonna look for binaries.
And, if you just do something like "echo $PATH", you're gonna get this list; all these places are gonna be consulted, in order. 
It's gonna go through all of them, 
and, in fact, - There is already... 
Did we cover which? + Yeah.
So, if you run "which", 
and a specific command, the shell is actually gonna tell you where it's finding this (command). 
Beyond that, there is like some conventions where a lot of programs will install their binaries 
and they're like /usr/bin (or at least they will include symlinks) in /usr/bin so you can find them.
There's also a /usr/local/bin. 
There are special directories. 
For example, /usr/sbin it's only for sudo user and some of these conventions are slightly different between different distros. 
So I know like some distros, for example, install the user libraries under /opt for example.
Yeah, I think one thing just to talk a little bit of more about /bin and then Anish maybe you can do the other folders. 
So when it comes to /bin, the convention: there are conventions, 
and the conventions are usually /bin are for essential system utilities, /usr/bin are for user programs, 
and /usr/local/bin are for user compiled programs, 
sort of so things that you installed that you intend the user to run, are in /usr/bin, 
things that a user has compiled themselves and stuck on your system, probably goes in /usr/local/bin but again, this varies a lot from machine to machine, 
and distro to distro.
On Arch Linux, for example, /bin is a symlink to /usr/bin. 
They're the same, 
and as Jose mentioned, there's also /sbin which is for programs that are intended to only be run as root, 
that also varies from distro to distro whether you even have that directory, 
and on many systems like /usr/local/bin might not even be in your PATH, or might not even exist on your system.
On BSD on the other hand /usr/local/bin is often used a lot more heavily, yeah. 
So what we were talking about so far, these are all ways that files and folders are organized on Linux. 
Things or Linux or BSD things vary a little bit between that and macOS or other platforms.
I think for the specific locations, if you want to know exactly what it's used for, you can look it up. 
But some general patterns to keep in mind or anything with /bin in it has binary executable programs in it, anything with \lib in it, 
has libraries in it so things that programs can link against, 
and then some other things that are useful to know are there's a /etc on many systems, which has configuration files in it, 
and then there's /home, which underneath that directory contains each user's home directory so like on a Linux box my username 
or if it's Anish will correspond to a home directory /home/anish.
Yeah, I guess there are a couple of others like /tmp is usually a temporary directory that gets erased when you reboot not always but sometimes, 
you should check on your system. 
There's a /var which often holds like files the change over time so these these are usually going to be things like lock files for package managers 
they're gonna be things like log files files to keep track of process IDs 
then there's /dev which shows devices so usually so these are special files that correspond to devices on your system we talked about /sys, Anish mentioned /etc. 
/opt is a common one for just like third-party software that basically it's usually for companies ported their software to Linux 
but they don't actually understand what running software on Linux is like, 
and so they just have a directory with all their stuff in it and when those get installed they usually get installed into /opt.
I think those are the ones off the top of my head, yeah. 
And we will list these in our lecture notes, which will produce after this lecture.
Next question: Should I apt-get install a Python whatever package or pip install that package? 
So, this is a good question that I think at a higher level, 
this question is asking should I use my system's package manager to install things 
or should I use some other package manager? 
Like in this case, one that's more specific to a particular language. 
And the answer here is also kind of, it depends. 
Sometimes it's nice to manage things using a system package manager so everything can be installed and upgraded in a single place, 
but I think oftentimes whatever is available in the system repositories, 
the things you can get via a tool like apt-get or something similar might be slightly out of date compared to the more language-specific repository.
So, for example, a lot of the Python packages I use, I really want the most up-to-date version, 
and so I use pip to install them. 
Then, to extend on that, 
sometimes the case, the system packages might require some other dependencies that you might not have realized about, 
and it might also be the case, for some systems, at least for like Alpine Linux, they don't have wheels for like a lot of the Python packages, 
so it will just take longer to compile them. 
It will take more space because they have to compile them from scratch. 
Whereas if you just go to pip, pip has binaries for a lot of different platforms and that will probably work.
You should also be aware that pip might not do the exact same thing in different computers. 
So, for example, if you are in a kind of laptop or like a desktop that is running like x86 or x86_64, you probably have binaries, 
but if you're running something like Raspberry Pi or some other kind of embedded device, these are running on a different kind of hardware architecture 
and you might not have binaries. 
I think that's also good to take into account. 
In that case, it might be worthwhile to use the system packages 
just because they will take much shorter to get them than to just to compile from scratch the entire Python installation. 
Apart from that, I don't think I can think of any exceptions where I would actually use the system packages instead of the Python provided ones.
So, one other thing to keep in mind is that sometimes you will have more than one program on your computer and you might be developing more than one program on your computer, 
and for some reason, not all programs are always built with the latest version of things, 
sometimes they are a little bit behind, 
and when you install something system-wide you can only... 
depends on your exact system, 
but often you just have one version. 
What pip lets you do, especially combined with something like python's virtualenv, 
and similar concepts exist for other languages, where you can sort of say I want to (NPM does the same thing as well with its node modules, for example) 
where I'm gonna compile the dependencies of this package in sort of a subdirectory of its own, 
and all of the versions that it requires are going to be built in there and you can do this separately for separate projects 
so there they have different dependencies or the same dependencies with different versions they still sort of kept separate. 
And that is one thing that's hard to achieve with system packages.
Next question: What's the easiest and best profiling tools to use to improve performance of my code?
This is a topic we could talk about for a very long time. 
The easiest and best is to print stuff using time. 
Like, I'm not joking, very often the easiest thing is in your code. 
At the top, you figure out what the current time is, 
and then you do sort of a binary search over your program of add a print statement that prints how much time has elapsed since the start of your program, 
and then you do that until you find the segment of code that took the longest. 
And then you go into that function and then you do the same thing again, 
and you keep doing this until you find roughly where the time was spent. 
It's not foolproof, 
but it is really easy and it gives you good information quickly. 
If you do need more advanced information, Valgrind has a tool called cache-grind or call grind. 
One of the two. 
And this tool lets you run your program and measure how long everything takes and all of the call stacks, like which function called which function, 
and what you end up with is a really neat annotation of your entire program source with the heat of every line, basically how much time was spent there. 
It does slow down your program by like an order of magnitude or more, 
and it doesn't really support threads, 
but it is really useful if you can use it. 
If you can't, then tools like perf or similar tools for other languages that do usually some kind of sampling profiling 
like we talked about in the profiler lecture can give you pretty useful data quickly, 
but it's a lot of data around this, 
but they're a little bit biased and what kind of things they usually highlight as a problem, 
and it can sometimes be hard to extract meaningful information about what should I change in response to them. 
Whereas the sort of print approach very quickly gives you like this section of code is bad or slow, I think would be my answer. 
Flamegraphs are great, they're a good way to visualize some of this information. 
Yeah, I just have one thing to add, oftentimes programming languages have language-specific tools for profiling, 
so to figure out what's the right tool to use for your language, like if you're doing JavaScript in the web browser, 
the web browser has a really nice tool for doing profiling, you should just use that. 
Or if you are using go, for example, go has a built-in profiler that is really good, you should just use that. 
A last thing to add to that, 
sometimes you might find that doing this binary search over time that you're kind of finding where the time is going, 
but this time is sometimes happening because you're waiting on the network, or you're waiting for some file, 
and in that case, you want to make sure that the time that is, 
if I want to write like 1 gigabyte file or like read 1 gigabyte file and put it into memory, 
you want to check that the actual time there is the minimum amount of time you actually have to wait. 
If it's ten times longer, you should try to use some other tools 
that we covered in the debugging and profiling section to see why you're not utilizing all your resources because 
that might be a lot of what's happening thing, like for example, in my research in machine learning workloads, a lot of time is loading data, 
and you have to make sure well like the time it takes to load data is actually the minimum amount of time you want to have that happening. 
And to build on that, there are actually specialized tools for doing things like analyzing wait times. 
Very often when you're waiting for something, what's really happening is you're issuing your system call, 
and that system call takes some amount of time to respond. 
Like you do a really large write, or a really large read or you do many of them, 
and one thing that can be really handy here is to try to get information out of the kernel about where your program is spending its time. 
And so there's (it's not new), 
but there's a relatively newly available thing called BPF or eBPF. 
Which is essentially kernel tracing and you can do some really cool things with it, 
and that includes tracing user programs. 
It can be a little bit awkward to get started with, there's a tool called BPF trace that I would recommend you looking to, 
if you need to do like this kind of low-level performance debugging. 
But it is really good for this kind of stuff. 
You can get things like histograms over how much time was spent in particular system calls. 
It's a great tool.
What browser plugins do you use? I try to use as few as I can get away with using because I don't like things being in my browser, 
but there are a couple of ones that are sort of staples. 
The first one is uBlock Origin. 
So uBlock Origin is one of many ad blockers but it's a little bit more than an ad blocker. 
It is (a what do they call it?) a network filtering tool so it lets you do more things than just block ads. 
It also lets you like block connections to certain domains, block connections for certain types of resources. 
So I have mine set up in what they call the Advanced Mode, where basically you can disable basically all network requests. 
But it's not just Network requests, It's also like I have disabled all inline scripts on every page and all third-party images and resources, 
and then you can sort of create a whitelist for every page so it gives you really low-level tools around how to how to improve the security of your browsing. 
But you can also set it in not the advanced mode, 
and then it does much of the same as a regular ad blocker would do, although in a fairly efficient way if you're looking at an ad blocker 
it's probably the one to use and it works on like every browser. 
That would be my top pick I think.
I think probably the one I use like the most actively is one called Stylus. 
It lets you modify the CSS or like the stylesheets that webpages have. 
And it's pretty neat, because sometimes you're looking at a website and you want to hide some part of the website you don't care about. 
Like maybe an ad, maybe some sidebar you're not finding useful. 
The thing is, at the end of the day these things are displaying in your browser, 
and you have control of what code is executing and similar to what Jon was saying, like you can customize this to no end, 
and what I have for a lot of web pages like hide this this part, 
or also trying to make like dark modes for them like you can change pretty much the color for every single website. 
And what is actually pretty neat is that there's like a repository online of people that have contributed this is stylesheets for the websites. 
So someone probably has (done) one for GitHub. 
Like I want dark GitHub and someone has already contributed one that makes that much more pleasing to browse.
Apart from that, one that it's not really fancy, 
but I have found incredibly helpful is one that just takes a screenshot an entire website. 
And It will scroll for you and make compound image of the entire website and that's really great for when you're trying to print a website and is just terrible. 
(It's built into Firefox). 
Oh interesting! Oh now that you mention built into Firefox, another one that I really like about Firefox is the multi-account containers. 
(Oh yeah, it's fantastic!) 
Which kind of lets you By default, a lot of web browsers, like for example Chrome, have this notion of like there's session that you have, 
where you have all your cookies and they are kind of all shared from the different websites in the sense of you keep opening new tabs 
and unless you go into incognito you kind of have the same profile. 
And that profile is the same for all websites, there is this. 
Is it an extension or is it built in? (it's a mix, it's complicated). 
So I think you actually have to say you want to install it or enable it, 
and again the name is Multi Account Containers, 
and these let you tell Firefox to have separate isolated sessions. 
So for example, you want to say I have a separate sessions for whenever I visit to Google or whenever I visit Amazon, 
and that can be pretty neat, because then you can. 
At a browser level, it's ensuring that no information sharing is happening between the two of them. 
And it's much more convenient than having to open an incognito window where it's gonna clean all the time the stuff. 
(One thing to mention is Stylus vs Stylish) Oh yeah, I forgot about that. 
One important thing is the browser extension for side loading CSS Stylesheets, it's called Stylus, 
and that's different from the older one that was called Stylish because that one got bought at some point by some shady company that started abusing 
it not only to have that functionality but also to read your entire browser history and send that back to their servers so they could data mine it. 
So then people just built this open-source alternative that is called Stylus, 
and that's the one we recommend. 
Said that, I think the repository for styles is the same for the two of them, 
but I would have to double-check that.
Do you have any browser plugins, Anish?
Yes, so I also have some recommendations for browser plugins. 
I also use uBlock Origin, 
and I also use Stylus, 
but one other one that I'd recommend is integration with a password manager. 
So this is a topic that we have in the lecture notes for the security lecture, 
but we didn't really get to talk about in detail. 
But basically, password managers do a really good job of increasing your security when working with online accounts, 
and having browser integration with your password manager can save you a lot of time. 
Like you can open up a website then it can autofill your login information for you, instead of you having to go and copy and paste it back and forth 
between a separate program if it's not integrated with your web browser. 
And it can also, this integration, can save you from certain attacks that would otherwise be possible if you were doing this manual copy-pasting. 
For example, phishing attacks. 
So you find a website that looks very similar to Facebook and you go to log in with your Facebook login credentials, 
and you go to your password manager and copy-paste the correct credentials into this funny website, 
and now all of a sudden it has your password. 
But if you have browser integration, then the extension can automatically check like. 
Am I on F A C E B O O K.com or is it some other domain that maybe looks similar, 
and it will not enter the login information if it's the wrong domain. 
So browser extension for password managing is good. 
Yeah, I agree.
Next question, what are other useful data wrangling tools?
So in yesterday's lecture, I mentioned curl. 
Curl is a fantastic tool for just making web requests and dumping them to your terminal. 
You can also use it for things like uploading files, which is really handy.
In the exercises of that lecture, 
we also talked about JQ and pup, which are command line tools that let you basically write queries over JSON and HTML documents respectively that can be really handy. 
Other data wrangling tools? Ah, Perl. 
The Perl programming language is often referred to as a write-only programming language because it's impossible to read, even if you wrote it. 
But it is fantastic at doing just like straight-up text processing, like nothing beats it there. 
So, maybe worth learning some very rudimentary Perl just to write some of those scripts. 
It's easier often than writing some like hacked-up combination of grep and awk and sed, 
and it will be much faster to just tack something up than writing it up in Python, for example. 
But apart from that, other data wrangling? No, not off the top of my head. 
Really, column -t. 
If you pipe any whitespace-separated input into column -t, it will align all the whitespace of the columns so that you get nicely aligned columns. 
That's, 
and head and tail, 
but we talked about those. 
I think a couple of additions to that, that I find myself using commonly: one is Vim. 
Vim can be pretty useful for like data wrangling on itself. 
Sometimes you might find that the operation that you're trying to do is hard to put down in terms of piping different operators. 
But if you can just open the file and just record a couple of quick Vim macros to do what you want it to do, it might be like much, much easier. 
That's one, and then the other one, if you're dealing with tabular data and you want to do more complex operations like sorting by one column, 
then grouping and then computing some sort of statistic, I think a lot of that workload I ended up just using Python and pandas because it's built for that. 
And one of the pretty neat features that I find myself also using is that it will export to many different formats. 
So this intermediate state has its own kind of pandas dataframe object, 
but it can export to HTM, LaTeX, a lot of different like table formats. 
So if your end product is some sort of summary table, then pandas I think it's a fantastic choice for that. 
I would second the Vim and also Python. 
I think those are two of my most used data wrangling tools. 
For the Vim one, last year we had a demo in the series in the lecture notes, 
but we didn't cover it in class. 
We had a demo of turning an XML file into a JSON version of that same data using only Vim macros. 
And I think that's actually the way I would do it in practice. 
I don't want to go find a tool that does this conversion. 
It is actually simple to encode as a Vim macro, then I just do it that way. 
And then also Python, especially in an interactive tool like a Jupyter notebook, is a really great way of doing data wrangling. 
A third tool I'd mention which I don't remember if we covered in the data wrangling lecture or elsewhere is a tool called pandoc, 
which can do transformations between different text document formats. 
So you can convert from plaintext to HTML or HTML to markdown or LaTeX to HTML or many other formats. 
It actually supports a large list of input formats and a large list of output formats. 
I think there's one last one which I mentioned briefly in the lecture on data wrangling, which is the R programming language. 
It's an awful (I think it's an awful) language to program in, 
and I would never use it in the middle of a data wrangling pipeline. 
But at the end, in order to like produce pretty plots and statistics, R is great. 

Because R is built for doing statistics and plotting, there's a library called ggplot which is just amazing. 
ggplot2, I guess technically, it's great. 
It produces very nice visualizations and it lets you do very easily do things like if you have a data set that has like multiple facets, 
like it's not just X and Y, it's like X Y Z and some other variable, 
and then you want to plot like the throughput grouped by all of those parameters at the same time and produce a visualization. 
R very easily lets you do this, 
and I haven't seen anywhere that lets you do that as easily.
Next question, what's the difference between Docker and a virtual machine? What's the easiest way to explain this? 
So, Docker starts something called containers, 
and Docker is not the only program that starts containers. 
There are many others, 
and usually they rely on some feature of the underlying kernel. 
In the case of Docker, they use something called LXC, which are Linux containers. 
The basic premise there is if you want to start what looks like a virtual machine that is running roughly the same operating system as you are already running on your computer, 
then you don't really need to run another instance of the kernel. 
Really, that other virtual machine can share a kernel, 
and you can just use the kernel's built-in isolation mechanisms to spin up a program that thinks it's running on its hardware, 
but in reality, it's sharing the kernel. 
And so this means that containers can often run with much lower overhead than a full virtual machine will do. 
But you should keep in mind that it also has somewhat weaker isolation because you are sharing a kernel between the two. 
If you spin up a virtual machine, the only thing that's shared is sort of the hardware and to some extent, the hypervisor, 
whereas with a Docker container, you're sharing the full kernel, 
and that is a different threat model that you might have to keep in mind.
One another small note there as Jon pointed out, to use containers, 
something like Docker, you need the underlying operating system to be roughly the same as whatever the program that's running on top of the container expects. 
And so if you're using macOS, for example, the way you use Docker is you run Linux inside a virtual machine, 
and then you can run Docker on top of Linux. 
So maybe if you're going for containers in order to get better performance, you're trading isolation for performance. 
If you're running on macOS, that may not work out exactly as expected. 
And one last note is that there is a slight difference. 
So with Docker and containers, one of the gotchas you have to be familiar with is that 
containers are more similar to virtual machines in the sense that they will persist all the storage that you have, whereas Docker by default won't have that. 
Like Docker is supposed to be running, 
so the main idea is like I want to run some software, 
and I get the image, 
and it runs, 
and if you want to have any kind of persistent storage that links to the host system, 
you have to kind of manually specify that, whereas a virtual machine is using some virtual disk that is being provided.
Next question, what are the advantages of each operating system, 
and how can we choose between them? For example, choosing the best Linux distribution for our purposes. 
I will say that for many, many tasks, 
The specific Linux distribution that you're running is not that important. 
The thing is, it's just what kind of knowing that there are different types or like groups of distributions. 
So, for example, there are some distributions that have really frequent updates, 
but they kind of break more easily. 
So, for example, Arch Linux has a rolling update way of pushing updates, 
where things might break but they're fine with the things being that way. 
Where maybe where you have some really important web server that is hosting all your business analytics, 
you want that thing to have like a much more steady way of updates. 
So that's, for example, why you will see distributions like Debian being much more conservative about what they push, 
or even for example Ubuntu makes a difference between the Long Term Releases that they are only updated every two years 
and the more periodic releases of one there is - it's like two a year that they make. 
So, kind of knowing that there's the difference. 
Apart from that, 
some distributions have different ways of providing the binaries to you and the way they have the repositories. 
So, I think a lot of Red Hat Linux don't want non-free drivers in their official repositories where I think Ubuntu is fine with some of them. 
Apart from that, I think like just a lot of what is core to most Linux distros is kind of shared between them, 
and there's a lot of learning in the common ground. 
So, you don't have to worry about the specifics.
Keeping with the theme of this class being somewhat opinionated, I'm gonna go ahead and say that if you're using Linux, especially for the first time, 
choose something like Ubuntu or Debian. 
So, you Ubuntu too is a Debian-based distribution but maybe is a little bit more friendly. 
Debian is a little bit more minimalist. 
I use Debian and all my servers, for example. 
And I use Debian desktop on my desktop computers that run Linux. 
If you're going for maybe trying to learn more things and you want a distribution that trades stability for having more up-to-date software 
maybe at the expense of you having to fix a broken distribution every once in a while, then maybe you can consider something like Arch Linux or Gentoo or Slackware. 
Oh man, I'd say that like if you're installing Linux and just like want to get work done, Debian is a great choice. 
Yeah, I think I agree with that.
The other observation is like you couldn't install BSD. 
BSD has come a long way from where it was. 
There's still a bunch of software you can't really get for BSD, 
but it gives you a very well-documented experience, 
and one thing that's different about BSD compared to Linux is that in BSD when you install BSD, you get a full operating system, mostly. 
So many of the programs are maintained by the same team that maintains the kernel and everything is sort of upgraded together, 
which is a little different than how things work in the Linux world. 
It does mean that things often move a little bit slower. 
I would not use it for things like gaming either because driver support is meh. 
But it is an interesting environment to look at.
And then for things like Mac OS and Windows, I think if you are a programmer, 
I don't know why you are using Windows unless you are building things for Windows, 
or you want to be able to do gaming and stuff, 
but in that case, maybe try dual booting, even though that's a pain too. 
Mac OS is a good sort of middle point between the two where you get a system that is relatively nicely polished for you. 
But you still have access to some of the lower-level bits at least to a certain extent. 
It's also really easy to dual boot Mac OS and Windows.It is not quite the case with like Mac OS and Linux or Linux and Windows. 
Alright, for the rest of the questions, 
so these are all 0 upvote questions, 
so maybe we can go through them quickly in the last five or so minutes of class. 
So the next one is Vim versus Emacs? Vim! Easy answer, 
but a more serious answer is like I think all three of us use vim as our primary editor. 
I use Emacs for some research-specific stuff which requires Emacs, 
but at a higher level, both editors have interesting ideas behind them. 
And if you have the time, it's worth exploring both to see which fits you better. 
Also, you can use Emacs and run it in a vim emulation mode. 
I actually know a good number of people who do that, 
so they get access to some of the cool Emacs functionality and some of the cool philosophy behind that. 
Like Emacs is programmable through Lisp, which is kind of cool. 
Much better than vimscript, 
but people like vim's modal editing, 
so there's an emacs plugin called evil mode which gives you vim modal editing within Emacs. 
So it's not necessarily a binary choice, you can kind of combine both tools if you want to. 
And it's worth exploring both if you have the time.
Next question: Any tips or tricks for machine learning applications? I think, like knowing how a lot of these tools, 
mainly the data wrangling a lot of the shell tools, 
it's really important because it seems a lot of what you're doing as a machine learning researcher is trying different things. 
But I think one core aspect of doing that, 
and like a lot of scientific work, is being able to have reproducible results and logging them in a sensible way. 
So for example, instead of trying to come up with really hacky solutions of how you name your folders to make sense of the experiments, 
maybe it's just worth having, for example, what I do is have like a JSON file that describes the entire experiment. 
I know like all the parameters that are within 
and then I can really quickly, using the tools that we have covered, 
query for all the experiments that have some specific purpose or use some dataset. 
Things like that. 
Apart from that, the other side of this is if you are running kind of things for training machine learning applications 
and you are not already using some sort of cluster, like university or your company is providing 
and you're just kind of manually sshing, like a lot of labs do because that's kind of the easy way. 
It's worth automating a lot of that job because it might not seem like it but manually doing a lot of these operations takes away a lot of your time 
and also kind of your mental energy for running these things.
Anymore vim tips? I have one. 
So in the vim lecture we tried not to link you to too many different vim plugins because we didn't want that lecture to be overwhelming. 
But I think it's actually worth exploring vim plugins because there are lots and lots of really cool ones out there. 
One resource you can use is the different instructors dotfiles. 
Like a lot of us, I think I use like two dozen vim plugins and I find a lot of them quite helpful and I use them every day. 
We all use slightly different subsets of them. 
So go look at what we use or look at some of the other resources we've linked to and you might find some stuff useful. 
A thing to add to that is, I don't think we went into a lot of detail in the lecture, correct me if I'm wrong. 
It's getting familiar with the leader key which is kind of a special key that a lot of programs will especially plugins, that will link to 
and for a lot of the common operations vim has short ways of doing it, 
But you can just figure out like quicker versions for doing them. 
So for example, like I know that you can do like semicolon WQ to save 
and exit or that you can do like capital ZZ but I just actually just do leader (which for me is the space) and then W.
And I have done that for a lot of a lot of kind of common operations that I keep doing all the time. 
Because just saving one keystroke for an extremely common operation is just saving thousands a month.
Yeah just to expand a little bit on what the leader key is, 
so in vim you can bind some keys. 
I can do like ctrl J does something like holding one key and then pressing another. 
I can bind that to something or I can bind a single keystroke to something.
What the leader key lets you do, is bind. 
So you can assign any key to be the leader key and then you can assign leader followed by some other key to some action. 
So for example, like Jose's leader key is space and they can combine space 
and then releasing space followed by some other key to an arbitrary vim command. 
It just gives you yet another way of binding like a whole set of key combinations. 
Leader key plus kind of any key on the keyboard to some functionality.
I think I've I forget whether we covered macros in the vim uh sure but like vim macros are worth learning. 
They're not that complicated but knowing that they're there and knowing how to use them is going to save you so much time.
The other one is something called marks. 
So in vim you can press m and then any letter on your keyboard to make a mark in that file 
and then you can press apostrophe on the same letter to jump back to the same place. 
This is really useful if you're like moving back and forth between two different parts of your code for example. 
You can mark one as A and one as B and you can then jump between them with tick A and tick B.
There's also Ctrl+O which jumps to the previous place you were in the file no matter what caused you to move. 
So for example if I am in a some line and then I jump to B and then I jump to A, Ctrl+O will take me back to B and then back to the place I originally was. 
This can also be handy for things like if you're doing a search then the place that you started the search is a part of that stack. 
So I can do a search I can then like step through the results and like change them and then Ctrl+O all the way back up to the search. 
Ctrl+O also lets you move across files so if I go from one file to somewhere else in different file 
and somewhere else in the first file Ctrl+O will move me back through that stack and then there's Ctrl+I to move forward in that stack. 
So it's not as though you pop it and it goes away forever.
The command colon earlier is really handy. 
So, colon earlier gives you an earlier version of the same file and it does this based on time not based on actions. 
So for example if you press a bunch of like undo and redo and make some changes and stuff, 
earlier will take a literally earlier as in time version of your file and restore it to your buffer. 
This can sometimes be good if you like undid and then rewrote something and then realize you actually wanted the version 
that was there before you started undoing earlier lets you do this.
And there's a plug-in called undo tree or something like that. 
There are several of these, that let you actually explore the full tree of undo history the vim keeps,
Because it doesn't just keep a linear history, it actually keeps the full tree. 
Letting you explore that might in some cases save you from having to re-type stuff you typed in the past or stuff 
you just forgot exactly what you had there that used to work and no longer works. 
And this is one final one I want to mention, which is, we mentioned how in vim you have verbs and nouns, right? 
To your verbs like delete or yank, 
and then you have nouns like next of this character or percent to swap brackets and that sort of stuff. 
The search command is a noun, 
so you can do things like D slash and then a string, 
and it will delete up to the next match of that pattern. 
This is extremely useful and I use it all the time. 
One another neat addition on the undo stuff that I find incredibly valuable in an everyday basis is that 
like one of the built-in functionalities of vim is that you can specify an undo directory. 
And if you have specified an undo directory, by default vim, if you don't have this enabled, whenever you enter a file, your undo history is clean. 
There's nothing in there. 
And as you make changes and then undo them, you kind of create this history, 
but as soon as you exit the file, that's lost. 
Sorry, as soon as you exit vim, that's lost. 
However, if you have an undodir, vim is gonna persist all those changes into this directory, 
so no matter how many times you enter and leave that history is persisted, 
and it's incredibly helpful because even like it can be very helpful for some files that you modify often because then you can kind of keep the flow. 
But it's also sometimes really helpful if you modify your bashrc see and something broke like five days later and then you've vim again. 
Like what actually did I change, if you don't have say like version control, then you can just check the undos and that's actually what happened. 
And the last one, it's also really worth familiarizing yourself with registers and what different special registers vim uses. 
So for example, if you want to copy/paste really that's gone into in a specific register, 
and if you want to for example use the a OS a copy like the OS clipboard, you should be copying or yanking copying and pasting from a different register, 
and there's a lot of them. 
And yeah, I think that you should explore, there's a lot of things to know about registers. 
The next question is asking about two-factor authentication, 
and I'll just give a very quick answer to this one in the interest of time. 
So it's worth using two-factor auth for anything security-sensitive.
So I use it for my GitHub account and for my email and stuff like that. 
And there's a bunch of different types of two-factor auth. 
From SMS based to factor auth where you get special like a number texted to you when you try to log in you have to type that number 
and to other tools like universal to factor this is like those Yubikeys that you plug into your you have to tap it every time you login so not all, 
(yeah Jon is holding a Yubikey), not all two-factor auth is created equal and you really want to be using something like U2F rather than SMS based to factor auth. 
There something based on one-time pass codes that you have to type in 
we don't have time to get into the details of why some methods are better than others but at a high level use U2F 
and the Internet has plenty of explanations for why other methods are not a great idea.
Differences between web browsers, there are fewer and fewer differences between web browsers these day. 
At this point almost all web browsers are chrome Either because you're using Chrome or because you're using a browser that's using the same browser engine as Chrome. 
It's a little bit sad, one might say, 
but I think these days whether you choose Chrome is a great browser for security reasons 
if you want to have something that's more customizable or you don't want to be tied to Google then use Firefox, don't use Safari it's a worse version of Chrome. 
The new Internet Explorer edge is pretty decent and also uses the same browser engine as Chrome 
and that's probably fine although avoid it if you can because it has some like legacy modes you don't want to deal with. 
I think that's Oh, there's a cool new browser called flow that you can't use for anything useful yet 
but they're actually writing their own browser engine 
and that's really neat Firefox also has this project called servo which is they're really implementing their browser engine in Rust 
in order to write it to be like super concurrent 
and what they've done is they've started to take modules from that version 
and port them over to gecko or integrate them with gecko which is the main browser engine for Firefox 
just to get those speed ups there as well and that's a neat neat thing you can be watching out for.
That is all the questions, hey we did it. 
Nice I guess thanks for taking the missing semester class and let's do it again next year.