Okay, cool. 
So, welcome to the third lecture of the missing semester of your si a second. 
Today we're going to be talking about text editors. 
This is a topic that I really like. 
I think it's one of the most valuable topics that we're teaching in this class because as programmers, 
you spend so much of your time editing text and editing programs that if you invest time into making yourself more efficient at doing this, 
you'll save a ton of time, probably hundreds of hours over the course of your undergrad or over the course of your career. 
So text editors are a little bit different than other programs you might use to edit, say, 
things like English prose because programming is different than writing English prose. 
When you're programming, you spend a lot of time reading what you've written, you spend a lot of time navigating around a buffer, 
and you spend a lot of time making little edits to code all over the place, 
rather than just writing in a long stream like you do when you're writing an essay or something. 
And so it makes sense that there are different programs for these different purposes, right? 
So yeah, things like Microsoft Word for writing essays and things like Vim and Emacs and VS Code and Sublime for writing code. 
The way you learn a text editor and become really good at it is you start with a tutorial, 
and so that's basically going to be the function of today's lecture, plus the exercises we've given you. 
And then after the tutorial, you need to stick with the editor for all your editing tasks. 
And when you're learning a sophisticated tool, 
so today we're going to teach you Vim, which is one powerful editor that a lot of programmers use, when you're learning such a sophisticated tool, 
it may be the case that initially switching to this tool slows you down a little bit when you're programming, 
but stick with it because I'd say that in about 20 hours of programming using a new editor, you'll be back to the same speed at which you programmed using your old tool, 
and then after that, the benefits will start, 
and you'll get faster and faster as you learn more. 
With these sophisticated programs like Vim, it takes not way too long to learn the basics but a lifetime to master. 
And so throughout the time you're using this tool, make sure you look things up as you go. 
If you ever get to a point where you're like, "Oh, this is a really inefficient way of doing things. 
Is there a better way?" The answer is almost always yes because these text editors were written by programmers for programmers, 
and so of course, like the people who wrote these tools, ran into the same kinds of issues and fixed them so that you don't need to deal with these anymore. 
And so yeah, as you're learning, make sure you look things up as you go, either use Google or feel free to send us emails if you have questions or come to office hours, 
and we'll help you figure out how to do things really fast. 
As far as which editor to learn, in previous iterations of this class, we actually avoided teaching a specific editor because we didn't want to enforce our opinions on you guys, 
but we actually think that it's really useful to teach you how to use one particular tool and use it well. 
And so people have really strong opinions about editors, 
so you can see the course notes for more links on this topic. 
Looking at which editors have been popular over the years, Stack Overflow, 
I'm sure you've all heard of that, does a survey every year asking developers various questions, 
and one thing to ask is which text editor do you use, 
and it seems to be that currently, the most popular kind of graphical editor is VS Code, 
and the most popular editor that is based within a command line interface is Vim. 
So we're going to be teaching you Vim, 
and there are a couple of reasons for this. 
One is that all the instructors, 
so me, John, and Jose, use Vim as our primary editor, 
and we've been doing this for many years, 
and we've been very happy with it. 
We think that there are a lot of interesting ideas behind it, 
so even if you don't end up using this particular tool in the long term, I think it's valuable to learn these ideas. 
Also, a lot of tools have been really excited about the ideas in Vim, 
and they support a Vim emulation mode. 
For example, VS Code, which is apparently the most popular editor in use today, supports Vim bindings, 
and this Vim emulation mode as of now has like 1.4 million downloads. 
As you'll see over the course of this lecture, a lot of different tools, including your shell, including things like the Python REPL and Jupiter notebook, 
and all sorts of other things, even your web browser, can support a Vim emulation mode. 
So yeah, we're going to be teaching you this really neat tool today, 
and in this lecture, we can't really cover all of them right, it's a very powerful tool, 
but our goal is to teach you the core philosophy of them, like the really neat ideas behind it, 
and then in addition to that, 
some of the basics like how do you open a file, close a file, navigate around a file, make edits, 
and things like that. 
You may not remember every single little detail from this lecture because we're gonna go pretty fast through some of the material, 
but it's all in the lecture notes, 
and then the exercises actually give you links to some tutorials and things. 
So I highly recommend that you actually go through all the exercises, at least the non-advanced exercises. 
Any questions so far? Great. 
Okay, 
so one of the really cool ideas behind Vim is that Vim is a modal editor. 
What does this mean? Modal comes from the word mode, 
and this means that Vim has multiple operating modes, 
and this is kind of developed from the idea that when you're programming, there are often times where you're doing different types of things. 
Like sometimes you're reading code, 
sometimes you're making small edits to code, like you're finding a particular point and changing a little thing somewhere. 
Sometimes you're just writing a lot of code in one go, like suppose you're just writing a function from scratch. 
And so there are different operating modes for doing these different types of things. 
And so I'm actually going to write this down on the blackboard so I'll have a useful thing to refer to later. 
When you start Vim up, it starts up in what's called normal mode. 
And in this mode, all the different key combinations behave in one way. 
And then there are different key combinations that switch you between normal mode and other modes, which change the meaning of different keys. 
So for the most part, you'll be spending most of your time in Vim in normal mode or what is called insert mode. 
And to go to insert mode, you press the key "i" for normal mode, 
and to go from insert mode back to normal mode, you press the Escape key. 
A little note on notation because we'll need this later. 
In the notation I'm going to be using in this lecture and what's also in the lecture notes and what Vim uses to give you feedback, 
they have a couple of different ways of talking about different keys. 
So when they're talking about bare keys like just the "i" key on your keyboard, 
they'll just say 'i' But for different key combinations, like when you press control and something like say, control-V, it's notated in one of approximately three ways. 
One way that can be notated is a caret and then the control character. 
So this is control-V. 
Another way this might be written (I think we've written it this way in lecture notes) is control-v; this is probably the one you're more used to seeing. 
And then in some parts of them, this is written as angle brackets C - V close angle bracket. 
So just a little bit of notation that will be useful later. 
So yeah, Vim has a couple of different modes, where normal mode is designed for navigating around a file, reading things, going from file to file, things like that. 
And then insert mode is where you type in text. 
So most keys that you press here will just go into your text buffer, 
whereas keys that you press in normal mode are not being put into the buffer and instead are used for things like navigation or making edits. 
And actually, the picture is a little bit more complicated than this. 
There are a whole bunch of other modes, 
and I'm just gonna write them down here because we'll have them here to refer to later. 
And so Vim also has a replace mode, for rather than inserting text and kind of pushing what's ahead of it forward, it will overwrite text. 
And then it has a bunch of different modes for selection, 
so it has a mode called visual mode, 
and then it has visual line and visual block. 
This one is entered via the R key; this was entered via the V key; this one is entered via shift B, 
and this one is entered via control-V. 
And then there's the command line mode, which is entered via the colon key. 
Okay, 
so now that we have that on the board to refer to later, we can actually try some of this out. 
All right, 
so one thing we noticed looking at that picture is that to go from normal mode to any of the other modes, we press some key, 
but to go from any of the other modes back to normal mode, where we spend a lot of our time, we use the Escape key on our keyboard. 
So for this reason, since you'll be pressing the Escape key a lot when using Vim, 
a lot of programmers rebind one of the keys on their keyboard to be Escape, 
because it's really inconvenient to reach up with your left pinkie to press that tiny little Escape key in the corner of your keyboard. 
And so a lot of people use the caps lock key instead, 
so it's right there in the home row, 
and we have some links in the lecture notes for how you can do this key rebinding. 
Okay, 
so now that we've talked about kind of one of the core ideas of Vim, the idea of modal editing, we can talk about some of the basics, 
like how do you open up this text editor, how do you open file save files, 
and things like that. 
And so this is a command-line based program, although there are some graphical variants, 
and the way you start this program is by running Vim. 
One thing you might notice is that in the bottom left corner of my screen, they actually saw what I just typed. 
This will be useful later in this lecture, where I'm actually typing in commands for Vim, 
and I'll be saying what I'm typing, 
but you'll also see it on the screen. 
So if I press Ctrl+C, you'll see it says Ctrl+C over there. 
Is that text big enough for everybody to read? Great, okay.v so the way we open vim is just by running the program vim on our command line, 
and this comes pre-installed on most systems. 
If you don't have it, you can install it using your package manager. 
Vim can also take an argument if we want to use it to edit a particular file instead of just opening up the program. 
For example, I have a file in this directory; this is actually the lecture notes for this lecture. 
So, I can do "vim editors MD" and press ENTER, 
and then boom, it starts. 
In this lecture, I'm not running Vim in the completely Exton. 
I've configured a couple of things that behave a little bit nicer by default, little things like having line numbers on the left 
or having some more status information on the bottom. 
If you want to start with this default configuration, we have a link to this in the lecture notes so you can get a slightly more sane config by default. 
So, once you've opened Vim, what do you do? Well, as I said earlier, Vim starts in normal mode. 
So, if I just start typing letters like, say, type "X," it doesn't insert "X" into the buffer. 
You can see the cursor up in the top left: it actually deleted one of the characters. 
That's because I'm in normal mode, not insert mode. 
So, insert mode is basically what you're used to with all the other text editors you've used in the past, where there's a cursor somewhere, you press the character, 
and it just goes into your buffer. 
In Vim, you start in normal mode, 
and you can press "I" to go into insert mode. 
So, I've pressed "I," and then in the bottom left, notice that it says "-- INSERT." The bottom left always tells you what mode you're in, 
unless this normal mode, in which case it's blank. 
Now that I'm in insert mode, if I press the "X" character, for example, it just gets inserted into my text buffer, 
and I can backspace over it, type other stuff, 
and now my text editor kind of behaves like you'd expect any other program to behave. 
From this point, how do I go back to normal mode if I want to stop inserting characters? Yes, exactly. 
I press "escape." And that's the symbol my keystroke visualizer uses for escape, 
so just be aware of that. 
Vim has this idea that using the mouse is inefficient. 
Like, your hands are on the keyboard, moving your hand over to the mouse takes a lot of time, right? 
You don't want to waste those couple of milliseconds while you're programming, like in the middle of things. 
So, instead, all Vim functionality can be accessed just through the keyboard. 
And it's all sorts of things you might be used to doing, like opening files by going, like, "file open," or "file save," 
or things like that, or instead accessed through the keyboard. 
How is that done? That's done through one of the other Vim modes that are on the board over there. 
In particular, through command line mode. 
So, if you're in normal mode and you press the ":" key, you'll notice that the cursor jumps to the bottom left and it shows that ":" I just typed. 
Now, I can type in a command. 
So, you can think of this almost like the command shell that we've talked been talking about over the last few days, except this is Vim's command shell, 
so you give Vim commands here, instead of shell commands. 
And there are a bunch of built-in commands that do all the things that you're used to. 
 Like, for example, one command that you might want to know is how to quit this editor. 
You might notice that if you're in normal mode, I can press "Escape" to go back from command line mode to normal mode, 
and I press "control C", unlike what happens to a lot of programs, this doesn't quit Vim. 
So, how do I quit Vim? I can press ":", 
and then go into command line mode, 
and then I can type in the command "quit". 
"Q-U-I-T". 
You'll see that I - maybe I should move this over to the middle or something - see, it says ":quit" and I press ENTER, 
and it quits Vim. 
I can open Vim up again. 
There's actually a short form for this command, just ":q", 
and that'll do the same thing. 
And, there are a bunch of other useful commands like this. 
So, 
some other handy ones to know are how do you save a file? So, suppose I make some edits here, like "hello world". 
So, I pressed "i" to go into insert mode - or let me redo that - I press "i" to go into insert mode. 
Right now, I can use the down arrow to... 
I think I've slightly I should fix that. 
Can you fix the config, actually, John? Never mind that. 
Okay, 
so, suppose I go down to this line, 
and I press "i" to go into insert mode, 
and type in some text, 
and then press "escape" to go back to normal mode. 
Now, how do I actually save this file? Well, there's another command for that. 
So, ":" to go into command mode, 
and then I can type "W"... 
...and press "Enter". 
"W" stands for write. 
And, it says in the bottom "editors.md" whatever blah blah written. 
And, 
so, this means it saved the file and so now if I ":q" to quit and open the same file, again, you'll see that the changes have been persisted. 
There are a couple other there's... 
So, there's a ton of different Vim commands that are useful for different reasons. 
But, I'll just explain a couple more to you now. 
One command that's really useful is help. 
":help" And you can do ":help", 
and then type in a particular key, or a particular command, 
and get help for that keystroke or that command. 
So, if I want to know what ":w" does, I can do : help : W, 
and that'll give me the documentation on : w or : write. 
If I do : q, it'll close this window and bring me back to where I was before. 
And, notice that ":help :w" is different from ":help w", because the W key is the W that, like, when you're in normal mode and press W, 
what happens is just the W key here without the ":". 
And, if I look for help for ":w", that's the help for the W command. 
So, now you basically have the bare fundamentals needed to use them. 
right? You can open the editor, use it to edit a particular file, press "i" to go into insert mode and type in some text, press "escape" to go back to normal mode, 
and then :w to save your changes, :w to quit. 
So, like already you have the bare fundamentals necessary to edit files using Vim, albeit somewhat inefficiently. 
So, any questions so far?  Yeah in the back. 
Yeah so the question is, What's the benefit of the normal mode? And, we'll talk about that in more detail, in like five minutes. 
But, in short, insert mode is just for typing in text. 
So, I'm in insert mode, I can type in text. 
But, when I'm programming, I actually spend a lot of time moving around my file making small little changes. 
So, I go here and like, oh maybe I want to change this HTTPS link to an HTTP. 
I can make like small point edits, things like that, in normal mode. 
And we'll see a whole lot more of that in about five minutes. 
Good question! Any other questions? Okay cool. 
So, moving along, one other thing that's kind of useful to know, I think, is, at a high level, Vim's model of buffers versus windows versus tabs. 
So, it's probably the case that whatever program you were using before, like Sublime Text or VS Code or whatever, you could open multiple files in it, right, 
and you could probably have multiple tabs open and have multiple windows open of your editor. 
So, Vim also has a notion of those different things. 
But, its model is a little bit different than most other programs. 
So, Vim maintains a set of open buffers - that's the word it uses for open files - and so, it has a set of open files, 
and then kind of separately from that, you can have a number of tabs, 
and tabs can have windows. 
The kind of weird thing which makes Vim a little bit different than the program you've probably used in the past is that 
there isn't necessarily a one-to-one correspondence between buffers and windows. 
So, one thing I can do, for example, here - and we'll show you the key combinations and stuff for this later - but one thing you can do is create two different windows. 
So, I have one window up here, 
and then one window down here. 
And, notice that the same files open in both windows. 
So, if I make some edits over here, they actually happen in the bottom window, as well, because it's the same buffer that's open in both windows. 
And, this is kind of useful, for, say, looking at two different parts of a single file at the same time. 
Like, 
so you want to be able to look at the top of a file, say at an import to your program, while you're down below, working somewhere else. 
So, this is one helpful thing to keep in mind, that Vim has this idea of - there are a number of tabs, 
and each tab has some number of windows, 
and then each window has, uh, corresponds to some buffer. 
But, a particular buffer can be open in zero or more windows at a time. 
Just one thing that confused me when I was initially learning Vim, 
so I want to explain that early on. 
And then, the ":q" command, which we talked about earlier, is not exactly quit. 
It's kind of "close the current window", 
and then, when there are no more open windows, Vim will quit. 
So, here, if I do ":q", it'll only close the window, I think, on the top here because that's the one I was in, 
and, now, the remaining window becomes fullscreen. 
I can do : Q again to close this. 
Now we're in the second tab that I'd opened. 
If I do :Q a final time, okay, now, Vim exits. 
And if you don't want to press ":q" way too many times... 
Okay, so, here I have three split windows. 
If I do ":qa", for quit all, it closes all the open windows. 
All right, so, now, to answer your question of "What is normal mode actually for?" 
This is another, really cool idea in Vim, 
and I think this is actually, like, the most fundamentally interesting idea of this program. 
It's that, like, you're all programmers, you like programming; Vim has this idea that Vim's normal mode, like, Vim's interface, itself, is a programming language. 
And, let me repeat that. 
That's like a kind of fundamentally interesting idea: the interface is a programming language. 
What does that mean? It means that different key combinations have different effects, 
and, once you learn the different effects, you can actually combine them together - just like in a programming language - you can learn different functions and stuff and then glue them all together to make an interesting program. 
In the same way, once you learn Vim's different movement and editing commands, 
and things like that, you can talk to Vim by programming Vim through its interface. 
And, once this becomes muscle memory, you can basically edit files at the speed at which you think. 
Like at least for me, I don't think I've been able to do this with any other text editor that I've used in the past, 
but this one gets pretty close. 
So, let's dig into how exactly normal mode works. 
So, you can try to follow along with this, like, open up some random file in Vim, 
and follow some of the key combinations I type in. 
So, one basic thing that you might want to do, is just navigate around a buffer. 
Like, move your cursor up/down/ left/right. 
And, 
so the way you do that in Vim, is using the hjkl keys, not the arrow keys. 
Though they do work by default, try to avoid them, because you don't want to have to move your hand all the way over to the arrow keys. 
Like, there's a ton of time you're wasting, right? HJKL is right on the home row. 
And, so, J moves down, K moves up, H moves left, 
and L moves right. 
And, this may seem a little unintuitive now; there was some historical reason for it, like, the keyboard the original vi developer used had the hjkl keys, like, labeled, 
and arranged in a way that made this more reasonable. 
But, this will very soon become muscle memory. 
So, this is the basic way you can move your cursor around while in normal mode. 
Now, what else can you do? Well, if we had to move around files like this, it'd be really slow. 
We don't want to have to hold down these keys, 
and like, wait for a long time for Vim to do its thing. 
And so, there are all these other, different key combinations for doing different movements. 
Also, by the way, this is all in the lecture notes, 
so you don't need to memorize every single key and its meaning right now. 
Just try to understand the overall idea that Vim's interface is a programming language. 
So, another thing you can do is press the W key. 
This moves the cursor forward by one word. 
And then, similarly, the "B" key moves the cursor backward by one word. 
So, this allows slightly more efficient movement within the line. 
There's also the "E" key for moving to the end of a word. 
I'm going to move this over a little bit. 
So, if I'm here, for example, 
and I press the "E" key it'll go to the end of this word, end of this word, end of the next word and so on. 
You can also move by whole lines, 
so zero moves to the beginning of a line, dollar sign moves to the end of a line, 
and the caret key moves to the first non-empty character on a line. 
So, let me find one of those, for example. 
So, here, my cursor's right here; if I press 0, my cursor goes to the beginning of the line, dollar sign, end of the current line; 
and if I press caret, where, like, on what character will the curser end up? 
Can anybody guess? So, caret goes to the first non-empty character on a line, kind of like Regex caret. 
Yeah, exactly! It goes to this dash. 
Let's talk about some more movement commands. 
There're ways to scroll up and down in a buffer, 
so control U goes up, 
and control D scrolls down. 
So, this is better than holding down the K or J keys, for example. 
This is a lot slower than moving by entire pages. 
Control D and control U. 
There's also ways to move by the entire buffer. 
So, capital "G" moves all the way down... 
"gg" moves all the way up. 
Some of these movement keys are mnemonics; so, they're like, a little bit easier to remember for that reason, 
right, like, "W" is word, "B" is beginning of word, E is end of word. 
Those all seem pretty logical. 
0, caret and dollar, kind of inspired from Regex, 
so those make a little bit of sense. 
There's some other ones that, like, don't necessarily make way too much sense, 
but, there are only so many keys on your keyboard, 
so what are you going to do? For example, the "L" key moves your cursor to the lowest line that's shown on the screen. 
"L" for lowest makes sense, M for middle, 
and then H for highest, I guess. 
And, there's a whole bunch of other interesting movements like this. 
So, we're obviously not going to be able to cover all of them right now, 
but you'll be able to go through them in the Vim tutor exercise, which is exercise number one for this lecture. 
Some other ones I want to talk about now - maybe I'll talk about one more. 
There's another movement called "find". 
This is also kind of useful. 
Suppose I'm on this line, 
and I want to jump to the first character that equal's to... 
Like, I want to jump to the first "o". 
I can press "fo", 
and my cursor moves to the first "o". 
I've like, found "o". 
I can do fw and it'll move to the first "w", which I think is right here. 
fc: find the first C. 
I can also do the same thing, 
but backwards. 
If I do capital F, w, I can find the W that's before it. 
Capital F, s: find the s that's before that. 
And then, there's a variant of f, for find: t for to, 
so I can jump to O, 
and it jumps, like, until it's found o. 
But not on top of it, right before it. 
And capital T say, t, jumps backwards to the t except not all the way on top of it, one character before. 
And, 
so, you can already see that idea I talked about, of like, Vim is a programming language; you can, like, compose these commands. 
"F", 
and "T", are "find", 
and "to", 
and you can say "find" a particular character, or jump "to" a particular character. 
So, those are a couple of Vim movement commands. 
So, any questions about those so far? So this is - yeah, question? Or... 
no? Okay, cool. 
So, those are Vim movement commands. 
This is how you can navigate around a file quickly in normal mode. 
Now, another category of useful commands are editing commands. 
So, one we kind of already talked about is the "i" command for moving from "normal" mode to "insert" mode, where you can start just writing text. 
So, suppose I go up here and I press "i". 
Now I can type in whatever text I want "Hello world", "enter". 
Then, press "escape" to go back to normal mode, 
and I've made a change to my buffer. 
But, there are a whole bunch of other commands for making efficient edits that makes sense for when you're dealing with programming languages. 
So, one useful command that I accidentally used earlier, before teaching you about it, is the "o" command. 
So, suppose my cursor is, like, over here, 
and if I press "o", from normal mode, what it does, is it opens a new line below where my cursor is. 
That's what "o" stands for. 
And it, 
so it creates a new line, 
and it put me into insert mode. 
So, now I can start typing in some text, press escape, 
and go back to normal mode. 
And then, just like the "o" command, there's a capital "O" command, 
so if I'm here and I do capital "O", it puts me into insert mode above where I currently am. 
There's another Vim command for deleting things. 
So, suppose my cursor is, like, on top of this word right here, 
and I press the D key. 
"D" for delete. 
Oh, nothing happens; turns out that the D key needs to be combined with a movement command. 
So, remember we just talked about different movement commands, like hjkl, 
and, like, word, 
and backward word, 
and things like that. 
So, I press D. 
Whoops. 
I press D and I can press W, 
and it's deleted a word. 
So, let me undo that. 
Undoing in Vim is just u for undo. 
So, notice my cursor's right here. 
I do "dw": it's deleted a word. 
I can move around, 
and then delete another word. 
Suppose I'm - uh, keeps getting in the way Suppose I'm, like, 
somewhere in the middle of a word, 
and I want to delete to the end of a word. 
Any guesses for what combination of keys I'd use for that? "d" and what? de, exactly. 
Delete to the end of the word. 
Another useful editing command is the c command. 
c stands for change. 
So, change is really similar to delete, except change puts you in insert mode, because, like, I want to delete a thing, 
but change it to something else. 
So, if I'm here, 
and I do "ce", it's like, change to the end of the word. 
And, it gets rid of the contents until the end of the word, 
and notice it put me in insert mode. 
So now, whatever characters I type go into the buffer. 
If I press "escape", I go back into normal mode. 
And so, c and d are analogs: they both take motions as arguments. 
And, they will either delete that motion, or change that motion. 
So, for example, if you press the c key, there's also this pattern that, if you press a particular editing key twice, it'll have that effect on the given line. 
So, if I press "dd", that deletes the line. 
If I press "cc", that deletes the given line, 
but puts me in insert mode, 
so I can replace it with some other line. 
We'll cover a couple other, uh, editing commands, because then later we'll see how all these things interact together. 
So, another useful one is the x command. 
So, suppose my cursor is over some particular character. 
If I press "x", it just deletes that character. 
There's another command called r. 
If I'm over a particular character, 
and I press "r", it takes another character as an argument, 
and it replaces that particular character with some other character. 
And, I'll cover a couple more editing commands. 
So, I think one I talked about a moment ago - but, of course you can undo changes you've made in Vim. 
And the way you do that is by pressing "u" while you're in normal mode. 
So, u for undo is pretty easy to remember. 
So, I press "u" a whole bunch of times; it's undone all the changes I've made. 
And then, the opposite of undo is, of course, redo. 
And, the binding for that in Vim is control + R. 
All right, one other editing command I'm going to talk about is copy and paste because-oh yes, question? That's a - that's a great question! 
So, the question is, "Does 'undo' undo everything you've done since you've gone into insert mode, or just the last character?" 
It's - it's actually a little bit more complicated than that. 
"Undo" does, like, undoes the last change you've made. 
So, if you went into insert mode, 
and typed in some stuff, 
and went back into normal mode, 
and then press "u" for "undo", it'll undo all you've done in insert mode. 
But, if you've done some other type of editing command, like, say I press "x" to delete a character... 
If I do "u" for undo, it'll just undo that change that that editing command made. 
Now, does that answer the question? (Yeah) Great any other questions? Cool. 
So, I'll talk about copy and paste as well, because that's a popular one. 
The y command stands for copying, 
and the p command stands for pasting. 
y for "copy", because, yank. 
Like, that's the word they- that's the terminology that Vim uses for copying. 
And, these commands are- y also takes a motion as an argument. 
So if I do like, yy, it copies the current line. 
And, if I press "p" for "paste", notice that now these two lines are identical, because I've just pasted a line below. 
"u" for "undo". 
But if I do something like "yw", it's copied the word. 
And then I can do "p", 
and it just pasted that word again, right where my cursor was. 
One useful thing, especially in the context of copy and paste, is to be able to select a block of stuff and copy it, right? 
Like, this is probably how you used copy and paste in whatever editor you were using before. 
And so, that's where we get into the visual modes. 
So, these are another set of modes that are all related to each other, 
and that can be reached from normal mode, 
and they're used for selecting chunks of text. 
So, one mode is, just, regular visual mode. 
You can enter that by pressing v. 
And then, once you're in this mode, you can use most of the regular normal mode commands to move your pointer around. 
And it selects everything in between. 
So I can use, like, hjkl just to move the cursor, or I can use "w" to move by words, or different things like that, 
and it will select a block of text. 
And, once I've selected this block of text there are a whole bunch of different types of useful things you could do with it. 
One of the most popular things to do is copying this. 
So, once I've selected, I can do y to copy, 
and it puts me back into normal mode. 
And now, it's copied this to the - to the paste buffer. 
And then if I go somewhere else, 
and press "p", it pastes in that whole chunk of text I copied. 
And it's similar to visual mode, which selects kind of a contiguous stream of text. 
There's visual line mode: that can be reached by pressing capital V, 
and that selects whole lines at a time. 
And then there's VISUAL BLOCK mode, which can be selected by pressing "control" + " V", 
and that can select rectangular blocks of text. 
So this is something your old editor couldn't do. 
Alright, 
so, there's a lot more Vim editing commands to learn. 
There's lots of, like, really weird and fancy things. 
Like, for example, the tilde command changes the case of the character, or the selection that you've currently selected. 
So for example, I can take this, like, Visual Studio Code, 
and flip the case on the whole thing, by selecting it and pressing tilde. 
And, there's a whole bunch of other things like that; they get more and more esoteric as you go. 
So, we're not going to cover all of those, 
but you'll get to those in the exercises. 
So, those are Vim editing commands, 
and a lot of them can be composed with movement commands. 
So, any questions about either of those so far? Cool. 
So, moving along, another category of things -of commands- that are mostly relevant to normal mode are counts. 
So, you can give them a number, to do a particular thing, 
some number of times. 
So suppose my cursor is here, 
and I want to move down, like 1 2 3 4 lines. 
One way I can do that is by pressing "j" four times - go down four times. 
"kkkk" goes up four times. 
But, rather than pressing a particular key again, 
and again, I can use a count. 
So if I press "4", "j", it does j four times, right? Vim's interface is a programming language. 
If I do "4k", it moves up four times. 
If I am here, 
and I press "v" to go into visual mode... 
Okay so now I can move my cursor around, 
and select blocks of text. 
I can do, like, "eee" to select a couple of words, 
but, I could also go back here -v for visual mode- and press three e to select, like, three "ends of words" forward. 
And then of course these can also be combined with editing commands. 
So, like, suppose I want to delete seven words. 
I can do that by moving my cursor somewhere, 
and doing "7dw". 
Seven delete words. 
And so, this is particularly useful for things like, suppose my cursor is somewhere on the screen, 
and I'm looking somewhere else on the screen, or, I want my cursor to go to that particular line. 
Notice that I've set up relative line numbering on the left. 
So, wherever my cursor is, it shows the current line number, 
but everywhere else, it's just the offset from where I am. 
Now, suppose my cursor is here, 
but I want to move down to the like "Microsoft Word" down here, 
so that's eight lines down. 
So, what combination of keys would I press, to do that? Like, what's the most efficient way? Yeah, exactly! 
Let's try that out-8j-and my cursor moved down to this line. 
Okay. 
And then, one, final category of key meanings in Vim is something called modifiers. 
So we have, 
so far, movement, edits, counts, 
and, finally, we have modifiers. 
So, modifiers kind of change the meaning of a movement command a little bit. 
And, a couple modifiers that are especially useful are the "a" and "i" modifier. 
So, a stands for like around and "i" stands for inside. 
And, to see where this is really useful, I can move my cursor to somewhere like here, for example. 
So, hopefully, most of you are familiar with markdown syntax - and if not it doesn't matter too much. 
Uh, this is a link in markdown; it's a text rendered in square brackets, 
and then the link in parentheses. 
Suppose my cursor is inside here, 
and I want to change the text corresponding to this link. 
Well, one way I could do that is, like, move back here with b, 
and, like, 2dw, 
and then "i" to go into insert mode. 
That's one of the many ways I can make this change, 
and I can type in whatever other thing I want - u to undo, u to undo. 
Another way I could have done that is change two words - "c2w" - and then type in some other text. 
But, one final way I could do the same change is using the modifier commands to talk about 
how I want to interact with these different types of grouping things like parentheses and square brackets. 
So, one final way of doing this is change inside square brackets-"c" "i" "["-and that puts me into insert mode, after deleting the contents that are inside the brackets. 
So, do you see how we can take all these different ingredients, like we talked about "change", 
and we could combine that with different movement commands. 
We talked about inside, how it's a modifier. 
And then we talked about, uh... 
we didn't talk about parentheses. 
But, if your cursor is hovering over a different, uh, different types of grouping things like parentheses, or square brackets, 
you can press the percent movement key to jump back and forth between matching parentheses. 
If I go over here and I do d, i, (, I can delete the contents inside these parentheses. 
And so, those are Vim, uh, modifiers. 
I guess we talked about i, 
but we didn't talk about a. 
If I do "da(", it deletes a whole, like parenthesized group including the parentheses 
so I is inside is around or including all right so those are basically the different categories of things you can combine together, when interacting with Vim's interface. 
So, any questions about that or the overall idea of this interface being a programming language? Cool. 
So, let's do a quick demo, to kind of demonstrate the power of this editor. 
And, it will kind of help us see how this tool can work really fast and kind of match the speed at which we think. 
So, over here is a very broken "fizzbuzz" implementation that doesn't actually print anything. 
Uh, hopefully, most of you have heard of "fizzbuzz" - if not, I'll explain it super briefly. 
Uh, "fizzbuzz" is a programming exercise where you print the numbers 1 through n, 
but when the number is divisible by 3, you print fizz - when it's divisible by 5, you print buzz. 
And, when it's divisible by both 3, 
and 5. 
you print fizzbuzz. 
And, if none of those apply, you just print the number. 
So, you should print like 1, 2, fizz, 4, buzz, 
and so on. 
But, if I run this program, it doesn't print anything. 
Here, I have them on the left, in just a terminal. 
On the right, okay, 
so there's a bunch of issues with this. 
One is that main is never called, 
so let's start off with fixing that. 
So, here's how I would make this change and notice how few keystrokes this requires: capital G means go to the bottom of the file, o opens a new line below, 
and now I can just type in stuff. 
So, I'm in insert mode. 
Okay, 
so I've typed in whatever change I want to make, escape to go back to normal mode. 
If I do : W (command mode, right), let me go back here. 
Okay, now at least my program prints something when I run it. 
Another issue with this program is that it starts at 0 instead of 1, 
so let's go fix that. 
So, I want to go over to this range (whoops, this range thing), 
and it shouldn't be going from 0 to limit, it should be going from 1 to limit plus 1. 
One command which I didn't show you about is how you search in vim, 
so you press forward slash (/) to close this and restart it. 
If you press forward slash, it starts search, 
so if I type in "range" (enter), my cursor goes from wherever it was before to the first instance of "range" it found. 
So, it's a really efficient way of moving where I want to move. 
WW to move forward two words, I to go into insert mode, add the "1, " (comma space), escape, I'm back in normal mode. 
This is a very common pattern in vim: you stay in normal mode, you go somewhere, you go into insert mode, you make a tiny change, 
and you jump right back to normal mode like normal mode is home, 
and that's where you should be most of the time. 
I also want to add a "+1," so e to go to the end of this word, a for a pend, "+1," escape. 
Alright, fix that problem. 
Another issue is that this program prints "fizz" for both divisible by three and five, 
so let's fix that. 
Slash fizz searches for "fizz", been oppressed, 
and it goes to the next match. 
See, I press E, I quote, changes what's inside the quote, 
so it's deleted the "fizz" and put me in insert mode right in between those two quotes, 
and I can type in whatever I want, escape to go back to normal mode. 
So, great, I've fixed that particular problem. 
Another problem with this program is that it prints "fizz" and "Buzz" on separate lines for multiples of 15, 
so let's go and fix that. 
Let me go down to this line here. 
One way I can (don't actually worry about like the actual contents of this program, 
like this some stupid program that doesn't matter) pay attention to what keys I'm pressing in vim that allow me to make changes to this program really efficiently. 
So, my cursor is on this line. 
I press dollar to go to the end of this line, i for insert mode, okay, 
and I'm typing some stuff, escape to go back to normal mode. 
Now, I want to make the same change the print below. 
Look at this. 
JJ dot, 
so what dot does in vim is it repeats the previous editing command that was made, 
and so this is a really nice way of doing repetitive tasks without typing the same thing over and over again. 
So in that particular case, that inserted comma end quote and so it applied the same thing on this line when I press dot. 
And then when I guess one final part of this demo is we will fix the issue that 
this program maybe should take a command-line argument instead of having this hard-coded 10 down here. 
So how do we do that? I'll press GG to go to the top, capital O, 
so now I've opened a line above and I'm going to type in some text like imports this, enter, escape to go back to normal mode, 
and then I want to go down to where this 10 is so /10 makes me jump straight down there. 
CI pren to edit what's inside the parentheses 
and now I can type in like whatever thing I need to type in here and then once I've done this, my program does fizzbuzz correctly. 
I think I missed one change I wanted to make, 
but it doesn't matter. 
This demonstrates that you can make lots of changes really fast. 
So any questions about this demo or the overall idea we've been talking about? Okay, 
so this will be covered Tuesday, 
so the kind of outside environment I'm running vim on the left and my shell on the right, 
and then this is team ox on the outside. 
One variant of that question might be like how do you switch between different vim windows and you can see the lecture notes for that, 
but there's a key binding for that. 
So if you have the same window open or multiple things open, there's a way of doing that. 
Question: ah, good question. 
So delete takes a motion and then removes those contents but keeps you in normal mode, 
so you can keep just moving around in a file. 
What change does is very similar to delete, it takes motions and treats them in the same way, deletes those contents, 
but then puts you in insert mode, 
and so it saves you from typing one extra keystroke. 
So if I'm here, for example, I want to delete main, DW deletes a word, 
but now if I press whatever key likes I press J, it just moved me down. 
If I undo that, do cw4 change a word, now it's actually put me into insert mode, 
and I can type in whatever I want it to insert. 
So DWI is the same thing as CW, 
but it saves a keystroke. 
One thing we've linked in the resources is something called vim golf. 
Basically, people have set up a game online where you can get an editing task and try to figure out the minimal number of keystrokes necessary to complete that editing. 
It's actually really addictive. 
So I'd only suggest going on their chest and script time. 
I think I saw a hand for another question. 
Yeah, uh, period. 
Yeah, one of the most useful of vim commands. 
Good question. 
Any other questions? Cool. 
So I think we have about five minutes left, 
and I'm gonna briefly talk about a thing that's also covered in detail in the notes, 
so make sure you look at the notes for this. 
Vim is a programmer's text editor, 
and so of course, it's highly programmable. 
Not only through its interface that's a programming language but also a couple of different ways. 
There are lots of settings that you can tweak to match your preferences, 
and you can also install plugins for them that do all sorts of useful stuff. 
So the way vim is configured is through a file on disk called vim RC, 
and you'll see this is a common pattern in a lot of shell-based tools. 
There'll be a plain text file that configures how the Tool Works, 
and so if I edit this file and it may or may not exist on your machine yet, 
but I've downloaded the, we've created a kind of default vim RC for you and linked it on the course website, 
so you can start with that one. 
If I do vim tilde slash boom RC, I can see here a bunch of comments and then particular commands like by default, we want syntax highlighting on, 
and we want line numbers. 
If we didn't do some of these things like let me remove the stuff that sets line numbers, 
if I remove those configurations and relaunch vim, notice that I no longer have line numbers on the left. 
But yeah, 
so in short, there's a lot of stuff you can configure with Vim. 
We've given you a very basic configuration that tries to remove some of the kind of weird behavior that's on by default in Vim, 
but we don't really try to enforce too many of our other opinions on you. 
But of course, like the three of us used Vim a lot and we have heavily customized VimRCs. 
So we've linked to our personal configurations too if you want to take anything from that, 
and also, like thousands or millions of people share their VimRCs on Github, 
so there's lots of places to look for inspiration. 
There are also cool blog posts on this topic. 
Another thing you can do in Vim is you can extend it with plugins that do all sorts of useful things. 
This lets you do things like fuzzy file finding, which a lot of text editors come with by default, 
so you can get like a pop-up window, you can type in a name of a file or approximately the name of a file and find it very quickly, or there are things that show you like visualizations of undo history. 
There are things that show you like file explorers, things like that. 
So we've linked to a couple of our favorite plugins on the course website, 
and so I highly recommend becoming familiar with how to install a plugin because it takes like three seconds and some of them are really cool. 
And then finally, the last topic I'll briefly mention before we finish today's lecture is vim mode and other programs. 
So turns out that a lot of programmers were really excited about Vim's interface, 
and so they've implemented similar functionality in other tools. 
For example, like I've configured my Python REPL to run in Vim mode. 
So I can type in stuff here, 
and if I press escape, now I'm in normal mode in my Python REPL, 
and I can move back and forth and like press X here to delete a thing, like CW change a word and do all those good things. 
And it's not just the Python REPL. 
Like, I have my terminal behaving this way too. 
So like, I can type in whatever I want here and escape, 
and I'm in normal mode. 
I can go here and like go into visual mode inside my terminal and like select blocks of text, press tilde to change the case, whatever. 
So we've linked to how exactly you can enable Vim mode for like bash, zsh, fish, a lot of readline-based programs like Jupyter Notebook, a whole bunch of other things. 
And if it's not another place, you can probably find it by googling it because a lot of people like to have this sort of functionality. 
And if you're really gonna commit to learning Vim, I think it's valuable to enable this sort of Vim emulation mode in every tool you use. 
It's like one, or like make you learn the tool a lot better, 
and two, once you become good at Vim, like those skills will now transfer to all your other tools you use. 
Okay, so I think that's it for our rapid introduction to Vim. 
There's some other neat material that we weren't able to fit in today's lecture, 
but it's in the lecture notes. 
And then finally, I highly recommend going through the exercises for today. 
Like, at least for me personally, I think spending time learning my text editor has been like the most beneficial thing out of the kinds of things we're teaching in this class. 
So yeah, that's it for today's lecture, 
and we'll see you tomorrow. 
Note that we've changed tomorrow's lecture to data wrangling. 
Thursday and Tuesday lectures are now switched. 
This is reflected on the course website in case anybody was going to come to one but not the other. 
