


# kitchen-sessions
she.codes kitchen sessions Berlin

Documenting the 2nd generation (Nicole, Kathrin, Judith, Ela)

- [Session 1](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-1-2015-9-17)
- [Session 2](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-2-2015-9-24)
- [Session 3](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-3-2015-10-01)
- [Session 4](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-4-2015-10-08)
- [Session 5](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-5-2015-10-15)

# Session #5 2015-10-15

## Markup languages

We talked about language as a means to describe stuff. Bees describe the location of a food source to their fellow bees by dancing for example. We use natural language to talk to humans and formal languages if we want to describe something to a machine. We discussed that "describing something to a machine" really means to describe something in a completely unamiguous way. (people also say: _well defined_). This is probably how our code of law should have been written.

We can use formal alnguages to describe shapes and colors and documents. We talked about the language `GML` developed at IBM in the 60s. It was the basis of SGML, which in turn was used by _Tim Berners-Lee_ at _CERN_ as a basis for `HTML`. We also discussed another derivative of SGML: XML. (extendably markup language), which can be used to describe all kinds of data (like a record in a database). XML is much stricter than HTML. We also talked about XHTML, which tries to merge HTML and XML.

None of the above languages are programming languages because they do not describe a process or an algorithm. On the web, we use the languages HTML, CSS and JavaScript. Only the latter one is a programming language. We discussed that, among other things, the various programming languages vary in how hard they are to read and understand for humans and machines respectively. One extreme being `machine code`, a programming languages that is "very easy" to understand for a computer (it is its native language, so to speak) and very hard to read for a human. So-called "higher-level" programming languages are easier for humans to read and write abutrequire translator programs (called `interpreter` or `compiler`) to be understood by machines.

## A first pair-programming session

I gave you an assignment to create a very simple web page in a shared editing session using `ssh`, `tmux` and `vim`. After creating the file, you opend it with the `open` command, which opened a web-browser on my machine, because my machine was hosting the shared tmux session. We discussed that a web-server would be needed on the hosting machine, so that each one of you could open the html file in her own browser on her own machine.

> On Linux the command `xdg-open` is used instead of `open` (on Macs) to open an arbitrary file. It starts the application that would have started if you would have double-clicked on the file's icon.

## GitHub

We made sure all of you have GitHub accounts and uploaded your public keys to your GitHub profile. (Remember: you can safely share your public key with everyone). I showed you a convenient way to get the public key of any GitHub user:

``` sh
$ curl https://github.com/USERNAME.keys
```
_replace USERNAME with the actual name of the user you are interessted in._

> The `curl` command downloads a URL and outputs its content to the terminal.

I showed you how to redirect the output of curl into a file.

``` sh
$ curl https://github.com/USERNAME.keys > a_file
```
> the file (called `a_file` here) will be deleted if it existed before. After this command ran, a_file contains what otherwise would have appeared in the terminal (the public key of USERNAME).

I also showed you how to append the output of a command to a file.

``` sh
$ curl https://github.com/USERNAME.keys >> a_file
```

> With two closing angle brackets (`>>`), the file (here: `a_file`) will not be overwritten. Instead the command's output (here: the public key) will be _appended_ to the file.

In the next session, I'll show you how you can use this technique to allow each other access to a specific account on one another's computers, so one of you can host the pair programming sessions in the future!

## The weekly image decoding excercise

You decoded a manually encoded image by another she.codes pair. This time it was a 1bpp (1 bit per pixel) RLE compressed image (also called _bitmap_). We discussed how RLE (run length encoding) works and how, in this case, it did not actually reduce the image data, but quite contrary, made it bigger. We talked about that it would have been more efficient when the image was larger. RLE works by reducing stretches of repeated pixels (a `run`). If instead it would replace repeated _patterns_ of pixels with less bits (sort of like we use abbreviations in natural langage), it would have been nore efficient. This is exaclty what the so-called `LZ77` algorithm does. It is used by the PNG image format and by compression programms such as `zip` and  `gzip`.

## Homework

