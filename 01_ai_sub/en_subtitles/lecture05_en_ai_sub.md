Okay, can everyone hear me okay? Okay, so welcome back.
I'm gonna address a couple of items in kind of the administratrivia. With the end of the first week,
we sent an email, noticing you that we have uploaded the videos for the first week, so you can now find them online.
They have all the screen recordings for the things that we were doing, so you can go back to them.
Look if you're were confused about if we did something quick and, again, feel free to ask us any questions if anything in the lecture notes is not clear. We also sent you a
survey so you can give us feedback about what was not clear, what items you would want a more thorough explanation or
just any other item, if you're finding the exercises too hard, too easy,
go into that URL and we'll really appreciate getting that feedback, because that will make the course better
for the remaining lectures and for future iterations of the course. With that out of the way
Oh, and we're gonna try to upload the videos in a more timely manner. We don't want to kind of wait until the end of the week for that. So keep tuned for that.
That out of the way, now I'm gonna This lecture's called command-line environment and we're
going to cover a few different topics. So the main topics we're gonna
cover, so you can keep track, it's probably better here, keep track of what I'm talking.
The first is gonna be job control. The second one is going to be
terminal multiplexers.
Then I'm going to explain what dotfiles are and how to configure your shell.
And lastly, how to efficiently work with remote machines. So if things are not
fully clear, kind of keep the structure. They all kind of interact in some way, of how you use your terminal,
but they are somewhat separate topics, so keep that in mind. So let's go with job control. So far we have been using the shell in a very, kind of
mono-command way. Like, you execute a command and then the command executes, then you get some output, and that's all about what you can do.
And if you want to run several things, it's not clear how you will do it. Or if you want to stop the execution of a program, it's again,
like how do I know how to stop a program? Let's showcase this with a command called sleep.
Sleep is a command that takes an argument, and that argument is going to be an integer number, and it will sleep.
It will just kind of be there, on the background, for that many seconds. So if we do something like sleep 20, this process is gonna be sleeping for 20 seconds.
But we don't want to wait 20 seconds for the command to complete. So what we can do is type "Ctrl+C".
By typing "Ctrl+C" We can see that, here, the terminal let us know,
and it's part of the syntax that we covered in the editors / Vim lecture, that we typed "Ctrl+C" and it stopped the execution of the process.
What is actually going on here is that this is using a UNIX communication mechanism called signals.
When we type "Ctrl+C", what the terminal did for us, or the shell did for us,
is send a signal called SIGINT, that stands for SIGnal INTerrupt, that tells the program to stop itself.
And there are many, many, many signals of this kind. If you do man signal,
and just go down a little bit, here you have a list of them.
They all have number identifiers, they have kind of a short name and you can find a description.
So for example, the one I have just described is here, number 2, SIGINT.
This is the signal that a terminal will send to a program when it wants to interrupt its execution.
A few more to be familiar with is SIGQUIT, this is
again, if you work from a terminal and you want to quit the execution of a program.
For most programs it will do the same thing, but we're gonna showcase now a program which will be different,
and this is the signal that will be sent. It can be confusing sometimes. Looking at these signals, for example, the SIGTERM is
for most cases equivalent to SIGINT and SIGQUIT but it's just when it's not sent through a terminal.
A few more that we're gonna cover is SIGHUP, it's when there's like a hang-up in the terminal.
So for example, when you are in your terminal, if you close your terminal and there are still things running in the terminal,
that's the signal that the program is gonna send to all the processes to tell that they should close,
like there was a hang-up in the command line communication
and they should close now. Signals can do more things than just stopping, interrupting programs and asking them to finish.
You can for example use the SIGSTOP to pause the execution of the program, and then you can use the
SIGCONT command for continuing, to continue the execution of the program at a point later in time.
Since all of this might be slightly too abstract, let's see a few examples.
First, let's showcase a Python program. I'm going to very quickly go through the program.
This is a Python program, that like most python programs,
is importing this signal library and is defining this handler here. And this handler is writing,
"Oh, I got a SIGINT, but I'm not gonna stop here". And after that, we tell Python that we want this program, when it gets a SIGINT, to stop.
The rest of the program is a very silly program that is just going to be printing numbers. So let's see this in action.
We do Python SIGINT. And it's counting. We try doing "Ctrl+C", this sends a SIGINT,
but the program didn't actually stop. This is because we have a way in the program of
dealing with this exception, and we didn't want to exit. If we send a SIGQUIT, which is done through
"Ctrl+\", here, we can see that since the program doesn't have a way of dealing with SIGQUIT,
it does the default operation, which is terminate the program.
And you could use this, for example, if someone Ctrl+C's your program, and your program is supposed to do something,
like you maybe want to save the intermediate state of your program to a file, so you can recover it for later.
This is how you could write a handler like this.
Can you repeat the question? What did you type right now, when it stopped? So I...
So what I typed is, I type "Ctrl+C" to try to stop it but it didn't, because SIGINT is captured by the program. Then I type
"Ctrl+\", which sends a SIGQUIT, which is a different signal,
and this signal is not captured by the program. It's also worth mentioning that there is a couple of
signals that cannot be captured by software. There is a couple of signals
like SIGKILL that cannot be captured. Like that, it will
terminate the execution of the process, no matter what. And it can be sometimes harmful. You do not want to be using it by
default, because this can leave for example an orphan child, orphaned children processes. Like if a process has other small children processes that it started, and you
SIGKILL it, all of those will keep running in there, but they won't have a parent, and you can maybe have a really weird behavior going on.
What signal is given to the program if we log off? If you log off?
That would be... so for example, if you're in an SSH connection and you close the connection, that is the hang-up signal,
SIGHUP, which I'm gonna cover in an example. So this is what would be sent up.
And you could write for example, if you want the process to keep working even if you close
that, you can write a wrapper around that to ignore that signal.
Let's display what we could do with the stop and continue.
So, for example, we can start a really long process. Let's sleep a thousand, we're gonna take forever.
We can control-c, "Ctrl+Z", sorry, and if we do "Ctrl+Z" we can see that the terminal is saying "it's suspended".
What this actually meant is that this process was sent a SIGSTOP signal and now is
still there, you could continue its execution, but right now it's completely stopped and in the background
and we can launch a different program. When we try to run this program,
please notice that I have included an "&" at the end. This tells bash that I want this program to start running in the background.
This is kind of related to all these concepts of running programs in the shell, but backgrounded.
And what is gonna happen is the program is gonna start but it's not gonna take over my prompt.
If I just ran this command without this, I could not do anything. I would have no access to the prompt until the command either finished
or I ended it abruptly. But if I do this, it's saying "there's a new process which is this".
This is the process identifying number, we can ignore this for now. If I type the command "jobs", I get the output that I have a suspended job
that is the "sleep 1000" job. And then I have another running job,
which is this "NOHUP sleep 2000". Say I want to continue the first job.
The first job is suspended, it's not executing anymore. I can continue that doing "BG %1"
That "%" is referring to the fact that I want to refer to this specific
process. And now, if I do that and I look at the jobs, now this job is running again. Now
both of them are running. If I wanted to stop these all, I can use the kill command.
The kill command is for killing jobs,
which is just stopping them, intuitively, but actually it's really useful. The kill command just allows you to send any sort of Unix signal.
So here for example, instead of killing it completely, we just send a stop signal.
Here I'm gonna send a stop signal, which is gonna pause the process again. I still have to include the identifier,
because without the identifier the shell wouldn't know whether to stop the first one or the second one.
Now it's said this has been suspended, because there was a signal sent.
If I do "jobs", again, we can see that the second one is running and the first one has been stopped.
Going back to one of the questions, what happens when you close the cell, for example,
and why sometimes people will say that you should use this NOHUP command
before your run jobs in a remote session. This is because if we try to send a hung up command to the first job
it's gonna, in a similar fashion as the other signals, it's gonna hang it up and that's gonna terminate the job.
And the first job isn't there anymore whereas we have still the second job running.
However, if we try to send the signal to the second job what will happen if we close our terminal right now
is it's still running. Like NOHUP, what it's doing is kind of encapsulating
whatever command you're executing and ignoring wherever you get a hang up signal,
and just ignoring that so it can keep running.
And if we send the "kill" signal to the second job, that one can't be ignored and that will kill the job, no matter what.
And we don't have any jobs anymore. That kind of completes the section on job control.
Any questions so far? Anything that wasn't fully clear?
What does BG do? So BG... There are like two commands. Whenever you have a command that has been backgrounded
and is stopped you can use BG (short for background) to continue that process running on the background.
That's equivalent of just kind of sending it a continue signal, so it keeps running.
And then there's another one which is called FG, if you want to recover it to the foreground and you want to reattach your standard output.
Okay, good. Jobs are useful and in general, I think knowing about signals can be
really beneficial when dealing with some part of Unix but most of the time what you actually want to do is something along the lines of
having your editor in one side and then the program in another, and maybe
monitoring what the resource consumption is in our tab. We could achieve this using probably what you have seen a lot of the time,
which is just opening more windows. We can keep opening terminal windows. But the fact is there are kind of more convenient solutions to this and
this is what a terminal multiplexer does. A terminal multiplexer like tmux
will let you create different workspaces that you can work in, and quickly kind of,
this has a huge variety of functionality, It will let you rearrange the environment and it will let you have different sessions.
There's another more... older command, which is called "screen", that might be more readily available.
But I think the concept kind of extrapolates to both. We recommend tmux, that you go and learn it.
And in fact, we have exercises on it. I'm gonna showcase a different scenario right now. So whenever I talked...
Oh, let me make a quick note. There are kind of three core concepts in tmux, that I'm gonna go through and
the main idea is that there are what is called
"sessions". Sessions have "windows" and
windows have "panes". It's gonna be kind of useful to keep this hierarchy in mind.
You can pretty much equate "windows" to what "tabs" are in other editors and others,
like for example your web browser. I'm gonna go through the features, mainly what you can do at the different levels.
So first, when we do tmux, that starts a session. And here right now it seems like nothing changed
but what's happening right now is we're within a shell that is different from the one we started before.
So in our shell we started a process, that is tmux and that tmux started a different process, which is the shell we're currently in.
And the nice thing about this is that that tmux process is separate from the original shell process.
So here, we can do things. We can do "ls -la", for example, to tell us what is going on in here.
And then we can start running our program, and it will start running in there
and we can do "Ctrl+A d", for example, to detach
to detach from the session. And if we do "tmux a"
that's gonna reattach us to the session. So the process, we abandon the process counting numbers.
This really silly Python program that was just counting numbers, we left it running there.
And if we tmux... Hey, the process is still running there. And we could close this entire terminal and open a new one and
we could still reattach because this tmux session is still running.
Again, we can... Before I go any further.
Pretty much... Unlike Vim, where you have this notion of modes,
tmux will work in a more emacsy way, which is every command, pretty much every command in tmux,
you could enter it through the... it has a command line, that we could use. But I recommend you to get familiar with the key bindings.
It can be somehow non intuitive at first, but once you get used to them...
"Ctrl+C", yeah When you get familiar with them, you will be much faster just using the key bindings than using the commands.
One note about the key bindings: all the key bindings have a form that is like you type a prefix and then some key.
So for example, to detach we do "Ctrl+A" and then "D". This means you press "Ctrl+A" first, you release that, and then press "D" to detach.
On default tmux, the prefix is "Ctrl+B", but you will find that most people will have this remapped to "Ctrl+A"
because it's a much more ergonomic type on the keyboard. You can find more about how to do these things in one of the exercises,
where we link you to the basics and how to do some kind of quality of life modifications to tmux.
Going back to the concept of sessions, we can create a new session just doing something like tmux new
and we can give sessions names. So we can do like "tmux new -t foobar" and this is a completely different session, that we have started.
We can work here, we can detach from it. "tmux ls" will tell us that we have two different sessions:
the first one is named "0", because I didn't give it a name, and the second one is called "foobar".
I can attach the foobar session and I can end it.
And it's really nice because having this you can kind of work in completely different projects.
For example, having two different tmux sessions and different editor sessions, different processes running...
When you are within a session, we start with the concept of windows. Here we have a single window, but we can use "Ctrl+A c" (for "create")
to open a new window. And here nothing is executing.
What it's doing is, tmux has opened a new shell for us and we can start running another one of these programs here.
And to quickly jump between the tabs, we can do "Ctrl+A" and "previous",
"p" for "previous", and that will go up to the previous window.
"Ctrl+A" "next", to go to the next window. You can also use the numbers. So if we start opening a lot of these tabs,
we could use "Ctrl+A 1", to specifically jump to the to the window that is number "1".
And, lastly, it's also pretty useful to know sometimes that you can rename them.
For example here I'm executing this Python process, but that might not be really informative and I want...
I maybe want to have something like execution or something like that and that will rename the name of that window so you can have this really neatly organized.
This still doesn't solve the need when you want to have two things at the same time in your terminal,
like in the same display. This is what panes are for. Right now, here we have a window with a single pane
(all the windows that we have opened so far have a single pane). But if we do 'Ctrl+A "'
this will split the current display into two different panes.
So, you see, the one we open below is a different shell from the one we have above,
and we can run any process that we want here. We can keep splitting this, if we do "Ctrl+A %"
that will split vertically. And you can kind of rearrange these tabs using a lot of different commands.
One that I find very useful, when you are starting and it's kind of frustrating,
rearranging them. Before I explain that, to move through these panes, which is
something you want to be doing all the time You just do "Ctrl+A" and the arrow keys, and that will let you quickly
navigate through the different windows, and execute again...
I'm doing a lot of "ls -a" I can do "HTOP", that we'll explain in the debugging and profiling lecture.
And we can just navigate through them, again like to rearrange there's another slew of commands,
you will go through some in the Exercises "Ctrl+A" space is pretty neat, because it will kind of equispace the current ones
and let you through different layouts. Some of them are too small for my current
terminal config, but that covers, I think, most of it. Oh, there's also,
here, for example, this Vim execution that we have started,
is too small for what the current tmux pane is. So one of the things that really is much more convenient to do in tmux,
in contrast to having multiple terminal windows, is that you can zoom into this, you can ask by doing "Ctrl+A z", for "zoom".
It will expand the pane to take over all the space, and then "Ctrl+A z", again will go back to it.
Any questions for terminal multiplexers, or like, tmux concretely?
Is it running all the same thing? Like, is there any difference in execution between running it in different windows?
Is it really just doing it all the same, so that you can see it? Yeah, it wouldn't be any different from having two terminal windows open in your computer.
Like both of them are gonna be running. Of course, when it gets to the CPU, this is gonna be multiplexed again.
Like there's like a timesharing mechanism going there but there's no difference. tmux is just making this much more convenient to use by giving you this visual layout
that you can quickly manipulate through. And one of the main advantages will come when we reach the remote machines
because you can leave one of these, we can detach from one of these tmux systems,
close the connection and even if we close the connection and and the terminal is gonna send a hang-up signal,
that's not gonna close all the tmux's that have been started.
Any other questions?
Let me disable the key-caster.
So now we're gonna move into the topic of dotfiles and, in general, how to kind of configure your shell to do the things you want to do
and mainly how to do them quicker and in a more convenient way. I'm gonna motivate this using aliases first.
So what an alias is, is that by now, you might be starting to do something like
a lot of the time, I just want to LS a directory and I want to display all the contents into a list format
and in a human readable thing. And it's fine. Like it's not that long of a command.
But as you start building longer and longer commands, it can become kind of bothersome having to retype them again and again.
This is one of the reasons why aliases are useful. Alias is a command that will be a built-in in your shell,
and what it will do is it will remap a short sequence of characters to a longer sequence.
So if I do, for example, here alias ll="ls -lah"
If I execute this command, this is gonna call the "alias" command with this argument
and the LS is going to update the environment in my shell to be aware of this mapping.
So if I now do LL, it's executing that command without me having to type the entire command.
It can be really handy for many, many reasons. One thing to note before I go any further is that
here, alias is not anything special compared to other commands, it's just taking a single argument.
And there is no space around this equals and that's because alias takes a single argument
and if you try doing something like this, that's giving it more than one argument
and that's not gonna work because that's not the format it expects. So other use cases that work for aliases,
as I was saying, for some things it may be much more convenient,
like one of my favorites is git status. It's extremely long, and I don't like typing that long of a command every so often,
because you end up taking a lot of time. So GS will replace for doing the git status
You can also use them to alias things that you mistype often, so you can do "sl=ls",
that will work. Other useful mappings are,
you might want to alias a command to itself
but with a default flag. So here what is going on is I'm creating an alias
which is an alias for the move command, which is MV and I'm aliasing it to the same command but adding the "-i" flag.
And this "-i" flag, if you go through the man page and look at it, it stands for "interactive". And what it will do is it will prompt me before I do an overwrite.
So once I have executed this, I can do something like I want to move "aliases" into "case".
By default "move" won't ask, and if "case" already exists, it will be over.
That's fine, I'm going to overwrite whatever that's there. But here it's now expanded,
"move" has been expanded into this "move -i" and it's using that to ask me "Oh, are you sure you want to overwrite this?"
And I can say no, I don't want to lose that file. Lastly, you can use "alias move"
to ask for what this alias stands for. So it will tell you so you can quickly make sure
what the command that you are executing actually is. One inconvenient part about, for example, having aliases is how will you go about
persisting them into your current environment? Like, if I were to close this terminal now,
all these aliases will go away. And you don't want to be kind of retyping these commands and more generally, if you start configuring your shell more and more,
you want some way of bootstrapping all this configuration. You will find that most shell command programs
will use some sort of text based configuration file. And this is what we usually call "dotfiles", because they start with a dot for historical reasons.
So for bash in our case, which is a shell,
we can look at the bashrc. For demonstration purposes, here I have been using ZSH,
which is a different shell, and I'm going to be configuring bash, and starting bash. So if I create an entry here and I say
SL maps to LS And I have modified that, and now I start bash.
Bash is kind of completely unconfigured, but now if I do SL... Hm, that's unexpected.
Oh, good. Good getting that. So it matters where you config file is,
your config file needs to be in your home folder. So your configuration file for bash will live in that "~",
which will expand to your home directory, and then bashrc.
And here we can create the alias
and now we start a bash session and we do SL. Now it has been loaded, and this is loaded at the beginning when this
bash program is started. All this configuration is loaded and you can, not only use aliases, they can have a lot of parts of configuration.
So for example here, I have a prompt which is fairly useless. It has just given me the name of the shell, which is bash,
and the version, which is 5.0. I don't want this to be displayed and
as with many things in your shell, this is just an environment variable. So the "PS1" is just the prompt string
for your prompt and we can actually modify this to just be a "> " symbol.
and now that has been modified, and we have that. But if we exit and call bash again,
that was lost. However, if we add this entry and say, oh we want "PS1"
to be this and we call bash again, this has been persisted. And we can keep modifying this configuration.
So maybe we want to include where the
working directory that we are is in, and that's telling us the same information that we had in the other shell.
And there are many, many options, shells are highly, highly configurable, and
it's not only cells that are configured through these files, there are many other programs. As we saw for example in the editors lecture, Vim is also
configured this way. We gave you this vimrc file and told you to put it under your
home/.vimrc and this is the same concept, but just for Vim. It's just giving it a set of
instructions that it should load when it's started, so you can keep a configuration that you want.
And even non... kind of a lot of programs will support this. For instance, my terminal emulator, which is another concept,
which is the program that is running the shell, in a way, and displaying this into the screen in my computer.
It can also be configured this way, so if I modify this I can
change the size of the font. Like right now, for example, I have increased the font size a lot
for demonstration purposes, but if I change this entry and make it for example
28 and write this value, you see that the size of the font has changed,
because I edited this text file that specifies how my terminal emulator should work.
Any questions so far? With dotfiles.
Okay, it can be a bit daunting knowing that there is like this endless wall of configurations,
and how do you go about learning about what can be configured?
The good news is that we have linked you to really good resources in the lecture notes.
But the main idea is that a lot of people really like just configuring these tools and have uploaded
their configuration files to GitHub, another different kind of repositories online. So for example, here we are on GitHub,
we search for dotfiles, and can see that there are like thousands of repositories of people sharing their configuration files. We have also...
Like, the class instructors have linked our dotfiles. So if you really want to know how any part of our setup is working
you can go through it and try to figure it out. You can also feel free to ask us. If we go for example to this repository here
we can see that there's many, many files that you can configure. For example, there is one for bash, the first couple of ones are for git, that will be probably be covered in the
version control lecture tomorrow. If we go for example to the bash profile, which is a different form of what we saw in the bashrc,
it can be really useful because you can learn through just looking at the manual page, but the manual pages is, a lot of the time
just kind of like a descriptive explanation of all the different options
and sometimes it's more helpful going through examples of what people have done and trying to understand why they did it
and how it's helping their workflow. We can say here that this person has done case-insensitive globbing.
We covered globbing as this kind of filename expansion trick in the shell scripting and tools.
And here you say no, I don't want this to matter, whether using uppercase and lowercase, and just setting this option in the shell for these things to work this way
Similarly, there is for example aliases. Here you can see a lot of aliases that this person is doing. For example, "d" for
"d" for "Dropbox", sorry, because that's just much shorter. "g" for "git"...
Say we go, for example, with vimrc. It can be actually very, very informative, going through this and trying to extract useful information.
We do not recommend just kind of getting one huge blob of this and copying this into your config files,
because maybe things are prettier, but you might not really understand what is going on.
Lastly one thing I want to mention about dotfiles is that
people not only try to push these files into GitHub just so other people can read it, that's
one reason. They also make really sure they can reproduce their setup. And to do that they use a slew of different tools.
Oops, went a little too far. So GNU Stow is, for example, one of them
and the trick that they are doing is they are kind of putting all their dotfiles in a folder and they are
faking to the system, using a tool called symlinks, that they are actually what they're not. I'm gonna
draw really quick what I mean by that. So a common folder structure might look like you have your home folder and
in this home folder you might have your bashrc, that contains your bash configuration, you might have your vimrc and
it would be really great if you could keep this under version control. But the thing is, you might not want to have a git repository,
which will be covered tomorrow, in your home folder. So what people usually do is they create a dotfiles repository,
and then they have entries here for their bashrc and their vimrc. And this is where actually
the files are and what they are doing is they're just
telling the OS to forward, whenever anyone wants to read this file or write to this file, just forward this to this other file.
This is a concept called symlinks and it's useful in this scenario,
but it in general it's a really useful tool in UNIX that we haven't covered so far in the lectures
but you might be... that you should be familiar with. And in general, the syntax will be "ln -s"
for specifying a symbolic link and then you will put the path to the file
that you want to create and then the symlink that you want to create.
And All these all these kind of fancy tools that we're seeing here listed,
they all amount to doing some sort of this trick, so that you can have all your dotfiles neat and tidy
into a folder, and then they can be version-controlled, and they can be
symlinked so the rest of the programs can find them in their default locations.
Any questions regarding dotfiles?
Do you need to have the dotfiles in your home folder, and then also dotfiles in the version control folder?
So what you will have is, pretty much every program, for example bash,
will always look for "home/.bashrc". That's where the program is going to look for.
What you do when you do a symlink is, you place your "home/.bashrc"
it's just a file that is kind of a special file in UNIX, that says oh, whenever you want to read this file go to this other file.
There's no content, like there is no... your aliases are not part of this dotfile. That file is just kind of like a pointer, saying now you should
go that other way. And by doing that you can have your other file in that other folder.
If version controlling is not useful, think about what if you want to have them in your Dropbox folder, so they're synced to the cloud,
for example. That's kind of another use case where like symlinks could be really useful
So you don't need the folder dotfiles to be in the home directory, right? Because you can just use the symlink, that points somewhere else.
As long as you have a way for the default path to resolve wherever you have it, yeah.
Last thing I want to cover in the lecture... Oh, sorry, any other questions about dotfiles?
Last thing I want to cover in the lecture is working with remote machines, which is a thing that you will run into,
sooner or later. And there are a few things that will make your life much easier when dealing with remote machines
if you know about them. Right now maybe because you are using the Athena cluster,
but later on, during your programming career, it's pretty sure that there is a fairly ubiquitous concept of having your
local working environment and then having some production server that is actually running the
code, so it is really good to get familiar about how to work in/with remote machines.
So the main command for working with remote machines is SSH.
SSH is just like a secure shell, it's just gonna take the responsibility for
reaching wherever we want or tell it to go and trying to open a session there.
So here the syntax is: "JJGO" is the user that I want to use in the remote machine,
and this is because the user is different from the one I have my local machine, which will be the case a lot of the time,
then the "@" is telling the terminal that this separates what the user is from what the address is.
And here I'm using an IP address because what I'm actually doing is I have a virtual machine in my computer,
that is the one that is remote right now. And I'm gonna be SSH'ing into it. This is the
URL that I'm using, sorry, the IP that I'm using, but you might also see things like
oh I want to SSH as "JJGO" at "foobar.mit.edu"
That's probably something more common, if you are using some remote server that has a DNS name.
So going back to a regular command,
we try to SSH, it asks us for a password, really common thing. And now we're there. We have...
we're still in our same terminal emulator but right now SSH is kind of forwarding the entire virtual display to display what the
remote shell is displaying. And we can execute commands here and
we'll see the remote files A couple of handy things to know about SSH, that were briefly covered in the
data wrangling lecture, is that SSH is not only good for just
opening connections. It will also let you just execute commands remotely.
So for example, if I do that, it's gonna ask me what is my password?, again.
And it's executing this command then coming back to my terminal and piping the output of what that command was, in the remote machine,
through the standard output in my current cell. And I could have this in...
I could have this in a pipe, and this will work and we'll just
drop all this output and then have a local pipe where I can keep working.
So far, it has been kind of inconvenient, having to type our password. There's one really good trick for this.
It's we can use something called "SSH keys". SSH keys just use public key encryption
to create a pair of SSH keys, a public key and a private key, and then you can give the server the public part of the key.
So you copy the public key and then whenever you try to authenticate instead of using your password, it's gonna use the private key to
prove to the server that you are actually who you say you are.
We can quickly showcase how you will go about doing this.
Right now I don't have any SSH keys, so I'm gonna create a couple of them. First thing, it's just gonna ask me where I want this key to live.
Unsurprisingly, it's doing this. This is my home folder and then it's using this ".ssh" path,
which refers back to the same concept that we covered earlier about having dotfiles. Like ".ssh" is a folder that contains a lot of the
configuration files for how you want SSH to behave. So it will ask us a passphrase.
The passphrase is to encrypt the private part of the key because if someone gets your private key, if you don't have a password protected
private key, if they get that key they can use that key to impersonate you in any server. Whereas if you add a passphrase,
they will have to know what the passphrase is to actually use the key.
It has created a keeper. We can check that these two files are now under ssh.
And we can see...
We have these two files: we have the 25519 and the public key.
And if we "cat" through the output, that key is actually not like any fancy binary file, it's
just a text file that has the contents of the public key and some
alias name for it, so we can know what this public key is. The way we can tell the server that we're authorized to SSH there
is by just actually copying this file, like copying this string into a file,
that is ".ssh/authorized_keys". So here what I'm doing is I'm
catting the output of this file which is just this line of text that we want to copy
and I'm piping that into SSH and then remotely I'm asking "tee" to dump the contents of the standard input
into ".ssh/authorized_keys". And if we do that, obviously it's gonna ask us for a password.
It was copied, and now we can check that if we try to SSH again,
It's going to first ask us for a passphrase but you can arrange that so that it's saved in the session
and we didn't actually have to type the key for the server.
And I can kind of show that again.
More things that are useful. Oh, we can do... If that command seemed a little bit janky,
you can actually use this command that is built for this, so you don't have to kind of craft this "ssh t" command.
That is just called "ssh-copy-id". And we can do the same
and it's gonna copy the key. And now, if we try to SSH,
we can SSH without actually typing any key at all, or any password.
More things. We will probably want to copy files. You cannot use "CP" but you can use "SCP", for "SSH copy".
And here we can specify that we want to copy this local file called notes and the syntax is kind of similar.
We want to copy to this remote and then we have a semicolon to separate what the path is going to be.
And then we have oh, we want to copy this as notes but we could also copy this as foobar.
And if we do that, it has been executed and it's telling us that all the contents have been copied there.
If you're gonna be copying a lot of files, there is a better command that you should be using
that is called "RSYNC". For example, here just by specifying these three flags,
I'm telling RSYNC to kind of preserve all the permissions whenever possible
to try to check if the file has already been copied. For example, SCP will try to copy files that are already there.
This will happen for example if you are trying to copy and the connection interrupts in the middle of it. SCP will start from the very beginning, trying to copy every file,
whereas RSYNC will continue from where it stopped.
And here, we ask it to copy the entire folder and
it's just really quickly copied the entire folder. One of the other things to know about SSH is that
the equivalent of the dot file for SSH is the "SSH config".
So if we edit the SSH config to be
If I edit the SSH config to look something like this, instead of having to, every time, type "ssh jjgo",
having this really long string so I can like refer to this specific remote, I want to refer, with the specific user name,
I can have something here that says this is the username, this is the host name, that this
host is referring to and you should use this identity file. And if I copy this,
this is right now in my local folder, I can copy this into ssh.
Now, instead of having to do this really long command, I can just say I just want to SSH into the host called VM.
And by doing that, it's grabbing all that configuration from the SSH config and applying it here.
This solution is much better than something like creating an alias for SSH,
because other programs like SCP and RSYNC also know about the dotfiles for SSH and will use them whenever they are there.
Last thing I want to cover about remote machines is that here, for example, we'll have tmux and we can,
like I was saying before, we can start editing some file
and we can start running some job.
For example, something like HTOP. And this is running here, we can
detach from it, close the connection and then SSH back. And then, if you do "tmux a",
everything is as you left it, like nothing has really changed. And if you have things executing there in the background, they will keep executing.
I think that, pretty much, ends all I have to say for this tool.
Any questions related to remote machines?
That's a really good question. So what I do for that,
Oh, yes, sorry. So the question is, how do you deal with trying to use tmux in your local machine,
and also trying to use tmux in the remote machine? There are a couple of tricks for dealing with that.
The first one is changing the prefix. So what I do, for example, is in my local machine the prefix I have changed from "Ctrl+B" to "Ctrl+A" and
then in remove machines this is still "Ctrl+B". So I can kind of swap between,
if I want to do things to the local tmux I will do "Ctrl+A" and if I want to do things to the remote tmux I would do "Ctrl+B".
Another thing is that you can have separate configs, so I can do something like this, and then...
Ah, because I don't have my own ssh config, yeah. But if you...
Um, I can SSH "VM". Here, what you see,
the difference between these two bars, for example, is because the tmux config is different.
As you will see in the exercises, the tmux configuration is in
the tmux.conf
And in tmux.conf, here you can do a lot of things like changing the color depending on the host you are
so you can get like quick visual feedback about where you are, or if you have a nested session. Also, tmux will,
if you're in the same host and you try to tmux within a tmux session, it will kind of prevent you from doing it so you don't run into issues.
Any other questions related, to kind of all the topics we have covered.
Another answer to that question is also, if you type the prefix twice, it sends it once to the underlying shell.
So the local binding is "Ctrl+A" and the remote binding is "Ctrl+A", You could type "Ctrl+A", "Ctrl+A" and then "D", for example, detaches from the remote, basically.
I think that ends the class for today, there's a bunch of exercises related to all these main topics and
we're gonna be holding office hours today, too. So feel free to come and ask us any questions.