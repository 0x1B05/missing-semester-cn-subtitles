Hi, can everyone hear me okay? Okay, so welcome back to
the missing semester of your CS education, today we're having as a lecture topic "potpourri".
It's gonna be some miscellaneous combination of topics that we the instructors find that are interesting.
But none of them were on their own lecture because they're certain topics that we just want you to know about,
because they can be really helpful And since we're not gonna delve into a lot of detail in the topics.
If you're more interested about them just feel free to come and ask us questions at the end or as we go over them in the lecture.
So the first thing I want to talk about is keyboard re-mappings/
So by now you probably realized that we have encouraged you to use the keyboard as your main input method.
So for example, when we went into the editors lecture, one of the main ideas of Vim was using your keyboard as much as possible.
So you don't have to rely on going to the mouse because going to the mouse is slow
And the thing is your keyboard, as with many things in your computer is nothing kind of magical, it can be configured
And it's worth configuring because a lot of the defaults might not be optimal
The most simple modification that you can do is just to remap keys
So one of the things we alerted in the editor lecture is the Caps Lock key is a really good key
because it's kind of right in the home row and it's kind of large and there, but it's not useful
You'll probably realize that you don't use your Caps Lock as often as when you'd want to use that So you can just remap your Caps Lock key
to something more useful as we mentioned like Escape if you're a Vim user or like Control if you're an Emacs user
or useful re-mappings, like a lot of the upper row 'F' function keys, or like Print Screen
You can remap them for example to your media key so like when you type Print Screen,
you probably don't have to do Print Screen that often but you probably want to play or pause your music
And pretty much every operating system has some tools that you can use to configure this
I'm not gonna go into the details but there's some of them listed in the notes
Erm... Let me check Oh yeah, another thing that you can do with keyboard re-mappings is
that you can do more complex combinations You can have a combination of keys mapped to some action
So for example, I have keyboard re-mappings that whenever I do Ctrl+Enter, I open a new terminal window
because that's a thing I do fairly often and by default on Mac there is no key binding to do that
or Ctrl+Shift+Enter will open a new browser window, another operation that I do on a daily basis
So I don't have to grab my mouse and go to Chrome to do that
You can also do remapping to perform actions If you don't want to be typing your password all the...
Sorry, not your password Your Email or your password, or for example, your MIT ID like, you may not remember it by heart
then you can just have a keyboard combination that will just perform the action of pasting that text
Lastly, there are more... Right now it looks like you just have to do some function of
"this is the keys that you press and this is the action that happens" But actually there are more complex keyboard combinations, and
as you go through you'll learn so you can do keyboard sequences So for example when we were dealing with tmux,
in tmux there was this notion of first you press Ctrl+A or Ctrl+B, like you press some prefix
and then some other key and that means something A lot of these softwares allow that
So for example, in my keyboard, since I'm not using Caps Lock at all, but every so often I have to use when my undergrad has some software that
relied on Caps Lock for changing modes Then I can press Shift five times in a row quickly, and then
this software that is in the middle interpreting these commands and remapping to some other, will send a single Caps Lock command for that
Some more examples of that is that I mentioned that you can use your Caps Lock key to map to Escape or Control
but you actually can remap it to both So in my computer when I just tap the Caps Lock key, that's interpreted as an Escape
However if I press it and hold it, this software can understand the difference between quickly pressing it and
just holding it for using in combination with some other key and then in that case it's mapped to Control
So a lot of these more advanced configurations are supported on a lot of these tools
As I mentioned we have a short list of good defaults for these programs for Windows, Mac OS and Linux
Any questions on this topic?
Okay, now I'm going to cover an unrelated topic to keyboard mappings [chuckles] We're going to see a lot of these unrelated transitions in this lecture
And it's the concept of "daemons" So probably you have... Maybe if you are not familiar with the world, it might seem alien,
but the concept of daemon you're pretty familiar with Most computers when you are running them there's software that you start and run,
like the commands that we have been seeing You'd type "ls" and then you're calling the List command
The "ls" command executes because you asked it to execute and then it finishes But a lot of other programs are just running as background processes
and they're just executing in the background and waiting for events to happen
or enabling some sort of functionality in your computer Examples of these processes may be like your Network Man[ager],
like the part of your computer that is managing the network or the part of your computer that is managing the display
Things like that You will see that a lot of what is enabled by daemons is
usually programs that end with a 'd' So for instance, when you are SSHing into a computer,
the receiving computer has to have a SSH daemon and the program is called "sshd"
and if this program is not running then there's no way for me to SSH into the computer
If the program is running, then that program will be listening and when you do SSH to that server,
then some incoming request is gonna enter the computer the computer is gonna send it to this daemon that is running in the background
and then the daemon is gonna check whether you have authorization, and if so, it's gonna start some login shell that you can start executing from
And different OSes handle this somewhat differently
The main idea is they all have some sort of system daemon that responds a lot of these smaller daemons
In Linux, which is one of the OSes that we are choosing for lot of the examples, the tool that you're using is the "systemd"
Again, first the system daemon is gonna start a lot of these processes and if you use this "systemctl" command,
you can check for the status of different daemons you can check for which ones are running,
you can ask it to start processes, to stop them This is kind of an once-off operation
You can also enable it and then disable them, which will tell the system to run them at boot
or stop running the boot if they were enabled And perhaps more interestingly, you can configure your own "systemd" units
So, so far all the examples are a lot of what the computer has to do,
but say you want to run a web server. One solution, you could just like, every time you start your computer you could open a tmux session
and then execute the command, but that's not really the way that your computer expects daemons to be run
The way your computer expects daemons to be run is by using some sort of "systemd" unit
It's like a configuration that tells "systemd" how to execute this process
So an example of this is... Here's a very simple example
So what is happening here is we're describing to "systemd" what needs to be done for this program to execute.
This example is just running a simple Python app You can think of it as a web server that can be implemented using
some Python web server library. And here we're saying this is the description, we're saying after, like this is important
"Systemd" has a list of services that have to start Like, all these daemons have to be started
but maybe there are dependencies between these daemons, so here we're saying "no, you should only start this after the network has been set up"
because otherwise how will you even try to configure our web server if I cannot listen to a network port?
And then we are defining what users should run this, because you may want to run this as your user, or maybe other user,
or maybe the root user should be running this, and then what command to run and under what directory
And whenever you have this, there can be small corner cases that you might have to debug, but
this is kind of the core idea and it can be really useful to automate the process of running processes in the background
A small side note to this is the fact that if you just want to run a command every so often, like in some periodicity,
say every morning I want to do something in my computer, you could write a daemon that just does something and then sleeps for a day, but
actually Linux and Mac OS have already a daemon that does this called "crond"
and "crond" will take another type of configuration file where you can say oh, I want to run a command every day at 8 AM,
or I want to run a command every 5 minutes, and it will just check for this event and execute it
And with a lot of things you will find that there are already daemons that have been configured for that
Any questions regarding daemons?
[student talking indistinctly] So the question is whether there is
a folder in the computer where all of these are So yeah, like yes and no Some of these configuration files are in a couple of different folders
depending on whether they are system daemons or they are user daemons Here you can see at the very first line is where you will place this
for the system daemon to recognize that it has been installed but if you just want to list all the daemons that are running,
in Linux for example, you can just do "systemctl status" and that's gonna print a tree of all the systems
and which daemon was spawned by which other daemon and a lot of them will be spawned directly by "systemd"
The next topic is going to be file systems in user space So, kind of a quick intro to this is the fact that whenever you're using a modern..
Oh, yeah, sorry And whenever you're using a modern operating system,
you are not tied to a specific file system So modern systems are fairly modular and you can for example,
in Linux there are different file systems that you can use, and the way this works is because the kernel, which is what is running most of the
operating system, has some modules that know how to interact with a file system
So usually when you do something like "touch foobar",
this is happening at a user level
and then this is going through to the kernel level
and there is some kind of layer here that is checking
where this action is happening to figure out what file system it is under 
So for example, you will have multiple disks
and all the different disks have different file systems, so the kernel has to figure out which file system operations to use,
and say this file might be in an "Ext4", which is the most common Linux one,
then whenever you do "touch foobar", the kernel will hear that
and then it will try to figure out like, oh, this lives in an Ext4 file system and it will perform the associated instruction for creating a file in an Ext4 file system
However, the caveat of having a system like this is right now I cannot have user code that defines how to create a file
and that might be kind of useful in some cases Say I want to have a file system that every time someone creates a file,
it sends me an email, so I can know that people are creating these files Here I cannot modify the kernel to add this
So the solution to this is something called "FUSE" And FUSE is a way of having file systems in user space
So what FUSE will do is, if this file instead of being in Ext4, if this file is in a FUSE file system,
FUSE will forward this operation to some other part of user call
that will say "oh, create this file" And here I can have the part of the code that sends an email to me,
saying "oh, this file has been created" And in case you want to still create the file,
it can forward back the request to do some more kernel operations
It might not seem really practical, but this is just the theory In practice, why this is useful is because
now you can have user level code that executes arbitrary actions when you try to perform file system operations
A really Interesting example of this is called "SSHFS"
On an SSHFS FUSE file system, whenever you try to create, open, read, write to a file,
instead of trying to do that to a local file, it has an SSH connection to a remote server
So if I try to create a file here, It will use that SSH connection to forward that operation to the remote system
and then it will perform it there So to all my local computer, to the rest of the programs running in my computer,
there is this path that looks like it is here, but all the operations that are performed to the path are forwarded
to the remote file system And with this idea, you will get some examples in the notes,
and you will find more online, of ways people have leveraged this capability to do fairly interesting file systems
So for example, if instead of having SSH, you don't care about SSH because you use Dropbox or Google Drive,
it's fine, people have implemented FUSE file systems that will mount locally
and every time you try to do an operation locally, actually it goes to one of these cloud storage providers
so you can also use something like Amazon S3, or Google Cloud Storage, that don't have the same kind of UI system
that we synchronize, as Dropbox or Google Drive Another application of this that is not related to doing something remotely,
is something like an encrypted file system You may have a file system that every time you try to write to a file,
you will try to write it in plain text, but it will capture that operation it will encrypt on the go,
and then it will save it as a regular file in your file system, but that's actually encrypted 
And once you dismount the file system, once you remove the FUSE connection,
all that is left in your computer are just regular files that are encrypted 
The last topic I wanna cover is backups and some good practices about them
The main idea is that for every file that you care about,
if you don't have a backup of that file, if you don't have a backup stored of that file, you can pretty much lose it at any moment
There are many different failure scenarios One of them is just hardware failure. So your hard drive can fail at any moment
So if you are just making a copy of your files in the same drive, that's not useful
If your hard drive fails, the files are gone The same goes if you have an external drive where you are making a copy,
but if you are storing everything in your home, and your home burns down... 
Which yes, it's unlikely, but if it happens you just lost all your data
So you'd have some sort of off-site backup for having this solution
Another thing to take into account is that synchronization or mirroring options are not backups
So Google Drive, Dropbox that I was mentioning, they will just propagate whatever is happening in your computer
This goes also for hardware mirroring, like RAID They are just making a copy If you accidentally delete a file,
or someone maliciously deletes your files, or encrypts them using some ransomware,
then you might have a copy, but you have a copy of the same useless data You actually have to have a solution of how you're running your backups
And you should be asking yourself what actually someone needs to know/have about you in order to delete all your data
And we have linked different softwares in the notes about how to do this
The last thing I want to mention about backups is that a lot of the time when you think about backups, you just think about the local files
and like, all my photos and my tax return, and how can I make a backup of that? 
But increasingly in the modern age, there are more and more web applications
and a lot of data might only live in some cloud provider, like for example if you have Webmail,
and you're not synchronizing it to your computer, It's only living in that provider's servers
And if you don't have a copy for that and for some reason you lose access to that account because you forgot your password, you got hacked,
they think you have violated the terms of service... All that data is gone So you should look into some tools that people have developed
for making offline copies of all that data so you can make regular backups of that
And that kind of ends this short section on backups Any questions so far?
(student) When you said that a hard drive can fail at any time Is there a reason for it to fail?
[unintelligible] Like if I have my external hard drive sitting at my parents house or something
And my computer here, is that enough? Or any drive can just fail?
(Jose) Any drive can fail at any moment. Like we don't... Different media have different rates of failure
and there are really good statistics online So for example, spinning hard drives have a higher rate of failure than SSDs for example,
like Solid-State Drives And what's another case? Or like CD drives for example
But if you drop a hard drive it's a higher rate of that failing, of course
But in general we don't really have an end-all solution
for saying "this media is not gonna fail" Like pretty much, like SD cards, SSDs, hard drives, CDs, degrade with time
Pretty much every data is kind of bound to this degradation or like this fact that it could be lost at any moment
And you should also know that data can become corrupted, your disk might look like it's okay, but maybe some files were corrupted
and something like synchronization techniques, like Google Drive or Dropbox, will propagate that corruption
And by the time that you realize that things have gone wrong it's maybe too late
(Jon) Alright, we're gonna continue this trend of jumping between random topics and talk about APIs
So so far we've really been talking about how do you do things more efficiently locally on your computer?
Like I want to accomplish this task more efficiently. How do I like configure my editor? How do I use my shell?
But one thing you should realize is that very often you can integrate with the outside world as well 
Most services that you interact with in your day-to-day provide some kind of API
For you to interact with the data that they store or the services that they provide 
And usually those APIs are pretty well documented
If you looked at the APIs for things like Facebook, or Twitter, or Google Drive, or Gmail
Many of these have interfaces that you can interact with in order to use those services from your local machine
What's really neat is that you can often combine this with some of the stuff that we've talked about in lecture so far
Like for example in the Data Wrangling lecture, we looked at how you can create 
These pipelines to extract data from some source that has a different format than you expected
So for instance, the US government has a free service 
Where you can request the weather forecast for any given location in the US
And what you do is there is a URL that you request 
And if you set the right parameters in that URL and then just fetch it, what you get back is JSON
Which is sort of a well-defined data format that you can then parse 
And you can extract things like your 14-day weather forecast
And maybe you then pipe that into your shell and produce some kind of like Handy alias in your terminal that's just gonna print...
Of some handy reference for the next 14 days of weather in whatever location you were in
These are things you can pretty easily construct and there's some notes... There are some notes in the notes about how you might go about this
In general when you interact with these APIs, you're going to be using URLs of one form or another
And the exact format varies from service to service But in general the URL is going to contain some set of parameters
But ultimately you're just gonna issue a web request to them and you're gonna get data back in some format
One command you should be aware of for interacting with these types of things is one called "curl" So curl is a program that you invoke, you give it a URL
And it just fetches that URL and gives you back the response What you do with that response is entirely up to you
Maybe you pipe it through a program like "gq". Sorry, "jq" So jq is a JSON query tool that lets you take in data that's formatted as JSON
And then write a query over it to extract data that you're interested in 
And this is one of the ways in which you can layer these tools to extract the data that you're interested in
Some of these services also require that you authenticate in one way or another
Like for example if you want to interact with a Facebook API 
You need to have some authenticated token that proves who you are as far as Facebook is concerned
Otherwise, they can't say whether you're allowed to say, create a post as a given user 
Very often these things are gonna use something called "OAuth", although not always
And you should look at the documentation for every service you care about 
In general though you will get some kind of secret token back from the service
That you have to include in the requests that you make to them 
Either in the URL or in additional sort of web headers, which you can also send with curl
Keep in mind though that these tokens are secret. They are another representation of your user
And anyone who gets their hand on them can basically pretend to be you They can do whatever you can do with that token
So keep this in mind, don't stick them in your "dotfiles" and then push them onto GitHub That will land you in trouble. 
You should think of them as a password
There are also really neat tools online for integrating services So there's a service called "If This Then That"
Which basically provides integrations with a bunch of different services and lets you chain them together
And then also access them partially locally if you wish This is something that's worth looking into
If there's a particular service you would like to interact with in a more efficient manner
Any questions about APIs?
Alright Switching gears entirely, let's talk about command-line arguments So command-line tools. There are a lot of them
And most of them take different arguments because they do different things 
We've talked about looking at man pages for commands and that will tell you how this particular command works
What kind of flags and options you might give to it And what it actually does when you invoke it
But there are some common themes that are useful to know about Either in arguments that many programs take, or just concepts the many of them apply
The first of these we already mentioned a little bit in the lecture on command-line environments
Which is the "--help" flag. Very often you can pass this to a program and instead of running,
It will just print out information about how you can run this program, often in a very short, condensed way
A similar one is the "--version" flag, which just prints the version of the software you're using
This can be really handy if you're doing something like filing a bug report, 
Which Anish is going to talk a little bit about later And you want to report what version you're running on in case the bug has been fixed since
Often you can also do "-V" and that means the same as version. But again, check the man page
There's also "--verbose", or "-v" often
Which is a flag that lets you increase the output of the program It makes the program print more about what it is doing. And very often you can repeat this flag
So you can do like "-vvvvv" in order to get more information from that tool
And this can be especially useful if you're trying to debug a problem If you're like running "rsync" and you want to know why did it decide to copy this file?
Or why did it decide not to copy this file? That kind of debug output can be useful
And often there's a sort of an inverse flag called "quiet" or "silent" Which means that the tool will not print anything unless it was like an error
Anything else, it will stay quiet about Many tools, especially those that do destructive actions
Or some kind of action that you cannot undo Provide what's known as a "dry run flag"
Exactly how this is represented in the command line varies from tool to tool But essentially what this dry run mode will do is it will run the tool, 
but it will not actually make any changes
Instead it will just inform you of what it would have done if you hadn't run it with dry run
Many of these tools also have an interactive mode. So for example, th "rm" and "mv" tools both do
Often just "-i", although not always When you run a tool in interactive mode
It will usually prompt you whenever it's about to do an action that you can't undo And it will sort of prompt you for a confirmation that it should actually go ahead
When we're talking about destructive tools Many of them are non-recursive by default
If you try to remove a directory or you try to operate on a full directory They will not continue into the files inside of that directory
The reason being you might accidentally like, remove your entire hard drive and that seems bad
Therefore for many of these tools, they have a "recurse" flag Often "-r", but again not always
Which lets them traverse down into the tree to go deeper, but you need to opt-in to this behavior
So this is for example, the case for "rm". This is also the case for "cp"
In many tools, when they ask you to give a file name or a path And we talked about this a little bit in the data wrangling lecture
Instead of giving a file name, you can often just give a dash Just a single "-", and what that means is standard input or standard output
Depending on whether that argument is an input file or an output file 
This is handy to know about if you're trying to construct those kinds of data wrangling pipelines that we've talked about before
Many tools will also default to using standard in or standard out if you don't give any file name at all
Sometimes you want to pass something that looks like a flag or an option to a command
But you don't actually want it to be interpreted as a flag or an option Consider for example, if you wanted to remove a file called "-i", what would you do?
Right, if you write the following command... "rm -i"
Well, "-i" is a flag to "rm", so "rm" would, when you run this command say
Tell me what file to remove, you haven't given me a file And it's because it interprets this as a flag
Similarly, if you do something like... "ssh some machine, some command, and let's say, dash r"
So this is saying - run command "foo" on this machine over SSH, and I wanna pass "-r" to "foo"
Well, the way that both of these are gonna get interpreted is that these are flags
Or in this case, this is a flag. But to this command
Which is probably not what you expected Actually in the case of SSH, it has some weird special behavior for some of these
But often if you want something to not be interpreted as a flag There's a very simple way to opt-out of that, and that is using double dash
If you use double dash, what you tell the command Is that verything following this, you should not interpret
So it will not be considered a flag or an option. In the case of "rm" you can do this
And now "rm" you will see that the first argument is a "--" And then it will keep reading arguments, but it will not interpret them as flags
So when it gets the "-i", it will not interpret it as the dash i flag, but just as an argument dash i
Similarly, for SSH you can do this to indicate that these are both positional arguments
They are not flags or options and you should not interpret things that start with a dash
(student) But if you do like "--version", it's not gonna trigger that? (Jon) No, so this is a " -- ", with a space on both sides
Any questions about any of this sort of command line conventions business?
Alright then let's talk about window managers So most of you are used to some kind of drag-and-drop window manager
If you're running Windows, or Mac OS, or Ubuntu - what comes with the machine is like, there are windows and
They overlap partially on screen and you can like drag-and-drop and move them around and resize them and stuff
And that works fine But it is not the only way to manage windows on your computer it turns out
So what you are used to is something called a floating window manager, but not all window managers are floating
Often you can opt-in to other types of window managers that have different behavior for how they arrange your desktop
A common alternative is a tiling window manager So in a tiling window manager, rather than having floating windows,
Everything is set up into a tiled layout. When you start a program, its window is maximized
If you start another program, the original window shrinks in size And then the new window takes up some subset of the total desktop space
At no time is your desktop background visible unless you have no programs open 
All of the programs you have open on any given desktop are going to share that space
This looks a little bit like tmux panes, like we talked about earlier, where you can sort of split them in various directions
And one of the reasons why this is handy is it means you basically never need to go to your mouse
In order to move between windows, there are keyboard shortcuts to move to different windows 
There are keyboard shortcuts for resizing the windows or swapping them around on screen
And this turns out to be a pretty efficient way to manage windows in your computer
Well, I won't go into too much of detail of what kind of window managers you might use Just know that these exist out there and they're worth giving a look. 
They can be a lot more efficient to work with
Questions about window managers? All right, VPNs. Totally related to the previous topic
So VPNs are like all the rage these days and this makes me really sad It's not clear that VPNs are all the rage for any good reason
Because you should be aware of what a VPN does and does not get you A VPN, in the best case, is really just a way for you to change your internet service provider
It's a way for you to make traffic on the Internet seem like it's coming from somewhere else than where you actually are
While that might seem attractive for certain purposes, It's a little unclear what it buys you in terms of security
Because all you're really doing is shifting who you are trusting Rather than trusting who is providing your current internet service,
You're trusting that whatever business is giving you that VPN service...
You're trusting that they, first of all, have set up this VPN business correctly, But also that they are not tracking what you are doing
And it's not clear whether that change of trust is actually worth it If you're sitting at some like dodgy public Wi-Fi network then maybe
But if you're sitting at MIT, it's not clear Do you trust your VPN provider more than you trust MIT's IS&T?
Or maybe you do, but that is a decision that you need to make About what you trust, who you trust and why?
You should also know that much of your traffic, Especially the stuff that's on a sensitive nature on the Internet, is already encrypted
Whether that's sort of HTTPS or other protocols that use something like TLS, a lot of the sensitive data is already
Sent over encrypted channels and it doesn't really matter who your network provider is if you're on a dodgy Wi-Fi network
The stuff that matters is probably encrypted anyway. Might not be, but if it's not
Then your VPN provider can also see it in plain text just as much as whoever's hosting this dodgy Wi-Fi network
And notice that I said "in the best case above" There are VPN providers who have been shown to be malicious, that do logging of all your traffic,
That sell that traffic to third parties. There are VPN providers that have forgotten to enable encryption on the VPN
All of these are real problems And so you should think very carefully about whether a VPN actually serves any good purpose for you
Questions about VPNs? Yes? (student) So I have a question about public Wi-Fi networks, because
The traffic from your computer to the router isn't encrypted between the computer and the router, right?
Except for what normally is via HTTPS and [unintelligible] So then doesn't that mean that people could sniff out what domains I'm going to via the DNS request?
(Jon) So, it's a very good question If you're on a public Wi-Fi network, then the traffic between you and the wireless access point is not encrypted
At least it's not encrypted sort of on the outer layer, but it might be encrypted in like HTTPS, for example
And it is totally true the people observing that Wi-Fi network will be able to see anything that is not encrypted
But the solution to that is to encrypt all your traffic, rather than necessarily going through a VPN
So one way to do this for example is to use DNS over TLS or DNS over HTTPS, which gives you
A way to actually encrypt even information that might otherwise leak in plain text
Rather than try to sort of trust some provider to do that for you Now that said, 
in some cases you might have a trusted institution that provides a VPN network for you
So for example, MIT provides a VPN network for all MIT students and staff that you can sign up to use
And in that case you probably trust MIT more than the other networks you might be on So it might be worth it, but it's something for you to think about
(student) When you say you could encrypt it with, what was it, DNS, how would you do that?
(Jon) So, DNS is the way that people turn domain names, Or your computer turns domain names into IP addresses to know what computer to connect to
And that protocol by default is in plain text. There's nothing encrypted about it 
There are various ways to encrypt your DNS traffic
Some of them are standardized and some of them are not I won't go into the exact mechanics here, but you should google it and look at some of the ways
Okay. The last thing I want to talk about is Markdown 
So there is a high chance that some of you are going to write text over the remaining part of your life
And you will want to mark up that text in various simple ways And one thing you could do is start up Word or use LaTeX or something like that to mark up your documents
But that is a pretty heavy-handed approach Instead it would be nice if we could just sort of Write things the way we feel like they should be
I don't know how to describe it in a better way but sort of the natural way where if you want something, 
If you want to put emphasis on a word you just put like stars around it or something, and then it just works
Markdown is essentially that. It is a way to try to encode the way that we often write text somewhat naturally
Into a markup language that lets you write things like bold text, links, lists, that sort of stuff
In fact, all of the lecture notes for this class have been written using Markdown And Markdown is really very straightforward. The basic rules are in the notes
But the basic things you need to know is, in Markdown if you put stars
Around a word, that word is emphasized. Or some sequence of words. If you put double stars
That word is emphasized strongly, also known as bold There are various other things you could do, like if you put a dash before a line it is now a list
And there's one list item and you can amend list items If you put "1." in front, or some other number, it becomes a numbered list
If you put a pound sign in front of something it becomes a header, like some kind of title header
If you put multiple of them, they become subheadings and you can keep adding more to these
If you want to write code you can put a single backtick, followed by some code, followed by a backtick
And now that is rendered in Monospaced Font If you want multiple lines of code, you do a triple backtick, and then code,
And then maybe some more code and then triple backtick And in many cases like if you're on GitHub for example,
You can even type the name of a language up here after the backticks, without a space And it will be syntax highlighted in the language of your choice
This is a really handy thing that is supported in so many websites nowadays you might not even realize
Like in Facebook Messenger you can use many of these They don't actually officially say they support Markdown anywhere
But many of these things just like sort of happen to work and it's worth learning At least the basics and just start using them. 
You can do links and stuff as well, but that's already in the notes
Any questions about Markdown? Right, Anish, you're up
(Anish) Is my microphone working? Is this working? Can you guys hear me in the back?
The light's green. Oh, I think I can hear it. Okay, great
So, continuing with our theme of random topics that are all unrelated to the previous topics we've been talking about,
The next thing we're going to talk about is a program called "Hammerspoon" Which is a tool for doing desktop automation on Mac OS
And I think there's similar tools for Windows and Linux, a lot of the ideas can carry over 
You can google it if you want to figure out how to do these things on other platforms
But basically Hammerspoon is a program that lets you write Lua scripts, scripts in a programming language 
That interact with various operating system functionality
So you can write code that interacts with the keyboard and mouse and connects that to window management, 
To display management, the file system, battery and power management, Wi-Fi... All sorts of stuff
Like basically all the things that your operating system manages, this tool lets you hook into those things 
And so it can let you do all sorts of neat things by writing just a couple lines of code
Just some examples of cool things you can do with this tool are - you can bind hotkeys to move windows to
Specific locations. So a demonstration of this is here. I have this window open. I press, in my particular setup,
"Option+Command+Right" and this window moves to the right. "Option+Command+Left", this window moves to the left 
And I have a couple other shortcuts for moving things to various places
And so I can kind of have an effect similar to tiling window managers that Jon was talking about earlier
I can move windows to different parts of my screen to set things up in a particular way rather than have to use the mouse
To position things where I want them to be and then like click and drag to resize windows to the right shape 
Just a keyboard shortcut can do the trick
But this tool is not limited to just moving windows around and binding that to particularly keyboard shortcuts 
You can do other things like create a menu bar button with a bunch of different options
And you can bind those different options to do different things. So in my particular case I've created this little menu 
and then I have a bunch of different things that I do reasonably frequently and clicking on these things
Invokes a particular Lua function that I've written that interacts with this library. So for example
Here, this "Rescue windows" thing is a particular thing where I often work with multiple displays 
And sometimes my operating system gets confused and I have some window that ends up off of my display and
How do I how do I get this thing back? Well, that's what this... Whoops, not that
That's what this "Rescue windows" thing does. It brings windows that are off the screen back onto the screen
Another neat thing I have setup here is I have particular layouts that I've named So like a dorm, and a Lab and a Laptop layout
So for example in my Lab I have this screen and I have another screen and have another screen besides that in a different orientation
And I have this particular setup that I want where I want Maybe my terminal full screen on here, and my chat program over here and
This screen split up into five segments with different programs in different places 
Here I can, when I show up to Lab I can just go here and click "Layout Lab" and it will invoke some code
Which is not all that complicated like 10 lines of code describes a particular layout
And it will instantiate that layout and put all the things where they need to go 
I could even in theory automate some of these things where my computer could figure out like I plug in a display and my computer knows
Oh, this is the display that you have in your lab. Let me automatically instantiate this layout for you 
That's another thing you can do with Hammerspoon. 
And also other wacky things you can do like you can do things like
It can detect your Wi-Fi network that you're on so it knows kind of where you are Maybe I have a different Wi-Fi network name at home versus in lab 
and I can do things like when I show up to lab
Automatically mute my speakers. So I don't have like embarrassing music play out loud in my lab Another kind of cool example is
So I have a Mac It has a fancy power supply and a lot of my friends have Computers that look the same as this and their power supply bricks look the same as mine and
Sometimes I use their power brick because I forgot mine at home or something This tool can actually with like three or four lines of code
Do neat things like show you a warning like it'll pop up a warning if you've accidentally taken your friend's power supply and plugged it
Into your computer instead of using your own So at a high level this tool lets you run arbitrary Lua code and do things like bind it to menu buttons or key presses and
It interacts with a large part of the operating system in order to do all sorts of cool stuff 
So that is Hammerspoon, any questions about that?
Cool. Moving on to the next topic Completely unrelated to the previous one, it's booting and live USBs
So the operating system on your computer, Windows or Mac OS or whatever you're used to is not exactly the first thing that runs on your machine when it turns on. 
There's something else that
happens in the boot process before your operating system is loaded and There's some interesting stuff that you can do there. 
So you might have seen when you turn on your computer
It says something like press F9 to configure the BIOS or press F12 to enter the boot menu
The particular key sequences may depend on your machine and specific configuration But this is a general pattern 
and you can configure all sorts of interesting Hardware related stuff here
So it's worth checking out And another thing you can do in this boot menu is you can have your computer start off from an alternate boot device
so by default like my laptop here has a solid-state drive and it boots Mac OS when it turns on but I can also say plug in a USB flash drive that has an operating system installed on the flash drive and then at boot tell my computer to
Boot from that flash drive instead of the built-in solid-state disk, and this is useful for example If I've broken my operating system
Install and I want to do something like get the data off my computer or maybe want to fix the operating system
Like maybe there's some critical files somewhere that I've deleted or I forgot my password I need to go like tweak some files in order to reset it booting from a live USB
Booting from the separate operating system that's installed on a flash drive can let me do that like boot up my operating system Mount the hard disk that's on my current machine
I'm working on and then go make some tweaks or copy data off of that 
And so live USBs are really useful and in the lecture notes, we've linked to a tool that can help you create them really easily
Any questions about the boot process or live USBs?
All right, next topic is virtual machines, Vagrant, Docker, the cloud and OpenStack
I think last year we had an entire lecture on this topic this year We're going to condense it into like one minute
So at a high level virtual machines and similar tools like containers let you emulate a whole computer system within your current machine
so like I'm running Mac OS here, but within my Mac OS environment I can
Simulate a machine that's running say Ubuntu or some other operating system 
And this is a nice way of creating an isolated environment for testing or for development or for exploration
For example doing things like running potentially malicious code that should be isolated from my current environment. I
Think the most common use case for programmers is to use virtual machines are containers to create development environments
So I'm using Mac OS and I have some of services and stuff and libraries installed in my current machine
But I might want for example I'm working on some web programming project and I want it to run on an
Debian machine and I need Postgres, like a database server installed rather than install that all on my Mac OS machine
I can instantiate this new machine just for the development purposes
Now virtual machines, like that's a general concept. There are a bunch of programs that let you
that are called virtual machine hypervisors that 
Support this functionality on your machine and then there are tools that let you script these hypervisors in order to specify machine
Configurations like operating system and like what packages you want installed and what services you want installed in plain text
And so this is an example on the screen right here. 
And this is done using a system called Vagrant.
It's linked in the lecture notes. You can look into this if you're curious. 
So basically and the short plain text file I can specify okay. I want a machine that's running Debian
It should have "postgres" and Redis and Python and stuff installed on it. 
And then, once I have this configuration,
I can just type in "vagrant up", and what it does is, it reads this file and instantiates a new machine based on this
configuration. And then, after I've done that I can do "vagrant ssh" to SSH into this virtual machine.
So it's not a remote machine running on some other piece of hardware somewhere, It's just simulated on my own machine, but now here I have an Ubuntu box like I do now, let's be really stashe
It's like or sorry not a bunch of debian here with all the things I want installed in here and I can do my development inside this isolated environment,
and not, kind of, install all this junk on my MacOS machine. 
Now so that's vagrant there's similar tools like docker that are conceptually similar but use containers instead of virtual machines
It's a distinction that we're not going to talk about in too much detail right now 
And so you can run VMs on your own computer
but you can also rent virtual machines on the cloud and so It's a nice way to get instant access to like one example is you might want a computer
That's always on always connected to the internet and has a public IP address. Like maybe you want to run a web server 
That's always available or you want to run some other service like say a slack bot or something like that
Well a virtual machine rented on the cloud is one nice way to get that and these are pretty cheap for like a low capacity machine with a
small CP and small amount of disk space and You might want to do is get access to a machine that's really powerful like with a lot of CPU cores or with a lot
Of RAM or with a whole bunch of GPUs for some specific purpose like say you're doing deep learning or so You're doing some other sorts of sensitive computation
Well, that's another thing you can do with VMs on the cloud 
And finally, you can get access to many more machines than you have physical access to. Like if I need a thousand machines
But only for two minutes to do some very parallel tasks. That's something I can easily do with virtual machines and
Popular services for doing this are things like Amazon AWS or Google cloud 
And if you're a member of MIT CSAIL, you can also get free VMs for research purposes using the CSAIL OpenStack
And so this is also linked in the lecture notes So any questions about virtual machines, or Vagrant, Docker, or anything like that?
So the question is when I say I'm running Ubuntu, or, actually, in this case, it's Debian
when I'm running Ubuntu here, do I have, like, Ubuntu installed on my machine, or What exactly is going on here? 
So, basically, what Vagrant did for me when I type "vagrant up" is,
because I've specified I want Debian here, it downloaded Debian from the internet, like, set up a disk image for this new machine,
installed Debian into that disk image, then went to install these programs So, like, yes, this is on my computer. 
But, all of this is just in a particular file that's a disk image.
And then, I'm emulating a machine that is basically completely isolated from my current machine. 
This is being run as a process on my current machine. Does that answer the question? Any other questions about VMs?
Great. Next topic is also going to be a quick mention - So, a lot of you are programmers, and you're used to writing programs
in a tool like Vim, or some other editor that you're comfortable with. Another thing that can be really neat to use for particular tasks, is something called a notebook programming environment.
And this is a more interactive way of writing programs. 
Here on the screen, I have a demo. This is something called Jupiter notebook and it can be used for writing Python programs. I think they also support some other languages.
And, basically, this is a nice way of doing interactive programming. 
So, normally, you're used to writing a big program in a file, or a collection of files,
and once you're done writing it, you can just run the whole program. But, this lets you be a little bit more flexible and
run little snippets of code at a time. Like, for example, I can break my program into these little pieces. 
It's just some random code I wrote. And, I can say, "execute this cell", and I press a particular key combination to execute the cell.
But then, I can go back, and tweak my program a little bit. Like, say I want to have this be lowercase instead. 
Then, I can execute this cell, and then go and evaluate this thing, and
this way I can, kind of, run little snippets of code, within a Python environment.
And, it's a nice way of building up programs, piece by piece, rather than having to write everything at once. 
This is really useful for particular research purposes, like I think a lot of people use these for doing machine learning work, for example.
Any questions about the idea of notebook programming environments? They're worth checking out.
Oh, so the question is, "This looks like it's online, is there an offline version of Jupiter notebooks?" 
So, actually, this is the thing that runs in the browser, but it's running locally...
So, I don't know if you can see it on the screen, because it's kind of small, but up here it says, "localhost:8888".
Here, I have running on my own local machine, a Jupiter notebook. And, they've just built it
so it runs within the web browser. That being said, there are also online
Jupiter notebooks that you can use, where the Python kernel is actually running on some remote machine. You might want to do this
for example, like on my laptop, I don't have a fancy GPU, but in my room, 
I have a machine with a fancy GPU. And so, when I'm doing machine learning
work, I often SSH into that machine, run a Jupiter notebook on there, and then open up the interface, in my local web browser,
so I have access to that powerful GPU running on my different machine. Any other questions?
Great. The final thing we're going to talk about today is Github. 
So, we touched on this a little bit during the version control lecture.
But, Github is one of the most popular platforms for open-source software development. It hosts source code. 
It hosts git repositories. But, they also have other tools for managing a project.
And, like, a lot of the tools we've talked about in this class are hosted on Github. For example,
like, Hammerspoon, the thing we just talked about, is developed on Github It's really easy to get started
contributing to open-source projects on Github to help improve the tools that you use every day.
There are two primary ways you can contribute to projects on Github.
Let's open up some repository. We can actually go to the Github repository for the class website.
So, this is an open-source software project. 
And... Let's zoom in a little bit. So the two ways you can contribute to projects on Github -
The two main ways are through issues and pull requests One thing that's actually really helpful to developers, 
and also pretty lightweight and easy for users to do, is to report issues with a
software project. Like, say you're using somebody's program and you encounter some bug... 
Writing a high quality issue is actually super helpful to developers and hopefully doesn't take you too much time.
And so, you can go to here, like find the project on GitHub, go to the issues page, and click on new issue, and then
write some high quality bug report. 
And then, hopefully the developer will respond and fix the issue for you. So, for example, for this class,
like, one of the students in this class pointed out an issue with our lecture notes, and after she pointed it out,
I said, okay, like, that looks like a reasonable thing. Let's fix it. 
And in this particular case, instead of fixing it myself,
I actually asked this person, "Do they just want to fix it for me?" 
And so, that leads into the other thing I want to talk about: issues and pull requests. So, pull requests are the second way to contribute to projects on Github. 
And, this involves actually contributing code back to the project. 
And so if we look at the pull request for this particular project, you'll see that a bunch of people have submitted code changes.
And, the process for doing so - so this is showing the difference, the patch, that this person submitted.
Basically, the process for creating pull requests is a little bit more involved than submitting issues. Like you're not just submitting text:
you're actually going to modify their source code. 
And so, we've linked to some guides that explain the process in a little bit more detail.
But, at a high level, what you do is, you take the repository on Github, "fork" it, and then download it locally,
so now have your own, local copy. Then, you can go and work on it, do some development work, and fix a bug or add a feature, and then, eventually, you
send what's called a "pull request" back to the original developer. So, you say, 'Here. I've made some changes. 
Can you please incorporate them back into the original project?'
And, after that point, what usually happens with these projects, is that the maintainer will go back and forth with you, giving you feedback on
the changes you've proposed. 
And, eventually, once everybody's happy, they will "merge" in your changes, and they'll be available to everybody who uses the project.
So, that is how you can contribute to projects on GitHub and make software better for everybody. And, so any questions about GitHub?
Cool, okay. So, that is it for the topics for today. Any questions about the lecture overall?
Great, okay, so before - before we finish, a quick description about tomorrow's lecture: so, today was all the topics we thought are interesting,
we should talk about, tomorrow's lecture is gonna be about all the topics you think are interesting and that we should talk about. So, tomorrow is going to be
a Q&A lecture. And, after today, uh, after the lecture, we'll submit, we'll email out a link where you can submit questions for us to answer. 
And so, please go and fill that out,
otherwise, we won't have too much to talk about tomorrow. Great, so hopefully, see you tomorrow in our Q&A lecture.