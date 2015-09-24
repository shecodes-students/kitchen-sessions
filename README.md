


# kitchen-sessions
she.codes kitchen sessions Berlin

Documenting the 2nd generation (Nicole, Kathrin, Judith, Ela)

# Session #2 2015-9-24

# Session 2


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
We talked about the evolution of UNIX, starting at Bell Labs and MIT with Multics. The department of defence wanted an operating system with time-sharing (`mult` stand for multi, as in multi-user, multi-program). The project got cancelled after it became too complex. Dennis Ritchie and Ken Thompson, both working at Bell Labs were so much in love with the basic ideas (the UNIX philosophy) that they continued anyway under the name Unics (uni as in the opposite of multi). Sone the University of Berkeley got involved and improved on it, they made their own distribution (first one being illegal, infringing on AT&T's intellectual property ). They called it BSD (Berkeley Software Distribution). For the next release, they re-coded all the stuff that wes property of AT&T (the employer of Ritchie and Thompson), namely the userland tools (the project's name is GNU - Gnu Is Not Unix) and the University's lawyers wrote up the BSD license (it basically says: do what you want, we take no responsibilities. It allows commercial use, modification, redistribution and everything else you can think of). Since then, UNIX is free. FreeBSD, OpenBSD, NetBSD, PCBSD are all descendants of this distro (short for distribution). Darwin is another one, combined with their own kernel, this is the foundation of Mac OS X and iOS. Linus Torvalts wrote a kernel for IBM PCs. Together with the userland from the GNU project it is the foundation for Fedora, Suse, Debian, Ubuntu and Android.

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

`cd /foo/bar` runs `cd` with a single argument `bar`.
`ls foo bar baz` runs `ls` with three arguments
`pwd` runs `pwd` with no arguments.

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
