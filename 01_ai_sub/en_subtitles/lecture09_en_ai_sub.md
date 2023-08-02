Okay, so again, let's get started with today's lecture. 
So today, we're going to be talking about security and cryptography, 
and today's lecture is going to be a little bit different than our treatment of this topic in last year's class. 
So last year, we focused a little bit more on security and privacy from the perspective of a user of a computer, 
but today we're going to focus a little bit more on security and cryptography concepts that are relevant in understanding some of the tools that we talked about earlier in this class. 
For example, we talked about hash functions or cryptographic hash functions like sha-1 in the Git lecture, 
or we talked about public keys when we talked about SSH in the command line environment lecture. 
And so today, we'll talk about these different cryptographic primitives in more detail and get an understanding of how they work 
and how they're used in these different tools that we're teaching in this class.
This lecture is not a substitute for a more rigorous class in security. 
So there are a bunch of really good classes at MIT like 6.858, which is on computer system security, or 6.857 and 6.875, which are more focused on cryptography. 
So don't do security work without formal training in security from these classes or elsewhere. 
And unless you're an expert, don't roll your own crypto. 
Don't build your own crypto implementations or protocols. 
And the same principle applies to computer system security. 
This lecture is not about building your own stuff; it's about understanding what's already out there. 
And so this lecture will have a very informal but we think practical treatment of these basic cryptography concepts, 
and yeah, hopefully, it'll help you understand some of the tools we talked about earlier in this class.
Any questions about the plan for today's lecture? Great. 
So the first topic for today is something called entropy. 
Entropy is a measure of randomness, 
and this is useful, for example, when trying to determine the strength of a password. 
So let's take a look at this comic from xkcd. 
We're a big fan of xkcd comics. 
So this comic raises your hand if you've seen this before. 
[pause] Okay, a good number of you. 
So this comic is complaining about this common pattern that's been taught to users of computers, 
that when you design passwords, they should be things like "t#0rU&b40rM$" or "p3@ch3$" -- a string in the top left -- like we should design passwords 
that are full of funny characters and things like that to make it hard for attackers to guess. 
And yet, it turns out that passwords like that are actually pretty weak and guessable by computers that can guess passwords really fast and brute-force attacks. 
And on the other hand, passwords which maybe intuitively don't look as secure, like the one on the bottom left, "correct horse battery staple," 
that one turns out to be way more secure.
So how do I actually quantify the security of these different passwords? It's by measuring the amount of randomness in the password, how many bits of randomness are in there. 
And so entropy is measured in bits. 
This is like the same bits from information theory. 
And we're only going to talk about the simple case where you're trying to measure the amount of randomness when you're choosing from a set of things.
Uniformly at random, 
so for example, when you're constructing a password that's in the format of four random words, 
you're kind of considering all possible sequences of four random words made from some dictionary you have. 
You might have a dictionary with, say, a hundred thousand words and you're selecting each word uniformly at random. 
How many possibilities are there? Well, you can go and figure that out. 
In that example, once you know how many possibilities there are, the measure of entropy is log base 2 of the number of possibilities. 
And as that comic suggests, this is related to how long it'll take an attacker to try to brute-force through your different passwords. 
Like if you have a thousand possibilities, you're guessing passwords at a thousand passwords a second, that's not a very good password. 
So this is a couple of quick examples. 
A coin flip has two possibilities, 
and let's assume we have a fair coin. 
So a coin flip has log base 2 of 2, which is one bit of entropy. 
Another thing we might look at is something like a dice roll. 
So there are six possibilities, 
and log 2 of 6 is something like 2.6 bits of entropy. 
So that's how we quantify the amount of randomness in something.
Now, going back to that example in the xkcd comic, when we want to figure out how much entropy is in a password, 
we have to consider the model for how the password was generated. 
For example, in the top left, you could consider, okay, we take one dictionary word, 
make some substitutions of some of the characters with numbers that look similar to that character, add one punctuation mark at the end, 
and add one numeral after that. 
We can take that model and then use common rhetoric to figure out how many possibilities there are, 
and from that, we can derive how many bits of entropy are in that password. 
So in that particular example, I don't know exactly what model they were using for the password, 
but they calculated their 28 bits of entropy. 
Whereas in the bottom-left example, "correct horse battery staple," they assume that you're working from a dictionary of about 2,000 words. 
And so when you combine four of those words together, you get about 44 bits of entropy from that. 
So it's much more secure than the example before it.
So any questions about this definition of entropy or why it's useful? And when you're generating your own passwords, keep this in mind. 
You want a high-entropy password, 
and the exact number you need depends on exactly what you're trying to protect against. 
Like in general, a concept in securities, you have to keep in mind what your threat model is. 
Like what attackers you're concerned about, what kinds of technique the attackers might be using. 
For example, this comic refers to an attacker that can guess a thousand passwords a second. 
This might be something that's possible for say, 
a web service that allows people to try to log in with your email and then random passwords that the attacker is trying. 
But this thousand passwords a second model might not be accurate for other scenarios. 
For example, an offline password cracking scenario or maybe the attacker has broken into a website and downloaded their database 
and they have some obfuscated form of your password, 
and they're trying to figure out what the password is. 
Maybe they can parallelize this attack and make it go to a million guesses a second .
Guesses a second, 
and so exactly how much entropy you need depends on exactly what you're trying to protect against. 
But roughly forty bits of entropy might be good enough for, which is protected by a website and you're concerned about online password guesses. 
And then maybe something like 80 bits of entropy might be good if you're concerned about offline attacks and you want to be really, really secure. 
So they're rough guidelines you can use. 
And then how do you actually generate strong passwords? Well, you have some model for a password. 
For example, the for dictionary works thing, 
and you can actually get a dictionary. 
And then you can use methods like dice, where, 
so there's a song we linked to in the lecture notes where you can actually get physical dice and roll them 
and then map dice rolls to dictionary words in order to eventually turn that into a password. 
And doing something like this, using some kind of physical token that you know is random, like a balanced die or a coin that you know is balanced, 
is a good thing to do because humans are actually not good at choosing random numbers, right? If I just asked you to name a random number for 1 to 100,
chances are that you're probably not doing so uniformly at random very well. 
And so that's why it's actually good to use these physical tokens in order to produce randomness. 
So entropy, that's our first concept. 
Recovering any questions about that or about this comic? Great. 
So getting into slightly more interesting and complicated topics, the next thing we're going to talk about is hash functions. 
So hopefully, most of you were here during the get lecture where we talked about the SHA-1 hash function used in get. 
So now going into that topic in a little bit more detail, hash functions at a high level are functions that map a variable amount of data into a fixed size output. 
So for example, the SHA-1 hash function is one example of a hash function that takes in some input of some number of bytes and outputs exactly 160 bits of output. 
So that's kind of the type signature of this particular hash function. 
And then these functions have some number of properties that are useful. 
So at a high level, these can be thought about as hard-to-invert functions that have random-looking outputs. 
We can actually try this out on some random piece of data. 
For example, if I enter into my terminal "printf hello", this does exactly what you would expect it does, prints the set to standard out, 
and I can pipe this to the SHA-1 sum command. 
So this is a command-line program that accepts input via standard in and computes this SHA-1 function, 
which takes in some variable number of bytes from the input and produces a 160-bit output, which in this particular case is represented or encoded as a hexadecimal string. 
So it's a length 40 hexadecimal string, 
and you see this output right here. 
This "-" just means it took its input from standard in. 
So this output just looks like some random number, 
but one important thing is that this is a deterministic number. 
If you try the same command on your own computer, "printf hello | SHA-1 something", you will get the same number out. 
So SHA-1 is some well-known function that people have agreed upon for all its parameters. 
we'll see that if we tweak the input a little bit, like say changed 'hello' to 'holo' with a capital H, now I get a completely different looking output. 
And this also looks like some other kind of random-ish number, even though it is deterministic, 
and you could reproduce this on your own computer.
Hash functions have a number of properties that are pretty important. 
The first property that cryptographic hash functions have is that they're non-invertible. 
And what that means is that if you take the output from this function, for example, that is 'aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d', 
as shown there from that output, it's hard to figure out what the input was that produced that output. 
So you can go one way, compute the SHA-1 hash easily, 
but you can't go backwards.
Another property that these functions have is that they're collision-resistant. 
And what this property means is that it's hard to find two different inputs that produce the same output. 
So this basically describes what a cryptographic hash function is.
So any questions about the kind of specification of a cryptographic hash function? Okay, 
so what are these hash functions actually useful for? Well, we've already seen one application in Git for content address storage. 
So we want it get, we want some uniform way of naming different objects that are in the object store, 
and it turns out that Git addresses all of them by their SHA-1 hash.
So you have the actual data you want to store, 
and then to name that particular piece of data, you just name the SHA-1 hash. 
And all of that is stored in the object store in that particular way. 
We see this when looking at many different parts of Git. 
For example, right here, I'm going to Git repository. 
If I do 'git log', it shows me the commits. 
And for example, this number up here is the cryptographic hash function SHA-1 applied to the commit object that describes this particular commit.
So does anybody know why Git uses a cryptographic hash function here as opposed to... 
So you might have heard in your other computer science classes, like say your introductory algorithms class, 
there are things called hash functions without the word 'cryptographic' appended in front of them. 
And they have similar properties that they turn a variable-sized input into some fixed-size output. 
But they don't quite have these properties where it's hard to find an input that produces a particular output or things like that. 
It's a kind of weaker definition than this.
So why is it that in Git we care about having a cryptographic hash function as opposed to just a regular old hash function? 
Does anybody have any ideas? Yeah, that's basically it, that we don't want to have kind of conflicts in the output from this hash function. 
Like every commit is identified by a hash function, every file is identified by the hash of that file. 
If it were ever the case that two different pieces of content in practice produce the same output, that is, 
if the function were not collision-resistant, that could be really problematic, right? Because then you and I, 
we could have to do to get repos that we think are the same, we check out the same commit hash, 
and we might end up with different files. 
And this is concerning because Git is used to track software, a track development of software, 
and it's also kind of involved in making sure that the right people are authoring the software, nothing funny has happened in the process. 
For example, there are all these open-source projects like the Linux kernel where development is done using Git. 
It would be really bad if some contributor to Git could say edit some file and propose some change that looks pretty benign like, 
'Oh, let me go and improve this part of Linux,' submit that change request to the Linux developers, 
and then in practice actually supply a Git repository that has the same commit hash and whatnot, 
but actually the file contents are different. 
There's something malicious. 
So Git actually relies on this SHA-1 function being a cryptographic hash function in order to achieve security. 
Any questions about that and some other interesting applications of hash functions?
So, as we saw, hash functions turn big inputs into small outputs, 
and in a way, because the hash function is collision-resistant, the output can be used to kind of attest to or identify the input. 
And so you can think of a hash as a short summary of a file. 
For example, in this directory of a bunch of files, I can compute the SHA-1 sum of some file in this directory. 
And this is the SHA-1 algorithm applied to this readme MD file. 
And what's interesting is that it is computationally hard or like impossible, you can kind of think of it as impossible, to find any other file, 
so a different file that has the same hash output. 
And one scenario in which this is useful is when you download files from the internet. 
For example, there are lots of Linux distributions that distribute large CD or DVD images from their website. 
Like, I can go to Debian org and download the latest version of Debian. 
The thing is that hosting those files can be expensive. 
And so a lot of people are nice enough to host mirrors of these files. 
So instead of downloading Debian from Debian org, I can go to one of many other sites and download what are supposed to be the same files that are hosted at Debian org. 
But how do I know that I actually got the correct file? Like, 
what if I set up a malicious mirror and you go to like Anisha is evil Debian website com and then try to download Debian, 
turns out that your Linux installation is backdoored. 
Well, one thing you could do is download a copy from the original Debian website and then download my version and compare them. 
But that kind of defeats the purpose, right? Because we want to avoid downloading things from Debian org because hosting these files is expensive, 
and we want all these different people to be able to mirror copies of the files elsewhere. 
So, does anybody see how cryptographic hash functions could be useful to solve this problem? 
That I want to download a file from an untrusted source but and not from like the trusted source itself, 
But maybe I can get some small piece of information from this trusted source, 
in order to know whether the file I downloaded from the untrusted source is the thing I was supposed to get. 
Yes, like it's basically just a straightforward application of cryptographic hash functions. 
So what Debian org can do is they can produce their kind of correct ISO file or whatever they want, 
and instead of publishing the file itself on their website, they can publish a hash of that file. 
So, compared to the file itself which may be many gigabytes, this is only like in this particular case 160 bits of data, right? So very cheap to host. 
And then what I can do as a user is I can download that file from any random website, it could be an untrusted website, 
and after I download, I just double-check the sha-1 hash. 
And if the hash matches, then I know that I have the right file 
because it's computationally infeasible for somebody to give me some different file that happens to have the same hash, 
because hash functions are collision-resistant. 
So any questions about that application? Yeah, 
so that's a good question, like why do you need different people to host the information? 
Like wouldn't it be equally expensive for everybody? So the answer to that question is a little bit complicated, 
but like here's a partial answer. 
One thing is that downloading files from a server is affected by how far away the server is from you. 
So for example, if the server is in Massachusetts and you're in say China, you have to kind of make a big round trip across the internet, 
and that may be expensive for a number of reasons. 
Like the latency is high and the traffic needs to go through kind of lots of different wires to make its way all the way to where you are. 
And so one thing that these websites do is that they distribute their content to servers that are all over the world, 
and then as a user, you download from the server that's closest to you. 
Like for example, MIT maintains a Debian package repository and kind of mirrors all the Debian stuff. 
So if you're a Debian user at MIT, you can use the MIT copy of everything, 
and then you can kind of access it over our fast local network, 
and that traffic never needs to go to the outside Internet at all, 
so it's very fast. 
That's a good question. 
Any other questions? Okay, 
and then one final kind of interesting application of hash functions is something called a commitment scheme. 
So I want to play a game, 
and I need a volunteer for this. 
So you don't actually need to get up from your seat or anything, I just need you to talk with me. 
So any volunteers raise your hand? Yeah, okay, what's your name? Abdul Aziz? Okay, great. 
So Abdul Aziz, we're going to play a game where I'm going to flip a coin and then you're gonna call heads or tails, 
and if you call it right, you win, 
and if you call it wrong, you lose. 
And there are no stakes for this game, 
but just the pride of winning. 
Sadly, I checked my wallet and all I have is dollar bills, I don't have any coins with me. 
So instead, I'm just going to flip the coin in my head. 
Alright, 
so okay, I flip the coin, call heads or tails. 
Sorry, you lost, it was heads. 
I can cheat, right? I can just see what you say and say the opposite thing. 
So let's try fixing this game. 
How about you call heads or tails after I say what the flip result was? Okay, yeah, 
so if I say, "Oh, the result is tails," 
What are you gonna say? Are you gonna call tails? Yeah. 
So, is it possible to play this "Guess what the coin flip result is" game in a fair way without having a physical coin that we share? 
Like, because I can't really manipulate your physical reality. 
If I flip a coin in front of you, you probably trust that it's okay, right? 
So, it turns out that hash functions give us a kind of cool way to solve this problem through an idea called a commitment scheme. 
So, I can say, "Here's the construction of the solution. 
I can pick heads or tails, 
and I'm actually going to pick a big random number, say like this number here, 
and what I can do is compute the SHA-1 sum of this number. 
At this moment, you haven't seen this number yet, I'm just doing all this in my head. 
And then what I do is I tell you, "Okay, I flipped a coin, 
and I'm not going to tell you what the result is just yet because you haven't called heads or tails, 
but I'll tell you what the SHA-1 sum of the result is. 
Here you go," and I tell you this value. 
Now, after this, you can call heads or tails. 
So, what do you say? Like, say heads afterwards. 
What I can do is I can reveal to you what my input to this function was, 
and then you can cross-check this, right? You can compute the SHA-1 sum on the input to verify that the output is what I said it was earlier, 
and then we can have some way of mapping these numbers to heads or tails. 
So, I might have agreed upon beforehand that even numbers are heads and odd numbers are tails, 
and so this is a way of fixing that game. 
So, we can actually play this game in our heads. 
I can pick a value, 
but not reveal that value to you, 
but I can commit to the value. 
So, this is a kind of binding commitment scheme that I can't change my mind after I've told you this, 
but it doesn't reveal the original value to you. 
And so, this is one other neat application of cryptographic hash functions. 
Any questions about this particular construction? Okay, great. 
So, moving on to the next topic, we're going to talk about key derivation functions. 
(Applause) Often abbreviated as KDF. 
So, this is a concept that's very similar to hash functions, except it has kind of one extra property that it is slow to compute. 
For example, there's a hash function or key derivation function known as PBKDF2, password-based key derivation function, 
that has a kind of similar form as these hash functions we were talking about here, that they take in some variable length input and produce a fixed length output. 
But they're meant to be used for one particular purpose. 
The purpose is generally to use the fixed length output as a key in another cryptographic algorithm. 
And we'll talk about those algorithms, like what use the output of this thing for in a moment. 
But one property of these things is that they're slow. 
Does anybody have any idea why you'd want an algorithm to be slow? Like, 
normally we want algorithms to be fast, right? So why would we want an algorithm to be slow? 
Yes, yeah, that's exactly it. 
So, I'll repeat so it goes into the microphone.
The reason you want these to be slow is when you're actually using it for something like password authentication. 
Where you have the hash of a password saved and then somebody inputs the password, you want to know if that corresponds to the hash. 
It's okay if it's slow because you're only doing this check kind of once. 
But the other scenario in which you're going to be using this function is when somebody's trying to brute-force a password. 
Say a website has their password database stolen and somebody's going through all the accounts, trying to break all the passwords. 
Well, in that case, you want this to be slow because someone's gonna be doing this like millions and millions of times. 
And you can slow down the attacker a lot by making this function slow. 
And so it's fine if this takes you like one second upon logging in to compute this function. 
But when you're brute-forcing it, we don't go to a thousand guesses a second like in that XKCD comic. 
We can slow it down a little bit. 
So what is the output of key derivation functions actually used for? 
Well, the next topic we're going to talk about, probably like one of the most classic things when you think about cryptography, is encryption and decryption. 
The next topic is symmetric key cryptography. 
And like the rest of this lecture, we're not going to talk about how you implement these. 
We're going to talk about the API for a symmetric key, symmetric key crypto, like how it's used. 
So symmetric key cryptosystems have a couple different functions. 
They have a key generation function, which is a randomized function that produces a thing we call the key. 
And then they have a pair of functions, encrypt and decrypt. 
And encrypt take as input something we refer to as the plaintext, 
and this is just some sequence of bytes, 
some data. 
And it takes in a key, 
so something that came as an output of this key generation function, 
and produces what we call the ciphertext. 
And then decrypt does the opposite of this. 
So it takes the ciphertext along with the key and produces the plaintext. 
And this triple of functions has a couple properties. 
One is that, like one team you might expect, is that this thing doesn't really tell you all that much about this input to the encryption. 
So property number one is given the ciphertext, you can't figure out the plaintext without the key. 
And the other property is kind of the obvious correctness property, that if you take something and you encrypt it, 
some message M with a key K, 
and then you decrypt that ciphertext using the same key, that gives you back the same message. 
This is the kind of obvious correctness property. 
So, does this description make sense? 
Does it fit your kind of intuitive understanding of taking some piece of data and obscuring it so you can't really tell anything about the original input, 
but then taking that obscured result and then passing it through some decryption function given that key to retrieve the original input?
and this isn't really a rigorous definition of what it means for something to be secure, 
but it's a good enough intuitive definition that we can work with it. 
So any questions about that description there?
So where can symmetric key cryptography be useful? We'll talk about a whole bunch of examples later in this lecture, 
but one example we'll talk about right now is encrypting files for storage in an untrusted cloud service. 
So consider something like Dropbox or Google Drive, where you're uploading files and trusting the service to not look at your files or do anything malicious with them. 
These services, at least the ones I named, are not encrypted or anything like that. 
In theory, any employee of those companies could look at your files. 
Now, of course, these companies have lots of policies and technical controls in place for making sure that that sort of thing doesn't happen, 
but that doesn't mean that it's not technically possible. 
So one thing you might want to do if you don't want to trust these cloud services to not peek at your data, not do like machine learning over them, 
or do other sorts of things that you wouldn't really want, is you can just take your files and encrypt them before uploading them to these web services. 
So does that idea make sense? That I can take my file, like center pictures or whatever, pass it through an encryption function and produce the ciphertext, 
and then place that ciphertext on the web service safe for backup purposes or whatever, 
and if I ever need that, I can retrieve the ciphertext, then use my key to decrypt it back into the plaintext, 
and then use the result for doing whatever I need to do. 
Does that make sense?
Yeah, that's a good question. 
The question is, couldn't anybody else run it through the same encryption program? 
One detail maybe I should have explained in a little bit more detail is this key generation function is randomized, 
and this key has high entropy. 
So going back to that topic we talked about earlier, like an example is we might have AES-256. 
This is one particular symmetric cipher, 
and this, as the name might indicate, has 256 bits of entropy in the key. 
And so that means that as long as the attacker, whoever downloads the ciphertext from the web service, doesn't know your key, 
unless they have some better attack in place, they'll have to try all the different possible keys. 
And if there are 2 to the 256 keys, that's too many keys to try in a reasonable amount of time. 
Does that answer the question?
Okay, any other questions?
That's an excellent question, 
and that leads into what I was going to talk about next. 
So thanks for that question. 
As you point out, like if I lose my key, I'm kind of stuck, right? I need my key to decrypt. 
That's kind of the point of this thing. 
If I didn't need my key to decrypt, then this wouldn't be a very good cryptosystem. 
And so I can combine this idea of symmetric key cryptography with the topic we just talked about, key derivation functions. 
So instead of having some key that's randomly generated with my key generation function, say sampling entropy from somewhere on my machine, 
I can have a passphrase and pass it through my key derivation function box, 
and this gives me my key. 
Then, I can take my plaintext and combine it with my key in my encrypt function, 
and this produces my ciphertext. 
I store this ciphertext on the web service, 
but now I don't need to save this key. 
Instead, I can just remember my passphrase, 
and whenever I need my key, I can reconstruct it from the key derivation function. 
Question: yeah, 
so that's a good question. 
The question is, is the key derivation function slow enough to prevent brute-force guessing? The answer is, it depends on how long your passphrase is. 
For example, if your passphrase is like the string 'password', it is probably going to get broken very quickly. 
But as long as there's enough entropy in your passphrase, this is good enough. 
So, like if I was uploading something to Dropbox and I really want it to stay secret, I think a 64-bit passphrase, 
really a passphrase with 64 bits of entropy, it would be more than enough in that scenario, for example. 
And just a quick demo of this: so there are tools to make this really easy to do. 
This is actually one of the exercises, 
but we can take a tool, for example, called OpenSSL, 
and use it to apply a symmetric cipher to some file. 
So, I had my readme text here, for example, readme.md. 
It has a bunch of stuff in it, 
and I can do 'openssl aes 256 cbc', this is the name of a particular symmetric cipher, 
and I can say that I want to apply this to readme.md and produce 'readme.encrypted.md', let's give it some name, 
and then it's asking you for a password. 
So, by default, this works in this mode where I provide a passphrase, it's run through a KDF to produce a key, 
and that's used for encryption. 
So, I'll type in some password, type it in again, 
and now I produce this readme.encrypted.md file. 
If I look at this, it kind of looks like garbage, 
and that's, at a high level, the point of a symmetric cipher. 
It produces some ciphertext that should be kind of indistinguishable from random data. 
And when I want to decrypt this, I can run a similar command: 'openssl aes 256 cbc -d', for decrypt, take the input from readme.encrypted.md, 
and I like to do 'readme.decrypted.md' as the output. 
And I can compare these two files, 
and the correctness property of symmetric cryptography tells me that this should be identical. 
And this indeed is identical. 
If I look at the return value, compare return 0, 
so that means that are the same file. 
[Applause] So, any questions about symmetric key cryptography? Yeah, 
so the particular command did make a new file, 
so it took us input readme.md and produced output this file, 
so that is the encrypted version of the file.
It left the original untouched, 
but then of course I could delete it if I wanted to. 
Yeah, that's a good question. 
This is something I wasn't gonna talk about in too much detail. 
The question is, I provided the salt argument here, 
and where is that stored? So, the answer is that that is stored in this output here. 
So, this output format stores both the salt and the actual output ciphertext, 
so can be used in the reconstruction and decrypt. 
Yeah, that's correct. 
It doesn't keep any database or anything. 
This is fully self-contained. 
Yeah, 
and as John says, the salt is not the secret, like the passphrase is what is the secret thing here. 
Okay, 
so let's go back to the question. 
What is salt? The idea of a cryptographic salt is probably best explained in the context of hash functions. 
So, one common application of hash functions is to store passwords in a password database. 
If I have a website and I have logins for users, like people log in with their username and password, 
I don't actually want to store people's passwords in plain text just like as is. 
Does anybody know why I wouldn't want to do that? Yes, exactly. 
What if there was a breach and someone got all your data? So, it's really bad if you leak all your users' passwords. 
It's especially bad because a lot of people reuse their passwords across different sites. 
So, you'll see attackers break into one thing, like there was big Yahoo breach a while ago, 
and they find all these usernames and passwords, 
and then they go and try those same login credentials on Google and on Facebook and on YouTube and whatnot. 
These people reuse passwords, 
so it's bad to store plaintext passwords. 
So, one thing you should do is you should store hashed passwords with a hash function, or ideally a password hashing function that's intentionally designed to be slow. 
But one thing attackers started doing once they realized that people started storing hashed passwords is they built these things called rainbow tables. 
What people did was they took a way of generating big password lists, like the kind of model what passwords might look like. 
Say, take all the dictionary words, take all strings of like length from zero to eight and whatnot, 
take all of those and then hash them and produce a big database mapping hashes back to their pre-image. 
And so given the output of a hash function, rather than have to like brute force said on the fly, 
you can just go look up in this database, 'Oh, what is the input that corresponds to this output?' 
And people have built these for reasonably large password databases. 
And so one thing that you can do in reaction to that as a defense is rather than storing in your database as your to write it rather than storing just the hash of the password, 
what you do is you compute what's called a salt value. 
And what this is is a large random string. 
And then what you do is you store in your password database the salt, 
which is not really a secret like you can store this in your password database along with a hash of the password with the salt appended to it. 
Why is this useful?
Well, this salt is a random unique value for every user. 
And so if someone has the password 'safe password one two three' on one web service, 
if you are just storing the hash of the password, then the hash would be the same on both Web Services, right? Because this hash function is a deterministic function. 
But now, since we're using this randomized salt value, we store the hash of the password plus the salt. 
And so even if someone's using the same password on multiple sites, this thing looks different in both cases. 
And it makes it so these big databases mapping these short passwords or hash outputs to the short passwords that they came from, those are no longer useful. 
When you have salted passwords, you kind of need to do the brute-force attack for every user once you find their salt value, 
rather than being able to use this big precomputed database. 
Does that answer the question of what a salt is? And so that's what that salt argument is related to. 
Let's see, any questions about anything we talked about so far? Great, okay, 
so the I'm gonna go ahead and erase this, 
and then the last topic we'll talk about is one of the most exciting developments of cryptography. 
Happen quite a while ago, 
but it's still a really cool concept, 
something called symmetric key cryptography. 
And so this is an idea that actually enables a lot of the security and privacy-related features of basically anything you use today. 
Like when you need to go and type in www.google.com/mapmaker, cryptography is used as part of what goes on there. 
So this is going to look pretty similar to what we talked about in symmetric key cryptography, except with a twist. 
There's a key generation function, which similarly is randomized but instead of producing a single key, 
it produces a pair of keys, two different things, one of which is referred to as a public key and the other is referred to as a private key. 
And then these can be used for encryption and decryption in a manner kind of similar to symmetric key crypto, except these different keys have different uses now. 
So we have an encryption function which takes in a plaintext, I'll write P here, 
and it takes in the public key and produces the ciphertext. 
And then I have a decryption function which takes in my ciphertext and the private key and gives me back my plaintext. 
And then similarly to those two properties we had over there, given just the ciphertext, we can't figure out the plaintext unless we have the private key. 
And then we have the obvious correctness property that if we encrypt something with the private key, 
sorry, encrypt something with the public key, 
and then take that ciphertext and try decrypting it with the corresponding private key that came from this key generation process, 
and try decrypting it with the corresponding private key that came from this key generation process. 
That outputs these two different things at once, then I get the same result back. 
So this is very similar to what's above, 
but there's a twist that we have these two different keys that have different functions. 
It's really neat that this public key can actually be made, as the name indicates, public. 
Like I could be using a crypto system that works like this, post a public key on the internet for anybody to see, 
but keep my private key secret. 
And then I have this interesting property that anybody on the internet can take any piece of content and encrypt it for me using my public key, 
and send it over the internet to me. 
And then I can decrypt it using my private key, 
and as long as my private key stays secret, it doesn't matter if my public key is available to anybody on the internet.
So here's where the asymmetry comes from. 
Before, we were in a scenario where, suppose I was on the internet, 
but you weren't like talking to me face-to-face, 
and you wanted to send me some data over the internet over some unencrypted channel where anybody could listen on what you were saying, 
and you wanted to use symmetric key cryptography. 
Well, we need some way of exchanging a key in advance 
so that you could encrypt some plaintext with a key and give me that ciphertext over the internet so that I could decrypt it with that key.
In symmetric key crypto, if the keys public, it's game over, like anybody can decrypt your stuff. 
Whereas in asymmetric key cryptography, I could take my public key and post it on a bulletin board on the internet, 
and you can go look at that, take some contents and encrypt them for me, 
and then send them over, 
and that would be totally fine. 
You can only decrypt it with the private key.
So one analogy that may be helpful is comparing these mathematical ideas to physical locks. 
You probably have a lock on your door to your house, 
and you can put in a key and like turn the thing in order to lock the door or you can turn it the other way to unlock the door. 
So there's a single key, 
and it can both lock and unlock the door. 
But now consider this alternative construction, which you might use if, say, I want you to be able to send me a message and have it be sent over the internet, 
and you and I don't really need a way to exchange a key with you beforehand.
I could get a box which you could put a letter inside, 
and you can close the box. 
And I can get one of the padlock things, which I can give you by I could like take those padlock and open it and give it to you. 
And you, at your own leisure, could put your message inside a box and take this padlock, which is open, 
and shut it around the box and then send it over to me. 
And then I could put in my key and unlock it.
So do you see how there is this asymmetry there as opposed to the key that I used to open the door to my house, where the same key opens and closes the thing? 
Instead, I give you this open padlock that you have the ability to close but not open. 
And after you closed it, I can use my key, which I've kept secret, in order to open the thing and retrieve what's inside. 
Maybe this analogy is helpful, maybe it's not. 
The mathematical construction works just fine if that works for you.
So any questions about symmetric key encryption and decryption and how it relates to symmetric key crypto, how it's a little bit different?
So before we talk about applications of this idea, I'm going to talk about one other set of concepts in symmetric key cryptography. 
These crypto systems give you another set of tools which are related to encryption and decryption, 
something called signing and verifying. 
And this is kind of similar to the real world like I can get a document and sign it with my signature. 
Except real-world signatures are, I don't think that hard to forge. 
These are pretty hard to forge and, therefore, more useful. 
What do signature schemes look like? There's a function sign that takes us some message and the private key, 
so notice this, this is the private key, not the public key, 
and it produces a signature. 
And then there's another function verify which takes in the message, the signature, 
and the public key this time, 
and it tells me it returns a boolean whether or not the signature checks out. 
And then this pair of functions has the property that, again, these are kind of properties that follow the intuition that come from physical signatures that, 
without the private key, it's hard to produce a signature for any message such that you can give the message in the signature 
and the public key to the verify function to get it to return true. 
Like at a high level, it's hard to forge. 
It's hard to forge a signature, of course, without the private key. 
And then there's the obvious correctness property that if you signed a thing with a public key and then try verifying it with the corresponding, 
sorry, if you sign a thing with the private key and try to verify it with the corresponding public key, it returns okay that this verification checks out. 
So these are two different kinds of things you can do with asymmetric key cryptosystems. 
An example of an asymmetric key cryptosystem that you might have heard of is something called RSA. 
So RSA is designed by a number of people, one of whom is Ron Rivest who's a professor here. 
So there are a couple of interesting applications of asymmetric key crypto, actually like tons and tons and tons of, you spend like days talking about this, 
but a couple examples are email encryption. 
So we talked a little bit about sending messages. 
What we can do with asymmetric key crypto is that you can have public keys posted online. 
I think some of the instructors have PGP public keys on their website. 
So for example, you go to my website or John's website, you'll find a public key. 
And then what you can do is you can send us an encrypted email. 
And so even if that message goes through Gmail or whatever other mail service throughout my T's mail servers, 
if there happens to be an attacker snooping on the messages, they can't make any sense of their contents because they're all encrypted. 
And this is really cool because you can do this without kind of finding us in person and exchanging, which you might have to do in a symmetric key cryptosystem. 
You can just find our public key, which can be posted online without causing any issues, 
and then send us encrypted email. 
Another thing asymmetric key crypto is used for is private messaging.
So raise your hand if you've used anything like signal or telegram or I think what's up is in theory antenna encrypted, 
so a good number of you. 
These private messaging applications also use asymmetric key crypto to establish private communication channels. 
Basically, every person has associated with them a key pair, 
and so your device has run this key generation function produced a public key and a private key and automatically posted your public key to the internet. 
So, for example, if you're using signal, your public key is on the signal servers, 
and then when someone wants to contact you, their phone can look up your public key, retrieve it, 
and once it's retrieved your public key, they can encrypt information for you. 
This is a kind of approximation of how their algorithm works, 
but at a high level that's what's going on.
Another neat application of asymmetric key crypto is we were talking about earlier like making sure you have the right software we downloaded from the internet. 
Asymmetric key crypto can be used to sign software releases, 
and this is something that people do for example like Debian packages or whatever things you download from the internet. 
The developer will try to sign their software so that you can make sure 
that whatever you've downloaded from the internet is actually the right thing that came from the right person.
We talked about in the git lecture all the interesting things you can do with git. 
One thing we didn't cover was signing related functionality and git. 
So git has commits, 
and you can associate with commits something called tags. 
At a high level, you can basically take a git commit and attach a signature to it which binds your public key to this commit, 
and then anybody who has your public key can take the commit and your public key and make sure that there's a legitimate signature on the commit.
So let me go to like some random repository that I have. 
I can look at a bunch of tags associated with the repository. 
If I do look at the raw data associated with this tag, it has some metadata and then a blob of like ascii encoded information 
that I can use the get tagged - v4 verify command to make sure that oh this is a good signature from this person happens to be me 
so I sign the software release so that anybody who downloads it from the Internet can make sure that they actually got an authentic copy.
Yes, question. 
So the question is what exactly is the verify function doing or what is it checking against? 
If you want to know mathematically what's going on, talk to me after this lecture. 
But from kind of an API perspective, what's going on here is that the signature and also the message here are just a blob of bytes, 
and it happens to be the case that these things are designed such that basically if you take for some particular public key, like if you take my public key, 
It's impossible for you, without knowledge of my private key, for any message to find a second argument to this function that makes it return true. 
You can kind of compare it to signing a document. 
Like, you don't know how to forge my signature. 
I can take any piece of paper and sign it, 
and then anybody who knows what my signature looks like, I can show my document - you can be like, yeah, that checks out. 
But nobody without the private key can produce a signature that will make this function return true for any particular message. 
And any related questions started, you want me to explain any other way, or does that make sense? 
So, any questions about signing software or any of the other handful of applications talked about of asymmetric key crypto?
Well, so one final thing I want to talk about, we're almost out of time, is key distribution. 
This is a kind of interesting side effect of asymmetric key cryptography. 
It enables a bunch of interesting functionality like I can post my public key on the internet. 
You can go find it and send me encrypted email. 
But how do you know that the public key found is actually my public key? It seems like there's a bootstrapping problem here, right? 
So, there are a couple, this is like a really interesting and really hard real-world problem, 
and there are a couple different approaches you might take to this problem.
One is kind of a lame solution, 
but this thing solves a lot of cryptography problems. 
This exchange the information out-of-band. 
What that means is, you want to send me encrypted email, we'll just talk to me after class. 
I'll give you my public key on a piece of paper, 
and since you were talking to me in person, you know that it's actually my public key, not just somebody like hacked my website and stuck some random number on there. 
That solves the problem, 
but it's not the most elegant.
There are a couple other approaches that different applications use. 
So, those of you who use signal, have you ever encountered the phrase "safety number" like "verify your safety number with so and so"? 
So, with signal, they have a way of exchanging public keys which is through the signal servers. 
Whoever runs the signal service just maintains on their servers basically a mapping from phone numbers to public keys. 
And when I say, "Oh, I want to message this person with this number", 
my phone just goes and retrieves their public key from the internet and then encrypts the message for that public key.
Now, does anybody see a problem with the setup? Yeah, exactly. 
The signal servers are the point of failure there because if the signal servers give me the wrong public key, 
like suppose signal just produces a new key pair and give me their public key, now they can read all my messages. 
And they could even sit in between and transparently decrypt the messages I send them and then re-encrypt them and send them on to their final destination. 
Like, basically, I need some way of authenticating the public key I get.
And so, signal has one solution to this, which is also just kind of punting the issue to out-of-band key exchange. 
You can meet up with somebody, 
and they have a slightly streamlined flow where they show QR codes on the screen. 
You take one phone and take a picture of the other phone screen, 
and vice versa, 
and now you've exchanged public keys in person. 
And from that point on,
You've bootstrap your encrypted end-to-end communication. 
It also has an issue of, or it also has an approach of, pinning a public key. 
So once you know that a particular phone number has a particular public key, your phone remembers that, 
and if that ever changes, it'll complain to you. 
And then there are a couple of other solutions to this problem. 
PGP, one popular solution used to be popular a while ago, has this idea of a web of trust. 
So, like, I trust people who my friends trust. 
So if John has done an out-of-band exchange with, say, my professor, then I can probably email my professor because, 
like, I know that John trusts my professor and I trust John. 
So you got this chain of trust through there. 
That's one interesting approach. 
And then another model that's called pretty recently, 
as something that a tool called key base uses, this is a really neat whoops, there's a website called keybase.io, 
and they have a really interesting solution to this bootstrapping problem, which is social proof. 
So saying you probably have your friends on Facebook and on Twitter and whatnot, 
and it's probably pretty hard for an attacker to break into your friend's Facebook account at the same time as their Twitter account, 
at the same time as their hacker news account, 
and so on. 
And so there's this interesting way of binding public keys to a set of social identities such that you can retrieve a public key once you trust some number of social identities corresponding to your friend. 
We have links to these in the lecture notes if you want to see these things in more detail. 
So that's it for our security and cryptography lecture, 
and tomorrow's lecture will be on a random collection of topics that your instructors find interesting. 
So hopefully, see you in lecture tomorrow. 
I'll also be here for a couple of minutes after class if anybody has questions. 
Yes, okay, 
so John, feel free to leave if you have to leave, 
but I think nobody's using the classroom after us. 
I'm going to talk about one other interesting topic. 
So John brought up the fact that asymmetric key cryptography is slow and symmetric key cryptography is fast. 
And so in practice, you don't really use just a symmetric key cryptography by itself. 
It's usually used to bootstrap a more sophisticated protocol that you're using. 
One thing you might want to do is use symmetric key cryptography for signing encrypted email, right? We talked about that example. 
And the way that works isn't what you might have guessed from our straightforward explanation of asymmetric key crypto. 
Like, you don't just use that encrypt function up there and call it a day. 
In practice, what you do is you use hybrid encryption to use a combination of symmetric key and asymmetric key cryptography. 
What you do is, here, I'll draw this as a big block diagram. 
You take your message M, 
and then I have my public key that I want to encrypt for. 
But rather than just take these two things and pass it through the encryption up there, what I do is I use the symmetric key gen function to produce a symmetric key. 
Okay, I'm gonna, like, prepend this with "symmetric" so we can distinguish it from the public key key generation function. 
And then what I do is I take these two things, pass them through my symmetric encryption box. 
This produces the ciphertext, 
and now this by itself to the sender. 
Sorry, this by itself to the receiver who has the private key corresponding to this public key here, this is not really useful, right? 
Because this is encrypted with a symmetric cipher with this key K that came from this function that I ran on my local machine. 
So I need some way of getting this to the person who actually used to decrypt the email. 
And so what I do is I take this thing and now this email might have been big, 
and I use symmetric encryption with that because symmetric encryption is fast. 
But this key is small, like it might be 256 bits or something, 
so I can take this thing and encrypt it with a symmetric encryption using the public key, 
and this gives me an encrypted key. 
And this thing can be decrypted using the private key corresponding to that public key to reconstruct this. 
So this is on the sender's end. 
Now, the receiver gets this and this and kind of does these things backwards. 
So you start with the encrypted key and use asymmetric decryption using your public using your private key that corresponds to the posted public key to reconstruct this key that were used for the symmetric encryption box, 
and then use symmetric key decryption using that key that was reconstructed to take this ciphertext and produce the original message. 
So there's a kind of interesting example of how in practice symmetric and asymmetric key cryptography is combined. 
Question. 
So the question is, will you be using the same symmetric key generators? Yes. 
Okay, so you need to kind of agree ahead of time which box you're using here. 
So you might be like, oh, I'm going to use AES 256 GC up here, 
but this is a well-known function, 
and it's public. 
Like the attackers allowed to know all the parameters this function. 
This is the only secret thing that the attacker doesn't know, the key. 
Any other questions? Yeah, that's a really good question. 
What kind of data is important enough to encrypt? And I think that depends on your threat model. 
Like, who, what kind of attackers are you concerned about? What are you trying to protect against? 
So you might have the stance that you just don't really care, 
and that like anything you communicate with anybody is allowed to be public. 
I might be willing to post all my conversation with everybody for everybody to see publicly on the Internet. 
On the other hand, maybe you're doing some like security-sensitive works here, working under a contract for the US government, developing some sensitive military stuff. 
If you're sending that through the open Internet while you're traveling, 
you probably want to be pretty darn sure that no eavesdroppers or anybody else along the way can see what you're sending, 
and that whatever you're sending is in fact going to the right place, 
and that whoever is receiving it can authenticate that it in fact came from you. 
So you might be worried about all different kinds of adversaries depending on your scenario, from random script kiddies 
who are trying to break into websites to nation-state level attackers, 
and you'll need different types of techniques for defending against the different categories of attackers. 
Any other questions? Well, 
so hopefully, see some of you tomorrow for a random collection of things that John, Jose, 
and I find interesting.