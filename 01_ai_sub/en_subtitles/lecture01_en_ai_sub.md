All right everyone, thanks for coming in. 
This is the Missing Semester of your CS Education, at least that's what we chose to call the class. If you're not here for this class, then you're in the wrong room. 
We will be here for about an hour, just to set your expectations. 
And I want to talk to you a little bit first about why we're doing this class.
So, this class stems out of an observation that Anish and Jose and I have made while TA-ing various classes at MIT, which is that basically all of us computer scientists, we know that computers are great at doing these repetitive tasks and automating things. 
But we often fail to realize that there are lots of tools that can make our own development processes better. 
We can be a lot more efficient about how we use our computers because we can use the computer as a tool for ourselves, not just for building websites or software, those sorts of things. 
And this class is an attempt to address this, is an attempt to show you some of the tools that you can use to great effect in your day to day, in your research, and in your studies. 
And it's going to be a class where we want to teach you both how to make the most of the tools that you already know, but also hopefully teach you about some tools you don't know from before and how to combine those tools to produce more powerful things than you think you might be able to do with what you know today.
The class is going to be structured as a series of 11 one-hour lectures, and each one is going to cover a particular topic. 
You can see the website, which is also listed there, for the list of lecture topics and what date we'll do each one. 
They will mostly be independent, and so you can sort of show up for the ones that you're interested in, but we will sort of assume that you've been following along so that as we get to later lectures, I'm not going to be teaching you bash all over again, for example.
We are also going to post both the lecture notes and recordings of the lectures online. 
Exactly when we do that, we haven't established yet, but it will be after the lectures. 
Obviously, the videos have to be posted after.
The class is going to be run by me, John, and Anne, each sitting over there, and Jose who is not currently here, but we'll be holding tomorrow's lecture. 
And keep in mind that we're trying to cover a lot of ground over the course of just 11 one-hour lectures, and so we will be moving relatively rapidly. 
But please do stop us if there's anything where you feel like you're not following along. 
If you feel like there's something you wish we would spend more time on, just let us know. 
Please interrupt us with questions.
Also, after each lecture, we're going to hold office hours on the ninth floor of Building 30, the Stata Center of the computer science building. 
If you show up in the ninth-floor lounge there and the Gates Tower, then you can come and try some of the exercises that we give for each lecture or just ask us other questions about things we've talked about in the lecture or other things about using your computer efficiently.
Due to the limited time that we have available, we're not going to be able to cover all tools in full detail, and so what we'll try to do is highlight interesting tools and interesting ways to use them.
We won't necessarily dig into the deep details about how all of it works or more elaborate use cases.
But if you have questions about them, please come ask us about that too.
Many of these tools are tools that we have used for years, and we might be able to point you to additional interesting things you can do with them, sort of like take advantage of the fact that we're here.
This class is going to, I don't wanna say ramped up quickly, but what's going to happen over the course of this particular lecture is that we'll cover many of the basics that we assume that you will know for the rest of the semester.
Things like how to use your shell and your terminal, and I'll explain what those are - those who you're not familiar with them.
And then we'll pretty quickly ramp up into more advanced tools and how to use them.
You can already see from the lecture notes the kind of topics that we're going to be covering.
And so, that brings us to today's lecture, in which we are going to cover the shell.
And the shell is going to be one of the primary ways that you interact with your computer once you want to do more things than what the sort of visual interfaces you might be used to allow you to do.
The visual interfaces are sort of limited in what they allow you to do, because you can only do the things that there are buttons for, sliders for, input fields for. 
Often, these textual tools are built to be both composable with one another, but also to have tons of different ways to combine them or ways to program and automate them. 
And that is why, in this class, we will be focusing on these command line or text-based tools, and the shell is the place that you would do most of this work. 
So, for those of you who are not familiar with the shell, most platforms provide some kind of shell. 
On Windows, this is often PowerShell, but there are also other shells available. 
On Windows, you will find tons of terminals. 
These are windows that allow you to display shells, and you'll also find many different types of shells, the most common of which is bash or the born again shell. 
Because it's such a common shell, it is the one we're primarily going to be covering in these lectures. 
If you're on Mac OS, you will probably also have bash, maybe an older version of it, if you open the terminal app. 
And so, if you want to follow along on any of these platforms, feel free, but keep in mind that most of this is going to be sort of Linux-centric in terms of how we teach it, even though most of these tools work on all the platforms. 
If you want to install a terminal and a shell, and you don't know how to do it, well, we're happy to show you at office hours, or it's usually very easy to just Google like your platform plus like terminal, and you will get one. 
Now, when you open a terminal, you get something that looks a little bit like this. 
So, it will usually have just a single line at the top, and this is what's known as the shell prompt. 
You can see that my shell prompt looks like this, it has my user name, the name of the machine that I'm on, the current path I'm on (and we will talk about paths a little bit later), and then it's really just sort of blinking there, asking me for input. 
And this is the shell prompt, where you tell the shell what you want it to do. 
And you can customize this prompt a lot. 
And when you open it on your machine, it might not look exactly like this. 
It might look something like this if you've configured it a little, or it might look all sorts of different ways. 
We won't go too much into customizing your shell in this particularly. 
We'll do that later. 
Here, we're just going to talk about how do you use this shell to do useful things. 
And this is our the main textual interface you have to your computer. 
Through this shell on the shell prompt, you get to write commands. 
And commands can be relatively straightforward things. 
Usually, it'll be something like executing programs with arguments. 
What does that look like? Well, one program we can execute is the date program. 
We just type "date" and press enter, and then it will show you, unsurprisingly, the date and time. 
You can also execute a program with arguments. 
This is one way to modify the behavior of the program. 
So for example, there is a program called "echo", and "echo" just prints out the arguments that you give it. 
And arguments are just whitespace-separated things that follow the program name. 
So we can say "hello", and then it will print "hello" back. 
Perhaps not terribly surprising, but this is the very basics of arguments. 
One thing that you'll notice is that I said that arguments are separated by whitespace, and you might wonder, well, what if I want an argument as multiple words? You can also quote things, so you can do things like "echo hello world", and now the echo program receives one argument that contains the string "hello world" with a space. 
Well, you can also use single quotes for this, and the difference between single quotes and double quotes will get back to when we talk about bash scripting. 
You can also just escape single characters. 
So, for example, "hello\ world", this will also work just fine. 
All of these rules about how you escape and how you parse and quote various arguments and variables we'll cover a little bit later. 
Hopefully, you won't run into too many oddities about this. 
Just keep in mind at least that spaces separate arguments, so if you want to do something like make a directory called "my photos", you can't just type like "make directory my photos". 
It will create two directories, one called "my" and one called "photos", and that is probably not what you want.
Now, one thing you might ask is, how does the shell know what these programs are when I type "date" or when I type "echo"? 
How does it know what these programs are supposed to do? 
And the answer to this is, your computer has a bunch of built-in programs that comes with the machine. 
Just like you, your machine might ship with like the terminal app, or it might ship with like Windows Explorer, or it might ship with some kind of browser. 
It also ships with a bunch of terminal-centric applications, and these are stored on your file system. 
And your shell has a way to determine where a program is located. 
Basically, it has a way to search for programs. 
It does this through something called an environment variable. 
An environment variable is a variable, like you might be used to for programming languages. 
It turns out that the shell, and the Bourne-again shell in particular, is really a programming language. 
This prompt that you're given here is not just able to run a program with arguments. 
You can also do things like while loops, for loops, conditionals... 
All of these - you can define functions, you can have variables, and all of these things you can do in the shell. 
We'll cover a lot of that in the next lecture, on shell scripting. 
For now, though, let's just look at this particular environment variable. 
Environment variables are things that are set whenever you start your shell; they're not things you have to set every time you run your shell. 
There are a bunch of these that are set, things like where is your home directory, what is your username, and there's also one that's critical for this particular purpose, which is the path variable. 
So, if I echo out dollar path, this is going to show me all of the paths on my machine that the shell will search for programs. 
You'll notice that this is a list that is colon separated. 
It might be kind of long and hard to read, but the essential is that whenever you type the name of a program, it's going to search through this list of paths on your machine, and it's going to look in each directory for a program or a file whose name matches the command you try to run. 
So, in my case, when I try to run date or echo, it's going to walk through these one at a time until it finds one that contains the program called date or echo, and then it's going to run it.
If we want to know which one it actually runs, there's a command called which which lets us do that. 
So, I can type which echo, and it will tell me that if I were to run a program called echo, I would run this one. 
It's worth pausing here to talk about what paths are. 
So, paths are a way to name the location of a file on your computer. 
On Linux and macOS, these paths are separated by slashes (forward slashes). 
So, you'll see here that this is in the root directory, so the slash at the very beginning indicates that this is starting from the top of the file system. 
Then look inside the directory called USR, then look inside the directory bin, and then look for the file called echo. 
On Windows, paths like this are usually separated by backslashes instead.
We're on Linux and macOS, everything lives under the root namespace, so all paths start with a slash, or all absolute paths. 
On Windows, there is one root for every partition, so you might have seen things like C colon backslash or D colon backslash. 
So, Windows has separate file system path hierarchies for each drive that you have, whereas on Linux and macOS, these are all mounted under one namespace. 
You'll notice that I said the word absolute path, and you might not know what that means. 
Absolute paths are paths that fully determine the location of a file. 
So, in this case, this is saying this is talking only about a specific echo file, and it's giving you the full path to that file, but there are also things known as relative paths.
A relative path is relative to where you currently are, and the way we find out where we currently are is you can type PWD for present working directory. 
Present print working directory, so if I type PWD, it will print out the current path that I'm in. 
Right now, I'm in the home directory under the root and then John under that and then dev under that, etc. 
From here, I can then choose to change my current working directory, and all relative paths are relative to the current working directory, which is basically where you are. 
In this case, for example, I can do CD /home. 
CD stands for change directory; this is the way that I change what my current working directory is. 
In this case, I change to home, and you'll see my shell prompt change to say that I am now in home.
It just gives me the name of the last segment of the path, but you can also configure your terminal to give you the full path whenever you're anywhere. 
And now if I type PWD again, it will tell me I'm in "/home". 
There are also a couple of special directories that exist: ".", and "..".  "." means the current directory, and ".." means the parent directory. 
So this is a way that you can easily navigate around the system. 
For example, if I type CD "..", it will tell me that I am now in "/", so I'm now in the root of the file system. 
I was in "/home" now I'm in "/", and indeed, if I type PWD, it will do that right thing. 
And I can also then use relative paths to go down into the file system, right? So I can do CD "./home", and this is gonna CD into the home directory under the current directory, right? So this will bring me back to "/home". 
If I now tried CD "./home" again, it will say there's no such directory because there is no home directory under the current directory I'm on, which I changed by doing CD. 
And I can CD all the way back to the place that I was using relative paths, and I can also do things like "../../.." to get back to somewhere deep in my file system. 
This happens to be all the way back to the root, so here there's a "bin" directory, and another "bin", there's an "echo" file, and so then I could do "echo world" and that runs the "echo" program under "bin". 
Alright, so this is a way that you can construct paths to arbitrarily traverse your filesystem. 
Sometimes you want to use absolute paths, and sometimes you want relative ones. 
Usually, you want to use whichever one is shorter, but if you want to, for example, run a program or write a program that runs the program like "echo" or "date", and you want it to be able to be run from anywhere, you either want to just give the name of the program like "date" or "echo" and let the shell use the path to figure out where it is, or you want to give its absolute path because if you gave a relative path, then if I ran it in my home directory and you ran it in some other directory, it might work for me but not for you. 
In general, when we run a program, it is going to be operating on the current working directory, at least by default, unless we give it any other arguments. 
And this is really handy because it means that often we don't have to give full paths for things, we can just use the name of files and the directory that we're currently in. 
One thing that's really useful is to figure out what is in the current directory we're in. 
So we already saw PWD which prints where you currently are. 
There's a command called "LS" which will show you, it will list the files in the current directory. 
So if I type LS here, this is all the files in the current directory, right? And this is a handy way to just quickly navigate through the filesystem. 
You'll see that if I sort of CD "." and then do LS, it'll show me the files in that directory instead. 
But with LS, I can also give it "LS .." like I can give it a path and then it will LS that file instead of the one that I'm currently in, or LS that directory. 
And you can see this if I go all the way to the root as well, right? Root has different files.
One handy trick you might not know about here is there are two other special things you can do. 
One is the tilde character. 
This character brings you to your home directory, so tilde always expands to the home directory, and you can do relative paths to it. 
So, I can do tilde slash dev slash P DOS classes missing semester, and now I'm there because tilde expanded to slash home slash John. 
There is also for CD, in particular, a really handy argument you can give, which is -. 
If you do CD -, it will CD to the directory you were previously in. 
So, if I do CD -, I go back to root. 
If I do CD - again, I go back to missing semester. 
This is a handy way if you want to toggle between two different directories.
In the case of LS or in the case of CD, there might be arguments you don't know about. 
Right now, we haven't really been doing anything except giving paths, but how do you even discover that you can give a path to LS in the first place? Well, most programs take what are known as arguments, like flags and options. 
These are things that usually start with a -. 
One of the handy ones is -help. 
Most programs implement this, and if you run, for example, LS -help, it will helpfully print out a bunch of information about that command. 
And you'll see here that it says the usage is LS, and you can give some number of options and you can give some number of files.
The way to read that usage line is triple dot means one like zero or one or more, and the square bracket means optional. 
So, in this case, there's an optional number of options, and there's an optional number of files. 
And you'll see that it says what the program does and also specifies a number of different types of flags and options you can give. 
Usually, we call things that are a single dash and a single letter a flag, and anything that doesn't take a value, a flag, and anything that does take a value, an option. 
So, for example, -a and -all are both flags, and -C or -color R is an option.
One thing you'll see under here, if you scroll down far enough, is the -L flag. 
And that's unhelpful. 
The -L flag uses a long listing format. 
Now, that's not particularly helpful in and of itself, but let's see what it actually does. 
So, if I do LS -L, it still prints the files in the current directory, but it gives me a lot more information about those files. 
And this is something you'll find yourself using quite a lot because the additional information it gives you is often quite handy.
Let's look at what some of that information is. 
First of all, the D at the beginning of some of these entries indicate that something is a directory. 
So, the underscore data entry here, for example, is a directory, whereas for HTML is not a directory, it's a file. 
The following letters after that indicate the permissions that are set for that file. 
So, like we saw earlier, I might not be able to open a given file or I might not be able to CD into a given directory, and this is all dictated by the permissions on that particular file or directory. 
The way to read these is that the first group of three are the permissions are set for the owner of the file. 
All of these files, you'll see, are owned by me. 
The second group of three characters is for the permissions for the group that owns this file. 
In this case, all of these files are also known by the john group.
And a final group of three is a list of the permissions for everyone else. 
So, anyone who's not a user owner or a group owner, this directory is perhaps kind of boring because all of the things are owned by me. 
But if we do something like CD to slash and do LS dash L, you'll see that here all of them are owned by root. 
We'll get back to what the root user is, but here you see some of the permissions are a little bit more interesting. 
The groups of three are read, write, and execute. 
What these mean differs for files and for directories. 
For files, it's pretty straightforward. 
If you have read permissions on a file, then you can read its contents. 
If you have write permissions on a file, then you can save the file, you can add more to it or you can replace it entirely. 
And if you have execute to the X bit on a file, then you're allowed to execute that file. 
So, if we do LS al in slash bin, that's a novel and user bin, you'll see that all of them have the execute bit set, even for people who are not the owner of the file. 
And this is because the echo program, for example, we want everyone on the computer to be able to execute. 
There's no reason to say only certain users can run echo. 
That doesn't really make any make any sense. 
For directories, though, these permissions are a little bit different. 
So, read translates - are you allowed to see which files are inside this directory? So, think of read as lists for a directory. 
Are you allowed to list its contents? Write for a directory is whether you are allowed to rename, create, or remove files within that directory. 
So, it's still kind of right, but notice that this means that if you have write permissions on a file but you do not have write permissions on its directory, you cannot delete the file. 
You can empty it, but you cannot delete it because that would require writing to the directory itself. 
And finally, execute on directories is something that trips people up a lot. 

