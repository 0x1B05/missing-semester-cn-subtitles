Okay, welcome back. Today we're gonna cover a couple separate
two main topics related to the shell. First, we're gonna do some kind of shell scripting, mainly related to bash,
which is the shell that most of you will start in Mac, or like in most Linux systems, that's the default shell.
And it's also kind of backward compatible through other shells like zsh, it's pretty nice. And then we're gonna cover some other shell tools that are really convenient,
so you avoid doing really repetitive tasks, like looking for some piece of code
or for some elusive file. And there are already really nice built-in commands that will really help you to do those things.
So yesterday we already kind of introduced you to the shell and some of it's quirks,
and like how you start executing commands, redirecting them. Today, we're going to kind of cover more about
the syntax of the variables, the control flow, functions of the shell.
So for example, once you drop into a shell, say you want to
define a variable, which is one of the first things you learn to do in a programming language.
Here you could do something like foo equals bar. And now we can access the value of foo by doing "$foo".
And that's bar, perfect. One quirk that you need to be aware of is that
spaces are really critical when you're dealing with bash. Mainly because spaces are reserved, and that will be for separating arguments.
So, for example, something like foo equals bar won't work, and the shell is gonna tell you why it's not working.
It's because the foo command is not working, like foo is non-existent. And here what is actually happening, we're not assigning foo to bar,
what is happening is we're calling the foo program with the first argument "=" and the second argument "bar".
And in general, whenever you are having some issues, like some files with spaces
you will need to be careful about that. You need to be careful about quoting strings.
So, going into that, how you do strings in bash. There are two ways that you can define a string:
You can define strings using double quotes and you can define strings using single,
sorry, using single quotes. However, for literal strings they are equivalent,
but for the rest they are not equivalent. So, for example, if we do value is $foo,
the $foo has been expanded like a string, substituted to the
value of the foo variable in the shell. Whereas if we do this with a simple quote, we are just getting the $foo as it is
and single quotes won't be replacing. Again, it's really easy to write a script, assume that this is kind of like Python, that you might be
more familiar with, and not realize all that. And this is the way you will assign variables.
Then bash also has control flow techniques that we'll see later, like for loops, while loops, and one main thing is you can define functions.
We can access a function I have defined here. Here we have the MCD function, that has been defined, and the thing is
so far, we have just kind of seen how to execute several commands by piping into them, kind of saw that briefly yesterday.
But a lot of times you want to do first one thing and then another thing. And that's kind of like the
sequential execution that we get here. Here, for example, we're calling the MCD function.
We, first, are calling the makedir command, which is creating this directory.
Here, $1 is like a special variable. This is the way that bash works, whereas in other scripting languages there will be like argv,
the first item of the array argv will contain the argument. In bash it's $1. And in general, a lot
of things in bash will be dollar something and will be reserved, we will be seeing more examples later.
And once we have created the folder, we CD into that folder, which is kind of a fairly common pattern that you will see.
We will actually type this directly into our shell, and it will work and it will define this function. But sometimes it's nicer to write things in a file.
What we can do is we can source this. And that will execute this script in our shell and load it.
So now it looks like nothing happened, but now the MCD function has
been defined in our shell. So we can now for example do MCD test, and now we move from the tools directory to the test
directory. We both created the folder and we moved into it.
What else. So a result is... We can access the first argument with $1.
There's a lot more reserved commands, for example $0 will be the name of the script,
$2 through $9 will be the second through the ninth arguments
that the bash script takes. Some of these reserved keywords can be directly used in the shell, so for example
$? will get you the error code from the previous command,
which I'll also explain briefly. But for example, $_ will get you the last argument of the
previous command. So another way we could have done this is
we could have said like "mkdir test" and instead of rewriting test, we can access that last argument
as part of the (previous command), using $_
like, that will be replaced with test and now we go into test.
There are a lot of them, you should familiarize with them. Another one I often use is called "bang bang" ("!!"), you will run into this
whenever you, for example, are trying to create something and you don't have enough permissions. Then, you can do "sudo !!"
and then that will replace the command in there and now you can just try doing
that. And now it will prompt you for a password, because you have sudo permissions.
Before, I mentioned the, kind of the error command. Yesterday we saw that, in general, there are
different ways a process can communicate with other processes or commands.
We mentioned the standard input, which also was like getting stuff through the standard input,
putting stuff into the standard output. There are a couple more interesting things, there's also like a
standard error, a stream where you write errors that happen with your program and you don't want to pollute the standard output.
There's also the error code, which is like a general thing in a lot of programming languages,
some way of reporting how the entire run of something went. So if we do
something like echo hello and we
query for the value, it's zero. And it's zero because everything went okay and there weren't any issues. And a zero exit code is
the same as you will get in a language like C, like 0 means everything went fine, there were no errors.
However, sometimes things won't work. Sometimes, like if we try to grep for foobar in our MCD script,
and now we check for that value, it's 1. And that's because we tried to search for the foobar
string in the MCD script and it wasn't there. So grep doesn't print anything, but
let us know that things didn't work by giving us a 1 error code.
There are some interesting commands like "true", for example, will always have a zero
error code, and false will always have a one error code.
Then there are like these logical operators that you can use
to do some sort of conditionals. For example, one way... you also have IF's and ELSE's, that we will see later, but you can do
something like "false", and echo "Oops fail". So here we have two commands connected by this OR operator.
What bash is gonna do here, it's gonna execute the first one and if the first one didn't work, then it's
gonna execute the second one. So here we get it, because it's gonna try to do a logical OR. If the first one didn't have
a zero error code, it's gonna try to do the second one. Similarly, if we instead of use "false", we use something like "true",
since true will have a zero error code, then the second one will be short-circuited and
it won't be printed.
Similarly, we have an AND operator which will only execute the second part if the first one
ran without errors. And the same thing will happen.
If the first one fails, then the second part of this thing won't be executed.
Kind of not exactly related to that, but another thing that you will see is
that no matter what you execute, then you can concatenate commands using a semicolon in the same line,
and that will always print. Beyond that, what we haven't seen, for example, is how
you go about getting the output of a command into a variable.
And the way we can do that is doing something like this. What we're doing here is we're getting the output of the PWD command,
which is just printing the present working directory where we are right now. And then we're storing that into the foo variable.
So we do that and then we ask for foo, we view our string.
More generally, we can do this thing called command substitution
by putting it into any string. And since we're using double quotes instead of single quotes
that thing will be expanded and it will tell us that we are in this working folder.
Another interesting thing is, right now, what this is expanding to is a string
instead of It's just expanding as a string. Another nifty and lesser known tool is called process substitution,
which is kind of similar. What it will do...
it will, here for example, the "<(", some command and another parenthesis,
what that will do is: that will execute, that will get the output to kind of like a temporary file and it will give the file handle to the command.
So here what we're doing is we're getting... we're LS'ing the directory, putting it into a temporary file,
doing the same thing for the parent folder and then we're concatenating both files. And this
will, may be really handy, because some commands instead of expecting the input coming from the stdin, they are expecting things to
come from some file that is giving some of the arguments.
So we get both things concatenated.
I think so far there's been a lot of information, let's see a simple, an example script where we see a few of these things.
So for example here we have a string and we have this $date. So $date is a program.
Again there's a lot of programs in UNIX you will kind of slowly familiarize with a lot of them.
Date just prints what the current date is and you can specify different formats.
Then, we have these $0 here. $0 is the name of the script that we're running.
Then we have $#, that's the number of arguments that we are giving
to the command, and then $$ is the process ID of this command that is running.
Again, there's a lot of these dollar things, they're not intuitive because they don't have like a mnemonic
way of remembering, maybe, $#. But it can be... you will just be
seeing them and getting familiar with them. Here we have this $@, and that will expand to all the arguments.
So, instead of having to assume that, maybe say, we have three arguments and writing $1, $2, $3,
if we don't know how many arguments we can put all those arguments there. And that has been given to a for loop. And the for loop
will, in time, get the file variable
and it will be giving each one of the arguments. So what we're doing is, for every one of the arguments we're giving.
Then, in the next line we're running the grep command which is just search for a substring in some file and we're
searching for the string foobar in the file. Here, we have put the variable that the file took, to expand.
And yesterday we saw that if we care about the output of a program, we can
redirect it to somewhere, to save it or to connect it to some other file. But sometimes you want the opposite.
Sometimes, here for example, we care... we're gonna care about the error code. About this script, we're gonna care whether the
grep ran successfully or it didn't. So we can actually discard entirely what the output...
like both the standard output and the standard error of the grep command. And what we're doing is we're
redirecting the output to /dev/null which is kind of like a special device in UNIX
systems where you can like write and it will be discarded. Like you can write no matter how much you want,
there, and it will be discarded. And here's the ">" symbol that we saw yesterday for redirecting output. Here you have a "2>"
and, as some of you might have guessed by now, this is for redirecting the standard error, because those those two
streams are separate, and you kind of have to tell bash what to do with each one of them.
So here, we run, we check if the file has foobar, and if the file has foobar then it's
going to have a zero code. If it doesn't have foobar, it's gonna have a nonzero error code. So that's exactly what we
check. In this if part of the command we say "get me the error code". Again, this $?
And then we have a comparison operator which is "-ne", for "non equal". And some
other programming languages will have "==", "!=", these
symbols. In bash there's like a reserved set of comparisons and
it's mainly because there's a lot of things you might want to test for when you're in the shell. Here for example
we're just checking for two values, two integer values, being the same. Or for example here, the "-F" check will let
us know if a file exists, which is something that you will run into very, very commonly. I'm going back to the
example. Then, what happens when we
if the file did not have foobar, like there was a nonzero error code, then we print
"this file doesn't have any foobar, we're going to add one". And what we do is we echo this "# foobar", hoping this
is a comment to the file and then we're using the operator ">>" to append at the end of
the file. Here since the file has been fed through the script, and we don't know it beforehand, we have to substitute
the variable of the filename. We can actually run this. We already have
correct permissions in this script and we can give a few examples. We have a few files in this folder, "mcd" is the
one we saw at the beginning for the MCD function, some other "script" function and we can even feed the own script to itself to check if it has foobar in it.
And we run it and first we can see that there's different variables that we saw, that have been
successfully expanded. We have the date, that has been replaced to the current time, then
we're running this program, with three arguments, this randomized PID, and then
it's telling us MCD doesn't have any foobar, so we are adding a new one, and this script file doesn't
have one. So now for example let's look at MCD and it has the comment that we were looking for.
One other thing to know when you're executing scripts is that
here we have like three completely different arguments but very commonly you will be giving arguments that
can be more succinctly given in some way. So for example here if we wanted to
refer to all the ".sh" scripts we
could just do something like "ls *.sh"
and this is a way of filename expansion that most shells have that's called "globbing". Here, as you
might expect, this is gonna say anything that has any kind of sort of characters and ends up with "sh".
Unsurprisingly, we get "example.sh" and "mcd.sh". We also have these
"project1" and "project2", and if there were like a... we can do a "project42", for example
And now if we just want to refer to the projects that have a single character, but not two characters
afterwards, like any other characters, we can use the question mark. So "?" will expand to only a single one.
And we get, LS'ing, first "project1" and then "project2".
In general, globbing can be very powerful. You can also combine it.
A common pattern is to use what is called curly braces. So let's say we have an image, that we have in this folder
and we want to convert this image from PNG to JPG or we could maybe copy it, or...
it's a really common pattern, to have two or more arguments that are fairly similar and you want to do something with them as arguments to some command.
You could do it this way, or more succinctly, you can just do
"image.{png,jpg}"
And here, I'm getting some color feedback, but what this will do, is it'll expand into the line above.
Actually, I can ask zsh to do that for me. And that what's happening here.
This is really powerful. So for example you can do something like... we could do...
"touch" on a bunch of foo's, and all of this will be expanded.
You can also do it at several levels and you will do the Cartesian...
if we have something like this, we have one group here, "{1,2}"
and then here there's "{1,2,3}", and this is going to do the Cartesian product of these
two expansions and it will expand into all these things, that we can quickly "touch".
You can also combine the asterisk glob with the curly braces glob.
You can even use kind of ranges. Like, we can do "mkdir"
and we create the "foo" and the "bar" directories, and then we can do something along these lines. This
is going to expand to "fooa", "foob"... like all these combinations, through "j", and
then the same for "bar". I haven't really tested it... but yeah, we're getting all these combinations that we
can "touch". And now, if we touch something that is different between these two [directories], we
can again showcase the process substitution that we saw
earlier. Say we want to check what files are different between these two folders. For us it's obvious, we just saw it, it's X and Y,
but we can ask the shell to do this "diff" for us between the output of one LS and the other LS.
Unsurprisingly we're getting: X is only in the first folder and Y is only in the second folder. What is more
is, right now, we have only seen bash scripts. If you like other
scripts, like for some tasks bash is probably not the best, it can be tricky. You can actually write scripts that
interact with the shell implemented in a lot of different languages. So for example, let's see here a
Python script that has a magic line at the beginning that I'm not explaining for now.
Then we have "import sys", it's kind of like... Python is not, by default, trying to interact
with the shell so you will have to import some library. And then we're doing a
really silly thing of just iterating over "sys.argv[1:]".
"sys.argv" is kind of similar to what in bash we're getting as $0, $1, &c.
Like the vector of the arguments, we're printing it in the reversed order. And the magic line at the beginning is
called a shebang and is the way that the shell will know how to run this program. You can always do something like
"python script.py", and then "a b c" and that will work, always, like that. But
what if we want to make this to be executable from the shell? The way the shell knows that it has to use python as the
interpreter to run this file is using that first line. And that first line is
giving it the path to where that thing lives.
However, you might not know. Like, different machines will have probably different places where they put python
and you might not want to assume where python is installed, or any other interpreter. So one thing that you can do is use the
"env" command. You can also give arguments in the shebang, so
what we're doing here is specifying run the "env" command, that is for pretty much every system, there are some exceptions, but like for
pretty much every system it's is in "usr/bin", where a lot of binaries live, and then we're calling it with the
argument "python". And then that will make use of the path environment variable
that we saw in the first lecture. It's gonna search in that path for the Python binary and then it's gonna use that to
interpret this file. And that will make this more portable so it can be run in my machine, and your machine and some other machine.
Another thing is that the bash is not really like modern, it was
developed a while ago. And sometimes it can be tricky to debug. By default, and the ways it will fail
sometimes are intuitive like the way we saw before of like foo command not existing, sometimes it's not. So there's
like a really nifty tool that we have linked in the lecture notes, which is called
"shellcheck", that will kind of give you both warnings and syntactic errors
and other things that you might not have quoted properly, or you might have misplaced spaces in
your files. So for example for extremely simple "mcd.sh" file we're getting a couple
of errors saying hey, surprisingly, we're missing a shebang, like this might not interpret it correctly if you're
it at a different system. Also, this CD is taking a command and it might not
expand properly so instead of using CD you might want to use something like CD
and then an OR and then an "exit". We go back to what we explained earlier, what
this will do is like if the CD doesn't end correctly, you cannot CD
into the folder because either you don't have permissions, it doesn't exist... That will give a nonzero error
command, so you will execute exit and that will stop the script
instead of continue executing as if you were in a place that you are actually not in. And actually I haven't tested, but I
think we can check for "example.sh" and here we're getting that we should be
checking the exit code in a different way, because it's probably not the best way, doing it this
way. One last remark I want to make is that when you're writing bash scripts
or functions for that matter, there's kind of a difference between writing bash scripts in isolation like a
thing that you're gonna run, and a thing that you're gonna load into your shell. We will see some of this in the command
line environment lecture, where we will kind of be tooling with the bashrc and the sshrc. But in general, if you make
changes to for example where you are, like if you CD into a bash script and you just execute that bash script, it won't CD
into the shell are right now. But if you have loaded the code directly into
your shell, for example you load... you source the function and then you execute
the function then you will get those side effects. And the same goes for defining variables into the shell.
Now I'm going to talk about some tools that I think are nifty when
working with the shell. The first was also briefly introduced yesterday.
How do you know what flags, or like what exact commands are. Like how I am
supposed to know that LS minus L will list the files in a list format, or that
if I do "move - i", it's gonna like prom me for stuff. For that what you have is the "man"
command. And the man command will kind of have like a lot of information of how will you go about... so for example here it
will explain for the "-i" flag, there are all these options you can do. That's
actually pretty useful and it will work not only for really simple commands that come packaged with your OS
but will also work with some tools that you install from the internet for example, if the person that did the
installation made it so that the man package were also installed. So for example
a tool that we're gonna cover in a bit which is called "ripgrep" and is called with RG, this didn't
come with my system but it has installed its own man page and I have it here and I can access it. For some commands the
man page is useful but sometimes it can be tricky to decipher because it's more
kind of a documentation and a description of all the things the tool can do. Sometimes it will have
examples but sometimes not, and sometimes the tool can do a lot of things so a
couple of good tools that I use commonly are "convert" or "ffmpeg", which deal with images and video respectively and
the man pages are like enormous. So there's one neat tool called "tldr" that you can install and you will have like
some nice kind of explanatory examples of how you want to use this command. And you
can always Google for this, but I find myself saving going into the browser, looking about some examples and
coming back, whereas "tldr" are community contributed and they're fairly useful. Then,
the one for "ffmpeg" has a lot of useful examples that are more nicely
formatted (if you don't have a huge font size for recording). Or even
simple commands like "tar", that have a lot of options that you are combining. So for example, here you can be combining 2, 3...
different flags and it can not be obvious, when you want to combine
different ones. That's how you
would go about finding more about these tools. On the topic of finding, let's try
learning how to find files. You can always go "ls", and like you can go like
"ls project1", and keep LS'ing all the way through. But
maybe, if we already know that we want to look for all the folders called
"src", then there's probably a better command for doing that. And that's "find".
Find is the tool that, pretty much comes with every UNIX system. And find,
we're gonna give it... here we're saying we want to call find in the
current folder, remember that "." stands for the current folder, and we want the name to be "src" and we want the type to be a directory. And by typing that it's
gonna recursively go through the current directory and look for all these files,
or folders in this case, that match this pattern. Find has a lot of useful
flags. So for example, you can even test for the path to be in a way. Here we're
saying we want some number of folders, we don't really care how many folders, and then we care about all the Python
scripts, all the things with the extension ".py", that are within a test folder. And we're also checking, just in
cases really but we're checking just that it's also a type F, which stands for file. We're getting all these files.
You can also use different flags for things that are not the path or the name.
You could check things that have been modified ("-mtime" is for the modification time), things that have been
modified in the last day, which is gonna be pretty much everything. So this is gonna print a lot of the files we created and files
that were already there. You can even use other things like size, the owner,
permissions, you name it. What is even more powerful is, "find" can find stuff
but it also can do stuff when you find those files. So we could look for all
the files that have a TMP extension, which is a temporary extension, and
then, we can tell "find" that for every one of those files, just execute the "rm" command for them. And
that will just be calling "rm" with all these files. So let's first execute it
without, and then we execute it with it. Again, as with the command line
philosophy, it looks like nothing happened. But since we have a zero error code, something
happened - just that everything went correct and everything is fine. And now, if we look for these files, they aren't there anymore.
Another nice thing about the shell in general is that there are
these tools, but people will keep finding new ways, so alternative
ways of writing these tools. It's nice to know about it. So, for example find if you just want to match the things that end in "tmp"
it can be sometimes weird to do this thing, it has a long command. There's things like "fd",
for example, that is a shorter command that by default will use regex and will ignore your gitfiles, so you
don't even search for them. It will color-code, it will have better Unicode support... It's nice to
know about some of these tools. But, again, the main idea is that if you are aware that these tools exist, you can
save yourself a lot of time from doing kind of menial and repetitive tasks.
Another command to bear in mind is like "find". Some of you may be wondering, "find" is probably just
actually going through a directory structure and looking for things but
what if I'm doing a lot of "finds" a day? Wouldn't it be better, doing kind of a database approach and build an index first, and then use that index
and update it in some way. Well, actually most Unix systems already do it and this is through the "locate" command and
the way that the locate will be used... it will just look for paths in
your file system that have the substring that you want. I actually don't know if it will work... Okay, it worked. Let me try to
do something like "missing-semester".
You're gonna take a while but it found all these files that are somewhere in my file system and since it has
built an index already on them, it's much faster. And then, to keep it updated,
using the "updatedb" command that is running through cron,
to update this database. Finding files, again, is really useful. Sometimes you're actually concerned about, not the files themselves,
but the content of the files. For that you can use the grep command that we
have seen so far. So you could do something like grep foobar in MCD, it's there.
What if you want to, again, recursively search through the current
structure and look for more files, right? We don't want to do this manually.
We could use "find", and the "-exec", but actually "grep" has the "-R" flag that will go through the entire
directory, here. And it's telling us that oh we have the foobar line in example.sh
at these three places and in this other two places in foobar. This can be
really convenient. Mainly, the use case for this is you know you have written some code in some programming
language, and you know it's somewhere in your file system but you actually don't know. But you can actually quickly search.
So for example, I can quickly search
for all the Python files that I have in my scratch folder where I used the request library.
And if I run this, it's giving me through all these files, exactly in
what line it has been found. And here instead of using grep, which is fine,
you could also do this, I'm using "ripgrep", which is kind of the same idea but again trying to bring some more
niceties like color coding or file processing and other things. It think it has, also, unicode support. It's also pretty
fast so you are not paying like a trade-off on this being slower and
there's a lot of useful flags. You can say, oh, I actually want to get some context around those results.
So I want to get like five lines of context around that, so you can see where that import lives and see code around it.
Here in the import it's not really useful but like if you're looking for where you use the function, for example, it will
be very handy. We can also do things like we can search, for example here,.
A more advanced use, we can say,
"-u" is for don't ignore hidden files, sometimes
you want to be ignoring hidden files, except if you want to search config files, that are by default hidden. Then, instead of printing
the matches, we're asking to do something that would be kind of hard, I think, to do with grep, out of my head, which is
"I want you to print all the files that don't match the pattern I'm giving you", which
may be a weird thing to ask here but then we keep going... And this pattern here
is a small regex which is saying at the beginning of the line I have a
"#" and a "!", and that's a shebang. Like that, we're searching here for all
the files that don't have a shebang and then we're giving it, here,
a "-t sh" to only look for "sh" files, because maybe all your Python or text files are fine
without a shebang. And here it's telling us "oh, MCD is obviously missing a shebang"
We can even... It has like some nice flags, so for example if we include the "stats" flag
it will get all these results but it will also tell us information about all
the things that it searched. For example, the number of matches that it found, the lines, the file searched,
the bytes that it printed, &c. Similar as with "fd", sometimes it's not as useful
using one specific tool or another and in fact, as ripgrep, there are several other tools. Like "ack",
is the original grep alternative that was written. Then the silver searcher, "ag", was another one... and they're all
pretty much interchangeable so maybe you're at a system that has one and not the other, just knowing that you can
use these things with these tools can be fairly useful. Lastly, I want to cover
how you go about, not finding files or code, but how you go about finding commands that you already
some time figured out. The first, obvious way is just using the up arrow,
and slowly going through all your history, looking for these matches. This is actually not very efficient, as
you probably guessed. So the bash has ways to do this more easily.
There is the "history" command, that will print your history. Here I'm in zsh and it only prints some of my history, but
if I say, I want you to print everything from the beginning of time, it will print everything from the beginning of whatever this history is.
And since this is a lot of results, maybe we care about the ones where we use the "convert" command to go from some type of file to some other type of file.
Some image, sorry. Then, we're getting all these results here, about all the ones
that match this substring.
Even more, pretty much all shells by default will link "Ctrl+R", the keybinding,
to do backward search. Here we have backward search, where we can type "convert" and it's finding the
command that we just typed. And if we just keep hitting "Ctrl+R", it will kind of go through these matches and
it will let re-execute it in place. Another thing that you can do,
related to that, is you can use this really nifty tool called "fzf", which is like a fuzzy finder, like it will...
It will let you do kind of like an interactive grep. We could do
for example this, where we can cat our example.sh command, that will print
print to the standard output, and then we can pipe it through fzf. It's just getting all the lines and then we can interactively look for the
string that we care about. And the nice thing about fzf is that, if you enable the default bindings, it will bind to
your "Ctrl+R" shell execution and now
you can quickly and dynamically like look for all the times you try to convert a favicon in your history.
And it's also like fuzzy matching, whereas like by default in grep or these things you have to write a regex or some
expression that will match within here. Here I'm just typing "convert" and "favicon" and
it's just trying to do the best scan, doing the match in the lines it has.
Lastly, a tool that probably you have already seen, that I've been using for not retyping these extremely long
commands is this "history substring search", where
as I type in my shell, and both F fail to mention but both face
which I think was originally introduced, this concept, and then zsh has a really nice implementation)
what it'll let you do is as you type the command, it will dynamically search back in your
history to the same command that has a common prefix, and then, if you...
it will change as the match list stops working and then as you do the
right arrow you can select that command and then re-execute it.
We've seen a bunch of stuff... I think I have a few minutes left so I'm going to cover a couple of tools to do
really quick directory listing and directory navigation. So you can always use the "-R" to recursively list some directory structure,
but that can be suboptimal, I cannot really make sense of this easily.
There's tool called "tree" that will be the much more friendly form of
printing all the stuff, it will also color code based on... here for example "foo" is blue because it's a directory and
this is red because it has execute permissions. But we can go even further than that. There's really nice tools
like a recent one called "broot" that will do the same thing but here for example instead of doing this thing of listing
every single file, for example in bar we have these "a" through "j" files, it will say "oh there are more, unlisted here".
I can actually start typing and it will again again facily match to the files that are there
and I can quickly select them and navigate through them. So, again, it's good to know that
these things exist so you don't lose a large amount of time
going for these files. There are also, I think I have it installed
also something more similar to what you would expect your OS to have, like Nautilus or one of the Mac finders that have like an
interactive input where you can just use your navigation arrows and quickly explore.
It might be overkill but you'll be surprised how quickly you can make sense of some directory structure by just navigating through it.
And pretty much all of these tools will let you edit, copy files... if you just look for the options for them.
The last addendum is kind of going places. We have "cd", and "cd" is nice, it will get you
to a lot of places. But it's pretty handy if you can like quickly go places,
either you have been to recently or that you go frequently. And you can do this in
many ways there's probably... you can start thinking, oh I can make bookmarks, I can make... I can make aliases in the shell,
that we will cover at some point, symlinks... But at this point,
programmers have like built all these tools, so programmers have already figured out a really nice way of doing this.
One way of doing this is using what is called "auto jump", which I think is not loaded here...
Okay, don't worry. I will cover it in the command line environment.
I think it's because I disabled the "Ctrl+R" and that also affected other parts of the script. I think at this point if anyone has
any questions that are related to this, I'll be more than happy to answer them, if anything was left unclear.
Otherwise, a there's a bunch of exercises that we wrote, kind of
touching on these topics and we encourage you to try them and come to office hours, where we can help
you figure out how to do them, or some bash quirks that are not clear.