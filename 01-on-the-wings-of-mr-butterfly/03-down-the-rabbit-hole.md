## Down the rabbit hole

"*What we have here is a keyboard, a very special one.*" - Mr. Mouse said, then
continued: "*It's just a bit of electronics now, but we'll add a little bit of
magic we call the `firmware`, and it will be able to do wonders! I hope you are
prepared for a great adventure, kids!*" - and prepared they were, or so they
thought. The Twins sat beside Mr. Mouse, who leaned forward, plugged the new
device in, and started typing on one of his other keyboards furiously with his
little paws. Meanwhile Mr. Butterfly sat on the back of his chair.

 <!-- TODO -->
 ![Working on the keyboard](../data/frame.png)

A few minutes later, Mr. Mouse paused, and asked the Twins: "*In all this
excitement, I forgot to ask you: do you wish to see what we've been up to with
Mr. Butterfly? Or are we boring you?*". The Twins immediately expressed their
desire to see what their hosts have been working on, and so Mr. Mouse continued:
"*Splendid! We'll start from the beginning, so you can do this at home too,
would you want to!*".

### Getting started

An so Mr. Mouse began to explain how to start:

*Our keyboard uses a micro controller that is compatible with Arduino. You
don't need to know what either of these are - though I'm sure Mr. Butterfly
would be happy to explain, if you are interested! -, it is enough to know that
we will be using Arduino-provided tools to program the keyboard. We want to
program it, so it does more than just look pretty. You see, we need a program to
drive the electronics. You can think of this as if the hardware, the electronics
on this table are the body and brain of the keyboard, and the program are its
thoughts. We want it to think, right? Of course we do.*

*So first, we install the tools we need, starting with [Arduino][arduino]. It is
as simple as downloading it, and installing it like any other application. I
won't bore you with the details, this is the easy part! Afterwards, we install
something called [git][git], a version control system. We use this to manage the
source code of the program that will drive our keyboards. Now, installing git is
a little bit more involved, because it depends on what operating system we are
using. For now, lets work on Linux, because that's what I have here, which I can
show you. Fortunately for us, `git` came pre-installed, so we have nothing to do
this time! Either Mr. Butterfly or myself can show you later how to install
these tools on other operating systems, if you'd like to know. But for now, lets
get to the hardest part of it all: getting hold of the sources!*

 [arduino]: https://www.arduino.cc/en/Main/Software#download
 [git]: https://git-scm.com/downloads

*Depending on how we use `git`, via the command-line, or a graphical tool, will
determine how we get ahold of the sources. Myself, I love to type, so I'm using
the command-line. The important part, however, is where to get the sources from:
we need to grab them from two places. Why? Because our firmware - that's what we
call the program running on the keyboard - is built up in a modular way, from
small components we can piece together. It's not a big pile of complex code we
must wade through if we want to change it, but small, discrete parts. Think of
it like a kit of building blocks! You can arrange the pieces in whatever way you
want, and build anything you can imagine! And so is our firmware built, too. We
first have to download the building blocks, and then we download a kind of...
lets say, schematic, that tells our tools how to put the pieces together.*

Mr. Mouse paused for a moment, seeing the Twins aching to ask a question: "*So,
this program is like a LEGO? Where we have the elements, and a guide on how to
put them together?*" "*Yes, exactly.*" - Mr. mouse answered. "*And we can choose
not to follow the guide, but build something on our own?*" - they asked in
tandem. "*Precisely!*" - Mr. Butterfly exclaimed enthusiastically from the back
of the chair.

"*Lets see this in action, shall we?*" - Mr. Mouse continued, and without taking
a breath, he went on explaining the next steps:

*One thing to keep in mind, is that we must make the source code of our program
accessible to the `Arduino` tools, so we will download them to a place where it
can find them. On Linux, this is the `~/Arduino` directory. We need to place our
building blocks in a special directory, so that the tools can recognise them as
such. Thus, we create the `~/Arduino/hardware/keyboardio` directory, and clone
the sources there, with the following commands:*

```bash
mkdir -p ~/Arduino/hardware/keyboardio
git clone https://github.com/keyboardio/Arduino-Boards.git \
          ~/Arduino/hardware/keyboardio/avr
```