Execute on a directory is what's known as search, and that's not terribly helpful a name, but what that means is: Are you allowed to enter this directory if you want to get to a file, if you want to open it or read it or write it, whatever you want to do, basically, to CD into a directory, you must have the execute permission on all parent directories of that directory and the directory itself. 

So, for example, for me to access a file inside slash user slash bin, such as user bin echo, I must have executed on root, I must have execute on user, and I must have execute on bin. 
If I do not have all those execute bits, I will not be allowed to access that file because I won't be able to enter the directories along the way. 
There are a number of other bits that you might come across. 
Like you might see esses or T's in these lists, you might see LS, those we can talk about in office hours if you're curious. 
They will mostly not matter for anything you will do in this class, but they are handy to know about. 
So, if you're curious about them, look them up on your own or come ask us in office hours. 
There are some other programs that are handy to know about. 
Oh, sorry, there's one more
thing, as I mentioned, if you just have a dash it means you do not have that permission. 
Right, so if it says, for example, our dash X, it means that you have read and execute, but you do not have right. 
There are some other handy programs to know about at this point. 
One of them is move, or the MV command. 
So, if I CD back to missing semester, here MV lets me rename a file. 
And rename here takes two paths, it takes the old path in the new path. 
This means that move lets you both rename a file, like if you change the name of the file but not the directory, or it lets you move a file to a completely different directory. 
It just you give the path to the current file and the path to where you want that file to be, and that can change its location and its name. 
So, for example, I can move dot files dot MD to be food MD unhelpfully, right? And similarly, I can move it back. 
There's also the CP command, the CP or copy is very similar. 
It lets you copy a file. 
CP also takes two arguments, it takes the path you want to copy from and the path you want to copy to, and these are full paths. 
So, I could use this, for example, to say I want to copy dot files out MD - dot dot slash food MD, sure food MD. 
And now, if I do LS dot, you'll see that there's a food MD file in that directory. 
So, CP as well take two paths, it does not have to be in the same directory. 
And similarly, there's the RM command which lets you remove a file, and there - you can give paths. 
In this case, I'm removing dot dot slash food. 
You should be aware, for removing, especially on Linux, removal is by default not recursive. 
So, you cannot remove a directory using RM. 
You can pass the - our flag which lets you do a recursive remove and then give a path that you want to remove and it will remove everything below it. 
There is also the RM dr dir command which lets you remove a directory, but it only lets you remove that directory if it is empty. 
So, the idea here is to sort of be a safety mechanism for you so you don't accidentally throw away a bunch of your files. 
And the final little command that's handy to use is make, there which lets you create a new directory. 
And as we talked about before, you don't want to do something like this because it will create two directories for you, one called my and one called photos. 
If you actually want to create a directory like this, you would either escape the space or quote the string. 
If you ever want more information about how any command to basically on these platforms work, there's a really handy command for that as well. 
There is the program called man for manual pages. 
This program takes as an argument the name of another program and gives you its manual page. 
So, for example, we could do man LS and this shows us a manual page for LS. 
You'll notice that in the case of LS, it is fairly similar to what we got with LS - help, but it's a little easier to navigate, a little easier to read. 
Usually towards the bottom, you will also get examples, information about who wrote it, where you can find more information, and that sort of stuff. 
One thing that can be confusing sometimes, at least until a recent version where they added this three at the bottom which says Q to quit, they do not use to say this. 
You press Q to quit this program.
It can be really hard to quit it if you do not know that a handy keyboard shortcut here, by the way, is Ctrl + L, which lets you clear your terminal and go back to the top. 
So far, we've only talked about programs in isolation, but where much of the power of the shell really comes through is once you start combining different programs. 
Right, so rather than just like running CDE, running LS, and etc., you might want to chain multiple programs together. 
You might want to interact with files and have files operate in between programs. 
And the way we can do this is using this notion of streams that the shell gives us. 
Every program by default has, I'm gonna simplify a little and say, two primary streams. 
It has an input stream and an output stream. 
By default, the input stream is your keyboard, basically. 
The input stream is your terminal, and whatever you type into your terminal is going to end up in the program, and it has a default output stream, which is whenever the program prints something, it's gonna print to that stream, and by default, that is also your terminal. 
This is why when I type echo hello, it gets printed back to my terminal, but the shell gives you a way to rewire these streams to change where the input/output of a programmer pointed. 
The way the most straightforward way you do this is using the angle bracket signs. 
So you can write something like this: 'echo hello > hello.txt' or you can write something like this: 'cat < hello.txt > hello2.txt'. 
The left angle bracket indicates rewire the input for this program to be the contents of this file, and the end angle bracket means rewire the output of the preceding program into this file. 
So let's look at an example of what that would look like. 
If I do 'echo hello > hello.txt', I can say I want that context, the content to be stored in a file called hello.txt, and because I gave this as a relative path, this will construct a file in the current directory called hello.txt, and at least in theory, its contents should be the word 'hello'.
 So if I run this, notice that nothing got printed to my output. 
