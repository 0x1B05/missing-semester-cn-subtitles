So welcome back. Today we are gonna cover debugging and profiling. 
Before I get into it we're gonna make another reminder to fill in the survey.
Just one of the main things we want to get from you is questions, because the last day
is gonna be questions from you guys: about things that we haven't covered, or like you want us to kind of talk more in depth.
The more questions we get, the more interesting we can make that section, so please go on and fill in the survey.
So today's lecture is gonna be a lot of topics. All the topics revolve around the concept of
what do you do when you have a program that has some bugs. Which is most of the time, like when you are programming, you're kind of thinking
about how you implement something and there's like a half life of fixing all the issues that that program has. 
And even if your program behaves like you want, it might be that it's
really slow, or it's taking a lot of resources in the process. 
So today we're gonna see a lot of different approaches of dealing with these problems.
So first, the first section is on debugging. Debugging can be done in many different ways,
there are all kinds of... The most simple approach that, pretty much, all
CS students will go through, will be just: you have some code, 
and it's not behaving like you want, so you probe the code by adding
print statements. This is called "printf debugging" and it works pretty well. Like, I have to be honest,
I use it a lot of the time because of how simple to set up and how quick the feedback can be.
One of the issues with printf debugging is that you can get a lot of output and maybe you don't want
to get as much output as you're getting. There has... people have thought of slightly more complex ways of doing printf debugging and
one of these ways is what is usually referred to as "logging". 
So the advantage of doing logging versus doing printf debugging is that, when you're creating logs,
you're not necessarily creating the logs because there's a specific issue you want to fix; it's mostly because you have built a
more complex software system and you want to log when some events happen. One of the core advantages of using a logging library is that
you can can define severity levels, 
and you can filter based on those.
Let's see an example of how we can do something like that. Yeah, everything fits here. This is a really silly example:
We're just gonna sample random numbers and, depending on the value of the number, that we can interpret as a kind of "how wrong things are going".
We're going to log the value of the number and then we can see what is going on.
I need to disable these formatters...
And if we were just to execute the code as it is, we just get the output and we just keep getting more and more output.
But you have to kind of stare at it and make sense of what is going on, 
and we don't know
what is the relative timing between printfs, we don't really know whether this is just an information message
or a message of whether something went wrong. If we just go in,
and undo, not that one...
That one, we can set that formatter. Now the output looks something more like this
So for example, 
if you have several different modules that you are programming with, you can identify them with like different levels.
Here, we have, we have debug levels, we have critical info, different levels. 
And it might be handy because here we might only care about the error messages.
Like those are like, the... We have been working on our code, so far so good, 
and suddenly we get some error.
We can log that to identify where it's happening. But maybe there's a lot of information messages, 
but we can deal with that
by just changing the level to error level. 
And now if we were to run this again, we are only going to get those errors in the output, 
and we can just look through those to make sense of what is going on.
Another really useful tool when you're dealing with logs is
As you kind of look at this, it has become easier because now we have this critical and error levels that we can quickly identify.
But since humans are fairly visual creatures,
one thing that you can do is use colors from your terminal to identify these things. 
So now, changing the formatter,
what I've done is slightly change how the output is formatted.
When I do that, now whenever I get a warning message, it's color coded by yellow;
whenever I get like an error, faded red; and when it's critical, I have a bold red indicating something went wrong.
And here it's a really short output, 
but when you start having thousands and thousands of lines of log,
which is not unrealistic and happens every single day in a lot of apps, quickly browsing through them and identifying where the error or the red patches are
can be really useful. A quick aside is, you might be curious about how the terminal is displaying these colors.
At the end of the day, the terminal is only outputting characters.
Like, how is this program or how are other programs, like LS, that has all these fancy colors. How are they telling the terminal that it should use these different colors?
This is nothing extremely fancy, what these tools are doing, is something along these lines.
Here we have... I can clear the rest of the output, so we can focus on this. There's some special characters, some escape characters here,
then we have some text and then we have some other special characters. 
And if we execute this line
we get a red "This is red". 
And you might have picked up on the fact that we have a "255;0;0" here,
this is just telling the RGB values of the color we want in the terminal. 
And you pretty much can do this in any piece of code that you have, 
and like that you can color code the output.
Your terminal is fairly fancy and supports a lot of different colors in the output. This is not even all of them, this is like a sixteenth of them.
I think it can be fairly useful to know about that.
Another thing is maybe you don't enjoy or you don't think logs are really fit for you.
The thing is a lot of other systems that you might start using will use logs. As you start building larger and larger systems,
you might rely on other dependencies. Common dependencies might be web servers or databases, it's a really common one.
And those will be logging their errors or exceptions in their own logs.
Of course, you will get some client-side error, 
but those sometimes are not informative enough for you to figure out what is going on.
In most UNIX systems, the logs are usually placed under a folder called "/var/log"
and if we list it, we can see there's a bunch of logs in here.
So we have like the shutdown monitor log, or some weekly logs.
Things related to the Wi-Fi, for example. 
And if we output the
System log, which contains a lot of information about the system, we can get information about what's going on.
Similarly, there are tools that will let you more sanely go through this output. But here, looking at the system log,
I can look at this, 
and say: oh there's some service that is exiting with some abnormal code
and based on that information, I can go and try to figure out what's going on,
like what's going wrong. One thing to know when you're working with logs is that
more traditionally, every software had their own log, 
but it has been increasingly more popular to have a unified system log where everything is placed.
Pretty much any application can log into the system log, 
but instead of being in a plain text format,
it will be compressed in some special format. An example of this, it was what we covered in the data wrangling lecture.
In the data wrangling lecture we were using the "journalctl", which is accessing the log and outputting all that output.
Here in Mac, now the command is "log show", which will display a lot of information.
I'm gonna just display the last ten seconds, because logs are really, really verbose and
just displaying the last 10 seconds is still gonna output a fairly large amount of lines.
So if we go back through what's going on, we here see that a lot of Apple things are going on, since this is a macbook.
Maybe we could find errors about like some system issue here.
Again they're fairly verbose, so you might want to practice your data wrangling techniques here,
like 10 seconds equal to like 500 lines of logs, so you can kind of get an idea of how many lines per second you're getting.
They're not only useful for figuring out some other programs' output, they're also useful for you, 
if you want to log there instead of into your own file.
So using the "logger" command, in both linux and mac,
You can say okay I'm gonna log this "Hello Logs" into this system log.
We execute the command and then we can check by going through the last minute of logs,
since it's gonna be fairly recent, 
and grepping for that "Hello" we find our entry. Fairly recent entry, that we just created that said "Hello Logs".
As you become more and more familiar with these tools, you will find yourself using
the logs more and more often, since even if you have some bug that you haven't detected, 
and the program has been running for a while,
maybe the information is already in the log and can tell you enough to figure out what is going on.
However, printf debugging is not everything. 
So now I'm going to be covering debuggers.
But first any questions on logs so far? So what kind of things can you figure out from the logs?
like this Hello Logs says that you did something with Hello at that time? Yeah, like say, for example, I can write a bash script that detects...
Well, that checks every time what Wi-Fi network I'm connected to. 
And every time it detects that it has changed, it makes an entry in the logs and says
Oh now it looks like we have changed Wi-Fi networks. 
and then you might go back and parse through the logs and take like, okay
When did my computer change from one Wi-Fi network to another. 
And this is just kind of a simple example
But there are many, many ways, many types of information that you could be logging here.
More commonly, you will probably want to check if your computer, for example, is
entering sleep, for example, for some unknown reason. Like it's on hibernation mode.
There's probably some information in the logs about who asked that to happen, or why it's that happening.
Any other questions? Okay. 
So when printf debugging is not enough,
the best alternative after that is using...
[Exit that]
So, it's using a debugger. 
So a debugger is a tool that will wrap around your code and will let you run your code,
but it will kind of keep control over it. 
So it will let you step through the code and execute it and set breakpoints.
You probably have seen debuggers in some way, 
if you have ever used something like an IDE, because IDEs have this kind of fancy: set a breakpoint here, execute, ...
But at the end of the day what these tools are using is just these command line debuggers and they're just presenting them in a really fancy format.
Here we have a completely broken bubble sort, a simple sorting algorithm.
Don't worry about the details, 
but we just want to sort this array that we have here.
We can try doing that by just doing Python bubble.py
And when we do that... Oh there's some index error, list index out of range. We could start adding prints
but if have a really long string, we can get a lot of information. 
So how about we go up to the moment that we crashed?
We can go to that moment and examine what the current state of the program was.
So for doing that I'm gonna run the program using the Python debugger.
Here I'm using technically the ipython debugger, just because it has nice coloring syntax so it's probably easier for both of us to understand
what's going on in the output. But they're pretty much identical anyway.
So we execute this, 
and now we are given a prompt where we're being told that we are here, at the very first line of our program.
And we can... "L" stands for "List", so as with many of these tools
there's kind of like a language of operations that you can do, 
and they are often mnemonic, as it was the case with VIM or TMUX.
So here, "L" is for "Listing" the code, 
and we can see the entire code.
"S" is for "Step" and will let us kind of one line at a time, go through the execution.
The thing is we're only triggering the error some time later.
So we can restart the program and instead of trying to step until we get to the issue,
we can just ask for the program to continue which is the "C" command and
hey, we reached the issue. We got to this line where everything crashed,
we're getting this list index out of range. 
And now that we are here we can say, huh?
Okay, first, let's print the value of the array. This is the value of the current array
So we have six items. Okay. What is the value of "J" here? So we look at the value of "J". "J" is 5 here, which will be the last element, 
but
"J" plus 1 is going to be 6, so that's triggering the out of bounds error.
So what we have to do is this "N", instead of "N" has to be "N minus one". We have identified that the error lies there.
So we can quit, which is "Q". Again, because it's a post-mortem debugger.
We go back to the code and say okay,
we need to append this "N minus one". That will prevent the list index out of range and
if we run this again without the debugger, okay, no errors now. But this is not our sorted list.
This is sorted, 
but it's not our list. We are missing entries from our list, so there is some behavioral issue that we're reaching here.
Again, we could start using printf debugging but kind of a hunch now is that probably the way we're swapping entries in the bubble sort program is wrong.
We can use the debugger for this. We can go through them to the moment we're doing a swap and
check how the swap is being performed. 
So a quick overview, we have two for loops and in the most nested loop,
we are checking if the array is larger than the other array. The thing is if we just try to execute until this line,
it's only going to trigger whenever we make a swap. 
So what we can do is we can set a breakpoint in the sixth line.
We can create a breakpoint in this line and then the program will execute and the moment we try to swap variables is when the program is going to stop.
So we create a breakpoint there and then we continue the execution of the program. The program halts
and says hey, I have executed and I have reached this line. Now I can use "locals()", which is a Python function that returns a dictionary with all the values
to quickly see the entire context. The string, the array is fine and is six, again, just the beginning and
I step, go to the next line. Oh, 
and I identify the issue: I'm swapping one item at a time, instead of simultaneously,
so that's what's triggering the fact that we're losing variables as we go through.
That's kind of a very simple example, 
but debuggers are really powerful.
Most programming languages will give you some sort of debugger, 
and when you go to more low level debugging you might run into tools like...
You might want to use something like GDB.
And GDB has one nice property: GDB works really well with C/C++ and all these C-like languages.
But GDB actually lets you work with pretty much any binary that you can execute. 
So for example here we have sleep, 
which is just a program that's going to sleep for 20 seconds.
It's loaded and then we can do run, 
and then we can interrupt this sending an interrupt signal.
And GDB is displaying for us, here, very low-level information about what's going on in the program.
So we're getting the stack trace, we're seeing we are in this nanosleep function,
we can see the values of all the hardware registers in your machine. 
So you can get a lot of low-level detail using these tools.
I think that's all I want to cover for debuggers. Any questions related to that?
Another interesting tool when you're trying to debug is that sometimes you want to debug as if
your program is a black box. 
So you, maybe, know what the internals of the program but at the same time
your computer knows whenever your program is trying to do some operations.
So this is in UNIX systems, there's this notion of like user level code and kernel level code.
And when you try to do some operations like reading a file or like reading the network connection
you will have to do something called system calls. You can get a program and go through those operations and ask
what operations did this software do? So for example, 
if you have like a Python function
that is only supposed to do a mathematical operation and you run it through this program,
and it's actually reading files, Why is it reading files? It shouldn't be reading files. 
So, let's see.
This is "strace". 
So for example, we can do it something like this. 
So here we're gonna run the "LS - L"
And then we're ignoring the output of LS, 
but we are not ignoring the output of STRACE.
So if we execute that... We're gonna get a lot of output.
This is all the different system calls
That this LS has executed. You will see a bunch of OPEN, you will see FSTAT.
And for example, since it has to list all the properties of the files that are in this folder, we can
check for the LSTAT call. 
So the LSTAT call will check for the properties of the files and
we can see that, effectively, all the files and folders that are in this directory
have been accessed through a system call, through LS.
Interestingly, sometimes you actually don't need to run your code to
figure out that there is something wrong with your code. 
So far we have seen enough ways of identifying issues by running the code,
but what if you... you can look at a piece of code like this, like the one I have shown right now in this screen,
and identify an issue. 
So for example here, we have some really silly piece of code. It defines a function, prints a few variables,
multiplies some variables, it sleeps for a while and then we try to print BAZ. 
And you could try to look at this and say, hey, BAZ has
never been defined anywhere. This is a new variable. You probably meant to say BAR
but you just mistyped it. Thing is, 
if we try to run this program,
it's gonna take 60 seconds, because like we have to wait until this time.sleep function finishes. Here, sleep is just for
motivating the example but in general you may be loading a data set that takes really long because you have to copy everything into memory.
And the thing is, there are programs that will take source code as input, will process it and will say, oh probably this is wrong about this piece of code. 
So in Python,
or in general, these are called static analysis tools.
In Python we have for example pyflakes. If we get this piece of code and run it through pyflakes,
pyflakes is gonna give us a couple of issues. First one is the one.... The second one is the one we identified: here's an undefined name called BAZ.
You probably should be doing something about that. 
And the other one is like oh, you're redefining the
the FOO variable name in that line. 
So here we have a FOO function and then we are kind of
shadowing that function by using a loop variable here. 
So now that FOO function that we defined is not accessible anymore
and then if we try to call it afterwards, we will get into errors.
There are other types of Static Analysis tools. MYPY is a different one. MYPY is gonna report the same two errors, 
but it's also
going to complain about type checking. 
So it's gonna say, oh here you're multiplying an int by a float and
if you care about the type checking of your code, you should not be mixing those up.
it can be kind of inconvenient, having to run this, look at the line, going back to your
VIM or like your editor, 
and figuring out what the error matches to.
There are already solutions for that. One way is that you can integrate most editors with these tools and here..
You can see there is like some red highlighting on the bash, 
and it will read the last line here.
So, undefined named 'baz'. 
So as I'm editing this piece of Python code,
my editor is gonna give me feedback about what's going wrong with this. Or like here have another one saying the redefinition of unused foo.
And even, there are some stylistic complaints.
So, oh, I will expect two empty lines. 
So like in Python, you should be having two empty lines between a function definition.
There are... there is a resource on the lecture notes about pretty much static analyzers for a lot of different programming languages.
There are even static analyzers for English.
So I have my notes
for the class here, 
and if I run it through this static analyzer for English, that is "writegood".
It's going to complain about some stylistic properties. 
So like, oh, I'm using "very", which is a weasel word and I shouldn't be using it.
Or "quickly" can weaken meaning, 
and you can have this for spell checking, or for a lot of different
types of stylistic analysis.
Any questions so far?
Oh, I forgot to mention... Depending on the task that you're performing, there will be different types of debuggers.
For example, 
if you're doing web development, both Firefox and Chrome
have a really really good set of tools for doing debugging for websites.
So here we go and say inspect element, we can get the... do you know? how to make this larger...
We're getting the entire source code for the web page for the class.
Oh, yeah, here we go. Is that better?
And we can actually go and change properties about the course. 
So we can say... we can edit the title.
Say, this is not a class on debugging and profiling. 
And now the code for the website has changed.
This is one of the reasons why you should never trust any screenshots of websites, because they can be completely modified.
And you can also modify this style. Like, here I have things using the
the dark mode preference, 
but we can alter that. Because at the end of the day, the browser is rendering this for us.
We can check the cookies, 
but there's like a lot of different operations. There's also a built-in debugger for JavaScript, so you can step through JavaScript code.
So kind of the takeaway is, depending on what you are doing, you will probably want to search for what tools
programmers have built for them.
Now I'm gonna switch gears and stop talking about debugging, which is kind of finding issues with the code, right?
kind of more about the behavior, 
and then start talking about like how you can use profiling.
And profiling is how to optimize the code. It might be because you want to optimize the CPU, the memory, the network, ...
There are many different reasons that you want to be optimizing it. As it was the case with debugging, the kind of first-order approach
that a lot of people have experience with already is oh, let's use just printf profiling, so to say, like we can just take...
Let me make this larger. We can take the current time here,
then we can check, we can do some execution and then we can take the time again and
subtract it from the original time. 
And by doing this you can kind of narrow down and fence some different parts of your code 
and try to figure out what is the time taken between those two parts.
And that's good. But sometimes it can be interesting, the results. 
So here, we're sleeping for
0.5 seconds and the output is saying, oh it's 0.5 plus some extra time,
which is kind of interesting. 
And if we keep running it, we see there's like some small error and the thing is
here, what we're actually measuring is what is usually referred to as the "real time".
Real time is as if you get like a clock, 
and you start it when your program starts, 
and you stop it when your program ends.
But the thing is, in your computer it is not only your program that is running. There are many other programs running at the same time and those might
be the ones that are taking the CPU. 
So, to try to make sense of that,
A lot of... you'll see a lot of programs using the terminology that is
real time, user time and system time. Real time is what I explained, which is kind of the entire length of time from start to finish.
Then there is the user time, which is the amount of time your program spent on the CPU doing user level cycles.
So as I was mentioning, in UNIX, you can be running user level code or kernel level code.
System is kind of the opposite, it's the amount of CPU, like the amount of time that your program spent on the CPU
executing kernel mode instructions. 
So let's show this with an example.
Here I'm going to "time", which is a command, a shell command that's gonna get these three metrics for the following command, 
and then I'm just
grabbing a URL from a website that is hosted in Spain. 
So that's gonna take some extra time to go over there and then go back.
If we see, here, 
if we were to just... We have two prints, between the beginning and the end of the program.
We could think that this program is taking like 600 milliseconds to execute, 
but actually
most of that time was spent just waiting for the response on the other side of the network and
we actually only spent 16 milliseconds at the user level and like 9 seconds, in total 25 milliseconds, actually
executing CURL code. Everything else was just waiting.
Any questions related to timing?
Ok, so timing can be can become tricky, it's also kind of a black box solution. Or if you start adding print statements,
it's kind of hard to add print statements, with time everywhere. 
So programmers have figured out better tools.
These are usually referred to as "profilers". One quick note that I'm gonna make, is that
profilers, like usually when people refer to profilers they usually talk about CPU profilers because they are the most common, 
at identifying where like time is being spent on the CPU.
Profilers usually come in kind of two flavors: there's tracing profilers and sampling profilers.
and it's kind of good to know the difference because the output might be different.
Tracing profilers kind of instrument your code. 
So they kind of execute with your code and every time your code enters a function call,
they kind of take a note of it. It's like, oh we're entering this function call at this moment in time and
they keep going and, once they finish, they can report oh, you spent this much time executing in this function and
this much time in this other function. 
So on, so forth, which is the example that we're gonna see now.
Another type of tools are tracing, sorry, sampling profilers. The issue with tracing profilers is they add a lot of overhead. 
Like you might be running your code and having these kind of
profiling next to you making all these counts, will hinder the performance of your program, so you might get counts that are slightly off.
A sampling profiler, what it's gonna do is gonna execute your program and every 100 milliseconds, 10 milliseconds, like some defined period, 
it's gonna stop your program. It's gonna halt it,
it's gonna look at the stack trace and say, oh, you're right now in this point in the hierarchy, 
and
identify which function is gonna be executing at that point. The idea is that as long as you execute this for long enough,
you're gonna get enough statistics to know where most of the time is being spent.
So, let's see an example of a tracing profiling. 
So here we have a piece of code that is just like a
really simple re-implementation of grep done in Python. What we want to check is what is the bottleneck of this program? Like we're just opening a bunch of files,
trying to match this pattern, 
and then printing whenever we find a match. 
And maybe it's the regex, maybe it's the print...
We don't really know. 
So to do this in Python, we have the "cProfile".
And here I'm just calling this module and saying I want to sort this by the total amount of time, that
we're gonna see briefly. I'm calling the program we just saw in the editor.
I'm gonna execute this a thousand times and then I want to match (the grep
Arguments here) is I want to match these regex to all the Python files in here. 
And this is gonna output some...
This is gonna produce some output, then we're gonna look at it. First, is all the output from the greps, 
but at the very end, we're getting
output from the profiler itself. If we go up
we can see that, hey, by sorting we can see that the total number of calls. 
So we did 8000 calls, because we executed this 1000 times and
this is the total amount of time we spent in this function (cumulative time). 
And here we can start to identify
where the bottleneck is. 
So here, this built-in method IO open, is saying that we're spending a lot of the time just waiting for
reading from the disk or... There, we can check, hey, a lot of time is also being spent trying to match the regex.
Which is something that you will expect. One of the caveats of using this
tracing profiler is that, as you can see, here we're seeing our function but we're also seeing a lot of functions that correspond to built-ins.
So like, functions that are third party functions from the libraries. 
And as you start building more and more complex code,
This is gonna be much harder. 
So here is another piece of Python code that,
don't read through it, what it's doing is just grabbing the course website and then it's printing all the...
It's parsing it, 
and then it's printing all the hyperlinks that it has found. 
So there are like these two operations:
going there, grabbing a website, 
and then parsing it, printing the links. 
And we might want to get a sense of
how those two operations compare to each other. If we just try to execute the
cProfiler here and we're gonna do the same, this is not gonna print anything. I'm using a tool we haven't seen so far,
but I think it's pretty nice. It's "TAC", which is the opposite of "CAT", 
and it is going to reverse the output so I don't have to go up and look.
So we do this and... Hey, we get some interesting output.
we're spending a bunch of time in this built-in method socket_getaddr_info and like in _imp_create_dynamic and
method_connect and posix_stat... nothing in my code is directly calling these functions so I don't really know what is the split between the operation of
making a web request and parsing the output of that web request. 
So, for that, we can use
a different type of profiler which is a line profiler. 
And the line profiler is just going to present the same results
but in a more human-readable way, which is just, for this line of code, this is the amount of time things took.
So it knows it has to do that, we have to add a decorator to the Python function, we do that.
And as we do that, we now get slightly cropped output, 
but the main idea, 
we can look at the percentage of time and we can see that making this request, 
get operation, took 88% of the time, whereas parsing the response took only 10.9% of the time.
This can be really informative and a lot of different programming languages will support this type of a line profiling.
Sometimes, you might not care about CPU. Maybe you care about the memory or like some other resource. Similarly, there are memory profilers: in Python
there is "memory_profiler", for C you will have "Valgrind". 
So here is a fairly simple example,
we just create this list with a million elements. That's going to consume like megabytes of space and
we do the same, creating another one with 20 million elements.
To check, what was the memory allocation? How it's gonna happen, what's the consumption? We can go through one memory profiler and
we execute it, 
and it's telling us the total memory usage and the increments.
And we can see that we have some overhead, because this is an interpreted language and when we create
this million,
this list with a million entries, we're gonna need this many megabytes of information. 
Then we were getting another 150 megabytes. Then, we're freeing this entry and that's decreasing the total amount.
We are not getting a negative increment because of a bug, probably in the profiler. 
But if you know that your program is taking a huge amount of memory and you don't know why, maybe because you're copying
objects where you should be doing things in place, then using a memory profiler can be really useful.
And in fact there's an exercise that will kind of work you through that, comparing an in-place version of quicksort with like a
non-inplace, that keeps making new and new copies. 
And if you using the memory profiler you can get a really good comparison between the two of them
Any questions so far, with profiling? Is the memory profiler running the program in order to get that?
Yeah... you might be able to figure out like just looking at the code.
But as you get more and more complex (for this code at least) But you get more and more complex programs what this is doing is running through the program
and for every line, at the very beginning, it's looking at the heap and saying
"What are the objects that I have allocated now?" "I have seven megabytes of objects", 
and then goes to the next line,
looks again, "Oh now I have 50, so I have now added 43 there".
Again, you could do this yourself by asking for those operations in your code, every single line.
But that's not how you should be doing things since people have already written these tools for you to use.
As it was the case with...
So as in the case with strace, you can do something similar in profiling.
You might not care about the specific lines of code that you have,
but maybe you want to check for outside events. Like, you maybe want to check how many
CPU cycles your computer program is using, or how many page faults it's creating.
Maybe you have like bad cache locality and that's being manifested somehow. 
So for that, there is the "perf" command.
The perf command is gonna do this, where it is gonna run your program and it's gonna
keep track of all these statistics and report them back to you. 
And this can be really helpful if you are working at a lower level. 
So
we execute this command, I'm gonna explain briefly what it's doing.
And this stress program is just running in the CPU, 
and it's just a program to just
hog one CPU and like test that you can hog the CPU. 
And now if we Ctrl-C,
we can go back and we get some information about the number of page faults that we have or the number of
CPU cycles that we utilize, 
and other useful metrics from our code. For some programs you can
look at what the functions that were being used were. 
So we can record what this program is doing,
which we don't know about because it's a program someone else has written. 
And
we can report what it was doing by looking at the stack trace and we can say oh, It's spending a bunch of time in this
__random_r standard library function. 
And it's mainly because the way of hogging a CPU is by just creating more and more pseudo-random numbers.
There are some other functions that have not been mapped, because they belong to the program, 
but if you know about your program
you can display this information using more flags, about perf. There are really good tutorials online about how to use this tool.
Oh One one more thing regarding profilers is, so far,
we have seen that these profilers are really good at aggregating all this information and giving you a lot of these numbers so you can
optimize your code or you can reason about what is happening, 
but the thing is
humans are not really good at making sense of lots of numbers and since humans are more visual creatures, it's much
easier to kind of have some sort of visualization. Again, programmers have already thought about this and have come up with solutions.
A couple of popular ones, is a FlameGraph. A FlameGraph is a
sampling profiler. 
So this is just running your code and taking samples And then on the y-axis here
we have the depth of the stack so we know that the bash function called this other function, 
and this called this other function,
so on, so forth. 
And on the x-axis it's not time, it's not the timestamps.
Like it's not this function run before, 
but it's just time taken. Because, again, this is a sampling profiler:
we're just getting small glimpses of what was it going on in the program. But we know that, for example,
this main program took the most time because the x-axis is proportional to that.
They are interactive and they can be really useful to identify the hot spots in your program.
Another way of displaying information, 
and there is also an exercise on how to do this, is using a call graph.
So a call graph is going to be displaying information, 
and it's gonna create a graph of which function called which other function.
And then you get information about, like, oh, we know that "__main__" called this "Person" function ten times and
it took this much time. 
And as you have larger and larger programs, looking at one of these call graphs can be useful to identify
what piece of your code is calling this really expensive IO operation, for example.
With that I'm gonna cover the last part of the lecture, which is that
sometimes, you might not even know what exact resource is constrained in your program.
Like how do I know how much CPU my program is using, 
and I can quickly look in there, or how much memory.
So there are a bunch of really nifty tools for doing that one of them is
HTOP. 
so HTOP is an interactive command-line tool and here it's displaying all the CPUs this machine has,
which is 12. It's displaying the amount of memory, it says I'm consuming almost a gigabyte of the 32 gigabytes my machine has.
And then I'm getting all the different processes. 
So for example we have
zsh, mysql and other processes that are running in this machine, 
and I can sort through the amount of CPU
they're consuming or through the priority they're running at.
We can check this, for example. Here we have the stress command again and we're going to
run it to take over four CPUs and check that we can see that in HTOP.
So we did spot those four CPU jobs, 
and now I have seen that
besides the ones we had before, now I have this...
Like this "stress -c" command running and taking a bunch of our CPU.
Even though you could use a profiler to get similar information to this, the way HTOP displays this kind of in a live interactive
fashion can be much quicker and much easier to parse. In the notes, there's a
really long list of different tools for evaluating different parts of your system.
So that might be tools for analyzing the network performance, about looking the
number of IO operations, so you know whether you're saturating the
the reads from your disks, you can also look at what is the space usage.
Which, I think, here...
So NCDU... There's a tool called "du" which stands for "disk usage" and
we have the "-h" flag for "human readable output".
We can do videos and we can get output about the size of all the files in this folder.
Yeah, there we go. There are also interactive versions, like HTOP was an interactive version.
So NCDU is an interactive version that will let me navigate through the folders and I can see quickly that
oh, we have... This is one of the folders for the video lectures,
and we can see there are these four files that have like almost 9 GB each and I could quickly delete them through this interface.
Another neat tool is "LSOF" which stands for "LIST OF OPEN FILES".
Another pattern that you may encounter is you know some process is using a file, 
but you don't know exactly which process is using that file.
Or, similarly, some process is listening in
a port, 
but again, how do you find out which one it is? So to set an example.
We just run a Python HTTP server on port 444
Running there. Maybe we don't know that that's running, 
but then we can
use... we can use LSOF.
Yeah, we can use LSOF, 
and the thing is LSOF is gonna print a lot of information.
You need SUDO permissions because this is gonna ask for who has all these items.
Since we only care about the one who is listening in this 444 port we can ask
grep for that. 
And we can see, oh, there's like this Python process, with this identifier, that is using the port and then we can
kill it, 
and that terminates that process.
Again, there's a lot of different tools. There's even tools for
doing what is called benchmarking. 
So in the shell tools and scripting lecture, I said like for some tasks "fd" is much faster than "find"
But like how will you check that? I can test that with "hyperfine" and I have here two commands: one with "fd" that is just
searching for JPEG files and the same one with "find". If I execute them, it's gonna benchmark these scripts and give me some output about
how much faster "fd" is compared to "find".
So I think that kind of concludes... yeah, like 23 times for this task.
So that kind of concludes the whole overview. I know that there's like a lot of different topics and there's like a lot of
perspectives on doing these things, 
but again I want to reinforce the idea that you don't need to be a master of all these topics but more...
To be aware that all these things exist. 
So if you run into these issues you don't reinvent the wheel, 
and you reuse all that other programmers have done.
Given that, I'm happy to take any questions related to this last section or anything in the lecture.
Is there any way to sort of think about how long a program should take? You know, 
if it's taking a while to run
you know, should you be worried? Or depending on your process, let me wait another ten minutes before I start looking at why it's taking so long.
Okay, so the... The task of knowing how long a program
should run is pretty infeasible to figure out. It will depend on the type of program.
It depends on whether you're making HTTP requests or you're reading data... one thing that you can do is if you have
for example, 
if you know you have to read two gigabytes from memory, like from disk, 
and load that into memory, you can make
back-of-the-envelope calculation. 
So like that shouldn't take longer than like X seconds because this is
how things are set up. Or if you are reading some files from the network and you know kind of what the network link is and they are taking say five times longer than
what you would expect then you could try to do that. Otherwise, 
if you don't really know. Say you're trying to do some mathematical operation in your code and you're not really sure
about how long that will take you can use something like logging and try to kind of print intermediate
stages to get a sense of like, oh I need to do a thousand operations of this and
three iterations took ten seconds. Then this is gonna take much longer than I can handle in my case.
So I think there are there are ways, it will again like depend on the task, 
but definitely, given all the tools we've seen really, we probably have like
a couple of really good ways to start tackling that.
Any other questions? You can also do things like run HTOP and see if anything is running.
Like if your CPU is at 0%, something is probably wrong.
Okay. There's a lot of exercises for all the topics that we have covered in today's class,
so feel free to do the ones that are more interesting. We're gonna be holding office hours again today.
Just a reminder, office hours. You can come and ask questions about any lecture. Like we're not gonna expect you to kind of do the exercises in a couple of minutes.
They take a really long while to get through them, 
but we're gonna be there
to answer any questions from previous classes, or even not related to exercises. Like if you want to know more about how you
would use TMUX in a way to kind of quickly switch between panes, anything that comes to your mind.