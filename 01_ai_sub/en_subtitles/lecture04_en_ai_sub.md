All right, so welcome to today's lecture which is going to be on data wrangling. 
And data wrangling might be a phrase that sounds a little bit odd to you, 
but the basic idea of data wrangling is that you have data in one format and you want it in some different format, 
and this happens all the time. 
I'm not just talking about like converting images, 
but it could be like you have a text file or a log file and what you really want this data in some other format. 
Like you want a graph or you want statistics over the data. 
Anything that goes from one piece of data to another representation of that data is what I would call data wrangling. 
We've seen some examples of this kind of data wrangling already previously in the semester, 
like basically whenever you use the pipe operator that lets you sort of take output from one program and feed it through another program, 
you are doing data wrangling in one way or another. 
But what we're going to do in this lecture is take a look at some of the fancier ways you can do data wrangling and some of the really useful ways you can do data wrangling. 
In order to do any kind of data wrangling, though, you need a data source. 
You need some data to operate on in the first place, 
and there are a lot of good candidates for that kind of data. 
We give some examples in the exercise section for today's lecture notes. 
In this particular one, though, I'm going to be using a system log. 
So, I have a server that's running somewhere in the Netherlands because that seemed like a reasonable thing at the time, 
and on that server, it's running sort of a regular logging daemon that comes with SystemD. 
It's a sort of relatively standard Linux logging mechanism, 
and there's a command called journal CTL on Linux systems that will let you view the system log. 
And so what I'm gonna do is I'm gonna do some transformations over that log and see if we can extract something interesting from it. 
You'll see, though, that if I run this command, I end up with a lot of data because this is a log that has just like there's a lot of stuff in it, right? 
A lot of things have happened on my server, 
and this goes back to like January first, 
and there are logs that go even further back on this. 
There's a lot of stuff. 
So the first thing we're gonna do is try to limit it down to only one piece of content. 
And here the grep command is your friend. 
So we're gonna pipe this through grep, 
and we're gonna pipe for SSH, right? 
So SSH we haven't really talked to you about yet, 
but it is a way to access computers remotely through the command line. 
And in particular, what happens when you put a server on the public Internet is that 
lots and lots of people around the world try to connect to it and log in and take over your server. 
And so I want to see how those people are trying to do that, 
and so I'm going to grep for SSH, 
and you'll see pretty quickly that this also generates a bunch of content. 
At least in theory, this is gonna be real slow. 
There we go. 
So this generates tons and tons and tons of content, 
and it's really hard to even just visualize what's going on here. 
So let's look at only what user names people have used to try to log into my server. 
So you'll see some of these lines say disconnected, disconnected from invalid user, 
And then, some user name. 
I want only those lines, that's all I really care about. 
I'm gonna make one more change here though, which is if you think about how this pipeline does, 
if I here do this connected from, so this pipeline at the bottom here, 
what that will do is it will send the entire log file over the network to my machine 
and then locally run grep to find only the lines to contained ssh and then locally filter them further. 
This seems a little bit wasteful because I don't care about most of these lines, 
and the remote site is also running a shell, so what I can actually do is I can have that entire command run on the server. 
Right, so I'm telling you, SSH, the command I want you to run on the server is this pipeline of three things, 
and then what I get back, I want to pipe through less. 
So, what does this do? Well, it's gonna do that same filtering that we did, 
but it's gonna do it on the server side, 
and the server is only going to send me those lines that I care about. 
And then when I pipe it locally through the program called less, less is a pager. 
You'll see some examples of this, you've actually seen some of them already, like when you type man and some command that opens in a pager, 
and a pager is a convenient way to take a long piece of content and fit it into your term window 
and have you scrolled down and scroll up and navigate it so that it doesn't just like scroll past your screen. 
And so, if I run this, it still takes a little while because it has to parse through a lot of log files, 
and in particular, grep is buffering and therefore it decides to be relatively unhelpful. 
I may do this without, let's see if that's more helpful. 
Why doesn't it want to be helpful to me? Fine, I'm gonna cheat a little, just ignore me. 
Or the internet is really slow, those are two possible options. 
Luckily, there's a fix for that because previously I have run the following command. 
So this command just takes the output of that command and sticks it into a file locally on my computer. 
Alright, so I ran this when I was up in my office, 
and so what this did is it downloaded all of the SSH log entries that matched disconnect from, so I have those locally, 
and this is really handy, right? There's no reason for me to stream the full log every single time because I know that that starting pattern is what I'm going to want anyway. 
So we can take a look at SSH dot log, 
and you will see there are lots and lots and lots of lines that all say disconnected from, invalid user, authenticating users, etc., right? 
So these are the lines that we have to work on, 
and this also means that going forward, we don't have to go through this whole SSH process. 
We can just cat that file and then operate it on it directly. 
So here I can also demonstrate this pager, so if I do cat s is a cat SSH dot log and I pipe it through less, it gives me a pager where I can scroll up and down. 
Make that a little bit smaller maybe so I can scroll this file screw through this file,
And I can do so with what are roughly Vim bindings. 
So, control you to scroll up, control D to scroll down, 
and cue to exit. 
This is still a lot of content though, 
and these lines contain a bunch of garbage that I'm not really interested in. 
What I really want to see is what are these user names. 
And here, the tool that we're going to start using is one called sed. 
Sed is a stream editor that modifies or is a modification of a much earlier program called edie, which was a really weird editor that none of you will probably want to use. 
Yeah, Oh tsp is the name of the remote computer I'm connecting to. 
So, sed is a stream editor, 
and it basically lets you make changes to the contents of a stream. 
You can think of it a little bit like doing replacements, 
but it's actually a full programming language over the stream. 
One of the most common things you do with sed though is to just run replacement expressions on an input stream. 
What do these look like? Well, let me show you. 
Here, I'm gonna pipe this to sed, 
and I'm going to say that I want to remove everything that comes before 'disconnected from.' This might look a little weird. 
The observation is that the date and the hostname and the sort of process ID of the SSH daemon, I don't care about. 
I can just remove that straightaway, 
and I can also remove that 'disconnected from' bit because that seems to be present in every single log entry. 
So, what I write is a sed expression. 
In this particular case, it's an S expression, which is a substitute expression. 
It takes two arguments that are basically enclosed in these slashes. 
So, the first one is the search string, 
and the second one, which is currently empty, is a replacement string. 
So, here, I'm saying search for the following pattern and replace it with blank, 
and then I'm gonna pipe it into less at the end. 
Do you see that? Now, what it's done is trim off the beginning of all these lines, 
and that seems really handy. 
But you might wonder, what is this pattern that I've built up here, right? This is this dot star. 
What does that mean? This is an example of a regular expression. 
And regular expressions are something that you may have come across in programming in the past, 
but it's something that once you go into the command line, you will find yourself using a lot, especially for this kind of data wrangling. 
Regular expressions are essentially a powerful way to match text. 
You can use it for other things than text too, 
but text is the most common example. 
And in regular expressions, you have a number of special characters that say don't just match this character, 
but match, for example, a particular type of character or a particular set of options. 
It essentially generates a program for you that searches the given text. 
Dot, for example, means any single character, 
and star, 
if you follow a character with a star, it means zero or more of that character. 
And so, 
So in this case, this pattern is saying zero or more of any character followed by the literal string 'disconnected from'. 
I'm saying match that and then replace it with blank. 
Regular expressions have a number of these kinds of special characters that have various meanings. 
You can take advantage of them. 
I talked about star, which is zero or more. 
There's also Plus, which is one or more. 
This is saying I want the previous expression to match at least once. 
You also have square brackets. 
Square brackets let you match one of many different characters. 
So here, let's build up a string list something like AB, 
and I want to substitute A and B with nothing. 
Okay, so here, what I'm telling the pattern to do is to replace any character that is either A or B with nothing. 
So if I make the first character B, it will still produce BA. 
You might wonder though why did it only replace once? 
Well, it's because what regular expressions will do, especially in this default mode, 
is they will just match the pattern once and then apply the replacement once per line. 
That is what sed normally does. 
You can provide the G modifier which says do this as many times as it keeps matching, 
which in this case would erase the entire line because every single character is either an A or a B. 
If I added a C here and removed everything but the C, 
if I added other characters in the middle of this string somewhere, they would all be preserved but anything that is an A or a B is removed. 
You can also do things like add modifiers to this. 
For example, what would this do? This is saying I want zero or more of the string AB, 
and I'm gonna replace them with nothing. 
This means that if I have a standalone A, it will not be replaced. 
If I have a standalone B, it will not be replaced, 
but if I have the string AB, it will be removed, which, yeah, what are they? sed is stupid. 
The -a here is because sed is a really old tool, 
and so it supports only a very old version of regular expressions. 
Generally, you will want to run it with -E (capital E), which makes it use a more modern syntax that supports more things. 
If you are in a place where you can't, you have to prefix these with backslashes to say 'I want the special meaning of parenthesis,' 
otherwise, they will just match a literal parenthesis, which is probably not what you want. 
So notice how this replaced the AB here and it replaced the AB here, 
but it left this C, 
and it also left the A at the end because that A does not match this pattern anymore. 
And you can group these patterns in whatever ways you want. 
You also have things like alternations. 
You can say anything that matches AB or BC, I want to remove. 
And here you'll notice that this AB got removed. 
This BC did not get removed, even though it matches the pattern, 
because the AB had already been removed. 
This AB is removed right, 
but the C stays in place.
This a b is removed and this c states because it still does not match. 
That if I made this, 
if I remove this a, then now this a B pattern will not match this B, so it'll be preserved, 
and then BC will match BC and it'll go away. 
Regular expressions can be all sorts of complicated when you first encounter them, 
and even once you get more experience with them, they can be daunting to look at. 
And this is why very often you want to use something like a regular expression debugger, which we'll look at in a little bit. 
But first, let's try to make up a pattern that will match the logs and match the logs that we've been working with so far. 
So here, I'm gonna just sort of extract a couple of lines from this file, let's say the first five. 
So these lines all now look like this, right? And what we want to do is we want to only have the user name. 
Okay, so what might this look like? Well, here's one thing we could try to do. 
Actually, let me show you one, except one thing first. 
Let me take a line that says something like 'disconnected from invalid user disconnected from maybe four to one one whatever.' 
Okay, so this is an example of a login line where someone tried to login with the username 'disconnected from missing an S.' 
You'll notice that this actually removed the username as well, 
and this is because when you use dot star and any of these sort of range expressions, indirect expressions, they are greedy. 
They will match as much as they can. 
So in this case, this was the username that we wanted to retain, 
but this pattern actually matched all the way up until the second occurrence of it or the last occurrence of it, 
and so everything before it, including the username itself, got removed. 
And so we need to come up with a slightly clever or matching strategy than just saying sort of dot star because it means that if we have particularly adversarial input, 
we might end up with something that we didn't expect. 
Okay, so let's see how we might try to match these lines. 
Let's just do a head-first. 
Well, let's try to construct this up from the beginning. 
We first of all know that we want a dash capital E, right? Because we want to not have to put all these backslashes everywhere. 
These lines look like they say 'from,' and then some of them say 'invalid,' but some of them do not, right? This line has 'invalid,' that one does not. 
Question mark here is saying zero or one, so I want zero or zero or one of 'invalid space user.' What else? Well, that's going to be a double space, so we can't have that. 
And then there's gonna be some username, 
and then there's gonna be what exactly is gonna be what looks like an IP address.
So here we can use our range syntax and say zero to nine and a dot, right? That's what IP addresses are, 
and we want many of those. 
Then it says "port," so we're just going to match a literal port and then another number zero to nine, 
and we're going to wand plus of that. 
The other thing we're going to do here is we're going to do what's known as anchoring the regular expression. 
So, there are two special characters in regular expressions: there's carrot or hat, which matches the beginning of a line, 
and there's dollar, which matches the end of a line. 
So, here we're going to say that this regular expression has to match the complete line. 
The reason we do this is because imagine that someone made their username the entire log string. 
Then, 
if you try to match this pattern, it would match the username itself, which is not what we want. 
Generally, you will want to try to anchor your patterns wherever you can to avoid those kind of oddities. 
Okay, let's see what that gave us. 
That removed many of the lines but not all of them. 
So, this one, for example, includes this "pre-off" at the end, so we'll want to cut that off. 
If there's a space, "pre-off," square brackets are specials, we need to escape them, right? Now, let's see what happens if we try more lines of this. 
No, it still gets something weird. 
Some of these lines are not empty, right, which means that the pattern did not match. 
This one, for example, says "authenticating user" instead of "invalid user." 
Okay, so as to match "invalid" or "authenticated" zero or one time before "user," how about now? 
Okay, that looks pretty promising, 
but this output is not particularly helpful, right? Here we've just erased every line of our log files successfully, which is not very helpful. 
Instead, what we really wanted to do is when we match the username, right over here, 
we really wanted to remember what that username was because that is what we want to print out. 
And the way we can do that in regular expressions is using something like capture groups. 
So, capture groups are a way to say that I want to remember this value and reuse it later, 
and in regular expressions, any bracketed expression, any parenthesis expression, is going to be such a capture group. 
So, we already actually have one here, which is this first group, 
and now we're creating a second one here. 
Notice that these parentheses don't do anything to the matching, right, 
because they're just saying this expression as a unit, 
but we don't have any modifiers after it, so it's just match one-time. 
And the reason matching groups are useful or capture groups are useful is because you can refer back to them in the replacement. 
So, in the replacement here, I can say backslash two. 
This is the way that you refer to the name of a capture group. 
In this case, I'm saying match the entire line, 
and then in the replacement, put in the value you captured in the second capture group. 
Remember, this is the first capture group, 
and this is the second one.
And this gives me all the usernames. 
Now, 
if you look back at what we wrote, this is pretty complicated, right? It might make sense now that we walk through it and why it had to be the way it was, 
but this is not obvious that this is how these lines work. 
And this is where a regular expression debugger can come in really, really handy. 
So we have one here, there are many online, 
but here I've sort of pre-filled in this expression that we just used. 
And notice that it tells me all the matching does, in fact, now. 
This window is a little small with this font size, 
but if I do here, this explanation says dot-star matches any character between zero and unlimited times, followed by disconnected from literally followed by a capture group, 
and then walks you through all the stuff. 
And that's one thing, 
but it also lets you've given a test string and then matches the pattern against every single test string 
that you give and highlights what the different capture groups, for example, are. 
So here, we made user a capture group, right? 
So it'll say okay, the full string matched, the whole thing is blue, so it matched. 
Green is the first capture group, red is the second capture group, 
and this is the third because pre-auth was also put into parenthesis. 
And this can be a handy way to try to debug your regular expressions. 
For example, if I put disconnected from, 
and let's add a new line here, 
and I make the username disconnected from, now that line already had the username be disconnect from. 
Great, here I'm thinking ahead. 
You'll notice that with this pattern, this was no longer a problem because it got matched the username. 
What happens if we take this entire line or this entire line and make that the username? Now what happens? It gets really confused, right? 
So this is where regular expressions can be a pain to get right because it now tries to match. 
It matches the first place where username appears, 
or the first invalid in this case, the second invalid, because this is greedy. 
We can make this non-greedy by putting a question mark here. 
So if you suffix a plus or a star with a question mark, it becomes a non-greedy match. 
So it will not try to match as much as possible. 
And then you see that this actually gets parsed correctly because this dot will stop at the first disconnected from, 
which is the one that's actually emitted by SSH, the one that actually appears in our logs. 
As you can probably tell from the explanation of this so far, regular expressions can get really complicated, 
and there are all sorts of weird modifiers that you might have to apply in your pattern. 
The only way to really learn them is to start with simple ones and then build them up until they match what you need. 
Often you're just doing some like one-off job like when we're hacking out the usernames here, 
and you don't need to care about all the special conditions, right?
Don't have to care about someone having the SSH username perfectly match your login format. 
That's probably not something that matters because you're just trying to find the usernames. 
But regular expressions are really powerful, 
and you want to be careful if you're doing something where it actually matters. 
You had a question: regular expressions by default only match per line anyway. 
They will not match across new lines. 
So, the way that sed works is that it operates per line, 
and so sed will do this expression for every line. 
Okay, questions about regular expressions or this pattern so far? It is a complicated pattern, so if it feels confusing, like don't be worried about it. 
Look at it in the debugger later. 
Keep in mind that we're assuming here that the user only has control over their username. 
So the worst that they could do is take like this entire entry and make that the username. 
Let's see what happens, right? 
So that's how it works. 
And the reason for this is this question mark means that the moment we hit the disconnect keyword, we start parsing the rest of the pattern. 
And the first occurrence of disconnected is printed by SSH before anything the user controls. 
So in this particular instance, even this will not confuse the pattern. 
Yep, if... 
Well, so if you're writing a... 
This sort of odd matching will, in general, when you're doing data wrangling, is like not security-related, 
but it might mean that you get really weird data back. 
And so if you're doing something like plotting data, you might drop data points that matter. 
You might parse out the wrong number, 
and then your plot suddenly has data points that weren't in the original data. 
And so it's more that if you find yourself writing a complicated regular expression, like double-check that it's actually matching what you think it's matching. 
And even if it's not security-related, as you can imagine, these patterns can get really complicated. 
Like, for example, there's a big debate about how do you match an email address with a regular expression? And you might think of something like this. 
So this is a very straightforward one that just says letters and numbers and underscore scores and percent followed by a plus because in Gmail, you can have pluses in email addresses with a suffix. 
In this case, the plus is just for any number of these, 
but at least one because you can't have an email address that doesn't have anything before the ad, 
and then similarly after the domain, right? And the top-level domain has to be at least two characters and can't include digits. 
Right? You can have it.com, 
but you can't have a.dap7. 
It turns out this is not really correct. 
There are a bunch of valid email addresses that will not be matched by this, 
and there are a bunch of invalid email addresses that will be matched by this. 
So there are many, many suggestions, 
and there are people who've built like full test suites to try to see which regular expression is best, 
and this particular one is for URLs. 
There are similar ones for email where they found that the best one is this one. 
I don't recommend you trying to understand this pattern, 
but this one apparently will almost perfectly match what the internet standard for email addresses says as a valid email address, 
and that includes all sorts of weird Unicode code points. 
This is just to say, regular expressions can be really hairy, 
and if you end up somewhere like this, there's probably a better way to do it. 
For example, if you find yourself trying to parse HTML or something, or parse JSON where there are expressions, you should probably use a different tool. 
And there is an exercise that has you do this, not with regular expressions. 
They give you deep dives into how they work if you want to look that up; it's in the lecture notes.
Okay, so now we have the list of usernames. 
Let's go back to data wrangling. 
This list of usernames is still not that interesting to me. 
Let's see how many lines there are. 
So if I do wc -l, there are 198,000 lines. 
wc is the word count program, -l makes it count the number of lines. 
This is a lot of lines. 
If I start scrolling through them, that still doesn't really help me. 
I need statistics over this, I need aggregates of some kind, 
and the sed tool is useful for many things, it gives you a full programming language, it can do weird things like insert text or only print matching lines, 
but it's not necessarily the perfect tool for everything. 
Sometimes there are better tools. 
For example, you could write a line counter. 
You just should never sed, it's a terrible programming language except for searching and replacing, 
but there are other useful tools. 
So, for example, there's a tool called sort. 
sort takes a bunch of lines of input, sorts them, 
and then prints them to your output. 
So in this case, I now get the sorted output of that list. 
It is still 200,000 lines long, so it's still not very helpful to me, 
but now I can combine it with a tool called uniq. 
uniq will look at a sorted list of lines and it will only print those that are unique. 
So if you have multiple instances of any given line, it will only print it once. 
And then I can say uniq -c, so this is going to say count the number of duplicates for any lines that are duplicated and eliminate them. 
What does this look like? Well, 
if I run it, it's going to take a while. 
There were thirteen zze usernames, there were ten ZXVF usernames, etc. 
There, and I can scroll through this. 
This is still a very long list, right, 
but at least now it's a little bit more collated than it was. 
Let's see how many lines I'm down to now. 
Okay, 24,000 lines. 
It's still too much, it's not useful information to me, 
But I can keep burning down this with more tools. 
For example, what I might care about is which user names have been used the most. 
Well, I can do sort again and I can say I want a numeric sort on the first column of the input, 
so -n says numeric sort, -K lets you select a white space separated column from the input to sort by. 
And the reason I'm giving one comma one here is because I want to start at the first column and stop at the first column. 
Alternatively, I could say I want you to sort by this list of columns, 
but in this case, I just want to sort by that column. 
And then I want only the ten last lines. 
So, sort by default will output in ascending order, so the ones with the highest counts are gonna be at the bottom, 
and then I want only lost ten lines. 
And now when I run this, I actually get a useful bit of data. 
Right, it tells me there were eleven thousand login attempts with the username root, there were four thousand with 123456 as the username, etc. 
And this is pretty handy, right? And now suddenly this giant log file actually produces useful information for me. 
This is what I really want from that log file.
Now, maybe I want to just like do a quick disabling of root, for example, for SSH login on my machine, which I recommend you will do, by the way. 
In this particular case, we don't actually need the -k4 sort because sort by default will sort by the entire line, 
and the number happens to come first. 
But it's useful to know about these additional flags, 
and you might wonder, well, how would I know that these flags exist? How would I know that these programs even exist? 
Well, the programs usually pick up just from being told about them in classes like here. 
The flags are usually like, "I want to sort by something that is not the full line." Your first instinct should be to type man sort and then read through the page, 
and then very quickly will tell you, "Here's how to select a pretty good column. 
Here's how to sort by a number, etc."
Okay, what if now that I have this top, let's say top 20 list, let's say I don't actually care about the counts, 
I just want like a comma separated list of the user names because I'm gonna send it to myself by email every day or something like that. 
Like, these are the top 20 usernames. 
Well, I can do this. 
Okay, that's a lot more weird commands, 
but their commands that are useful to know about. 
So, awk is a column-based stream processor. 
So, we talked about sed, which is a stream editor, so it tries to edit text primarily in the inputs. 
Awk, on the other hand, also lets you edit text. 
It is still a full programming language, 
but it's more focused on columnar data. 
So, in this case, awk by default will parse its input in whitespace-separated columns, 
and then that you operate on those columns separately. 
In this case, I'm saying just print the second column, which is the username. 
Right, "paste" is a command that takes a bunch of lines and pastes them together into a single line, that's the "-s" with the delimiter comma. 
So, in this case, for this, I want to get a comma-separated list of the top usernames, which I can then do whatever useful thing I might want. 
Maybe I want to stick this in a config file of disallowed usernames or something along those lines. 
Awk is worth talking a little bit more about because it turns out to be a really powerful language for this kind of data wrangling. 
We mentioned briefly what this "print $2" does, 
but it turns out that for awk, you can do some really, really fancy things. 
For example, let's go back to here where we just have the usernames. 
I say let's still do sort and unique because otherwise, the list gets far too long, 
and let's say that I only want to print the usernames that match a particular pattern. 
Let's say, for example, that I want to see all of the usernames that only appear once and that start with a C and end with an e. 
That's a really weird thing to look for, 
but all in all, it's really simple to express. 
I can say I want the first column to be 1, 
and I want the second column to match the following regular expression. 
Hey, this could probably just be dot, 
and then I want to print the whole line. 
So, unless I mess something up, this will give me all the usernames that start with a C, end with an e, 
and only appear once in my log. 
Now, that might not be a very useful thing to do with the data. 
What I'm trying to do in this lecture is show you the kind of tools that are available, 
and in this particular case, this pattern is not that complicated, even though what we're doing is sort of weird. 
This is because very often on Linux, with Linux tools in particular and command-line tools in general, the tools are built to be based on lines of input and lines of output, 
and very often, those lines are going to have multiple columns, 
and awk is great for operating over columns. 
Now, awk is not just able to do things like match per line, 
but it lets you do things like let's say I want the number of these. 
I want to know how many usernames match this pattern. 
Well, I can do "WCHL," that works just fine. 
Alright, there are 31 such usernames, 
but awk is a programming language. 
This is something that you will probably never end up doing yourself, 
but it's important to know that you can. 
Every now and again, it is actually useful to know about these. 
This might be hard to read on my screen, I just realized. 
Let me try to fix that in a second. 
Let's do... 
yeah, Apparently fish does not want me to do that. 
Um, so here begin is a special pattern that only matches the zeroth line. 
End is a special pattern that only matches after the last line. 
And then this is gonna be a normal pattern that's matched against every line. 
So what I'm saying here is on the zeroth line, set the variable rose to zero. 
On every line that matches this pattern, increment rose. 
And after you have matched the last line, print the value of rose. 
And this will have the same effect as running WCHL, 
but all within awk. 
His particular instance like WCHL is just fine, 
but sometimes you want to do things like you want to might want to keep a dictionary or a map of some kind. 
You might want to compute statistics. 
You might want to do things like, I want the second match of this pattern. 
So you need a stateful matcher like ignore the first match but then print everything following the second match. 
And for that, this kind of simple programming in awk can be useful to know about. 
In fact, we could, in this pattern, get rid of said and sort and unique and grep that we originally used to produce this file, 
and do it all in awk. 
But you probably don't want to do that. 
It would be probably too painful to be worth it. 
It's worth talking a little bit about the other kinds of tools that you might want to use on the command line. 
The first of these is a really handy program called BC. 
So BC is the Berkeley calculator, I believe. 
Man BC. 
I think BC is originally from Berkeley calculator anyway. 
It is a very simple command-line calculator but instead of giving you a prompt, it reads from standard in. 
So I can do something like echo 1 plus 2 and pipe it to BC. 
Shell because many of these programs normally operate in like a stupid mode where they're unhelpful. 
So here it prints 3. 
Wow, very impressive. 
But it turns out this can be really handy. 
Imagine you have a file with a bunch of lines, let's say something like, oh, I don't know, this file. 
And let's say I want to sum up the number of logins, the number of usernames that have not been used only once. 
Alright, so the ones where the count is not equal to one, I want to print just the count. 
Right, this is me, give me the counts for all the non single-use usernames. 
And then I want to know how many are there of these. 
Notice that I can't just count the lines, that wouldn't work right because there are numbers on each line. 
I want to sum. 
Well, I can use paste to paste by plus. 
So this paste every line together into a plus expression, right? And this is now an arithmetic expression, so I can pipe it through BCL. 
And now there have been 191,000 logins that share to username with at least one other login. 
Again, probably not something you really care about, 
but this is just to show you that you can extract this data pretty easily. 
And there's all sorts of other stuff you can do with this. 
For example, there are tools that let you compute statistics over inputs. 
So, for this list of numbers that I just printed out, just the distribution of numbers, I could do things like use R. 
R is a separate programming language that's specifically built for statistical analysis. 
And I can say, let's see if I got this right...this is again a different programming language that you would have to learn, 
but if you already know R or you can pipe them through other languages too, like so. 
This gives me summary statistics over that input stream of numbers. 
So, the median number of login attempts per username is 3, the max is 10,000 (that was route we saw before), 
and it tells me the average was 8. 
For this particular instance, this might not matter, 
and these might not be interesting numbers, 
but if you're looking at things like output from your benchmarking script or something else where you have some numerical distribution and you want to look at them, 
these tools are really handy. 
We can even do some simple plotting if we wanted to. 
Right, so this has a bunch of numbers. 
Let's go back to our sort and k-11 and look at only the two top 5. 
GNU plot is a plotter that lets you take things from standard in. 
I'm not expecting you to know all of these programming languages because they really are programming languages in their own right, 
but it's just to show you what is possible. 
So, this is now a histogram of how many times each of the top 5 usernames have been used for my server since January 1st, 
and it's just one command-line. 
It's a somewhat complicated command line, 
but it's just one command-line thing that you can do.
There are two special types of data wrangling that I want to talk to you about in the last little bit of time that we have, 
and the first one is command-line argument wrangling. 
Sometimes, you might have something that, actually, we looked at in the last lecture, 
like you have things like find that produce a list of files or maybe something that produces a list of arguments for your benchmarking script, 
like you want to run it with a particular distribution of arguments. 
Let's say you had a script that printed the number of iterations to run a particular project, 
and you wanted, like, an exponential distribution or something, 
and this prints the number of iterations on each line, 
and you were to run your benchmark for each one. 
Well, here is a tool called xargs that's your friend. 
xargs takes lines of input and turns them into arguments, 
and this might look a little weird. 
See if I can come up with a good example for this. 
So, I program in Rust, 
And rust lets you install multiple versions of the compiler. 
So in this case, you can see that I have stable beta, I have a couple of earlier stable releases, 
and I've launched a different dated Knightley's. 
And this is all very well, 
but over time like I don't really need the nightly version from like March of last year anymore. 
I can probably delete that every now and again, 
and maybe I want to clean these up a little. 
Well, this is a list of lines, so I can get for nightly, I can get rid of. 
So -V is don't match, I don't want to match to the current nightly. 
Okay, so this is a list of dated Knightley's. 
Maybe I want only the ones from 2019, 
and now I want to remove each of these tool chains for my machine. 
I could copy paste each one into - there's a rust up tool chain remove or uninstall, maybe tool chain uninstall, right? 
So I could manually type out the name of each one or copy/paste them, 
but that's getting gets annoying really quickly because I have the list right here. 
So instead, how about I said away this sort of this suffix that it adds? 
Right, so now it's just that, and then I use ex args. 
So ex args takes a list of inputs and turns them into arguments. 
So I want this to become arguments to rust up tool chain uninstall, 
and just for my own sanity's sake, I'm gonna make this echo just so it's going to show which command it's gonna run. 
Well, it's relatively unhelpful, 
but are hard to read, at least you see the command it's going to execute. 
If I remove this echo, it's rust up tool chain uninstall, 
and then the list of Knightley's as arguments to that program. 
And so if I run this, it uninstalls every tool chain instead of me having to copy paste them. 
So this is one example where this kind of data wrangling actually can be useful for other tasks than just looking at data. 
It's just going from one format to another. 
You can also wrangle binary data. 
So a good example of this is stuff like videos and images where you might actually want to operate over them in some interesting way. 
So for example, there's a tool called ffmpeg. 
ffmpeg is for encoding and decoding video and to some extent images. 
I'm gonna set its log level to panic because otherwise it prints a bunch of stuff. 
I want it to read from dev video 0, which is my video of my webcam video device, 
and I wanted to take the first frame, so I just wanted to take a picture, 
and I wanted to take an image rather than a single frame video file, 
and I wanted to print its output. 
- is usually the way you tell the program to use standard input or output rather than a given file. 
So here it expects a file name, 
and the file name - means standard output in this context. 
And then I want to pipe that through a parameter called convert. 
Convert is an image manipulation program. 
I want to tell convert to read from standard input and turn the image into the color space gray, 
and then write the resulting image into the file -, which is standard output. 
And I don't want to pipe that into gzip; we're just gonna compress this image file, 
and that's also going to just operate on standard input, standard output. 
And then I'm going to pipe that to my remote server, 
and on that, I'm going to decode that image, 
and then I'm gonna store a copy of that image. 
So remember, tee reads input, prints it to standard out and to a file. 
This is gonna make a copy of the decoded image file as copy about PNG, 
and then it's gonna continue to stream that out. 
So now I'm gonna bring that back into a local stream, 
and here I'm going to display that in an image display. 
Err, let's see if that works. 
Hey, right, so this now did a round-trip to my server and then came back over pipes, 
and there's now a computer, there's a decompressed version of this file, at least in theory, on my server. 
Let's see if that's there: a CPT's p copy PNG 2 here, 
and CP 8. 
Yeah, hey, same file ended up on the server, so our pipeline worked. 
Again, this is a sort of silly example, 
but let's you see the power of building these pipelines where it doesn't have to be textual data; it's just going from data in any format to any other. 
Like, for example, 
if I wanted to, I can do cat dev video 0 and then pipe that to a server that Anish controls, 
and then he could watch that video stream by piping it into a video player on his machine. 
If we wanted to write it, we just need to know that these things exist. 
There are a bunch of exercises for this lab, 
and some of them rely on you having a data source that looks a little bit like a log on Mac OS and Linux. 
We give you some commands you can try to experiment with, 
but keep in mind that it's not that important exactly what data source you use. 
This is more finding some data source where you think there might be an interesting signal, 
and then try to extract something interesting from it. 
That is what all of the exercises are about. 
We will not have class on Monday because it's MLK Day, so next lecture will be Tuesday on command line environments. 
Any questions about what we've covered so far or the pipelines or regular expressions? I really recommend that you look into regular expressions and try to learn them. 
They are extremely handy, both for this and in programming in general, 
and if you have any questions, come to office hours, 
and we'll help you out.