The previous time when I ran echo hello, it printed hello. 
Now that 'hello' is going gone into a file called hello.txt, and I can verify this by using the program called cat. 
So cat prints the contents of a file. 
So I can do 'cat hello.txt', and there it shows me 'hello'. 
But cat is also something that supports this kind of wiring, so I can say 'cat', which by default just prints its input, it just duplicates its input to its output, I can say I want you to take your input from 'hello.txt'. 
What will happen in this case is that the shell is going to open 'hello.txt', take its contents, and set that to be the input of cat, and then cat is going to just print that to its output, which since I haven't rewired it is gonna be my terminal. 
So this will just print 'hello' to the output, and I can use both of these at the same time. 
So, for example, if I want to copy a file, and I don't want to use the CP command for some reason, I can do this, and in this case, I'm telling the cat program nothing at all, I'm just saying do your normal thing. 
Right, the cat program does not know anything about this redirection, but I'm telling the shell to use 'hello.txt' as the input for cat and to write anything that cat prints - 'hello2.txt'. 
Again, this prints nothing to my terminal.
But if I cat hello.txt, I get the output as I would have expected, which is a copy of the original file. 
There is also a double end bracket which is append instead of just overwrite, so you'll notice that if I do cat hello.txt >> hello2.txt, and then I cat hello2.txt, it still just contains 'hello', even though it already contained 'hello'. 
If I switch that to instead be a double end bracket, it means append, and if I now cat that file, it has 'hello' twice. 
These are pretty straightforward, they're usually just ways to interact with files, but where it gets really interesting is an additional operator the shell gives you called the pipe character. 
So, pipe is just a vertical bar, and what pipe means is take the output of the program to the left and make it the input of the program to the right, right? So, what does this look like? Well, let's take the example of 'ls /' or 'ls -l /'. 
This prints a bunch of things. 
Let's say that I only wanted the last line of this output. 
Well, there's a command called 'tail', and tail prints the last n lines of its input, and I can do -n1, so this is a flag called 'n' (you can also use '--lines' if you want to use it as a longer option), but in this case, this is saying just print the last line. 
And I can wire these together, so I can say 'ls -l / | tail -n1', and notice here that 'ls' does not know about 'tail', and 'tail' does not know about 'ls'. 
They are different programs and have never been programmed to be compatible with one another. 
All they know how to do is read from input and write to output, and then the pipe is what wires them together, and in this particular case, I'm saying I want the output of 'ls' to be the input to 'tail', and then I want the output of 'tail' to just go to my terminal because I haven't rewired it. 
I could also rewire this to say I want the output to go to 'ls.txt', and in this case, if I 'cat ls.txt', I would get the appropriate output. 
And it turns out you can do some really neat things with this. 
We're gonna cover this a lot more in the data wrangling lecture there will be in like four days or something on the kind of fancy stuff you can do when you start building more advanced pipelines. 
One to give you one example, we can do something like 'curl --head --silent google.com' so just to show you what that looks like, this gives me all the HTTP headers for accessing google.com, and I can pipe that to 'grep -a' (like '-A --ignore-case' or just '-i' if I want content length), so this is gonna print the content length header. 
'Grep' is a program that we'll talk about later, they'll let you search in an input stream for a given keyword. 
We can pipe that through say, the 'cut' command, which takes a delimiter set that to be space, and I want the second field, and this prints just the content length. 
This is sort of a silly example, right? Like this just lets you extract the content length in bytes of google.com from the command line. 
It's not a very useful thing to do, but you can see how by chaining these together you can achieve a bunch of really interesting text manipulation effects. 
And it turns out pipes are not just for textual data, you can do this for things like images as well.
You can have a program that manipulates a binary image on its input and writes a binary image to its output, and you can chain them together in this way. 
And we'll talk about some of those kinds of examples later on - you can even do this for video if you want. 
You can stream; this is, for example, a great way if you have a Chromecast at home. 
You can stream a video file like this by having the last program in your pipe be a Chromecast send program. 
So you stream a video file into it, and it streams or HTTP to your Chromecast. 
We'll talk a lot more about this in the data wrangling lecture. 
But there's one more thing that I wanted to talk to you about about sort of how to use the terminal in a more interesting and perhaps more powerful way that you might be used to. 
And this is perhaps even going to be interesting for the ones of you who feel like you're already comfortable with the term, but first, we need to cover an important topic when it comes to Linux systems and macOS systems, in particular, which is the notion of the root user. 
The root user is sort of like the administrator user on Windows, and it has user IDs zero. 
The root user is special because it is allowed to do whatever it wants on your system, even if a file is not readable by anyone, or if it's not writable by anyone, root can still access that file. 
Root is sort of a super user that gets to do whatever they want, and most of the time, you will not be operating as the super user. 
You will not be root. 
You will be a user like John or whatever your name is, and that's going to be the user you act with because if you were operating your computer as the root user at all times, if you ran the wrong program, they could just completely destroy your computer, and you don't want that, right? But every now and again, you want to do something that requires that you are root. 
Usually, for these cases, you will use a program called sudo. 
Sudo or do as su, and su in this case is Super User, so this is a way to do the following thing as the super user. 
Usually, the way sudo works is you write sudo and then a command like you would normally on your terminal, and it will just run that command as if you were root, as opposed to the user you actually are. 
Where might you need something like this? Well, there is a special; there are many special file system on your computer, but in particular, there's one called sysfs. 
If you CD to slash sys, this is a whole new world. 
This file system is not actually files on your computer. 
Instead, these are various kernel parameters. 
So the kernel is like basically the core of your computer. 
This is a way for you to access various kernel parameters through what looks like a file system. 
You'll see here that if I CD into class, for example, it has directories for a bunch of different types of devices that I can interact with or various queues I can access or all sorts of weird knobs internally. 
And because they're exposed as files, it means we can also use all the tools we've been using so far in order to manipulate them. 
One example of this is if you go into sys class backlight, so this backlight directly and lets you configure the backlight on your laptop if you have one. 
So I can CD into Intel backlight; this is an Intel laptop. 
Inside here, you'll see there's a file called brightness, and I can cat the brightness; this is the current brightness of my screen.
But not only that, I can modify this too in order to change the brightness of my screen. 
So, you might think that I could do, let's see what the max brightness is here. 
Okay, so it's currently set to the max brightness. 
You might imagine that I could do something like if I do 'echo let's do half or something echo 500 to brightness'. 
If I do this, it says permission denied. 
I'm not allowed to modify brightness because, in order to basically change things in the kernel, you need to be the administrator. 
And you might imagine that the way to solve this is to write 'sudo echo 500', but I still get a permission denied error. 
But why is that? It's because, as I mentioned before, these redirections of input and output is not something the programs know about. 
When we piped 'LS' to 'tail', 'tail' did not know about 'LS' and 'LS' did not know about 'tail'. 
The pipe and the redirection was set up by the shell. 
So, in this case, what's happening is I'm telling my shell to run the program 'sudo' with the arguments 'echo' and '500' and send its output to the file called 'brightness', but the shell is what is opening the brightness file. 
It is not the 'sudo' program. 
So, in this case, the shell, which is running as me, tries to open the brightness file for writing and it's not allowed to do that. 
Therefore, I get a permission denied error. 
You might have seen this if you like search for something, end up on Stack Overflow, and it tells you to just run this command and you'll see that it does something like they give you instructions like 1, 2, sis, what's an example, 'net ipv4 forward', for example. 
This is something you may have seen if you're setting up a firewall and this command is intended to work because this little pound symbol indicates run this as root. 
This is something that is very rarely explained, but that is what the pound symbol means. 
You'll see on my prompt there's a dollar symbol instead, and the dollar indicates you are not running as root. 
So, the question is how do I get around this? Well, I could switch into a root terminal, so one way to do this is to run 'sudo su'. 
'sudo su' is saying run the following command as root, and 'su' is a complicated command that effectively gets you a shell as the super user. 
So, if I do this, type my password, then now you'll see that the username at the beginning changed from 'jon' to 'root', and the prompt changed from a dollar to a pound. 
If I now come in to that file, if I do 'echo 500 to brightness', my screen got a little dimmer but you can't see it, you just have to trust me, and now I didn't get an error. 
And this is because the shell is now running as root. 
It is not running as Jon, and the root user is allowed to open this file. 
But, given our knowledge that we have of the terminal now, there's actually a way for us to do this without having to drop to a root shell, and that is as follows...That's, I guess, restore it to 1060. 
Do you see why this is different? Here, I'm telling my shell to run the 'echo 1060' command, which is gonna echo 1060, and I'm telling it to run the 'sudo tee brightness' command, and I'm telling you to send the output of 'echo' into 'sudo tee'.
In order to understand this, you need to know what the tee command does. 
The tee command takes its input and writes it to a file but also to standard out. 
So, tee is a convenient way if you have say a log file that you want to like send to a file to store for later, but you also want to see it to yourself, then you can pipe it through tee, give it the name of a file and it will write whatever its input is both to that file and to your screen. 
And here, I'm taking advantage of that program. 
I'm saying run tee as route and have tee right into the brightness file. 
And so, in this case, the tee program, which is what is opening the brightness file, is running as root and so it is allowed to do. 
If I run this, it will now, again, you can't see, but the brightness has been turned on by a laptop, and I don't get any errors, and I never had to drop into a root shell and run commands there, which can often be somewhat dangerous. 
If you want to explore this filesystem a little bit more, there's a lot of interesting stuff in here if you just start browsing around. 
You can find all sorts of fun things. 
So, for example, we noticed that there was a fun brightness command here. 
I wonder what other kinds of brightness I can set. 
So, I can use the find command which we will also talk about in a coming lecture. 
I wouldn't look on any file whose name it's a little like brightness in the current directory. 
That's unhelpful. 
Maybe they're not files. 
Did I misspell brightness? Yeah, why is it being annoying? Oh, apparently it does not want to search for brightness for me. 
How well, luckily for you, I know of one already handy, that there is a subdirectory called LEDs, and LEDs have brightness too. 
What kind of LEDs are there? Ooh, lots of things, for example, the scroll lock LED. 
Now, most of you probably don't know what the scroll lock LED is, or much less what scroll lock is. 
You might have seen a key on your keyboard neighbor named scroll lock. 
Basically, no one knows what it means anymore, no one really uses it for anything. 
It's mostly just a dead key and also a dead LED. 
What if you wanted to configure it so that every time you get email, your scroll lock LED lights up because there's no other reason why it would light up? Well, if we seed you into this particular directory that has a brightness place and it's set to zero, well, what happens if I write one into it? You probably should not just be writing random numbers into random files in this directory because you are affecting your kernel directly. 
Like, look up what the files do. 
In this particular case, I have warned safety goggles, and I've done my research, so now you can't tell, but on my keyboard, the scroll lock LED is now lit. 
So, now, if I wrote a program that, like, did some checking of mail and stuff, I could have it at the end run a program that echoes one into this file, and now I have a way for my LED to my keyboard to indicate when I've new email. 
At this point, you should know roughly your way around the terminal, around the shell, and know enough to accomplish these basic tasks, at least in theory. 
Now, you shouldn't need to use point-and-click interfaces to find files anymore. 
There's one remaining trick you might need, and that is the ability to open a file. 
So far, I've only really given you ways to find files.
But one thing you should know about is missing semester: xdg-open. 
This will probably only work on Linux. 
On Mac OS, I think it's just called 'open.' On Windows, who knows? xdg-open, you give the name of a file and it will open it in the appropriate program. 
So, if you do xdg-open an HTML file, that will open your browser and open that file. 
And once you have that program, in theory, you should no longer need to open like a Finder window ever again. 
You might want to for other reasons, but in theory, you can accomplish it all using the tools that we've learned today. 
This might all seem relatively basic for some of you, but as I mentioned, this is sort of the ramp-up period of now we all know how the shell works, and a lot of what we'll be doing in future lectures is using this knowledge to do really interesting things using the shell. 
That's sort of the interface that we're going to be using, and so it's important we all know it. 
We're gonna talk a lot more in the next lecture about how to automate tasks like this, how to write scripts that run a bunch of programs for you, and have to do things like conditionals and loops and stuff in your terminal, and do things like run a program until it fails, which can be handy in classes where you want to run something until your test suite fails, for example. 
So, that's the topic for next week's lecture. 
Did you have a question?
It's what you've been demoing, this 'assist' directory, that presumably will only work if you're running... 
That is a good question. 
I don't know whether the Windows subsystem for Linux will expose the sys file system. 
If it does, it probably only exposes a very small number of things. 
It might because there are... 
I don't know, check it out.
One thing you'll see is the lecture notes for this lecture are already online, and at the very bottom of the file, there are a bunch of exercises. 
Some of them are relatively easy, some of them are a little bit harder, and what we encourage you to do is to take a stab at going through them. 
If you know this stuff already, it should go really quickly. 
If you don't, it might teach you a bunch of things that you might not realize you didn't know. 
And for the office hours that we're gonna do right after this lecture, we will happily help you get through all of those, or if there are other commands that you learn in the process, you want to know how to use more efficiently. 
And then in the next lecture, which is tomorrow, we'll basically be assuming that you know the kind of stuff that the exercise is going to teach you.
There's also an email address on the website where you can send us questions if you think of something like after the office hours are finished. 
Are there any questions before we end today? No? Alright, well, we will have office hours on the ninth floor of the Gates Building, building 32, in like five minutes. 
Sweet, see you there.