1. Make sure you watched all the homework videos up to now and in addition watch these two:

  - [LZ77 Compression](https://www.youtube.com/watch?v=goOa3DGezUA)
  - [Command line basics: file compression](https://www.youtube.com/watch?v=SxlHniGiu90&list=PLVqGqrTs4ZWOhcApSWYIX_rnPMZDAClJa&index=4)

2. run `vimtutor` in your terminal.

# Session #4 2015-10-08

## Octal to Base64

We discussed your homework. None of you used the shortcut when converting from octal to Base64. We discussed that, depending on the values, it might be easier or not to convert a pair of octal digits to one base64 digit instead of going through binary as an intermediate step.

*Example*
- octal `00010203` can trivially be converted to base64: `ABCD`
- octal `57744372` requires to calculate 5*8+7, 7*8+4, 4*8+4 etc. Alternatively, you can trivially convert it to binary `101111 111100 100011 111010` and then go from these groups of six bits to the base64 symbol. (that's not really easier though, depends on taste)

We also realized that the base64 table is actually pretty easy to remember:
- 26 upper case letters
- 26 lower case letters
- 0 to 9
- the symbols `+` and `/`

## Revisting assymetric cryptography

We revisted Diffie-Hellman key exchange and the properties of public key cryptography. It allows you to
- encrypt a message that only the intended recipient can decrypt (use the recipient's public key)
- digitally sign a message you have written and make sure it is not modfied by a man-in-the-middle. (encode with your own private key)
- combine encryption with a digital signature

We discussed that encryption and authentication is especiialy important when remotely logging into a computer via the Internet.

## SSH

We talked about `ssh` and `sshd` (the ssh _daemon_) and how they use public key crypto to make sure the two partners of communication really are who they think they are, and also to create a shared secret (think of the color mixing video from last week). The shared secret (a random number) is then used as the _key_ to symmetrically encrypt the data that flows between the two computers. AES (advanced encryption standard) is used as the symmetric cipher.

> A `daemon` is a program that keeps running (is undead). Normal programs (like `cat`, `ls`) simply quit after they've done their job. `sshd`'s job however is to listen for incoming "calls" (think of a zombie sitting next to a telephone), it needs to run forever. Other words for _daemon_ are: server or service. You've probably read the word `daemon` in an email that could not be delivered and therefore bounced back to you. It was the `mail daemon` that sent it back to you.

We checked if you already have a key pair (private and public key) by looking for a `.ssh` directory in your home directoy. The key pair is saved there as two files, `id_rsa` (private key) and `id_rsa.pub` (public key).

We then created a key pair by running `ssh-keygen` and looked at your public key with the command

``` sh
$ cat ~/.ssh/id_rsa.pub
```

The public key is a very long string of bits (typically 2048 or 4096 bits). To save it in a text file in a space-efficient way, it is base64 encoded.

**Homework**
Watch [this video](https://www.youtube.com/watch?v=rm6pewTcSro&list=PLVqGqrTs4ZWOhcApSWYIX_rnPMZDAClJa&index=6) on ssh and scp.

## Key Exchange

We briefly mentioned the existance of key-servers and the problem of identity theaft. (I could upload a public key and claim that it is yours, then I would be able to decrypt secret messages people sent using that public key) [keybase.io](https://keybase.io) is a web-based service that uses github, twitter, facebook etc. to build trust that a user really is the person she claims to be.

We used `cat` and `pbcopy` (OS X) or `xsel` to copy the public key into the clipboard:

__OS X__
``` sh
$ cat ~/.ssh/id_rsa.pub | pbcopy
```

__GNU/Linux__
``` sh
$ cat ~/.ssh/id_rsa.pub | xsel --clipboard
```

> If you struggle with finding special characters on your German Mac keyboard, [this charts  helps](https://blogs.oracle.com/blogfinger/entry/mac_os_x_often_used).

You then simply pasted your key into an email and sent them to me. After I put your keys into the `authorized_key` file in `.ssh` directory of the home directory of a user called `pair` on my computer, you were able to log in remotely.

``` sh
$ ssh pair@terrorbird.local
```

You both used the same user account (`pair`) on my computer and were able to run commands on my machine without me even noticing.

> Even though both of you shared one user account on the same machine, you could work independantly from each other. Using `cd` for example did not affect the other user's working directory. That's because you both had your own shell. For each user that logs in, a new shell is started.

## Sharing a terminal session with tmux

For pair-programming however, we all want to work in the same shell and share one terminal session as if we would all be sitting in front of the same computer (but more convenient).

We discussed the program `tmux`. Originally intended as a _terminal multiplexer_ ("multiplex" means "many in one", think of a multiplex cinema), we use `tmux` mainly because it also allows to share a terminal among users on the same computer (users that ssh'ed into the same computer)

When you host a pair programming session on your computer, you start a tmux session by running

``` sh
$ tmux -S /tmp/pair new
$ chmod 777 /tmp/pair
```

As a guest, you join (attach) to a tmux session like this:

``` sh
$ tmux -S /tmp/pair attach
```
(you typically find this command line in the command history of the `pair` user, so after `ssh`ing into the host computer, you simply press the `up arrow` key, and there it is!

In a shared tmux session, all participants can type on their keyboard and the keystrokes are handled as if the host typed them. We all become one user (typically we use the user account of the person that hosts) and everyone sees the same terminal output, however in their own favorite font and colors, because your terminal emulator still rules over these aspects. To make this work, the shared area is as big as the smallest terminal in the session. All other terminals are artificcially made smaller by filling some area with dots.

We discussed that this form of pair programming is superior to screen sharing (transfering a compressed image of the computer desktop over the network) in low-bandwidth situations or when the host computer has no graphical user interface (GUI), like a server for example. It also is more inclusive; even people that use a [Braille display](https://en.wikipedia.org/wiki/Refreshable_braille_display) can participate. If you are developing an application with a GUI however, it becomes harder to have a shared view of your work's result.

We briefly experimented with opening `panels` (terminals in terminals) inside tmux and navigating between panels. Expect more `tmux` wizardry later.

## History of UNIX

I gave you a more detailed history of UNIX and we talked about some influencial persons and organisations and how development of UNIX and the Internet (not to be confused with the web) were closely connected.

Here's the timeline we went through:

- 1957-10-04 Launch of Sputnik 1 (first satellite) causes _Sputnik crisis_ in the US
- 1958-02 Eisenhower authorises creation of _DARPA_ as a response to the launch of Sputnik 1, 13 employees manage an initial budget of $520 Mio (2.92 billion as of 2015)
- 1958-06 _NASA_ act signed
- 1964 _Gneral Electric (GE)_, _Bell Labs_ and _MIT_ collaborate on _Multics_, a time-sharing operating system (OS) for a GE mainframe, MIT's MAC project is sponsored by DARPA since '63 with $2 Mio
- 1968 After visiting MIT and realising that keeping a phone line open is one of the major cost factrors, Donald Davies develops the concept of _packet switching_ and presents it in Edinburgh
1969 Davies publication and the fact that Bob Tylor needed to use three different terminals in his Pentagon office to remote-control three ARPA-sponsored computers across the nation, inspire ARPANET
- 1969 Unhappy with Multics, MIT AI staff (including Richard M. Stallman) start work on their own OS: _ITS_ ("the incompatible time sharing system", a name that expresses frustration), a completely open (as in: no security), wiki-like, post-privacy OS.
- 1969 Bell Labs leaves multics consortium because they consider it to be too comprex, complicated and unelegant
    - **Ken Thompson** (26) and **Dennis Ritchie** (28) start work on _Unics_ (pun: eunuchs) on a PDP-7 as a platform for their game "Space Travel".
    - Thompson creates the system programming language _B_ to support multiple target architectures
    - Thompson and Ritchie create _ed_, a modal editor with almost no feedback (as approriate for ASSR 33 TTYs)
- 1971 First release of _UNIX_ ("ics" now replaced with "ix"), entirely written in assembly for PDP-11
    - Because of antitrust laws, AT&T had to license the Unix source code to anyone who asked for it (for a little handling fee)
- 197x Ritchie focuses on **C**, the successor of B, Thompson focuses on Unix
- 1973 Unix is re-written in C
- 1973 Lynn Conway joins _XEROX PARC_ (The Palo Alto Research Center of Xerox, the copier company), at PARC the concept of a computer mouse and a graphical user interface (GUI) with windows and buttons was invented and presented to vistors like **Bill Gates** and **Steve Jobs** who both were very inspired by what they saw.
- 1975 First license of "Research UNIX"
- 1974 UC Berkeley acquires a UNIX source license, runs it on a PDP 11
- 1975 Thompson at Berkeley as visiting professor
- 1976 US Copyright Act, software is now protected by copyright laws
- 1976 Students at Berkeley start improving AT&T Unix and call it **BSD** (Berkeley Software Distribution), **Bill Joy** leads these efforts.
- 1977 Release of Version 7 UNIX (bourne shell replaces thompson shell)
- 1978 1BSD (an add-on to Unix 6) was sent to ~30 recipient.
- 1978 Conway teaches VLSI (very large scale integration, i.e. computer-aided chip design) at MIT
- 1979 MIT AI Lab dissolves, (founding of Symbolics and LMI), leaving Stallman without the community of **Hackers** (they invented the term; it means: programmers (that hack on keyboards), later the media incorrectly used the word to mean "people that commit cypercrime")
- 1979-05 2BSD contains csh and **vi** (both written by Joy) and Berknet, developed by Eric Schmidt as part of his master's thesis work. 2BSD was maintained until 2008
- 1979-09 3BSD contains the Unix port to VAX and a re-written kernel with virtual memory managment called _vmunix_
- 198x Lynn Cornway joins DARPA (from XEROX PARC)
    - DARPA's VLSI Project (founded by Lynn Cornway) funds further development of BSD and Stanford University Network (S.U.N. Workstation) to make it easier to design complex CPUs > 1k transistors. The basic idea is: separate design of logic circuits from the nitty-gritty details of physics, ley computers calculate the circuit layout, this requires software and hardware)
- 1980 Joy re-implements **TCP/IP** instead of integrating DARPA-sponsored BBN version
- 1981 _MOSIS_ service launched as one of the first services on the ARPANET (aka the Internet). Allows students to upload their chip-designs via FTP and they receive the finished chip by mail. This is the implementation of a VLSI system.
- 1981 Berkeley students publish their _RISC1_ design (DARPA VLSI funded, they uses MOSIS)
- 1982 Joy and Stanford students found _SUN Microsystems_
- 1984 After splitting up AT&T's local telephone businesses and therefor freed from antitrrust regulations, they start to sell Unix as a commercial product ($16k for administrative use by universities and $800 for education with an addition per-CPU fee)
- 1983-09 **Richard Stallman** launches the **GNU project** and starts working on gnu emacs, **gcc**
- 1985-10 Stallman founds the **Free Software Foundation** (FSF)
    - FSF funds development of **bash** and many ohter userland tools
- 1989 Release of BSD "Net/1" (TCP/Stack only under a free license for non-AT&T licencees)
- 1988 First release of NextStep, an OS based on BSD
- 1990 Using NextStep, **Tim Berners-Lee** writes the first web browser. It's name: _WorldWideWeb_
- 1991 Release of BSD "Net/2", whole OS with AT&T code (almost) replaced
- 1991 **Linus Torvalds** releases first version of **Linux**, a kernel to be combined with a GNU userland
- 1992 Lawsuit AT&T vs BSDi, uncertainty of legal status of BSD, boosts GNU/Linux popularty
- 1992 Thompson co-invents **UTF-8** (backward-compatibleish replacement for ASCII)
- 1993 **FreeBSD** project release of a PC port of BSD
- 1994 FreeBSD 2.0 is free of any AT&T code
- 2000 **Apple** releases **Darwin**, the core of **OS X** [_oh es ten_] and *iOS*, based on BSD and NextStep
- 200x Thompson works on the Go language at **Google** and now uses Linux
- 2005-06 Google acquires Android Inc. for at least $50 million
- 2011-10-12 Dennis Ritchie found dead one week after Steve Jobs, but without much media coverage. 
    - Fedora 16 (Linux) and FreeBSD 9 both are dedicated to his memory.

### Side notes:

- At SUN Bill Joy also worked on SPARC, NFS and **Java**
- Stallman co-authored EMACS and wrote texinfo, a replacment for Scribe, the first markup system
- Bob Taylor worked at NASA until 65. Joined XEROS PARC in 1970. Worked at **Digital Equipment Corporation** (DEC) until he retired. Taylor about the Internet: "Will it be freely available to everyone? If not, it will be a big disappointment"

- Playstation operating system is based on FreeBSD 
- Android is based on Linux
- GNU/Linux dominates on servers and supercomputers

## Message to the others

In this fun and popular section of each kitchen session, we talked about simple image compression using [RLE](https://en.wikipedia.org/wiki/Run-length_encoding) (run-length encoding). But: _shh!_, the others don't know yet!

# Session #3 2015-10-01

## Revisiting positional notation

I gave you the task of converting a largish decimal number (1052) to binary (base2), octal (base8), hexadecimal (base16) and base64. We found out: the higher the base, the shorter the resulting notation. Therefore, to transfer a string of bits over a transmission line that can only transport 7-bit ASCII (telegraphy) the most space-efficient encoding is base64. It uses (nearly) all of the printibale characters in the ASCII code table.

We also found out about a neat relationship between binary and octal, binary and hex and binary and base64

- there are 8 ways to combine three bits -> three bits can be represented by one octal digit
- there are 16 ways to combine four bits -> four bits can be represented by one hex digit
- there are 64 ways to combine six bits -> six bits can be represented by one base64 digit

So, in addition to base64 (space-efficient), hex is pretty nice too, because eight bits (=one byte, which is a commonly used grouping of bits) can be represented by two hex digits.

We found another relationship:

- there are 64 ways to combine two octal digits -> two octal digits can be represented by one base64 digit

We used these obervations to quickly convert a long binary number to hex, octal or base64 by grouping the bits into groups of 3, 4 or 6 repspectively. You can then convert the groups separately, which is much simpler than dealing with the whole thing at once.

### Homework:

- convert these binary numbers into octal, hex and base64
    - 101100101101
    - 100111011100
    - 1001001
    - 111110001

- convert octal 77043 to base64 and binary
- convert hex ACFF01 to binary

As you will see: numbers like 2, 4, 8, 16, 32, 64, 128 will occur all over the place. A fun way to get familiar with these numbers is to play [2048](https://gabrielecirulli.github.io/2048/) (_BEWARE_ it's addictive!)

## A Message From The Others

I presented you an encoded message from another she.codes team and we discussed possible encodings. First we talked about the base they might have used for number notation (tunred out to be hex) then we talked about the meaning of those numbers. It turned out that the message is an 8-bit-wide bitmap (or raster image) where each 1 represents a dark pixel and each zero represents a transparent pixel. After successful decoding, you prepared a message for another team (maybe using a different encoding ... we won't discolse it here)

### Homework
- think about how you could make an image file that uses more colors than just two.

## More userland commands

We talked about the userland tools `mkdir` (make directory), `chmod` (change file mode), `chown` (change owner) and the text file editor `vim`.

### Homework:
- make sure you've watched [Command Line Basics Videos](https://www.youtube.com/playlist?list=PLVqGqrTs4ZWOhcApSWYIX_rnPMZDAClJa) #1, #2, #3, #9 and #10
- play [vim adventures](http://vim-adventures.com/)

## Public Key Cryptography

We briefly talked about symmetric and asymmetric cryptography. In symmetric crypto, one key is used to encrypt and also to decrypt a message. Both parties (sender and receiver) need to agree on the same key and they need to keep it a secret. The [_Ceasar Cipher_](https://en.wikipedia.org/wiki/Caesar_cipher) is an example for symmetric crypto (and also an example for a very weak cipher that can easily be broken).

> In cryptography, a **cipher** (or cypher) is an algorithm for performing encryption or decryption -— a series of well-defined steps that can be followed as a procedure.

We talked about taht symmetric ciphers only work after the secret key has been transferred between sender and receiver, whcih in turn requires a secure communication channel. Historically, this is solved by the two parties meeting in person to exhcnage the key.

We discussed that it would be much more practical if tehre was a way to securely exchange a secret over an unsecure channel.

### Homework
  - watch [this video](https://www.youtube.com/watch?v=3QnD2c4Xovk) on Diffie-Hellman Key Exchange.
  - With [this video](https://www.youtube.com/watch?v=GSIDS_lvRv4) on asymmetric cryptography

# Session #2 2015-9-24

## The relation between binary, octal and hexadecimal
We talked about how it is much easier to convert between binary and octal, or binary and hexadecimal than it is to convert between decimal and ... well, really everything else. It is our so familiar decimal system that is the outsider here. The reason for this: 3bits fit exactly into one octal digit and four bits fit exactly into one hexadecimal digit. 
>There are 8 ways to arrange 3 bits and octal has a symbol for each one of them. There are 16 ways to arrange 4 bits and hexadecimal has a symbol for each one.

## base60 and base64
We also talked about base 60 (used in clocks, like 1:43:32, meaning one hour, 43 minutes, 32 seconds. This is a base60 system where each symbol consists of two decimal digits. This is totally sick if you think about it. Certainly more complex than anything you find in the tidy world of programming!
There's a much less familiar one: base64, the biggest number system we can do using only visible characters from the ASCII table. It is the most compact way to transfer a very large sets of bits over a traditional 7bit telegraphy line.

We talked about how this could be used for a telegraphy-age telefax, a thing that actually did exist! We talked about the _stack of encodings_ that is necessary to make this work:
- Pixels are encoded as ones and zeros
- six of them are grouped an their binary value is used to map them to a character in the set of characters used by base64 (2^6 = 64)
- these characters (all printable ASCII characters) can now be sent over a traditional telegraph wire, just like any other telegram.
- the receiver does the above steps on reverse.

This is a very common pattern you find everywhere in computing: different layers of encoding on top of each other.

## A Closer look at the Keyboard
On a terminal most keypresses result in a 7-bit binary number (the character's ASCII code) to be sent over the serial output line. However, there are some keys that are meant to be pressed only in combination with another key. They change that other key's effect by making accessible a 2nd or even 3rd key assignment. Those keys are called _meta-keys_. On the original terminals there were two meta-keys: Shift and Control.

On modern computers there are some additional meta-keye:, Option (Apple) or Alt (other brands, alt stands for Alternative), Command (Apple), Alt-Gr (other brands, "Gr" stands for graphics) and on some Laptops: Fn (short for "Function"). On modern computers the effect of a meta-key combinations is defined by the operating system and/or the application that owns the active window. 

## Control Codes defined by ASCII
When the Control-key is pressed in combination with a letter (for example Control-A) on a terminal (or with the window of the terminal emulator being the active window) then a well-defined 7-bit value is sent. For Control-A, thats a 1, for Control-B it's a 2 and so forth all the way to 31. All of these _control codes_ or _control characters_ have a special meaning that is defined by the ASCII standard: 13 (Control-M) for example is Carriage Return (CR), that's the same as pressing the Enter key. Control-J (10) is Line Feed (LF), Bell (7) is Control-G and makes the receiving end beep! The `escape` character is Control-[.
 
> **How keystrokes are printed in documentation written in ASCII**
>`Ctrl-J` , `C-J` or `^J` means "hold down Control and then press J, then release Control".

## ANSI Escape sequences
We talked about that Video Terminals, most famously the terminals built by Digital Equipment Corporation (DEC), introduced a new set of features that were impossible in the world of TTYs (the typewriter-like terminals). Things like clearing the screen or moving the cursor upwards or anywhere on the screen, setting the text color or enabling underlining or bold characters. The dilemma: there's no space left in the ASCII table for additional control characters. We talked about how this was solved in a similar way to how Baudot solved the problem of not having enough code-points for both, letters _and_ numbers. The solution is, to switch the receiver into a different _mode_ by sending a mode-switching character or _escape_ character. The idea is to escape from normal interpretation, sort of like saying: what you receive now is not to be interpreted in the normal way. As usual different vendors invented different proprietary extensions until the chaos became a problem for operability and ANSI (American National Standards Institute) solved the problem by releasing an official standard.   [read more on Wikipedia](https://en.wikipedia.org/wiki/ANSI_escape_code#Sequence_elements)

## Terminal Emulators
We talked about the difference between emulation and simulation: An emulator behaves _exactly_ like the thing it mimics, while a simulator tries to do act similar. (A flight simulator does not actually fly).

Originally, video terminals were connected via serial lines to a mainframe (big, noise computer) in the cellar. Today both devices are merged into one (laptop). This fact is a source of confusion. Think about it this way: Like a compact stereo, or a cameraphone, your laptop is really two devices in one: a terminal and a computer. They sort of share the keyboard. When the terminal window is in focus, the keyboard belongs to the terminal, when some other application is in focus, the keyboard act differently. A terminal emulator is like a very fancy video terminal. It supports also the ASCII control codes and the ANSI control sequences and you can select font, text size, background color and text color.

Originally the other end of the terminal, the thing you "chat" with, was a program that was already running on the computer in the cellar. Today that program (the _shell_) is started for you the moment you start the terminal emulator. This is another source of confusion.

> **Important Distinction**
> The terminal emulator and the shell are two different things. The terminal emulator is a piece of virtual hardware, a dumb thing, not programmable. It simply sends all keystrokes over a virtual wire, and when it receives characters over another virtual wire, it displays those characters and moves the cursor. That's all. The shell, on the other hand, is your _dialog partner_, the thing you converse with, a program whose sole purpose is to interpret what you write and act on it. Think of it this way: The terminal is a phone, the shell is the person on the other end.

## Kernel, Shell, Userland

We talked about the three components of a UNIX system. The _kernel_ is the overlord of the system, it makes sure that users that use the computer at the same time (_concurrently_) are protected from each other and also that programs that run concurrently cannot overwrite each other's data. Furthermore it makes sure that the time-sharing slices are shared in a fair manner. (whatever "fair" means).

The shell is your interface to the machine. It let's you start programs and combine them, building complex data flows through multiple programs and more. You cannot interact with the kernel directly. Put programs can. The kernel might however disallow certain programs from doing certain things, depending on their privilege level. (more on this later)

The _userland_ is where those programs live. It's also where the shell lives, because the shell simply is a program like all the others, it's just the one that happens to be in charge of the terminal input/output connections. Consequently the sum of all those little programs (like `ls`, `pwd`, `cd`) is called the userland.

## UNIX family tree
We talked about the evolution of Unix. A joint-venutre of Bell Labs, MIT and General Electric started developing  Multics. DARPA (Defense Advanced Research Projects Agency) wanted an operating system with time-sharing (`mult` as in multi-user, multi-program). Bell Labs left the consortium after it became obvious that Mutics would become too complex. Dennis Ritchie and Ken Thompson, both working at Bell Labs were so much in love with the basic ideas (the _UNIX philosophy_) that they started a new, much simpler project with the same basic ideas. They called it "Unics" (uni as in the opposite of multi). The University of Berkeley made their own distribution (the first one infringing on AT&T's copyright). They called it BSD (Berkeley Software Distribution). BSD received DAROA funding. For the next release, they re-coded all the stuff that wes property of AT&T (the employer of Ritchie and Thompson) and the University's lawyers wrote up the BSD license (it basically says: do what you want, we take no responsibilities. It allows commercial use, modification, redistribution and everything else you can think of). Since then, UNIX is free. FreeBSD, OpenBSD, NetBSD, PCBSD are all descendants of this distro (short for distribution). Darwin is another one, combined with their own kernel, this is the foundation of Mac OS X and iOS.

A former emplyee at MIT's AI Lab, Richard Stallman founded the Free Sftware Foundation (FSF). He started to re-implement UNIX' shell and userland tools and was soon joined by many other programmers. That's what he calls the free software movment. The project's name is GNU (stands for "GNU's Not Unix"). Together with a kernel for IBM compatible PCs written by Linus Torvalds, this is the operating system known as "Linux". (However it _should_ be known as "GNU/Linux" because Linux is really just the kernel)

So, all the operating systems are closely related, they all are Unix, or _unix-like_. The one exception being: everything from Microsoft. Windows is a descendant of DR-DOS by Digital Research which in turn is related to CP/M.

## foo, bar, baz
We talked about where the word `foo` comes from (a comic from the 30s) and what it means (nothing, it's nonsense). We also talked about the US military term FUBAR (fucked up beyond all repair). These two things seem to have interacted in people's head and lead to `foo` and `bar` being used very frequently as placeholders for words. Sort of like `lorem ipsum` but for single words rather than a whole body of text.

## file paths
We talked about relative and absolute paths. You recognise an absolute path by a leading slash (`/`) or tilde (`~`), where tilde is short for "my home directory" and slash stands for the _root_ of the file system (the uppermost directory level). The root has no name, it just has a symbol (`/`).

We talked about `.` and `..` as placeholders for the current directory and the _parent_ directory (one above the current one)

## The grammar of bash
We talked about the basic grammar of a bash command:

`foo bar baz`

this would run the program called `foo` and pass `bar` and `baz` as arguments to the program. The program can do with these arguments whatever it wants. It's the program that defines the _semantics_ of its arguments, to the shell they are just strings of characters with no particular meaning.

A few examples

- `cd /foo/bar` runs `cd` with a single argument `bar`.
- `ls foo bar baz` runs `ls` with three arguments
- `pwd` runs `pwd` with no arguments.

Even though the shell (bash) has no clue what the arguments are, it sort of guesses that they are paths and therefor helps you creating those paths. (Most of the time it is correct, many arguments are actually paths). It helps you by completing path segments when you press the tab key or by displaying a list of all possibilities when you double-tap the tab key.

## de-facto argument grammar
Event though each program can decide how it wants to interpret the characters passed in as arguments, there's a convention:

- argument staring with two dashes (`--`) are the long form of a switch
	- `ls --all --list`
- arguments starting with a single dash (`-`) are the short form of a switch
	- `ls -l -a`
- short forms can be combined (shortcut of the shortcut)
	- `ls -la`

A switch (also called _flag_) is a one-bit-value. It is either turned on or off. Some are turned on by default and you can turn them off if you want, some are turned off by default and you can turn them on.

For `ls` both flags `list` and `all` are turned off by default. In the examples above, they are turned on.

## Basic UNIX commands: navigation
We talked about `pwd`, `cd` and `ls`

---

# Session #1 2015-9-17

### Basics

#### Numbers

- [The Weird Truth About Arabic Numerals](https://youtu.be/Ar7CNsJUm58)

- [Decimal, Binary, Octal, hex – the sheep video](https://youtu.be/5sS7w-CMHkU)

#### Symbols

We talked about how to pronounce the special characters, some of which have more than one name.

- [octothorpe video](https://www.youtube.com/watch?v=HEVOM0VycMI)

## History of Computing

We talked about how information technology is the convergence of two threads: the need to communicate over long distances on one hand and the desire to get rid of/accelerate dull calculation tasks. We focused on the history of communication first.

### Long-Distance Communication

#### Visual Communication
We talked about the history of long-distance communication. Starting with smoke-signs, then people on hills with flags (semaphores), then chains of semaphore buildings on mountain tops. Here we introduced the term `relay` to refer to a link in a chain that re-amplifies a signal or message. (the term comes from sports)

- [Monty Python Semaphore video](https://youtu.be/WmudPExkKJ4?t=45s)

#### Electricity
We talked about the discovery of electricity (rubbing amber on a cat, electric fish) and electromagnetism, how "electricians" were booked by party organisers to entertain people. We talked about the battle between the Italians Volta and Galvani about the existence of _animal electricity_.

We talked about how Galvani doscovered the battery by accidents by using two wires made of different metals.

We talked about early experiments to use electricity for communication purposes in Europe and that Samual Morse was introduced to this idea on a journey.

#### Morse
We talked about the tragic story of the death of Morse's wife which led to the invention of the `Morse Code` and the first digital network for long-distance communication in the US.
We talked about how electro-magnetic switches were used to relay the digital signals and that those electromagnetic switches are therefor called `relay`; together with a battery, they are the main component of a Morse relay station.
We talked about how batteries and relays made possible the first trans-contiental telegraphy line in the US and the first transatlantic cable.

#### Baudot
We talked about how in 1870, French telegraph employee Émile Baudot improved upon the Morse system with a more convenient input device (5 key piano-like keyboard) which improved usability by putting less stress on the human sender because the singnal generation is done by a clock-driven device rather than the human operator herself. Baudot's decision to use impulses of just one length (as opposed to morse) turned out to have a huge impact, because it is the key to automation.
We talked about the 5-bit Baudot code and how its design was influenced by ergnomic concerns. The clock speed in a Baudaut system is called `baud rate` and is equivalent to `bits per seconds` (`bps` for short). Until the 90s modem speed was meassured in baud (e.g. "I have a 2400 baud modem').

We talked about parameters (baud rate, start bit, stop bit) that needed to be known to the sender and receiver prior to transmission in order for the system to work.

We talked about the start bit being used to start the clock at the receiving end, therefor minimising the problem of clocks not running 100% at the same speed.

Baudot invented `serial communication`: the bits that make up a character are being input in parallel (by pressing multiple keys at once), but they are then sent in sequence (=in series, serial) over the wire. This same priniciple is used until today in USB (`universal serial bus`, Ethernet (network cables), SATA (the cables that connect harddisks to the motherboard inside the computer) etc.

#### 5-bit Paper tape
We talked about the resemblance of the Baudot code table to a paper tape with holes in it and how such paper tapes were indeed used to further improve usability of devices used for sending telegrams over the telegraph network.
These new devices resembled mechanic typewriters, but instead of printing characters on paper, they would punch holes (according to the Baudot code) into paper tapes. The resulting tape would then be "read" by a simple machine that  replaces the human at the "Baudot-piano". We talked about how this setup, using tha paper tape as a temporary storage or `Buffer`, allows the operator to type at a higher speed then the machine can send, building up some slack in the tape which allows her to have a zip or coffee or tiny breaks until the sending machine caught up and the paper tape becomes straightend again.
The paper tape in this setup is a `FIFO` (first-in, first-out) memory buffer. (In contrast, with the original Baudot system, the operator was forced to type in the "accords" at the exact pace of the machine).
The paper tapes can also be used to archive messages, re-sending them later or relaying messages received from one station to another.

- [5 hole paper tape](https://youtu.be/JafQYA7vV6s)

#### Routing
We talked about how addresses at the start of a telegram message could be used to route a telegram through a network of stations. We gave the example of a message originating in Frankfurt that is transmitted to a station in London, then to the transatlantic cable station in Scotland, then to New York and then to San Francisco. We talked about how this routing could be done manually by humans or even automatically by machines when using a machine-readable station-code. (sort of an IP-Address)

#### The Victorian Internet
We talked about how Telegraphy company became wealthy and powerful and the technology having a social an economic impact (banking/stock market), sort of like an early Internet.

- [Western union commercial](https://youtu.be/4ziHpB-Dw7o)

#### ASCII
We talked about how different companies and vendors extended the Baudot code (to get rid of figure mode/letter mode?) to 7-bit code. They all did it in different ways, which hurt inter-operatability. To solve this issue, ASCII was introduced. (We found out that is happend in the 60s, so the cain of events probably was a bit different than presented here.)
We talked about how ASCII's design could be free of ergonomic concearns, because of a better de-coupling between the human and the mechanics of signal-generation. Instead it is neatly organised into sections (lower-case letters, upper-case letters, numbers, control-codes)
You can identify the section by the first couple of bits and the rest of the bits tell you the number of the symbol within that section (1 for A etc). This makes it possible to read ASCII without having to memorize the entire code table (as opposed to Morse/Baudot)

- [Tom Scott Video: How to read binary ASCII](https://youtu.be/wCQSIub_g7M)

#### Proprietary extensions to ASCII
We talked about how the 7-bit ASCII code was extended with an eighth bit to make room for German Umlauts, French accents, the Copyright symbol etc., and how, again, each vendor introduced their _own_ code. We all remember the messed up Umlauts in emails sent from a Windows computer to a Mac. Lots of  different 8-Bit encodings co-existed. They had names like `latin-1`, `mac-roman`, `iso-whatever`.

#### Unicode
We discussed how `Unicode` tries to solves this issue _once and for all_ by simply assigning a number to each character, glyph and symbol any human ever came up with. (many tens of thousands, including musical notes, emojis, dead languages). Unicode explicity _avoids_ to specify a number of bits to transfer those numbers. The problem of creating bit patterns for each `codepoint` (symbol number) is left to other standards.
We talked about how Microsoft uses a 16 bit code (sort of learning nothing from history) while UTF-8 is an elegant solutions that, by using 8 bit as default, minimises the amount of data for English text and expands the number of bits per character as needed.

- [Tom Scott's UTF8-Video](https://youtu.be/MijmeoH9LT4)

### Dull calculations

#### Ada Lovelace and Charles Babbage 
We talked about how Charles Babbage tried to build a machine that does simple calculus and how it was Ada Lovelace (his pen-friend) that realised the full potential of this idea and who envisioned the first programmable machine and thus invented the concept of software.

- [Great Minds – Ada Lovelace](https://youtu.be/uBbVbqRvqTM)

#### Konrad Zuse
Konrad Zuse worked at an airplane company. He had to calculate the most efficient shapes for wings. Tired of this work, he started to build a machine that would do this for him. This happened in his parent's living room in Berlin. Konrad used mechanical switches in his first version and Relays (electromagnetic switches) in the later versions. The 3rd version (Z3) he built during World War II was the first working computer.

- [Die Zuse Story (German)](https://youtu.be/TUPj-Aep9PI)

#### "Computer" as a job title

  - [Great Minds – "human computer" Astronomer Henrietta Leavitt](https://youtu.be/2FrY6gRPC7k)

We talked about how during World War II female students were recruited as computers to perform tedious calculations. They made tables of numbers needed at the front.

#### Women of the ENIAC
We talked about how one of the first electronic computers, the ENIAC, was built in the US using tubes, and how five of the human "computers" had the job of turning their knowledge of how to calculate those number tables into instructions that could be performed by the machine. They were the first actual programmers.

- [Top Secret Rosies: The Female Computers of WWII - Trailer](https://youtu.be/qK9OQ7yL-60)
- [Grace Hopper at Letterman](https://youtu.be/1-vcErOPofQ)

#### Crypto Wars

Meanwhile in Britain, a group of scientists including Alan Turing were building machines to break the encryption code used by the German military. Alan Turing had a great impact on the outcome of the war, nevertheless he was pushed into suicide by homophobic authorities. Turing started the field of Computer Science.

- [My favourite Scientist – Alan Turing](https://youtu.be/u3Ue7r5Xsyo)

#### Graham Bell
We talked about how Graham Bell invented the telephone, using money that was supposed to go into research for the `multi-telegraph`, a device that would transfer multiple telegrams over the same line at once. The patent lawsuit that followed the invention of the telephone ended in the foundation of AT&T. AT&T owned the monopoly for telephone equipment and networks throughout the entire US until the mid 1980ies, accumulating a lot of money.

#### Invention of the Transistor
AT&T had a famous research and development department: Bell Labs (named after Graham Bell). At Bell Labs a huge number of important inventions were made and for decades many of the nobel prizes went to employees of Bell Labs. Among this inventions: laser, radar, transistor, UNIX.
We talked about how a transistor is built from metal and Silicon, a semi-conductor, and can replace relays and tubes at a much lower cost and much higher lifespan. It also switches a lot faster and consumes less energy.

- [How does a Transistor Work?](https://youtu.be/IcrBqCFLHIY)

#### Integrated Circuits
We briefly talked about how a novel photographic process lead to entire electric circuits on a tiny piece of Silicon (a "chip" of silicon). We looked at some, packaged in black plastic with a lot of metal legs, giving them the appearance of a bug.

- [historic film: Fairchild Semiconductor, inventor of the IC](https://www.youtube.com/watch?v=z47Gv2cdFtA)

#### Time Sharing
We talked about that the first computers were so bug and expensive that universities or companies could only afford one of them (if at all). We talked about how users had to wait a long time before the computer would run their programs. Cpmputing time was the limiting factor. Usually, depending on how important you were, you could hope that your program would maybe run sometime during the night and you would pick up the printed result (usually an error message) the next day. The idea of time sharing is to divide the computer's time into very short time slices and run a different user's program in each of those slices. By connecting multiple terminals to the same computer, users could work interactively and at the same time with one computer.

- [Time Sharing at MIT, historic film](https://youtu.be/Q07PhW5sCEk)

#### Magnetic storage
We briefly talked about the roles of magnetic tapes, harddisks and floppy disks to store bits. They were used instead of paper tape when large amounts of data needed to be stored and accessed (relatively) quickly.

#### Invention of UNIX at Bell Labs
We talked about how Dennis Ritchie and Ken Thompson worked on a time-sharing system at Bell Labs and how the project (a joint-venture of AT&T, MIT and General Electric) was finally cancelled. Dennis and Ken went on anyway because they liked the idea (and needed it to run the game they were developing) and re-wrote a much more simple system that first was called Unics, then UNIX. Unix turned out to take over the world.

- [Brian Kernighan about working at Bell Labs](https://youtu.be/QFK6RG47bww)

#### Video Terminals
 Cathode Ray Tubes (CRT) are being used as screens for TVs and for a new generation of "Glass" terminals or _video terminals_. The VT100 became a popular model in the the 70s. These video terminals almost look like personal computers, but they remain as "dumb" as the earlier models that look like mechanical typewriters (teletype, or TTY for short). The print everything they receive on a serial input line (basically the system that baudot invented) and, if you press a key, they send it over a serial output line. You can connect two of these terminals together to have a simple chat system, or you can connect them to a computer to feed keystrokes as input into a program and visualise the program's output.

- [The Unix revolution – computing in the 70s](https://youtu.be/-rPPqm44xLs)
