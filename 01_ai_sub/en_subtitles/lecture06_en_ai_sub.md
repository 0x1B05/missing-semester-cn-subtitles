Alright, let's get started with today's lecture.
So actually, before we get started, one quick note about office hours. 
It seemed from the poll that some people were under the impression that the office hours that follows each lecture is just about that day's lectures topics, 
and this is not the case. 
You can come to office hours and ask us questions about any lecture, 
whether it's the previous day or from the previous week, or even things not exactly covered in this class that you're just curious about. 
So yeah, come to office hours with questions about anything. 
Office hours are in the 32 g9 lounge, so building 32, also known as the Stata Center, has two towers, the G tower and the D tower. 
So we're in the Gates tower on the ninth floor. 
So if you take the elevator all the way up, there's the lounge right in front of you. 
Okay, cool. 
So today, we're going to be talking about version control systems. 
So I just want to get a sense of whether you guys have used version control systems before. 
So could you raise your hand if you have any experience with git or any other version control system, like subversion or mercurial or anything else? 
Oh great, so that's a good number of you. 
So I won't talk about version control systems in general way too much, then we'll pretty quickly get into the details of git and like its data model and its internals. 
But just as a quick summary, version control systems are tools that are used to keep track of changes to source code or other collections of files or folders. 
And as the name implies, these tools help track the history of changes to some set of documents. 
And in addition to doing that, they facilitate collaboration, so they're really useful for working with a group of people on a software project. 
Version control systems track changes to a folder and its contents in a series of snapshots. 
So you capture the entire state of a folder and everything inside, like a software project, 
and you have multiple of these in a series of snapshots. 
Each snapshot encapsulates the entire set of files and folders contained within some top-level directory. 
And then version control systems also maintain a bunch of metadata along with the actual changes to the content, 
and this is to make it possible to figure things out, like who authored a particular change to a particular file, or when was a particular change made. 
And so version control systems maintain metadata like authors and commit timestamps, 
and you can also attach extra messages to these snapshots and things like that. 
And so why is version control useful? Well, it's useful even when you're working on projects by yourself. 
So you can use it to look at old versions of code you've written, figure out why something was changed by looking at commit messages, 
work on different things in parallel without conflicts by using different branches of development, 
or be able to work on bug fixes while keeping work on different features independent, things like that. 
And so it's an invaluable tool even if you're working just by yourself, even on a small scale project. 
Like I think the instructors of this course use git even on things like homework assignments or class projects, even small scale things. 
In addition to our research or larger software projects. 
And then of course, version control is a really powerful tool for working with other people. 
So it's useful for sending patches of code around, resolving conflicts when different people are working on the same piece of code at the same time, things like that. 
And so it's a really powerful tool for working by yourself or with others.
And it also has a neat functionality to let you answer questions that would otherwise be kind of hard to answer, 
like who wrote a particular module in a software project or who edited a particular line in a particular software project, 
why was this particular line change, when was it changed, by whom, things like that. 
And version control systems also have some really powerful functionality that we might cover at the end of today's lecture, 
or you can find the lecture notes if we don't have time to do. 
Things like, suppose you have some project you've been working on for a couple years, 
and then you notice that some funny thing about the project was broken, like you have some unit test that doesn't pass anymore, 
and it wasn't broken just now, it was broken some time ago, 
and you don't know exactly when this regression was introduced. 
Well-written control systems have a way of automatically identifying this, 
like you can take it and give it a unit test that's currently failing but you know was passing at some point in the past, 
and it can binary search your history and figure out exactly what change to your code made it break. 
So lots of really powerful, fancy features if you know how to use these tools properly.
There are a number of version control systems out there, 
and Git has become kind of the de facto standard for version control, so that's what we're going to be covering in today's lecture. 
One comic I want to show you, which was on the screen before hand, let me bring it back up. 
So this is an xkcd comic that illustrates Git's reputation. 
Let me read it out loud for you. 
'Git tries collaborative work on projects through a beautiful distributed graph theory tree model. 
Cool. 
How do we use it? No idea. 
Just memorize these shell commands and type them to sync up. 
If you get errors, save your work elsewhere, delete the project and download a fresh copy.' 
So maybe some people may not want to raise their hands for this, but raise your hand if you've ever done this before. 
I certainly have when I was learning this tool, so a good number of you here have done this before.
So, the goal of this lecture is to make it so you don't have to do this anymore. 
Unfortunately, as this comic illustrates, Git's interface is a pretty terribly designed interface. 
It's a leaky abstraction, 
and so for this reason, we believe that learning Git top-down, starting with the interface, is maybe not the best way to go, 
and it can lead to some confusion. 
It's possible, like this comic shows, to memorize a handful of commands and think of them as magic incantations, 
and why everything's working all right. 
It kind of works out all right, but then you have to follow the approach of this comic whenever things go wrong.
While Git has an ugly interface, its underlying design and ideas are actually pretty beautiful. 
An ugly interface has to be memorized, but the beautiful ideas underlying Git can actually be understood. 
And once you understand Git's internals, its data model, which is actually not that complicated, then you can learn the interface to Git. 
You'll have to memorize some things, but you can understand what exactly certain commands do by understanding how they manipulate the underlying data model. 
And so the way we're going to teach Git today is first talk about the data model, almost in abstract, 
talk about how we might model files and folders, snapshots of history, 
and how they relate to each other. 
Then after that, we'll walk you through some Git commands, 
and then finally, in the resources and exercises, we'll link you to tutorials that'll teach you all the specifics, 
Because there are lots of different commands that you will need to learn eventually. 
Any questions so far about our teaching approach for today? Cool, great. 
So let's get started. 
There are probably many ad hoc approaches you could take to version control, 
and I'm guessing some of you may have done this before. 
Like say you have some file or folder, we have a bunch of different files corresponding to a system software project, 
and you want to track changes. 
You could just say every day, make a copy of that entire folder and give that folder a timestamp. 
When you want to do things like collaborate with other people, you could take the entire folder, turn it into a zip archive, 
and email it to somebody. 
And then, whenever you and your buddy are working on two different features of a software project, you can work on them in parallel. 
Then, one of you emails the zip file to the other person, 
and then you manually copy and paste the appropriate segments from their code into your code 
so that eventually you end up with one piece of code that has both of your features in it. 
This kind of sort of works. 
Raise your hand if you've done this before. 
I certainly have. 
Still, a decent number of you get, let's us not do this sort of thing. 
It is a well-thought-out model that kind of facilitates these sorts of interactions, 
things that you might want to do like tracking your own history on your project or collaboration or things like that. 
So Git has a well-thought-out model that enables things like branches and collaboration and merging changes from other people, all sorts of neat stuff. 
Git models history as a collection of files and folders within some top-level directory. 
So you're probably familiar with this abstraction just from files and folders on your own computer. 
And so here's one example: you might have some top-level directory, I'll just call this like root in parentheses, 
and this directory might have, say, a folder in it called foo, 
and this folder inside of it might have a file called bar.txt, 
and this might have some contents in it like say, "Hello world." And then maybe this top-level directory, it has one folder in it, it can also have another file in it. 
So say there's some other file, 
and this file also has some contents in it. 
Alright, simple enough. 
The terminology Git uses for these different things, for files and folders, is this: and the top-level thing are called trees. 
So this is a folder, 
and then these things what we normally call files are called blobs. 
Alright, okay. 
So now we have a model of files and folders, 
and this is a recursive data structure. 
Trees can contain other trees, 
and then trees can contain both trees and files. 
Obviously, files can't contain trees. 
Alright, so now we have a model of files and folders, 
and the kind of top-level of this thing, the thing I've just labeled "root," is the directory being tracked. 
Like you might have some folder on your computer corresponding to a software project. 
Now, how do you model history? Once you have a model of files and folders, well, you can imagine one way of doing it, which is you take a snapshot of this entire thing, 
and then history is just a linear sequence of snapshots. 
Like you might imagine that it's, you can almost think of it like you have copies of the folder which are dated and time-stamped. 
Well, it doesn't use a simple linear model like that. 
It uses something a little bit fancier. 
You might have heard this terminology before, but Git uses a directed acyclic graph to model history. 
And this might sound like a bunch of fancy math words, but it's actually not all that complicated. 
So in Git, each snapshot has some number of parents, 
and basically, we want to know what change preceded what other change. 
So suppose here, I'm going to use circles to refer to individual snapshots. 
This is the entire contents within this tree, so all the files and folders in my project. 
My entire project may be in some state, 
and then I edit some files, 
and now it's in some other state. 
And then I add some more files, 
and that's in some other state. 
And every state points back to which state preceded it. 
This so far is a linear history, but it lets us do something a little bit fancier than this. 
You can also from a certain snapshot fork your history and say, 'I want to base changes off of this version and create a new snapshot like this.' 
So this way of modeling history allows you to do things like, 'Okay, I'm working on my project. 
This is my main line of development. 
I go up to here, 
and now I have two different tasks I want to work on.' Suppose on one hand, I have some fancy new feature I want to add to my project, 
and so I'm going to be working on that for a couple of days. 
But separately from that, somebody's reported a bug to me, 
and I need to go chase down that bug and fix it. 
Well, instead of working on all that stuff kind of concurrently at the same time in the same line of development, 
Git has its way of branching the history into two separate forks and working on different things in parallel temporarily in a way that are unrelated to each other. 
So I could take this base snapshot like my project is in some state where it works, 
and then from here, I could implement a new feature that creates a new snapshot. 
So this has the base project plus a new feature. 
Similarly, separately from this, I could go back to this original snapshot because I don't want to do bug fixing while implementing my new feature, go here, 
and then work on my bug fix and create a different snapshot. 
So this has only the bug fix but not the feature. 
And then finally, once I've done these two separate things in parallel, eventually, 
I want to incorporate them all into my common source code that has both the feature and the bug fix. 
So eventually, I might author a new snapshot by merging the changes present in these two different snapshots. 
And so this one, I'll have both of these snapshots as parents, 
and this version here will have both the feature and my bug fix. 
So does it make sense why Git models history in a way that's a little bit fancier than just a sequence of snapshots of my files and folders? 
Why I want to be able to support branching to work on things in parallel and then also merging to combine changes from different parallel branches of development?
Question: Yeah, so that's an excellent point. 
It seems that when you merge things, you could create errors that weren't anticipated. 
You could imagine here that this feature actually changes something that makes this bug-fix redundant, 
or you could imagine this bug fix breaking this feature or something like that.
Answer: Oh, that's a really good point. 
That's something known as merge conflicts, 
and this is something that Git will try to do when you merge your parallel branches of development. 
It will try to automatically combine the changes in a way such that it retains all the important changes. 
But if it gets confused
But if it gets confused, it will report a merge conflict and then leave it up to you, the programmer, 
to figure out how to combine kind of concurrent changes to the same files or things like that. 
And then Git has some tools for facilitating this. 
Any other questions? Great. 
Ok, so now we have a model of files and folders, 
and then we have a model of history, how different snapshots of our code relate to each other. 
One little detail here is that each of these circles, so they kind of correspond to a snapshot like a tree with files and folders, but they also have a little bit of metadata. 
So like inside here we might have like the author of this commit is me, 
and we might have other metadata like some message associated with this commit. 
I might describe what kinds of changes I've made that are present in this snapshot but not the previous one. 
That is not really the chair class, so next we're going to talk about kind of one level lower than this, like how exactly is this represented as a data structure inside Git. 
And so I'm actually going to write down pseudocode because I think it's actually easiest to understand this way. 
So first we have files. 
So a blob is just a bunch of bytes, so I'll say this is an array of bytes. 
Okay, then what is a tree? Remember that this is just a folder, so what are folders? 
They're mappings from the filename or directory name to the actual contents, 
and the contents are either another tree, like a subtree, or the file. 
And then finally we have the last thing there, what I've been calling snapshots so far, 
and in Git terminology those are called commits. 
And so what does a commit do? It's a bunch of stuff. 
Commits have parents that describe what preceded them, so in the case of most normal commits, they have one parent like what they came from. 
What merge commits can have multiple parents, so parents are an array of commits, 
and then I have some metadata like the author and maybe a message, 
and then finally the actual contents, the snapshot, which is a tree that's the top-level tree corresponding to a particular commitment. 
So this is a really clean simple model of history, 
and this is basically all there is to how Git models history. 
Any questions about that? All right, so now we have that going a little bit deeper. 
Let's talk about how it actually stores and addresses this actual data. 
Like at some point, this actually has to turn to data on disk, right? So Git defines an object, 
kind of a big standing term, but an object is any one of those three things, so it's a blob, a tree, or a commit. 
And then in Git, all objects are content addressed, 
So what Get maintains on disk, 
and you can actually look at this later, is a set of objects maintained as this content address store. 
So if you have any one of these objects, the way you put it into this store is its key is the hash of the object. 
So like in pseudocode, I might say that to store a particular object o, what I do is I compute its ID by taking the SHA-1 hash of o, 
and then I put it into my objects map, store it to disk. 
A quick show of hands, who here knows what a hash function is? Alright, so I'll quickly summarize. 
Basically, a hash function is, you can think of it as like this magical function that takes a big piece of data and turns it into a short string. 
At a high level, these are used to, or maybe that's like a sufficient clinician, I won't go into too much more detail here, but you can ask me afterwards if you're curious. 
So basically, they give you a way to name a thing in a way that's kind of deterministic based on the contents of the thing it takes into thing as input and gives you a short name for it. 
And then the opposite of stores, load, the way we can load things from the store, you might have just guessed, you can look them up by their ID. 
And this is just, we retrieve it from the object store by ID and it gives us back the contents. 
Any questions about this so far? Question, that's a good question. 
What language is it all written in? It's written in the language I just made up. 
So it's pseudocode. 
The Get implementation itself is a mix of C, it's mostly C, 
and then some Bash and Perl scripts, I think. 
Any other questions? Is this made-up language clear enough or do I need to explain any aspects of it? Great, okay. 
Blobs, trees, 
and commits in Git are unified in this way. 
They're all objects. 
And also, as you might think, given my description here, it looks like commits contain a whole bunch of other commits and contain a snapshot and things like that. 
In practice, it doesn't actually work that way. 
Instead, all these are pointers. 
So a commit will be able to reference a bunch of parents by their IDs. 
So this is actually not an array of commits themselves, but IDs. 
And similarly, the snapshot inside a commit is not the actual tree object. 
It's the ID of the tree. 
And so all these objects are kind of stored on their own in this object store. 
And then all the references to different objects are just by their ID, by their SHA-1 hash. 
Does that make sense? You can almost in your head map it to like these are objects in a programming language like Java, 
and then this is a reference to a tree. 
So it's like a pointer, 
and then that is your realm. 
Maybe this not he helps, maybe it doesn't. 
Yeah, yeah, exactly. 
So I'll just repeat that for everybody to hear on the microphone. 
This is Git's on-disk data store. 
It's a content address store where objects are addressed by their hash. 
Any questions about that so far? Ok, so now we have a way of identifying. 
We've unified all the different types of objects into one type of thing we call object, 
and we have a way of identifying objects by their sha-1 hash. 
What do these actual sha-1 hashes look like? Well, they're hexadecimal strings that are 40 characters long. 
Like sha-1 is a 160-bit hash, 
and so one of the actual IDs returned by that sha-1 function is going to be a really long string. 
And so given that, we'll have ways of identifying these different things. 
Like this, we'll have corresponding to it an ID, like for a 3-2 CEB or something, something. 
So now we have a way of naming everything in this commit graph, but these names are really inconvenient because they're super long and they're like text strings. 
They're not meaningful to humans in any way. 
So its solution to this problem is one other thing. 
So Git maintains a set of objects, 
and then it maintains a set of references. 
What are references? Here, I'll erase this bit on the left. 
This part's pretty logical, that's the irony, another time. 
So references are all right here. 
So this is another piece of data that Git maintains internally. 
References are a map from string to string, 
and you can think of this as mapping human-readable names. 
Like I might have a name like 'fix encoding bug'-- 'fix-encoding-bug' is a human-readable name, 
and this would be mapped to that long hexadecimal string there. 
And so with these references, you can imagine how we might have ways of creating new references and updating references and things like that. 
With this, I can now refer to things in my commit graph by name, so I might have the same be called like 'fix bug' 
or I might have a name for something over here, things like that. 
And so, yeah, with this, Git can use human-readable names to refer to particular snapshots in the history, instead of these long hexadecimal strings. 
One other thing to be aware of here is that given Git's design for history, this entire graph is actually immutable. 
You can add new stuff to it, but you can't actually manipulate anything in here. 
I won't go into the details of exactly how or why, but just assume that that's the case. 
However, references are immutable. 
So as you're updating the history, like suppose you keep working on this piece of software, you create a new commit, so I'm representing that by the circle. 
This points to the previous commit. 
I can actually have, say, my 'fixed bug' reference is pointing here. 
I can update this reference to now point over here. 
However, I can't, for example, make this point over here. 
That's not even a meaningful thing to say because this is just the hash of this object. 
To change this hash, I'd need to change the contents of the object, which doesn't really make sense. 
All right, any questions about that so far? That's basically it for Git's data model, 
And then we'll go into actually interacting with Git via the command line, 
and we'll see how Git commands correspond with manipulations of a graph data structure. 
So, any questions about modeling history as trees of trees and blobs, 
and then snapshots, these things called commits being chained together, 
and you have references that can point to particular nodes in this graph. 
Cool, no questions? So basically, once we have objects and references, that's basically all there is to a Git repository. 
Those are the two pieces of data that it stores, 
and at a high level, all Git command line commands are just manipulations of either the references data or the objects data. 
Okay, so for the rest of this lecture, I'm going to go through some Git commands. 
It's basically going to be an interactive demo, similar to the Vim lecture, 
and then you can refer to the notes for full information on these commands. 
Look, of course, it's a really powerful tool, we can't cover everything in what 20 minutes. 
All right, so I'm going to go over to this folder called playground, 
and I'm going to make a new directory called demo. 
CD into demo, 
and this directory is going to represent the top level of my project. 
It's currently empty because I just created it. 
If I want to turn this into a Git repository, I use the 'git init' command. 
'Git init' stands for Git initialize, 
and we see that it says 'initialized empty Git repository in blah blah slash dot Git'. 
If I do 'ls', I still see nothing, but if I do 'ls -a', there's a hidden file in this directory called '.git'. 
If I do 'ls .git', there's a bunch of stuff in here. 
This is the directory on disk where Git stores all of its internal data, namely the objects and the references, 
and you actually see here objects and refs as two directories in here, 
and all the repository data will be stored underneath those two directories. 
One-letter command to keep in mind as we're going through these is something called 'git help'. 
'Git help' takes a sub-command as an argument, it gives you some help on it. 
So, if I do 'git help init', for example, it'll tell me about the 'git init' command.
Now there are some commands for figuring out what's going on with a Git repository, like 'git status'. 
At a high level, it says what is going on right now, 
and we see here (let's ignore the first line for now), the second line says 'no commits yet'. 
That's because we just initialized a fresh repository, 
and so there is no history yet. 
I'm actually going to... 
does anybody still want this... 
are kind of clear this part of the board? I'm going to, as we go along, draw how the underlying objects and references data is changing when I type in certain Git commands. 
So right now, this picture or lack of picture represents the current state of our repository. 
It's empty. 
There are no snapshots. 
So let's fix that. 
Let's add something to our history. 
Here we have no files, so let me just go ahead and create a file 'hello.txt' with the content 'Hello, world!'. 
Normally you'd have your source code with actually useful stuff in it. 
Now what I want to do is I want to take the current contents of this directory and turn it into a new snapshot to represent say the first state my project was in. 
You might imagine an interface for doing this where there is like a git snapshot command 
or get something else command which takes a snapshot of the entire state of the current directory. 
For a number of reasons, Git doesn't have a command that works exactly like that because Git wants to give you a little bit of flexibility as to what changes to include in the next snapshot you take. 
This is something that's kind of confusing to beginners sometimes, so I'll try to explain it right now. 
Git has a concept of something called a staging area, 
and at a high level, it's where you tell Git what changes should be included in the next snapshot you take. 
If we do git status here, we'll see that Git says "no commits yet" like it said before, 
and it says "untracked files hello.txt". 
So, this is saying that Git notices that there's a new file in the current directory, but it is not going to be included in the next snapshot. 
Git's kind of ignoring it for now. 
But if I do git add hello.txt and if I do git status again, it says "now changes to be committed: new file hello.txt", 
and so now if I do the git snapshot command which is actually git commit, which creates a new one of those circles I drew on the board over there, this file will be included in that snapshot I'm about to take. 
So let me go ahead and run git commit. 
What this does is it pops up my text editor and it lets me type in a message that will be associated with this commit. 
And it's really good to write high-quality commit messages because then later when you're looking back at your project's version history, you'll know why you made certain changes. 
I'm going to add this relatively useless commit message, but we have a link in the lecture notes for a guide on how to write high-quality commit messages. 
So now that I've done that, Git prints out some output. 
Master, ignore that bit for now. 
This thing is the hash of the commit I just created. 
So now I have in my history a single node. 
This has in it a tree that has a single blob, a single file hello.txt with the contents "hello world". 
And then this has the SHA-1 hash for 2fb something something something (it's actually truncated in the Git interface as well). 
This is just printing out my commit message again and it says, as a reminder, I just added hello.txt. 
And so now if I use the git log command, which is really useful in that it helps you visualize the history, the commit graph, if I do... 
That's a great question. 
So, the question is, what exactly does this hash correspond to? So, this is the hash of the commit. 
The commit contains inside of it the hash of the tree, along with whatever other information. 
So I can actually use git cat-file -p this number. 
This is kind of like a Git internals command that will print out the contents of this commit, 
so you can see this kind of maps to the data structure I drew on the board over there. 
So this commit has inside of it this tree and then I'm the author and this is the commit message and so on, 
and I can continue digging down here. 
So, you can take this hash of this tree and do git cat-file -p this hash here.
It says that this tree has inside of it a single entry hello text, 
and that file has its a blob, 
and it has this hash. 
I can do git cat-file -p <hash> and it will show me the actual contents of that file. 
So, these are like internal Git commands to explore objects in the object store.
Question, that's a great question. 
So the question is, why did I have to use git add? Why can't you just commit all changes? And the answer is, 
well, there kind of is a way to commit all changes. 
If you do git commit -a, this commits all the changes that were made to files that are already being tracked by Git. 
So anything that was included in the previous snapshot but has been modified since then, it doesn't include new things.
There are also variants of git add. 
Like, if you do git add :/, this will add everything in the top from the top level down of your repository. 
But at a higher level, the reason we have this separation between git add and git commit 
and why git commit doesn't just snapshot the entire directory is that there are often situations where you don't want to include everything in the current snapshot.
Like, here's a couple of examples. 
One is that I might be working on my project and I go ahead and implement two features. 
Maybe I don't want to have a single snapshot that comes after this one that's like, "I implemented feature A and feature B." 
Maybe I want to create two separate nodes in the history so that it looks like first I implemented feature A and then after that, I implemented feature B. 
So, I have one snapshot that only includes A, 
and then the next one includes both A and B. 
git add is a tool and like the staging area in general is a tool that will allow me to do that sort of thing.
Another example is, suppose I'm working on a bug fix and I have printf statements I've put all over my code, 
and then finally I find the bug and there's a +1 somewhere where there shouldn't be a +1. 
So, I go fix that, 
and then I want to take a new snapshot with my fix, but the snapshot probably shouldn't include all of my print statements. 
It just needs to include the fix of removing that +1. 
So, one way I could solve that issue is I can go in and manually remove all the print statements, but Git has a much better way of doing that. 
There's actually a way to specify that I only want to add the change of removing that +1, then I can commit that, take the new snapshot, 
and then I can throw away all the other changes. 
There are commands for doing that, 
and some of them are linked in the lecture notes.
So, those are two ways in which you can use the staging area to help you and why there isn't just like a "snapshot everything" command. 
Yeah, John points out yet another example is you might have log files in your current directory that your program runs when you run it, 
and you probably don't want to include those when you take a snapshot. 
There's probably other things like if you compile your project, you end up with a bunch of .o and like ELF files. 
You probably don't want those to be part of your history.
So, going back to what I was showing you before, I'm going to clear the terminal screen and then show you the git log command. 
So, git log lets you visualize the version history, 
and this is an incredibly helpful command. 
By default, git log shows you a flattened version of
you visualize the version history and this is an incredibly helpful command 
By default, git log shows you a flattened version of the version history. 
So even though the version history is a graph, this will linearize it and just show things in order. 
I personally find that confusing, so I almost never use git log. 
And instead, git log takes some arguments that actually show the history as a graph. 
So you can treat this as a magic incantation for now, 
and you can read the documentation if you want to figure out exactly what each of those flags does. 
But for now, this doesn't look all that different because we only have one node in our graph. 
So visualizing it as a flattened thing versus a graph doesn't look all that different.
Let me go ahead and create a new snapshot, 
and then we can run this command again and see exactly what it does. 
So I will put another line into hello.txt, 
and if I cat hello.txt, it has the thing it had before plus this. 
I can do git commit and notice this doesn't do anything. 
It just says "no changes added to commit" or "no changes staged for commit." Why is that? It's because I didn't add this to the staging area. 
I didn't tell git, "like, this is something that should be included in the next snapshot."
So if I do git add hello.txt, git status says "Okay, this change is ready to be committed, this modification to this file." And now I can do git commit. 
I'm gonna put in a useless commit message, 
and the new changes have been made. 
And so now my history has another node in it, 
and then this node has some hash that's shown on the screen. 
And now if I rerun that command from earlier, the git log with all these arguments, it actually starts looking more like a graph.
Here, notice that this is like that graph turned this way. 
The more recent, so it's shown vertically, not horizontally. 
And the more recent commits are shown at the top. 
This is showing one commit. 
It shows a commit hash, shows a bunch of metadata including the commit message. 
And then this is the part I want to talk about next. 
So remember, we talked about objects like the actual contents of your repository, 
and then we talked about references, ways of naming things in the repository with human-readable names.
So "master" is one reference that's created by default when you initialize a git repository. 
And by convention, it generally refers to the main branch of development in your code. 
So "master" will represent like the most up-to-date version of your project. 
So here, you can think of "master" as a pointer to this commit. 
And as we add more commits, this pointer will be mutated to point to later commits. 
Then we also see here "HEAD." This is a special reference in git.
It's a reference like "master," but it's special in some way. 
And "HEAD" basically is used to refer to where you are currently looking right now. 
Any questions so far? Yeah, question. 
That's an excellent question. 
So the question is, "Have you worked with GitHub before? And you have to create an account to do that. 
How does GitHub relate to git?" And the answer to that question is, GitHub is a repository host for git.
So you can create an account on GitHub and store a git repository there and use that to collaborate with other people. 
But git as a command-line tool is just independent from github.
So you don't have to use Github to use Git. 
You don't have to use Github, declare it with Git either like there are other providers of Git repositories like Bitbucket or GitLab or things like that, 
and so yeah Github is a host for Github repositories. 
Any other questions? Yeah, so the question is if you want this repository to end up on Github, how do you do that? Yeah, there's a separate set of commands for doing that. 
There's a concept of having your local copy of version history interact with another copy, so the other copy is called a remote, 
and then there are set of commands for interacting with Git remotes and sending data from your remote or from your copy to Git remotes 
and getting data from Git remotes into your local copy, 
and we'll cover that later in this lecture or maybe in the lecture notes. 
Ron might make a supplemental video to go along with this lecture. 
Any other questions? Okay, a couple of other basic commands to show you. 
So, so far I've shown you a version history and we've taken a file and modified it, but we haven't really made use of the history in any way besides reading the messages. 
One useful Git command is something called Git checkout, 
and this is a kind of wacky command. 
It lets you do a bunch of different things, but one thing it lets you do is move around in your version history. 
So, one thing I can do is give Git checkout the commit hash of a previous commit, 
and I don't need to type the whole thing. 
I can give it a prefix and it's to figure out what I'm talking about. 
And what this will do is it will change the state of my working directory to how it was at that commit. 
So, here if I do cat hello.txt, recall that I had only one line in here before at the first commit, 
and later I added that second line. 
Now, if I do that Git log command, 
and this command is super helpful, like it shows you all the things, if I do this command, notice that this output looks a little bit different than before. 
Like my actual history contents, the commits themselves, 
and the way they relate to each other and all that have not changed, but the references have. 
So, notice that HEAD is down here, even the master is still up here. 
So, at high level, what this is telling me is this is what I'm looking at right now. 
If I want to go back here, I could type Git checkout and this commit hash. 
Does anybody know a different thing I could type here instead of this long hash in order to go back to this commit? 
Yeah, you can give it the name of this branch colored in green here, 
and it refers to this commit. 
So, I can give it the short name or the human-readable name instead, 
and now if I do cat hello.txt, notice that it has that second line. 
Yeah, yeah, so to repeat that, Git checkout actually changes the contents of your working directory, 
and so in that way, it can be a somewhat dangerous command if you misuse it. 
For example, you can see if I modify hello.txt and then try that Git checkout command from earlier, actually notice here that it says error. 
It says there's a file that's been modified, 
and the Git checkout would destroy your modification. 
You probably want to do something about that, but there are flags like, for example, Git checkout -f, does this forcibly, 
and now it's throwing away my changes. 
So yeah, get checkout has the potential to, well, it certainly does modify things in your working directory and it can actually destroy changes if you're not careful. 
Question: Exactly, yeah, this is exactly what I want you to be thinking about, 
how these like the crazy get interface commands correspond to mutations to this graph and mutations to the reference 
or like additions to the graph in mutations to the references map. 
So yeah, exactly, get checkout moves the head pointer and then also mutates the contents of your working directory with the contents that the head pointer now points to. 
Of course, my name for that commit. 
Any other questions?
All right, so one other basic command I want to show you is the git diff command. 
So I'm going to modify this file and put some changes in it. 
The git diff command can show you what's changed since the last snapshot. 
It's just helpful for like knowing what's going on with your project. 
Git diff can also take extra arguments like you can do git diff and say compute a diff not with respect to the last snapshot, the last commit, but with respect to this and say, 
"Okay, two lines have been added since this point to hello dot text." 
Question: So, your question is what does this command do without this extra argument here? 
That's a good question.
What this does is it computes a diff with respect to head and looking at my git log hat is pointing to here. 
So it's doing a git diff with respect to this commit and you can actually specify that explicitly. 
You can do git diff head hello text. 
Okay, yes, uh-huh. 
So that's a good question. 
It's like how can hello dot text be different than head because head refers to where you currently are. 
So, to clarify, head refers to the last snapshot, so like in my picture here, head and master are both here and the current working directory is kind of independent of this. 
Like, you're going to delete all the files in here, it doesn't change the history graph or the references, 
and so yeah, you can have differences between here and here and at a high level, this is how you work on a project. 
Like, you make some changes here, you get add them to stage them, 
and then you get commit, 
and that creates a new snapshot here. 
Good question. 
Any other questions?
So the question is, does git actually save all this stuff kind of in the obvious way or is it doing something fancier? The answer is, 
it is doing something a little bit fancier, but you can, it has an interface that lets you think of it like it's stored that way. 
In practice, git uses Delta compression, it also does some other stuff, but yeah, the on-disk representation is actually reasonably efficient. 
Question: That's a good question. 
So the question is, here we were comparing the current working directory with a particular snapshot in the past. 
Can we compare two snapshots with each other, like at two different points in the history? And yeah, git diff can take yet another argument here. 
So I can, for example, compare head with, it did in the wrong order, 
I can compare what change from here to head in hello text, 
and it shows me that I added the second line in there. 
Any other questions?"
"Yeah, so the question is, you're working on a shared project in a Dropbox folder, 
and anyone can migrate to Git. 
Does it make sense to turn the Dropbox folder into a Git repo? Do not use Git inside Dropbox. 
Dropbox will corrupt your Git repo. 
There are good solutions to doing that. 
One is just use Github. 
Otherwise, talk to me after class, 
and there are ways of using Dropbox as a Git remote safely. 
Any other questions? Next, we're going to talk about branching and merging, 
which is another powerful feature of Git that you almost certainly use both when working on your own projects and when collaborating with others. 
For this series of demos, we're going to, 
rather than work with a simple text file, actually write a simple computer program because it'll better illustrate the concepts of branching and merging. 
And as we go through this demonstration, we'll keep in mind how the Git interface commands connect to the underlying data model, connect to objects and references, 
and how these commands modify those two data structures. 
Let me do a 'git status' to see the current state of my repository. 
Here, I've modified hello text. 
I actually don't really care about this modification anymore. 
This is some random file. 
If I do 'git checkout folio text,' this is another different use of the 'checkout' command, 
which basically throws away the changes that I've made in the working directory and sets the contents of hello text back to the way it was in the snapshot that 'HEAD' points to. 
If I like it, 'git log --all --graph --decorate' will show me that here I added the initial text, 
and it added that single line here. 
And so now, 'hello text' doesn't have that third line I'd added. 
It just has the original. 
Next time, we should write a very simple program. 
We'll call this program 'animal.py,' and let me just go ahead and write a program that it prints a little bit of output when I run it. 
Let's see. 
[Applause] So when I run this program, it runs 'main,' calls 'default,' and then let me go right ahead and define 'default.' 
'Default' is going to just print 'hello.' So this is a program that greets its user. 
And so if I run 'animal.py,' I'll see that it just prints 'hello.' So that'll be our starting point. 
If I do 'git status,' it shows me that 'animal.py' is an untracked file. 
To begin with, I want this to be part of my snapshot, so I'm going to 'git add animal.py' to add it to the staging area and then do a 'git commit.'
Here, I'm going to write yet another useless commit message. 
Don't actually write commit messages like this in real projects, but for now, this is fine. 
So now I have this basic 'animal.py,' and if I look at my 'git' history, now I have this latest snapshot. 
This is the commit hash, 
and this is where the master branch is pointing. 
Now we're actually way to demonstrate how to use Git branches to have parallel lines of development. 
The git branch command or the branch sub-command is used to access functionality related to branching. 
Just running git branch by itself lists all the branches that are present in the local repository. 
It can also take an extra argument -v to be extra verbose and print some extra information. 
If we do git branch and then specify the name for a new branch, Git will create a new branch which is just a reference that points to the same place where we're currently looking. 
So now there's a new reference called "cat" which points to wherever HEAD was pointing. 
If I look at the git log again, I'll see that HEAD points to master, master's over here, 
and this is also where the cat branches. 
So now I have two branches, two references that resolve to the same commit. 
Git is actually aware of not only which snapshot in the history we're currently looking at (so HEAD points to this commit), 
but it's also aware of HEAD kind of being associated with a branch. 
Here, HEAD is associated with master, 
and it's the case that if I create a new snapshot (if I type git commit at this point), the next snapshot will be created and I'll point to that new snapshot. 
master will be updated along with HEAD.
If I do git checkout cat, what this does is it switches to the branch cat. 
It replaces the contents of the working directory with whatever cat's pointing to, which in this case is the same as the contents before. 
But now, if I look at the git log again, now I have HEAD point to cat instead of master, 
and then master also points to the same place, the same underlying commit. 
And now at this point, if I make changes to my current working directory and make a new commit, the cat branch, 
the cat pointer will be updated to point to the new commit, whereas master will continue pointing wherever it pointed before.
So let me go ahead and modify animal.py to add some cat-related functionality. 
So I'm going to say that if sys.argv[1] is cat, then run the cat() function, otherwise run the default function. 
And then let me go ahead and import/define the cat() function. 
So cats don't say "hello", they say "meow" - straightforward enough. 
So now if I run animal.py and give it the cat argument, it says "meow". 
If I give it some other argument, it defaults back to "hello".
All right, so simple change I made. 
If I do a git status, that says that animal.py has been modified. 
git diff will show me what's changed since the last commit. 
So here I've added this cat() function, highlighted in green, then also changed the main() function a little bit. 
Now here, if I do git add animal.py, git commit (I mean actually you should write a slightly more useful commit message this time, like "Add cat functionality"), 
and now if I look at the git log, I see a little more stuff. 
I'm going to show you one more argument to this git log command - there's an argument --oneline (one line, spelled correctly) 
which shows a more compact representation of the graph. 
This should be a more useful thing to use because we're super zoomed into the screen and there isn't that much space to show a long commit history.
So here we see the sequence of commits is still linear, 
and we have master still pointing wherever it pointed before. 
Where we just had the basic underlying animal top high functionality, but now we have this cat branch which adds the cat functionality. 
We could, for example, get checkout master to go back to the master branch, 
and then here if we look at animal dot pie, it doesn't have the cat functionality anymore. 
If we look at the git log, we'll see that head is pointing to master, so we can jump back and forth between parallel lines of development. 
So now that we have the cat functionality, suppose that we want to work on adding dog functionality in parallel. 
And suppose that in this case, like the cat functionality is under development or maybe somebody else is working on it, 
so we just want to start from the base master commit and build the dog functionality starting from there. 
So now what do I want to do? I want to create a new branch, dog, for adding the dog-related functionality, 
and I'll eventually merge it in later. 
So I can use the git branch dog command followed by the git checkout dog command to create a new dog branch and then check it out. 
There's actually a short form for this, get checkout - b-dawg. 
So this does get branch dog, get checkout dog, 
and now if I look at my graph, I have cat where it was before, master where it was before, but now head instead of pointing to master as it did before, 
now head points to this newly created dog reference, which is also at the same commit. 
So at this base commit, 
and now I'll go ahead and add my dog functionality. 
So let me go and define my dog function, dogs don't say hello, they say woof. 
And then I'll add some similar functionality here to decide whether to run default or dog. 
So if the first argument is dog, then I want to run the dog function, otherwise, whoops, otherwise, I want to run the default function. 
So here's what I've changed with respect to the base commit wherever master is pointing. 
So I've added the dog function, 
and I've changed mean a little bit, so a kind of parallel modification to what I did in the cat branch. 
Let me go ahead and get add animal Titus add up to the staging area. 
If I do get status, I'll see that this change will be committed when I make the next commit. 
And then I do get commit add functionality. 
Now when I look at the git graph, it actually looks kind of interesting compared to the ones we've looked at before. 
This shows that these three commits are in common with the ones that come after it, but then the history is actually forked after this point, 
and I have this one commit that adds cat functionality in one line of development, 
and then I have this other commit that adds dog functionality in this other line of development. 
And then using the git checkout command, I can switch back and forth between dog and cat and master. 
So this is great, I can do development in parallel on different features, 
but this is only really useful if I can eventually combine those things back into my original line of development to have both features in a single version of my source code. 
So the command that's used to do that is git merge. 
So like git branch and git merge can kind of be thought of as opposites. 
Let me check out git checkout master, let me check out my master branch.
So now you see, head points to master, 
and then I want to merge the cat functionality and the dog functionality into master. 
And to do that, I can use the git merge command. 
Git merge is actually pretty fancy, 
and I can actually merge cat and dog at the same time. 
But for this demonstration, we're going to only merge one thing at a time.
So first, I'll type git merge cat, 
and it gets us some stuff here. 
It says "fast-forward." So what is going on here? Well, this is one interesting thing that git can do. 
When you're at a particular commit and you merge some other branch in where that other branch has the current commit as a predecessor, 
it's not necessary to create any new snapshots or do any other fancy stuff. 
Basically, this master branch here, this pointer to this commit, can just be moved to point here instead to incorporate that cat functionality. 
And so if we look at the git log again, we see that master is basically pointing to the same place as wherever cat was pointing. 
All right, so now we're on the master branch, 
and it has the cat functionality. 
Great, we're halfway there. 
If we look at animal.py, it has the cat functionality, but it's missing the dog stuff. 
So let's try git merge dog next. 
Something a little bit more interesting happens this time. 
So this time, the branch can't be fast-forwarded like it was before. 
It's not that one thing which is strictly older than the other thing. 
There's been parallel development that may be kind of incompatible with the current set of changes. 
And it does its best job at automatically merging the changes from this other branch. 
So it says "Auto-merging animal.py." But in this particular case, there's what's called a merge conflict. 
So it wasn't able to automatically resolve all the conflicts between these two parallel branches of development. 
And this is something you'll see in practice when you're working on real software projects, 
and they're complicated, slightly incompatible changes happening in parallel. 
So at this point, it's left up to the developer to fix this issue. 
And Git offers some functionality in order to help resolve merge conflicts. 
There's a program called git merge tool, 
and in my particular setup, this will launch Vimdiff. 
Actually, this is not configured. 
Vimdiff, I think, will start the right program. 
Let me set up my Git to launch the correct tool actually. 
Let's skip that part, 
and let's just manually look at this event. 
So there's a program called Vimdiff, which can be set up to be launched when you type in "git merge tool," 
which is a tool that you use when you try git merge and there are merge conflicts. 
But in this particular case, we'll just manually resolve them. 
So let me, I did "git merge --abort," so it put me back in the state I was before I tried that git merge. 
So this is the current state of my repository. 
I'm back to the case where master is at the same place as cat, 
and I'm about to merge in dog. 
So I do git merge dog, 
and it says "conflict merge conflict in animal.py." So let's just look at animal.py directly. 
So it looks like this top part looks pretty reasonable. 
It has both the cat function and the dog function, which is exactly what I want. 
But now I see some weird stuff in main, 
and this is where I added slightly incompatible changes. 
So here it says that in one thing, like basically the branch you were on, you had this content. 
And then the branch you're trying to merge had this content. 
And then these things here, the angle brackets and the equals, are conflict markers. 
So this is where you were, 
and this is the thing you're trying to merge in, 
and it's basically saying that it was this on one case, this in the other case, 
and it doesn't really know how to resolve these two. 
And it's left up to the programmer to fix this problem. 
So in this particular case, we can go ahead and delete the conflict markers. 
And then, turns out that we can actually concatenate this code together and does the right thing. 
Maybe we want to make a small change like this should be an if, this should be an else-if, 
and this should be an else. 
That might make a little bit more sense, actually. 
I think it's necessary for correctness here. 
So the programmer needed to modify the code a little bit in order to make it sensible when it's merged together. 
But once the programmer has fixed the merge conflicts, fixed the stuff between the conflict markers, 
you can save this file and we can do git merge --continue to tell git that we fixed the issues. 
It's necessary to re-add animal PI to tell git that we've actually fixed these issues. 
And then we need to git merge --continue. 
It pops up an editor and we can give a commit message for this new commit that we're about to create. 
And now if we look at the git history, we have the single commit that represents our merge commit that we just made, which merges in the dog functionality. 
And here, this has as parents both the dog commit and the cat commit. 
So both these branches appear in our history from this point backwards, 
and this current commit that we're on incorporates the functionality from both of these branches. 
So if we run animal duck fight with cat, it does the cat thing. 
If we run it with dog, it does the dog thing. 
And if we run it with anything else, it falls back to the default implementation. 
So this is a demonstration of how you branch and get to do development on different things in parallel, 
and then how you can use the merge command and git to resolve those different branches and combine them together into a single snapshot 
that includes all the functionality that was developed in parallel with each other. 
And then one thing that can happen when you're doing git branching and merging is you run into merge conflicts, 
and these conflicts show up as conflict markers and text files. 
You can manually resolve them, 
and git also has some tools that can help with this, though these tools are kind of advanced and will only refer to them in the lecture notes and not actually demonstrate them for you. 
So that's git branching and merging. 
Any questions? No? Great. 
So moving on to the next topic of this lecture, we will talk about git remotes. 
So this is basically how you collaborate with other people using git. 
A git repository, the stuff contained in this .git folder, represents kind of an entire copy of the history. 
It has the objects and the references and contains all the previous snapshots. 
And the way you collaborate with other people using git is that other people can also have copies of the entire git repository.
And then your get copy, your local instantiation of the repository, can be aware of the existence of other clones of the same repository. 
And this is a concept known as remotes. 
So, the git remote command will list all the remotes that git is aware of for the current repository. 
And in our case with this repository right here, this command get remote just doesn't print anything because we haven't configured any remotes. 
It is only aware of the single local copy of the repository that we're working with here. 
But in practice, if you're collaborating with other people, your git might be aware of the copy of the code that is on github. 
And then there's a set of commands to send changes from your local copy of the repository to a remote that your git is aware of. 
So, sending stuff from your computer to github, for example. 
And there's another set of commands for fetching changes made in a local repository to get changes from github into your own local copy. 
In this demonstration here, we actually won't go and configure a github account and log in and create a new repository on there. 
You can find other tutorials for doing that. 
We'll actually just use a separate folder on the same computer and treat it like a git remote. 
So let me, I'm in the demo folder here. 
Let me go up one directory. 
I have a directory called playground that has this demo folder. 
And I'll go ahead and create a new directory in here, 
and I'll call it remote. 
And then do get in it -- bear in here. 
Those are the command that you'll probably never need to use in regular usage. 
But now what I've done is made remote into a folder that's appropriate to use as a git remote. 
So now going back into my demo folder here, my git repository, I can do git remote to list the remotes. 
There's nothing yet, but I can use the git remote add functionality to make my local repository aware of the existence of a remote. 
So, I can do git remote add, 
and then the format for this is that remotes have names and then they have a URL. 
So in this case, I'll use the name origin, which is often used by convention as the name of the remote if you're only using one. 
And then for the URL, normally this will be like a github URL or something like that or bitbucket URL or gitlab URL if you're using an online repository hosting service. 
But in this case, it's just a path to a folder on my local machine. 
There's a folder in the parent directory called remote that will act as the git remote for this repository. 
So now, once I've done that, there's a set of commands for interacting with this remote. 
One command that's useful is the git push command. 
This command can send the changes from your computer to the remote. 
And the format for this command is that git push takes in the name of a remote and then it takes in a local branch name, a remote branch name. 
And what it does is it creates a new branch or updates a branch on the remote with the name specified here and sets it to the contents of the branch specified here. 
So a concrete use of this might look like git push. 
I've only one remote called origin, 
and then what should I push?
Let me look at my history graph. 
I have a bunch of things I could push. 
Let me get pushed to origin, the master branch from my local machine: master. 
So, I want to create a branch on the remote machine with the name master that is going to be the same as the master branch on my local machine. 
So, let me go ahead and run that command. 
It prints out some stuff, 
and it says, "On the remote, I created a new branch remote master points to the same branch as master on my local machine." And now, if I do a git log, it shows me. 
So, in blue is head, where I currently am. 
In green, are all the branches in my local git repository. 
And now, we see one new color here that we had not seen before. 
So, in red, git shows references that are present on the remotes that my local copy is aware of. 
So, on the remote origin, there's also a branch that happens to have the name master that points to the same place as my local branch master points. 
And so, now, if I make updates to my local copies, like suppose here I go in and change the capitalization of these things, 
and then git add animal.dot.hi, git commit, here's a short form for commit with a message so it doesn't pop up the editor. 
I'll give it a late and commit message. 
And now, if I look at the git graph, now I see that I've created this new snapshot here that has this lower casing stuff in it, but origin master is still back here. 
So, if somebody else looks at the remote, they will only see the changes up to here, 
and we can actually demonstrate this functionality. 
So, let me go ahead and open up a new tab here and go into my playground directory. 
The git clone command is a command that somebody can use to start from some copy of a repository somewhere and make their own local copy. 
So, this is often a command to use when starting out with a git repo. 
Like there might be something available on github, 
and you want to copy it all in your machine in order to look at it or start doing development. 
And so, the format for git clone is that it takes in a URL, 
and then it takes in a name for a folder for where to clone it. 
So, in our case here, we're just going to clone from this remote directory. 
We're pretending that this remote folder is actually a remote machine, 
and then we're all clone it into the folder called demo two. 
Cloning into demo 2 done, 
and I'm going to CD into that directory. 
And then now here, I'm going to rename these tabs at the bottom. 
I will say this one's machine one, 
and this one's machine two. 
So, you can think of these as two different people on different machines with their own copy of the repository, 
and they're both interacting with the single remote. 
So, if I do my git log command that I've been doing on machine one, I see on Machine 2, I see this portion of the history. 
So, master on machine 2 is pointing to the same places origin master, 
and it says, "merge branch dog." So, if I look at animal.dot.pie here, 
it doesn't have the changes that I made on machine to even though there are sorry on machine one 
where I have this new commit that is only present on this machine but not on the remote and not on machine two.
So if I want to fix that, if I want to send these changes up to the remote, like think of it as sending it up to GitHub, 
or up to the machine that's holding or maintaining the source code, I can use the git push command. 
Again, git push origin master colon master and this will work, but this is kind of annoying to type every time you want to do this. 
Like, this is a really common operation, so git has a way of making this a little bit simpler. 
It has a way of maintaining relationships between branches on your own local machine and branches on remote machines. 
It is a way of knowing what branch on a remote machine a local branch corresponds to so that you can type in a shortened version of git push, 
and it'll know what all the arguments to the expanded form would have been. 
And there are a couple of different syntaxes for doing this. 
One way is to use the git branch - - set up stream command, 
and what this does is for the branch that's currently checked out, which is master, it will set the upstream, 
and I'll type in origin master. 
And see, now it says 'branch master set up to track remote branch master from origin'. 
Now, if I type in git branch - VV (remember, this is telling me about all the branches that I know about in a very verbose way, 
that's what the -VV means), I have three branches on my local machine on machine one. 
I have cat, dog, 
and master, 
and master on my local machine corresponds to origin master. 
So now I can type in just git push without all the extra arguments. 
I could have done this as git push origin master colon master, but it wasn't necessary. 
It'll know that I want to push to origin master, 
and it will make that change. 
So now these changes are present on the remote. 
We can go over to machine two, pretend we're the other guy interacting with this repository, 
and if I do my git log command, I still don't see the changes. 
So what's going on here? Well, it's necessary in order to run a separate command. 
Or it's necessary to run a separate command in order to have these changes present here. 
By default, all the git commands don't talk to the internet. 
It all works locally, which means it works very fast, but then there are special commands for saying that you want to retrieve changes that have made somewhere else. 
And the command that's used for doing that is a command called git fetch. 
Git fetch takes the name of the remote as an argument, but if there's only one, it'll just use that. 
So you can type in git fetch, 
and then it's talked to this remote repository, 
and it says that there's some update on the remote, 
and we can visualize it by running git log. 
And now we see here another situation that we hadn't seen before. 
We have master on our local machine. 
The master branch doesn't change. 
The git fetch command doesn't change any of our local history, our local references like our branches. 
But now it's aware that origin master has been updated to point to this new commit, 
and there's a separate command we can do, git merge, in order to move master up to here. 
Or there's another command called git pull, which is the same as doing git fetch and then git merge. 
So if we just do git pull here, for example, it will say it's fast-forwarding, it's merging in origin master into our master, 
And now, if we look at the Git history graph, we've currently checked out master. 
Master points to the same place as the origin master that we're aware of, 
and all the changes between Machine 2 and Machine 1 are in sync. 
So those are the basic commands for interacting with Git remotes. 
So there's the Git remote command for listing remotes and adding and removing them and things like that. 
And then there's the Git push command for sending changes from your local copy of the repository to the remote. 
And then there's the Git fetch command, which is for retrieving changes to a repository that are present on a remote and getting the changes on your local machine. 
And once you retrieve those changes, you can use Git merge to update your local branch to point to the same place where the remote branch does, 
or you can use the Git pull command, which does basically the same thing as Git fetch plus Git merge. 
And then of course, separate from all these commands is the clone command that we talked about a little while ago, 
which is for taking a copy of a remote repository and initializing the local repository from that copy. 
So that's a quick overview of the different commands used to interact with Git remotes. 
And now these are kind of complicated, 
and it takes a while to master all the different variations of this and understand how they're actually used in practice, but hopefully, this acts as a quick introduction, 
and you can see how the different commands relate to the underlying data model. 
All these commands, all they do is fetch new objects from other places or send objects from the local mission to other places, 
and these commands mutate references. 
So relating the interface of Git and some of these kind of badly designed commands to the underlying data model can help it make a lot more sense. 
The final topic we're going to cover today is a kind of overview of other things that Git can do that we're not going to go into detail in teaching you how to do, 
but we just want to tell you that these functionalities exist in case you need to do these things yourself. 
You can look up the documentation and find out exactly how to do it. 
One thing is the Git config command. 
Like a lot of tools we've looked at, like the shell and TMUX and things like that, Git is highly configurable, 
and it's configured using a plain text file which can be edited either through the command-line interface. 
So Git config can take in flags that will modify this text file, or you can edit the .gitconfig file in the home folder with plain text configuration. 
And so for this lecture, I've actually cut out most of my Git config and only left in my username and email for what will go into Git commits. 
But there's a lot of stuff you can put in here which will make it behave nicer, behave the way you want it to, 
and you can look online for different ways people have configured their Git configs. 
Oftentimes, people have documentation in their Git configs, which can be found on GitHub. 
There's a couple of other random commands that could be useful. 
One is for when you want to clone a repository with Git clone that's really gigantic.
git cloned by default copies the entire version history for the remote it's downloading the repository from, but there's an argument you can pass it, 
which is --shallow, which will avoid doing that. 
So if there's some copy of some code on Github say that you want to get a copy of on your local machine, 
but that repository is really gigantic and has a billion commits, doing git clone --shallow will be much faster. 
But then of course, you won't have the version history on your local machine; you'll just have the latest snapshot.
Another command that we find really useful when doing development on real software projects is an interactive version of the git add command. 
So to demonstrate this, I'm going to go ahead and make a couple different changes to my animal.py. 
One change I'll make here, I'll change some text here, 
and then I'll put a new print statement here. 
So let's pretend that this first change was some real change I wanted to make, say it's a bug fix, 
and this other change here was a printf that I added for debugging, but I don't actually want to commit in the next snapshot.
If I do a git diff, it'll show me that yes, I've made these two changes, 
and if I do git add animal.py, it will stage both of those changes for a commit, 
and that's not what I want. 
I could go manually remove this debug print and then do git add animal.py, but there's an easier way to do it. 
There's this git add -p command which lets me interactively stage pieces of files for a commit, 
and so there's some interface for working with this.
Here it's saying, do I want to stage both of these changes, 
and no, I don't. 
But I'm going to split it into two smaller changes. 
This one I do want to keep, so I say Y for yes, 
and this one I don't want to keep, so I say n for no. 
And then if I do git diff --cached, this will show me what changes are staged for commit. 
So now it shows only the actual change I wanted to keep. 
If I do git diff, it'll still show me the other change that is not going to be part of the next commit, which is the change I didn't want to keep. 
And then with this, I can do git commit, specify some commit message, now I only have this change left, 
and then I can do git checkout animal to apply to throw away this change. 
So git add -p for interactive staging is a useful thing.
A couple of other commands that you can look up on your own are the git blame command, so this command is kind of ominous, 
but it can be used to figure who edited what line of a file, 
and you can also find the corresponding commit that was responsible for modifying that particular line of that file, 
and then you can look up commit messages associated with that and whatnot. 
So this is not that interesting to do in our current toy repository, but I'll go over to the repository for the class website, 
and we can look at some particular file here. 
And let me go to some particular line here, 
and I can be looking at this like, 'Oh, why was this particular line added? What does it mean?' And I can look at the git blame for this.
If I do git blame config.yml, it'll print out all the lines kind of in the right column, 
and then in the left side, it'll show me what commits that change was made in and by whom. 
And then looking at this, like I can go down to this collections line, it was made in this commit,
That's the last commit that modified that line, 
and now I can use the git show command to get information for that particular commit. 
Oh, and this is kind of useful - redo lectures is a collection. 
That's probably what was related to that 'collections' line. 
And then, beyond just showing the commit and the commit message, it also shows me the actual changes introduced in that particular commit, 
and I can go look through them and understand what's going on. 
Another kind of cool command is a command called git stash. 
So let's go back to our demo repository and demonstrate that here. 
Say if there are some changes here and I temporarily want to put them away, if I do git stash, it will revert my working directory to the state it was in at the last commit. 
So if I do cat hello.txt, that change is gone, but it's not just deleted - it's saved somewhere. 
And if I do git stash pop, it will undo the stash. 
So now if I look at hello.txt, it has the changes I made. 
Yet another useful command!
Another really neat command is something called git bisect, 
and this has a complicated interface that we're not going to demonstrate in detail. 
But basically, this is a tool that can be used to solve a bunch of problems where you need to manually search history for something. 
Suppose you're in a scenario where you've been working on a project for a long time. 
You have lots and lots of snapshots - you're a thousand commits in - and then you notice that some unit test doesn't pass anymore. 
But you know that this was passing, like, a year ago, 
and you're trying to figure out at what point did it break - like, at what point was this regression in your code introduced? 
So one thing you could do is manually check out - like, go back one commit and see if the unit test is still failing, go back one commit, see if it's still failing, 
and eventually, you'll find the first commit where the test stopped working, 
and it'll probably tell you what broke. 
But that's kind of annoying to do manually. 
git bisect automates that process, 
and it actually binary searches your history, so it does this in the most efficient way possible. 
And not only that, git bisect can take in scripts that it uses to try to figure out whether a commit it's looking at is good or bad. 
So it can be a fully automated process. 
Like, you can give git bisect a unit test and say, 'find the first commit where this unit test stopped passing'. 
It's a really powerful tool.
Another random thing that's kind of useful is something called a gitignore file. 
So by default, if you have random files in a directory - like, let me create the .DS_Store file - whoops, create the .DS_Store file, 
and then do git status. 
So .DS_Store is like some nuisance file that Mac OS creates. 
I don't know exactly what goes in here, but basically, once this file is in this directory, now whenever I do git status, it says, 
'oh, there's this new file that I've never heard of before, but it apparently here - do you want to add it?' And this sort of stuff gets annoying. 
And there's a lot of other stuff beyond OS-specific garbage that might be in a directory. 
Like, for example, if you're working with C code, you might compile it and produce .o files or executable files or things like that, 
and you probably don't want binaries to be part of your commit history. 
You only want the source code. 
And so git has a way of you being able to tell the tool that you don't care about a particular
You don't care about a particular set of files and to ignore them, 
and that's something called a git ignore file. 
So, if I go and modify the file called git ignore in the current directory, I can specify particular file names or patterns of file names. 
Like say, I can specify star dot o so any file ending in dot o, along with da store. 
And now if I touch food oh and now do a get status, I'll see that git says okay, I've hollowed out tax which I've modified sure and and I have get ignore. 
So you should track your git ignore file using it. 
But notice that it doesn't mention my dot d s store file or my food out o file that's present in the current directory because that has been get ignored. 
So that's a quick overview of a little bit of advanced git functionality, just to give you a flavor of what sorts of cool things this tool can do. 
And then finally, we have a couple of other topics that are covered in the lecture notes in more detail. 
I'll just quickly list them here so you know what to look for. 
One is that there are many graphical clients for git. 
We don't personally use them; we like the git command-line tool, but some of them are kind of okay, 
and you might want to check them out just to see if you prefer using those. 
Another thing is shell integration. 
So, you've noticed that in this tutorial I've done get status a whole bunch to see kind of what's going on with my repository. 
Well, that's kind of annoying to do, 
and a lot of people have their shell prompts set up so that just within this shell prompt itself, like on every line, 
it will show me a very succinct summary of what's going on with my repository. 
So, it might show me a summary of what branch I have currently checked out, along with maybe if I've modified files or untracked files. 
And so, we have a link in the lecture notes on how to get some nice shell integration for displaying kind of git-related information in your shell prompt. 
Similar to that, you can get integrations with your text editor. 
So, for example, I use vim, 
and I have a plug-in for vim that does all sorts of interesting git-related stuff. 
One thing I can do with this plug-in is look at git blame information. 
Remember, we just looked at this through the command line. 
Instead, I can look at it with this plug-in, 
and it lets me work with it a lot faster. 
I can look at git blame, press enter when hovering over a specific commit, 
and it shows me that particular commit in my text editor. 
It even hides all the other files and shows me just the one file I was looking at, which is presumably what I care about. 
So, we have links to that in the lecture notes as well. 
And there are a couple of other interesting things you could look at there if you're interested. 
Finally, this lecture by itself is probably not enough to teach you everything you need to know about git. 
It's a good start. 
We think that the right way of learning git was to learn about the underlying data model, the whole objects and references, 
and how git models history. 
And then we gave you an introduction to using the git commands. 
And if you want to become really proficient at this tool, in the resources section in the lecture notes for today, we have a link to a book called Pro Git. 
So, this is a free book. 
It's nicely written. 
It's pretty short, 
and I think going through the first couple of chapters of that book should teach you basically everything you need to know in order to use git proficiently for real software projects and for contributing.
It's a project on GitHub and things like that. 
And then finally, just like all the other lectures, we have a number of exercises you can go through if you want some interesting and challenging problems that you can figure out how to do.