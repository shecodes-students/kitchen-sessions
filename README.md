


# kitchen-sessions
she.codes kitchen sessions Berlin

Documenting the 2nd generation (Nicole, Kathrin, Judith, Ela)

- [Session 1](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-1-2015-9-17)
- [Session 2](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-2-2015-9-24)
- [Session 3](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-3-2015-10-01)
- [Session 4](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-4-2015-10-08)
- [Session 5](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-5-2015-10-15)
- [Session 6](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-6-2015-10-22)
- [Session 7](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-7-2015-10-29)
- [Session 8](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-8-2015-11-06)
- [Session 9](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-9-2015-11-13)
- [Session 10](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-10-2015-11-20)
- [Session 11](https://github.com/shecodes-students/kitchen-sessions/blob/master/README.md#session-11-2015-11-27)

# Session #11 2015-11-27

## vim and piping

The solution to last weeks' homework is:

1. Pipe a list of usernames in vim's current buffer through a shell command that downloads the public keys of github users and replaces the content of the buffer (the names) with the keys:

  ```
  :%!xargs -I NAME curl https://github.com/NAME.keys
  ```

2. Do the same as above, but only for the line where the cursor is:

  ```
  :.!xargs -I NAME curl https://github.com/NAME.keys
  ```

3. Map the `F2` key to replace a github username with her public key:

  ```
  :map <F2> :.!xargs -I NAME curl https://github.com/NAME.keys<CR>
  ```

As you can see, the solution to 1. and 2. only differ by a single character. It is `%` in the first case and `.` in the second. This character is what vim calls the _range_. With this character you specify what lines should be used for an `ex mode` command. (The `ex mode` is the mode you are in when you see the `:` prompt near the bottom of your terminal). A dot (`.`) means "only the current line" (the line the cursor was in when you entered `:` to get into `ex mode`), `%` means _all_ lines.

There are more ways to specify a range. You can simply give vim a number, then it will only use that line for the following command.

To replace the username in line 3, you could do:
```
:3!xargs -I NAME curl https://github.com/NAME.keys
```

And finally, you can specify a range by giving two line numbers: where the range starts and where it ends. You separate the two line numbers with a comma:

- `1,2` lines  one and two
- `1,6` lines 1,2,3,4,5 and 6
- `2,$` everything from line 2 (`$` means: the last line)

> When you are in `normal mode` and press `$`, you get to the end of the current line. Inside a range in `ex mode` however, `$` stands for the last _line_. Lilli memorizes this with a sentence like this: "From 0 to $ is like the American Dream!"

### Piping with `:write`

During your research to solve the first homework challenge, you came across something that almost worked:

```
:w !xargs -I NAME curl https://github.com/NAME.keys
```

This downloads all the keys of the github users in the buffer. so it almost looks like a solution. However, the buffer is not changed. This command simply outputs the keys to the terminal, not to the buffer.

`:w` is short for `:write`. The `write` command takes the content of a buffer and writes it somewhere, usally to a file. If you do not specify a file, it saves it to the file the buffer was read from previously. This is what happens when you type `:wq`, before vim then closes the buffer and exits.

If you give it a filename however, like so: `:w myfile` it will write the buffer content to the file "myfile". You can combine this with a range! `:2,5w part` would write lines 2, 3, 4 and 5 to the file "part". If you do not specify a range, `%` (all lines) is implied.

But what happens if you prefix the filename with an exclamation mark, as in `:w !cat`? It tells vim that `cat` is not a filename. Instead it is the name of a program you want to run. In this case, vim will run the program and _pipe_ the buffer content into the program's `stdin` channel. In the case of `cat`, this results in viewing the buffer content in the terminal.

> The exclamation mark (`!`) has two meanings in vim. If you postfix a command with it, like in `:q!` it means: "Do it! I know what I am doing", so you can force vim to do something it is not quite convinced of, like quitting without having written before. There is no space between the `q` and the `!`. The other use is to indicate that a _parameter_ to `:read` or `:write` is to be interpreted as a command line (program name), and not as a filename. Here you _prefix_ the _parameter_ with `!`, as in `:w !cat`. Notice: there is a space between `w` and `!`.

### Piping with `:read`

Instead of writing, `:r` reads the buffer from a file. Just like with `:w` you can specify an optional range. For example `:4r stuff` would insert the content of the file "stuff" after line 4. If you do not give a range, the file content will be appended after the last line.

Just like with `:w`, you can use `!` to indicate that you want to run a program instead of reading from a file. In the case of `:r` however, the program _produces_ the buffer content instead of _consuming_ it. The buffer content is being _read_ from a program (or command line).

For example:

```
:0r !date
```

Runs the program `date` and inserts its output (the current time and date) before the first line in the buffer. (that's actually handy)

### Going full circle: filtering

If you ant to _write_ the buffer to a command _and at the same time_ want to _read_ the commands output, you are going full circle. You pipe your buffer _through_ a command. Another term that is used for this is: You _filter_ your buffer through a command. That's what you did in your homework. The syntax is almost like with `:r !command` and `:w !command` (Note: `command` is a placeholder for the actual command line). But you neither specify `r` nor `w` and the range is not optional but mandatory. That's the story behind `:%!command`.

And now: pad yourself on the shoulder for having mastered it!

Homework:
- map the key sequence `,date` to insert the date after the current line.
- use `:r` and `:w` to read and write parts of the buffer from/to files
- make a file with names, filter them through `sort` and `uniq`.

## bash scripting

We piped stuff through various programs. Now we tried what happens if we pipe some text through `bash` as in:

```
cat myfile|bash
```

We've seen: `bash` executes the commands in the file, just like it does when we enter them on the keyboard! When thinking about it, this is not really a surprise. `bash` does what it always does: it reads characters from `stdin` and once it sees a newline character, it tries to make sense of the line and runs the programs mentioned in it. The only difference now is that `stdin` comes from a file and not from the terminal's keyboard.

The cool thing is that we no longer need to memorize and enter a complicated command line like `cat names|xargs -I curl ....`. Instead we can write it into a file and run it when necessary!

Try to do that. And when it works: Congratulations! You just wrote your first program!


# Session #10 2015-11-20

## TEN!

Because this is our tenth session, we did something special: First we played Jeopardy (not really – we played a quiz and had the Jeopardy theme song running in the background). I was very happy with how much you remember from the previous ten weeks!

In one team, We then had a retrospective where everyone wrote down on cards the things that were good, the things that were bad and the things that were ugly. The only negative thing (thus far) was that the homework assignments come too late (too close to the next session). We agreed that when they are late, we postpone checking hoemwork until one week later.

I am looking forward to what you can do ten more sessions from now!

## More on how UNIX programs communicate

### Standard I/O

Some time ago we established that the terminal internally really consists of two distinct devices:
- a keyboard connected to a Baudot-style serial transmitter, that, with each keypress, sends a series of bits (originally 7 bits according to `ASCII`, nowadays a varying number of bits according to `UTF-8`) down a transmit wire
- a Baudot-style receiver, that, when it receives a series of bits, displays the corresponding symbol (character) on a screen and advances a cursor. It also interprets special control characters and ANSI-Escape sequences to control cursor movment, text color, etc.

> I'll refer to these two components as "keyboard" and "screen" in the following paragraphs, or as "terminal" when I mean  both of them.

When you open your terminal emulator, a program, the shell, is started for you and this program, like all UNIX programs, has an input channel to receive data and an output channel to send data. These two channels are called `stdin` and `stdout` respectively. A third channel `stderr` exists for messages that aren't officially part of the program's output. This additional output channel can be used by the program to inform the user about success (usually this doesn't happen, no news is good news) or failure of the programs progress.

> The job of `ssh` and `sshd` is to check the authenticity of both sides of a remote connection. They then create encrypted network channels for `stdin`, `stdout` and `stderr` that go from one machine to another. `ssh` then instructs `sshd` to run a program on the remote computer and connect its three IO channels (`stdin`, `stdout`, `stderr`) to the encrypted network channels (like "cross-machine pipes"). The program being started usually is the shell. However you can configure `sshd` that some other program should be run for a certain user (or a certain public-key). And, as we've seen, you can tell `ssh` explicitly what program you want to run at the other end. (we used it with `tar`). So, even though SSH stands for "Secure Shell", it really isn't a shell at all. It is a remote program launcher that encrypts data channels after checking keys. But it's _main purpose_ is making remote shell sessions secure.

Imagine you login to a remote machine using `ssh` and type `vim` at the promte of the remote shell.
This is how the characters flows through the various programs.

```
Human fingers -> keyboard -> bash -> ssh -> sshd -> bash -> vim -\    (input)
Human eyes    <- screen   <- bash <-  ssh <- sshd  <-  bash    <-/    (output)
```

### Environment Variables

`stdin`, `stdout` and `stderr` – together they are called "Standard I/O" (I/O stands for **I**nput and **O**utput) are not the only ways that a program can communicate with the world though.

_Environment Variables_ are another one. They are strings with names. (A string is a couple of characters). There is for example a variable called `PATH` that specifies where `bash` looks for programs. It is a list of directories, spearated by colons (`:`) that bash looks into, one after another, to find an executable program. If the PATH is set to `/a:/b` for example, and you type `ls` at your bash prompt, `bash` would check if the file `/a/ls` exists and if that file has the `x` flag set. (Remember: `x` is one of the flags you set with `chmod`, it means that the file is executable). If this is the case, `bash` will run the program `/a/ls`. Otherwise it will do the same check for `/b/ls` and, if it passes the check, `bash` will execute that one. If both don't exist or are not executable, bash outputs: `bash: ls: command not found` on `stderr`.

We've ssen another environment variable when we've configured your bash promt: `PS1`. They are plenty of variables, you can see them all by typing:

```sh
$ env
```

A program can communicate with other programs by reading or setting environment variables.

### Exit Status

Because error messages are in no particular format, they are generally not machine-readable. Instead they are meant for human consumption. If, for example, you type into your shell:

``` sh
$ ls sdfsdf
ls: sdfsdf: No such file or directory
```

you see a message that you have probably no trouble understanding. For a program, that's harder. That's why the _exit status code_ (or _exit code_ or _exit status_) exist. When a program is done, it can indicate whether it was successful or not by returning a number (usaually an 8-bit number). To indicate success, a program exits with a status of zero. If instead there was a problem, it can chose different numbers to indicate different problems. (there's no standard for what numbers mean what, it varies from program to program, but 0 always means success.)

After you ran a command in the shell, the shell sets a special _environment variable_ to the value of the program's exit status. That variable's name is `?`.

> To see the content of an environment variable, you can use the `echo` command. You need to prefix the variable name with a dollar sign (`$`) to make this work:

``` sh
$ ls sdfsdf
ls: sdfsdf: No such file or directory
$ echo $?
1
$
```

> After `echo` has written the content of the `?` variable, that variable's content will be lost, because `echo` has an exit status too. After `echo` ran, `$?` will contain the exit code of `echo`, which will be zero, if `echo` was successful.

``` sh
$ ls sdfsdf
ls: sdfsdf: No such file or directory
$ echo $?
1
$ echo $?
0
$
```

A program uses the _exit status_ to indicate success or failure to the program it was started by.

### Arguments

The forth (and, for now, final) way to communicate are arguments. You've already used them a lot in the previous weeks. Whenever a program is started, a set of strings (the arguments) can be passed to alter the program's behaviour or to give it some information it needs to fullfill the desired task.

In this invocation of ls

``` sh
$ ls . -l -a
```

`ls` is started with three arguments: `.`, `-l` and `-a`. Here:

``` sh
$ ls . -la
```

`ls` is started with just two arguments.

> It is `ls` job to figure out that the two invocations actually mean the same.

But what happend when we did `echo $?`? When you enter a command line that contains a dollar sign, the first thing `bash` does is to _replace_ the dollar sign and the variable name that follows the dollar sign, with the _contents_ of the environment variable with that name. Only _after_ this replacement, `echo` is run.

> Because of this, `echo` never actually "sees" $? in its list of arguments! All it sees is "0" (or whatever the exit status of the last program might have been)

Because `bash` does a simple search-and-replace operation to the whole command line before executing anything, the use of variables is not limited to arguments. For example, try:

``` sh
$ $EDITOR
```

(If your EDITOR environment variable is set to `vim`, this will run vim)

### That's all?

Did we miss something? Of course. Programs can communicate with the _kernel_, the core of the operating system. The kernel has control over the machine and its various communication channels. It can use the network interfaces, (Wifi and wired network), it can use a sub-system, called _filesystem_ to read and write files from/to the harddisk or SSD and it can use the audio hardware, for example to playback or record sound. Programs can request to perform such operations and, depending on the privilege level and on security settings, the kernel either performs them or returns an error message along the lines of "permission denied".

> The mechanism used by a program to ask the kernel to perform a task is called a _system call_. A program requests some action from the _system_ (the kernel) by _calling_ a function that the kernel provides. You cannot run _system calls_ from the command line as a user. Only programs can do that. The kernel and you only communicate via third partys.

This opens up many more ways of communication for a program. The most important ones being offered by the network and the file system.

Programs are free to use any combination of stdio, arguments, environment variables and the network and file system. Here are a few examples.

- `cat ~/.vimrc` - `cat` receives a single argument "~/.vimrc". It uses the argument as a file name in a system call that reads a file from disk. `cat` then writes the result of the read operation to `stdout`. If the system call returns an error, `cat` writes an error message to `stderr`. `cat` does not use `stdin` in this case.
- `cat` - called with no arguments (or `-` as the only argument) copies everything it receives from `stdin` to `stdout`.
- `ls $HOME` - `bash` replaces the string "$HOME" with the value of the environment variable `HOME` (which is the file path to your home directory). `bash` then runs the program `ls` and passes that path as the only argument. `ls` takes the argument and performs a _system call_ that lists the content of a directory. `ls` then turn the result of the system call into readable text and outputs that text to `stdout`
- `curl http://www.kitchen-sessions.org/whatever.html` - `curl` takes a single argument (the URL) and splits it up into various parts: "http", "www.kitchen-session.org" and "/whatever.html". It then uses the network subsystem (through a system call) to find out the IP address of the computer called "www.kitchen-sessions.org". (it's hermes' IP address). It then uses a code library that knows how to speak `http` and tells it to download the file "/whatever.html" from that IP address. The library uses system calls to the network subsystem to communicate with the remote computer's `httpd` daemon and receives the content of "/whatever.html". `curl` then outputs that content to `stdout`. `curl` does not care about `stdin` at all.

## Upside-down duck

I often draw UNIX programs like this:

```
       arguments
          o   o
          _\_/_
stdin >==| cat |==> stdout
         |_____|==> stderr
            ||
          kernel

```
("cat" is used as an example here)

This is supposed to be a box with handles or leavers or switches on top of it. There's one pipe that goes into the box, and two pipes that go out of the box at the other end. At the bottom, there is a connection to the kernel.

The switches or handles or leavers at the top are the program's arguments. Imagine this to be a panel where you can configure how the box should behave. The pipe that goes into the box is `stdin` and the two pipes leading out of it are `stdout` and `stderr`. Imagine such a box for each UNIX program. You can connect the `stdout` (or `stderr`) of one program to the `stdin` of another (thats _piping_), or you can redirect one of those channels to come from, or go to, a file. (that's _redirecting_).  Each command might interact with the file system or network through the kernel.

In the world of UNIX, most problems are solved by choosing the right boxes, connecting them in a certain fashion and finding the right combination of arguments for each one.

One of you said that these diagrams look like an upside-down duck (works better when drawn on paper)

## The Challenge

We've used `curl` in a previous session to download a github user's public key. We've also used `cat` to output the contents of a file into `stdin` of another program.  I gave you the following challenge:

> Given a file with a list of she.code members' github usernames, write a command line that output all those members' public keys to `stdout`. This command line could be used to update the `authorized_keys` file, however it should _not_ manipulate `authorized_keys` by itself. Instead the user should have _a choice_ to redirect the output of your command line into her `authorized_keys` file like this:

``` sh
$ your-command-line > ~pair/.ssh/authorized_keys
```
(where _your-command-line_ is a placeholder for the stuff you should come up with in this challenge)

I told you that `cat` and `curl` will be part of the solution and that there's also a new piece that you'll need for this puzzle. It is called `xargs`. 

If we draw boxes for `cat` and `curl`, they'd look like this:

```
       names
          o   
          _\___
         | cat |==> stdout
         |_____|
            ||
        file system

```
We use cat to output the contents of the file "names" to `stdout`. We can then pipe it to another program.

```
       URL
          o   
          _\____
         | curl |==> stdout
         |______|
            ||
          network

```

We can use `curl` to download a file from the web and output its content to `stdout`. Unfortunately though, `curl` does not receive the URL through `stdin` but via an argument (similar to `cat`), so we cannot _pipe_ the URLs into `curl`, so something like `cat names | something | curl` won't work, because _piping_ means to connect the `stdout` of some program to the `stdin` of another, and `curl` simply ignore `stdin`.

> If you run `curl http://whatever.com/large_file.data` you will start downloading. If you type something into your shell while downloading, it has no effect, because `curl` ignores `stdin`.

We need some kind of translator between `stdio` and arguments, a box that looks like this:

```
       arguments
          o   
          _\_____      
     ===>| xargs |----o
         |_______|___/__ 
                | curl  |
                |_______|
```

We need a box that reads from `stdin` and spawns another box (runs a program) and passes the information from `stdin` as arguments to the program it is spawning. It must have arguments itself, so we can configure what program it should run. And we need a way to say that it should run that program (curl) one time for each line read from `stdin`. Furthermore, the information coming in from `stdin` is only _part_ of the argument we need to give to curl. (the username is only part of the URL).`xargs` can do all of that if we give it the right arguments.

This information and the _man page_ for `xargs` where your ony resources to solve the challenge.

The command line you came up with was:

``` sh
$ cat names | xargs -I USERNAME curl https://github.com/USERNAME.keys
```

It made all of us really happy to see it working! It's quite impressive that you can do this kind of crazy nerd stuff after just ten weeks!

## Homework

When you open a text file with `vim`, `vim` reads the content of the file into an area of the computer's memory. `vim` calls this area a _buffer_. When you then edit the text, you are not changing the file. Instead you are changing the content of the _buffer_. Only when you do `:w` you actually _write the buffer to the file_. But if you type `:q!` the content of the buffer will be lost and _not_ written to the file. Fort the homework it is important to make a distinction between the _file_ and the _buffer_.

## Your assignment

The command line you came up with in today's challenge reads from a file (called "names") and outputs the public keys of github users mentioned in the file. For the homework, you should come up with a `vim` command that pipes the content of the current _buffer_ through a command line (similar to the one you came up with today) and than replaces the current buffer's content with the output of the command line.

The goal is that you can open vim, type the names of your favourite she.coders (on separate lines) and then do a vim command (starting with `:`) and see the names magically being replaced with the corresponding public keys.

Use the web to find out what vim command you need and use the skills you learned today to make adjustments to the command line.

#### Bonus
- find a `vim` command that does the same as above, but only for the _current line_ (the line in the buffer where the cursor is)
- if you succeeded with the above, try to assign the command to a key using the `:map` command. (You'll need to google this)

Have fun!

# Session #9 2015-11-13

## Piping

We talked about how to combine small programs like `cat`, `sort` and `uniq` to process a stream of data.

In this example

``` sh
$ cat names1 names2 | sort | uniq
```

the program `cat` reads the two files `names1` and `names2`, con**cat**enates (hence the name) their content and pushes the resulting data stream to _stdout_.

> `stdout` is one of two output channels that UNIX programs can use to output data they produce. `stdout` is meant for results and the other one, `stderr,` is meant for error messages or other messages that are intended to be read by humans (as in "not meant to be interpreted by machine"). Both of these channels (or _streams_) are connected to the terminal unless you explicitly redirect them to files (using `>`) or pipe them to anohter program (using `|`).

The result of `cat` is then fed (_piped_) into `sort`. `sort` reads all of `cat`'s output into memory, sorts all lines of text aphabetically and outputs them again.

> Each UNIX program has one input channel called `stdin`. `stdin` normally is connected to the terminal as well. So, while a program runs, it can consume your keystrokes by reading from `stdin`. You can pipe another program's output into `stdin` (by using `|`) or you can redirect `stdin` to come from a file (using `<`).

The sorted output is then piped into `uniq`. `uniq` simply discards (filters) duplicate lines when these lines immediately follow each other.

> When you start a program that reads from `stdin` by default (like `sort` or `uniq`) without piping some other program's output into it and without redirecting `stdin` to come from a file, it will read your keystrokes. You will often have the impression that "nothing is happening" when you start such a program, because it typically does not prompt you for input. It will simply sit there and silently wait for you to type. You can tell that this is happening by the absence of your shell prompt. You simply start typing and, when done, press `Ctrl-D`, which is defined by ASCII to be the [End Of Transmission character](https://en.wikipedia.org/wiki/End-of-transmission_character) (EOT).

When you just run `uniq` for example, your cursor will  be placed in a new line with no prompt at all. `uniq` is trying to read from `stdin`, which is connected to your terminal. Hence, unless you tupe something, `uniq` will keep waiting. Whn you type a line of text and hit enter, `uniq` will output that exact line of text (so in your terminal you see it a second time). This happens for every line of input, unless it is _exactly_ the same line as the previous one, in which case `uniq` will not send it back to you (by writing it to `stdout`). `uniq` keeps reading until yout hit `Ctrl-D`.

Try this:

``` sh
$ uniq
a
a
b
b
b
a
a
a
^D
```

(`^D` means you press Control-D).

If you try the same with sort (do it now!), you will realise that `sort` will not output anything until you hit `^D`. That's because you can only sort a list of items if you know all the items! Developers say: "`sort` _buffers_ all the input". A `buffer` is a chunk of memory that is used temporarily. Remember the paper tape with 5 bit code that allows a telegraphy typist to have a sip of coffee? That's a buffer.

On the other hand, `cat` and `uniq` do not need to buffer. Developers say "`cat` and `uniq` _stream_ data from `stdin` to `stdout`. The mental image is that of a river. `cat` is the source in the mountains. Water flows down into `sort`. Because `sort` needs to buffer the data, it acts like a dam – it holds back the data (water) until it slurped up everything. Only then data starts streaming out of `sort` until its pool or lake (buffer) is empty. Downhill of the dam the water is streaming again. It flows into `uniq`, which filters out some dirt, but lets most of the water (lines of text) pass through without any holdup.

> Did you wonder why uniq only discards duplicate lines that immediatly follow each other instead of making sure that all duplicates are discarded no matter where they occur in the text? The answer is: It is designed this way so it does not need to buffer!

Why is buffering bad? Because it is "expensive". With "expensive" developers mean: it demands a lot of resources that are limited, like memory or CPU. Imagine, in the example above, the files names1 and names2 are one Terabyte in size each. That would not be a problem for `cat` or `uniq` at all, because _they_ only deal with a piece of the data at a time (a piece of data is often called a `chunk`). `sort` on the other hand needs to hold _all_ of the names in memory in order to produce output – that's 2 Terabyte! Most likely, this is impossible, because the computer does not have that much memory. (modern laptops now have between 2 and 16 Gigabytes of RAM, a Terabyte is 1024 Gigabytes. Servers might have 24 Gigabytes or even more, a Terabyte of RAM is very expensive and does not fit into most computers).

Let's get through the above example step by step.

``` sh
$ cat names1
jan
ela
judith

$ cat names2
nicole
kathrin
jan

$ cat names1 names2
jan
ela
judith
nicole
kathrin
jan

$ cat names1 names2 | sort
ela
jan
jan
judith
kathrin
nicole

$ cat names1 names2 | sort | uniq
ela
jan
judith
kathrin
nicole
```

You now know what `piping`, `streaming` and `buffering` mean. Or, at least, you have an idea.

## Manual Pages

UNIX comes with a build-in manual. The program `man` is used to display a manual for a specific program. These manual pages (man pages) are not meant as tutorials, they are fairly technical and need some time to get uesed to. Furthermore different styles exists. man pages written for th GNU project look a bit different than ones written for BSD. man pages are very useful if you already know what a program does in generalm but forgot about the exact details, like what the order of arguemnts in `chmod` are or if the portnumber in `scp` is specified with a lowercase or uppercase `p`.

The most important part is the `SYNOPSIS`. It lists all the different ways you can run the program and spcifies which parts are optional (by putting them in square brackets).

> man pages offer _a lot_ of information. Most of it is irrelevant for a given task and some of it you'll never need to understand at all. The trick is to find the parts that are relevant to you *now* and skim over the rest. Don't get discouraged when you do not understand 80% of what the man pages say, just ignore the parts that are confusing to you and keep looking for the piece of information you need. This requires some getting used to, but it is generally a good skill to have.

### Homework

- read the man pages of `sort` and `uniq`.
- Who wrote your versions of sort and uniq?
- What operating system was it written for?
- Given a file with numbers called `numbers`, write a command line that outputs all numbers thst appear more than once. Output the numbers in descending order.

    Example:
    ```
    1
    2
    3
    4
    3
    1
    130
    1
    ```
    
    result should be:
    
    ```
    3
    1
    ```
> The UNIX philosophy is to create small programs, that do one particular thing by streaming data from `stdin` to `stdout` and avoid buffering whenever possible. These programs can easily be combined to solve bigger problems, using a minumum of resources.

# more on ssh

We discovered that `ssh` takes a second, optional argument. Instead of connecting to a server to start a new remote shell like this:

``` sh
$ ssh -p 22000 me@blackbox
```

you can specify what command to run on the remote machine:

``` sh
$ ssh me@blackbox ls ~
```

After running the command (`ls ~` in this case), `ssh` ends the connection to the remote computer. The example above is a quick way to see the contents of me's home directory on "blackbox".

## Piping through ssh

The beauty of this is that `ssh` will connect it's own `stdin`, `stdout` and `stderr` to the streams of the command it executes remotely. So, incase blackbox does have 2 Terabytes of memory, we can solve our memory problem from above by just running `sort` on blackbox!

``` sh
$ cat names1 names2 | ssh me@blackbox sort | uniq
```

(of course this adds the penalty of having to transfer 2 Terabyte back and forth over the network)

## Tunneling

This technique is sometimes called "ssh tunnel". The mental image is: you open a portal on one computer that magically is connected to a portal on another computer. Whatever you put in one portal ends up on the other end – a bit like teleportation. An _ssh tunnel_ is an encrypted pipe betwwen computers.

Turns out that `scp` is using an ssh tunnel behind the scenes. When you copy a file, it does something like this:

``` sh
$ cat file  | ssh other_computer "cat - > file"
```

> the man page about `cat` says: When you specify '-' as the argument, `cat` simply outputs what it gets from `stdin`, it then acts like a neutral piece of piping. We need it anyway, because we need to specify a program to be run on the remote computer. So we use cat here simply to redirect `stdout` into a file on the remote computer.

> We need quotes around `cat - > file` to make sure that the whole thing is interpreted as the argument to `ssh`. If we don't use quotes, the "> file" part will happen on the local machine, not on the remote one!

## Streaming multiple files

`scp` also has the `-r` option, that recursively copies a whole directory of files. Who does it do that over ssh? The cat approach from above certainly doesn't work.

The solution is to combine all the files that need to be copied into one file, transfer that file and, on the other machine, separate the files again.

We've already seen that `cat` can be used to concatenate files, in other words: turning a bunch of files into one file. However, it does this in a way that is irreversable. After concatenation, it is not possible to find the boundaries of the original files within the big file. Consequently it is not possible to "unpack" them.

Furhtermore, a file is more that just its content. You also would want to copy the file's owner and group and the file's mode (permissions). Not even the file names are included in what `cat` produces.

The solution is a program that creates an `archive`. An archive is a file that contains multiple other files _with_ thair meta data. Originally this was invented to store files on magnetic tape. A tape is not more than a stream of bits that can be recorded and played back. So people needed a way to convert multiple files into a single stream, record that stream on tape and later, read back the stream and convert it back into the original files.

The program that does this is `tar`. The name stands for "tape archives". Tapes are long gone, but tar is still around, because the problem of combining multiple files into one stream in a way that is reversable still remains.

tarfiles (also called [tarballs](http://images.google.de/imgres?imgurl=http://supermensa.org/wiki/images/thumb/d/d0/Tar1.jpg/250px-Tar1.jpg&imgrefurl=http://supermensa.org/wiki/index.php/Tarball&h=162&w=250&tbnid=NMzeDxPeCDTwKM:&docid=WVlSXn9pNBhBdM&ei=nEBMVp-LPMH_O8mwv4gD&tbm=isch&iact=rc&uact=3&page=2&start=26&ved=0CJgBEK0DMCZqFQoTCJ_JrafQmckCFcH_DgodSdgPMQ) re often used as a way to publish software packages in the UNIX world. They work like zipfiles Zip started its life as a commercial product and is patented – that's why it is not very popular in the open source community.

We used `tar` to package up your website, copy it over to `hermes` and unpack it there again. I showed you how you can avoid creating a tarball at all, by streaming `tar -c` on the local machine into `tar -x` on the remote host!

``` sh
$ tar -c website | ssh -p 30000 hermes | tar -x -C /var/www
```

> The nice thing about these one-liners: after editing your website in `vim`, you can simply use `cursor up` to find this one line of shell code again and press `enter` et voila: the updated website is online.

This is how `scp -r` _could_ be implemented behind the scenes.

# Rsync

We discussed the limitations of the tar-through-ssh-tunnel approach and `scp`: They always transfer _all_ of the files, no matter if they have changed or not.

A solution to this is `rsync`. I showed you how you can specify the program that `rsync` should use for tunneling with the `-e` option.

``` sh
$ rsync -a -e "ssh -p 30000" wensite/ hermes:/var/www/
```

> `-a` stands for "archive". It instructs `rsync` to also copy meta data (owner, group, permissions)

Rsync uses `ssh` to start a version of itself on the remote machine. The two programs then communicate to find out what files (or what part of what file) need to be transferred and then transfer it.

> When you add `-v`, you can watch `rsync` doing it's magic. `v` stands for "verbose", a common term that means: "talk to me!"

``` sh
$ rsync -av -e "ssh -p 30000" wensite/ hermes:/var/www/
```

For completeness' sake: The programs `zip` and `unzip` also exists in the UNIX world. SO when you downloaded a zip file, you can easily unpack it using `unzip`. Use `man` to find out more!

We used unzip to unpack the fontello files.


# Session #8 2015-11-06

## Orientation

We used travelling from one city to another city in another country as a metaphor for remote login to another computer via ssh.

The computers are countries. In order to enter a country, you need to prove your identity (your passport is checked at the border. This is what `sshd` is doing when you travel to another computer. It check's whether you own a private key that is acceptable for the user account you want to use on the remote computer. You can even have multiple identities (imagine you are an agent), so you can be user "regular" on one computer (country) and user "pair" in anohter country (computer).

In order to not get confused about the **who** and **where** make a clear distinction between your account name (username) and your computer's name. It makes sense to name your computer after a *thing* rather than a person and use your initials, or some other combination of characters that represent you, as your account name. Try to use lowercase letters only, it makes life easier.

> While Apple's convention of assigning computers names like "Eva's Macbook Pro" supports making this distinction between machines and persons, it also results in very cumbersome machine names. Choose a shorter name that you and others can easily remember and that does not contain spaces or special characters like `'`.

Of course you can change your location *within a country* (on a computer). The current location on the computer is your *currrent working directory* (returned by the command `pwd`). It is sort of the city you are currently visiting, *or street address, if you prefer).

You can change your identity (who you are) by using `sudo` or `su`. Changing your identity (user account) will not change *where you are* (city or country).

You can change where you are within a country by using `cd`. Changing where you are will not change *who* you are.

The prompt might be configured to indicate your identity, country and current location (username, computer, current working directory) or it might be configured to only show some of that information (to save space for example).

Here's an example of a shell session.

``` sh
➜  dev$  ssh -p 30000 hermes
regular@hermes:~$ su
Password:
root@hermes:/home/regular# exit
exit
regular@hermes:~$ exit
logout
Connection to hermes closed.
➜ dev$
```

It starts on my own computer. The prompt does not show who I am or what computer I am on. The only thing it displays it the last segment of the current working directory. All I know is that I am in a directory called `dev`. I do not see where that directory is.

I then useed `ssh` to a computer called `hermes` that has `sshd` listening on port 30000. Since I did not specify any user account to ssh, ssh will try to log me in with the same username I currently have (which is `regular`).

On hermes my prompt is configured to show my account name, the name of the computer I am on, as well as the complete current working directory. Because I ended up in my home directory on hermes, the current working directory is displayed as `~`.

Than I used `su` (switch user) to change my identity to `root`. Notice how **my location did not change** but is **now displayed differently** (not as `~`). Because I am now root, I am no longer in my home directory, even though I am still in `/home/regular/`. It is not *my* home directory anymore, because I **am** no longer "regular", I am root now, and root has its home directory somwhere else. I changed *who* I am, not *where* I am. The new identity did however change my relationship to the directory "/home/regular". It used to be my home, now it is just some directroy without a special meaning, so it is not abbreviated to `~` any longer.

Because `su` starts a new shell, I used `exit` to get out of that new shell. Now I am regular again. `ssh` also started a new shell. I exit this one too, and I am back on my laptop.

As mentioned in the last session, it is critical to always be aware of what computer you are on, which directory is currently your working directory and what identity you are currently using. Use `hostname`, `pwd` and `whoami` when in doubt. Or, if your prompt is configured to display all that information, look at your prompt, it's your torch in the dark.

# Moving to a new computer

Some of you migrated to new computers, because your old ones broke or where too heavy to carry around all the time.

To lower the time you lose when you need to move from one computer to the next, you should remember to take some important files with you!

- ~/.vimrc – your vim configuration
- ~/.tmux.conf – your tmux configuration
- ~/.ssh – your private and public key

One easy way to do this:

- first install sshd on the new computer. On Linux simply run `sudo apt-get install openssh-server`
- make sure that you can login to your account on the new computer from the old computer by using your password
	
	``` sh
	me@oldcomputer ~$ ssh newcomputer
	Password:
	me@newcomputer ~$ exit
	me@oldcomputer ~$
	```
- now copy your stuff using scp

	``` sh
	me@oldcomputer ~$ scp .vimrc .tmux.conf.ssh newcomputer:~
	```
	
	(I am using `newcomputer` and `oldcomputer` as placholders for the names of your new and old computer)
	
	
# Session #7 2015-10-29

## Creating a team page

You were starting work on your kitchen-session team page. I asked you to pick 5 symbols from [fontello](http://fontello.com/) and it was your job to find out how to download and embed the font in your own html page.

## Make yourself at home on a remote server

We started a shared `tmux` session on my computer. Using `ssh`, I then logged into a server I rented. That server (it is called "hermes") is somewhere in a data-center in the south of Germany and it is on the public Internet, which means, it has its own unique *IP address* (more on this later) and can be reached by any other computer on the Internet.

> Our work computers are normally behind a firewall, which means they share one public IP address and cannot be reached from outside the LAN (local area network). Special steps must be taken if you want your computer to be exposed on the Internet (for example because you want to pair program with a remote partner). We'll talk about that in a later session. Inside the LAN, the computers *do* have unique IP addresses too, however, those are not valid outside the LAN.

Other than being publicly reachable, there's just one difference between our computers and hermes: It has no screen and no keyboard. Everything else is pretty much the same.

I used `su` to become superuser on hermes and than asked you to make user accounts for yourselves to practice what we've learned in the previous session.

We discussed the differences between `su` and `sudo`.

- `su` changes your identity by starting a new shell as a different user. By default you become super user (the user called `root` that has no restrictions whatsoever). However, you can also specify a different account name. For example `su pair` turns you into the pair user. `su` always asks for the password of the user you want to become. WHen done, you end the new shell by typing `exit`.

- `sudo` is used to *perform a single command as another user*. By default that user is `root`. However, it can be any other account if you use the `-u` option. An example would be `sudo -u pair cat /etc/passwd` to try and see if the pair user is allowed to read the `passwd` file. **Important** `sudo` is always asking for the password of *the user that runs the sudo command*, which is **not** the same password that `su` is asking for! Not all users are allowed to run `sudo` (You have to be a "sudoer"). Like `su`, `sudo` can also be used to start a new shell as a different user – just use the `-i` option: `sudo -i` starts a new shell as `root` but asks for the password of the user that runs the command. Like with `su`, you exit this new shell with `exit`.

> When you get confused what your account you are currently using, just type `whoami`. When you are confused on what computer you are, tyoe `hostname`, and when you are confused what your current directory is, type `pwd`. You should be aware of these three things at any time, that's why the normally are part of the shell prompt.

## Configuring the prompt

Along the way we got sidetracked and created a nicer shell prompt featuring a Unicode character depciting a sun symbol! We put a line setting a prompt into `~/.bashrc`

``` sh
export PS1= TODO: Judith or Ela, pleas put your prompt code here!
```

> the file `~/.bashrc` is being executed whenever you start a shell. You can use it to configure your shell experience.

Just like `/etc/sshd_conf`, `~/.bashrc` is a configuration file. However, `.bashrc` is *personal* (it is different for each user), whereas `sshd_conf` is system-wiede (the same for each user). That's why `sshd_conf` is located in `/etc` and `.bashrc` is located in a user's home.

## Using tmux panes

Inside our shared tmux session, we used multiple so-called "panes" – little sub-terminals with their own shell. In each pane we ssh'ed into a different machine or su'ed into a different user on the same machine. This is a convenient way to quickly switch between accounts and machines. We used `Ctrl-B` `%` to create a new pane and `Ctrl-B` and a cursor key to switch between panes. See the [tmux cheat sheet](https://gist.github.com/MohamedAlaa/2961058#panes-splits) to find out more.

> "tmux" stands for "terminal multiplexer", provoding multiple sub-terminals in one is its main purpose.

In the end you were able to passwordlessly login from your computer to your own account on hermes using `ssh` and you even know how to set a custom prompt on this computer!

Next time we will use hermes to host your team page on the web.

## Homework

1. Many people re-configure the so-called "Prefix-key" in `tmux` to be `Ctrl-A` instad of `Ctrl-B`. Find out how to do that!
2. Many people also re-configure the useless (and annoying) `Caps-Lock` key to be another `Ctrl` key. Find out how to do that in your operating system.
3. Start tmux, create some panes and switch between them, using `Caps-Lock A` as your prefix key!
4. Remember LZ77? Think about how you can use it to encode an image for another kitchen-session team with better compression than RLE.

# Session #6 2015-10-22

## Seting up your computers to host a pair programming session

### ssh and sshd
We revisited what needs to be set-up in order to allow a user to log in remotely into a computer via `ssh`.

0. we need to have a user-account on the remote machine. For now we all share the same account name: `pair`
1. `sshd` (the ssh daemon) must be running on the remote machine (*remote machine* and *server* are used as synonyms here)
2. `sshd` should be configured to disallow password-based authentication and demand authentication based on a private/public key-pair. This prevents an attacker to gain access by guessing the password.
3. the private key must be on the client computer (typically it is called `id_rsa` and resides in the the `.ssh` dorectory which in turn is located in the user's home directory)
4. the public key must be on the server in the `authorized_keys` file in the `.ssh` directory in the home directory of the account that we want to have access to remotely.
5. the file modes and ownerships of all files inside `.ssh` on the client and the server need to be pretty restrictive.

### Creating a user account

We created the user account `pair` on both of your computers. The pair user will be used by all she.coders to take part in a pair programming session that you host. The normal approach would be to create one account for each user and than put the public key of each she.coder into the respective .ssh directory of that user. We save a lot of time by using just one account for "the others".

On OSX we simply used the *Users and Groups* panel in *System Preferences* to create a user account called `pair`. Set all other values to `pair` too, including the password. We will disable password-based authentication altogether in one of the next steps.

On Linux we used the useradd script to add the user and the `passwd` command to set the password.

``` sh
$ sudo useradd pair
$ sudo passwd pair
```

We then created the new user's home directory

``` sh
$ sudo mkdir /home/pair
```

You can now try it out by entering

``` sh
su pair
```

It will ask for pair's password and when you can login, you know you successfully created the user account.

Incase the pair user is configured to use a shell different from `bash`, you can correct this in the file `/etc/passwd` (use `sudo vim /etc/passwd` and be very careful not to mess up the file)

### Installing sshd

On Ubuntu we installed `sshd` like this:

``` sh
$ sudo apt-get install openssh-server
```

On Mac OSX, `sshd` is pre-installed. You just need to enable it by clicking the checkbox next to *Remote Login* in the *Sharing* panel of *System Preferences*. You can restrict who can login remotely in this panel too. (restrict it to the user `pair`)

### First attempt to login remotely

You can now try to login as `pair` on your partner's machine. For this to work, you need to know the name of your partner's computer. You can find out yours with

``` sh
$ hostname
```

You should be able to login as `pair` on your partner's computer by doing this:

``` sh
$ ssh pair@computername
```

You will be asked for the password of the `pair` user and after that you are talking to a shell that runs on the other one's computer!

### Public-Key Authentication

Now we tell `sshd` to not accept passwords but to only allow access if the user can proof her identity by owning a specific private key. For this to work, we need to put her *public key* on the host computer.

> The *host computer* (or server) is where sshd is running, the client computer is where `ssh` is running.

You created a directory called `.ssh` in the home directory of the pair user.

On OS X, you do:

``` sh
$ sudo mkdir /Users/pair/.ssh
```

on Linux, you do

``` sh
$ mkdir /home/pair/.ssh
```

Because you created the directory using sudo, it was created by the `root` user and therefor is owned by `root`. You need to change the directory's ownership by using `chown` and set its permissions to `755` using `chmod`.

Now we put your partner's public key into `.ssh/authorized_keys` in pair's home directory. We did this using `vim`.
I showd you the vim command `:!r`. It executes a shell comand from within vim (much like `:!`), but in addition, it captures the command's output and appends it to the text that is currently being edited! We used

```
:r! curl https://github.com/username.keys
```

to quickly download a public key from github and add it to the file. We realized that this command also adds a couple of lines of garbage to the files. We discussed that this is the result of `curl`'s attempt to display information about the download progress in the terminal. We talked about the two different output channels `stdout` and `stderr` of a UNIX program and that `curl` is using `stderr` to output the characters that are itended for human consumption on `stderr`. `stdout` on the other hand is used to output the actual data that is being downloaded. In general terms: `stdout` often is intended to be further processed by another program, while `stderr` often is intended for a human.

Because `curl` expects `stderr` to be connected to a terminal, it uses control sequences that a terminal would interpret as "move the cursor to the beginning of the current line" and such. However, in our scenario using `:r!` both `stdout` and `stderr` were caputred by vim and ended up in the text file.

I showdd to you that `ls` is smarter than `curl`. If you do

```
:!r ls
```
inside vim, a listing of the current directory will be appended to the text in the editor. `ls` however detects that `stdout` is not connected to a terminal and adapts it's behaviour. It is not using ANSI sequences to make the output colorful, and it even puts each name on its own line rather than using multiple columns.

To fix `curl`'s behaviour, we added the line

```
set shellredir=>
```

to your `.vimrc` file. Now only `stdout` will be caputred by vim.

> Internally `vim` uses the same output redirection we used in the last session. The internal variable `shellredir` tells vim what character to use to redirect the shell's output. By default, `vim` uses a longer sequence of cahracters that redirects `stderr` to `stdout`. More on this in a later session.

#### Configuring `sshd`

You've already encountered the directory `etc` in the root of the file system when we talked about `/etc/passwd`

> In the context of the file-system `root` means the top-level directory)

`/etc` contains configuration files for various `daemons`.

First you need to find locate the file  `sshd_conf`. It is a text file containing the settings of the ssh daemon. It is located eihter at `/etc/sshd_conf` or at `/etc/ssh/sshd_conf`, depending on your UNIX distribution.

Make sure you have the following lines in this file and that they do not start with a hash (#) character.

```
RSAAuthentication yes
PubkeyAuthentication yes
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no
```

> Lines starting with a hash are comments and are being ignored when `sshd` reads the file. They are intended for instructions and for conveniently providing you with "templates" – settings you can easily enable by removing the hash at the beginning. To do this in vim, you move the cursor to the specific line (you can search using `/`) and then press `0` to get to the first column, then `x` to delete the character under the cursor.

To restart `sshd` on Linux, you doL

``` sh
$ service ssh restart
```

On OSX, you simply uncheck the checkbox next to *Renote Login*, count to three and check it again.

> Whenever you changed `sshd_conf` you need to restart `sshd` in order for the changes to take effect. `sshd` is not consantly checking for changes in that file, instead it reads it once when it starts.

When you try to login to your partner's machine now, you should not be asked for a password anymore!
If the login does not work, check the file permissions.

- `authorized_keys` should be `644`
- `.ssh` should be `744`
- `id_rsa.pub` should be `644`
- `id_rsa` should be `600`

Also make sure that all the files in pair's home directory are actually owned by pair!

We had some trouble getting the password-less `ssh`-login to work. In the end we realized the problems were caused by `authorized_keys` being at the wrong location (in `~pair` instead of `~pair/.ssh`) and/or having the wrong permissions and/or owner.

> `~pair` is short for "pair's home directory". You can use it with `cd` for example: `cd ~pair`.

### tmux

The only thing missing to host a pair programming session is `tmux`

On OSX, you need to install [homebrew](http://brew.sh/) first. Then you can

``` sh
$ brew install tmux
```

On Linux, you simply

``` sh
$ sudo apt-get install tmux
```

See last session's protocol for a reminder of how to start or join a `tmux` session.

# Session #5 2015-10-15

## Markup languages

We talked about language as a means to describe stuff. Bees describe the location of a food source to their fellow bees by dancing for example. We use natural language to talk to humans and formal languages if we want to describe something to a machine. We discussed that "describing something to a machine" really means to describe something in a completely unamiguous way. (people also say: _well defined_). This is probably how our code of law should have been written.

We can use formal alnguages to describe shapes and colors and documents. We talked about the language `GML` developed at IBM in the 60s. It was the basis of SGML, which in turn was used by _Tim Berners-Lee_ at _CERN_ as a basis for `HTML`. We also discussed another derivative of SGML: XML. (extendably markup language), which can be used to describe all kinds of data (like a record in a database). XML is much stricter than HTML. We also talked about XHTML, which tries to merge HTML and XML.

None of the above languages are programming languages because they do not describe a process or an algorithm. On the web, we use the languages HTML, CSS and JavaScript. Only the latter one is a programming language. We discussed that, among other things, the various programming languages vary in how hard they are to read and understand for humans and machines respectively. One extreme being `machine code`, a programming languages that is "very easy" to understand for a computer (it is its native language, so to speak) and very hard to read for a human. So-called "higher-level" programming languages are easier for humans to read and write but require translator programs (called `interpreters` or `compilers`) to be understood by machines.

## A first pair-programming session

I gave you an assignment to create a very simple web page in a shared editing session using `ssh`, `tmux` and `vim`. After creating the file, you opend it with the `open` command, which opened a web-browser on my machine, because my machine was hosting the shared tmux session. We discussed that a web-server would be needed on the hosting machine, so that each of you could open the html file in her own browser on her own machine.

> On Linux the command `xdg-open` is used instead of the Mac OS command `open` to open an arbitrary file. It starts the application that would have started if you had double-clicked the file's icon.

We ran the `open` command without leaving vim by entering `:!open %`. `:` enters _command mode_ (also called _ex_ mode), `!` indicates that you want to run something in a shell, `open %` is the shell command to run. Before vim starts a new shell and runs your command however, it replaces `%` with the absolute path of the file that is currently open in vim. This is a quick way to test your work.

## GitHub

We made sure you have GitHub accounts and uploaded your public keys to your GitHub profiles. (Remember: you can safely share your public key with everyone). I showed you a convenient way to get the public key of any GitHub user:

``` sh
$ curl https://github.com/USERNAME.keys
```
_replace USERNAME with the actual name of the user you are interessted in._

> The `curl` command downloads a URL and outputs its content to the terminal.

I showed you how to redirect the output of curl into a file.

``` sh
$ curl https://github.com/USERNAME.keys > a_file
```
> the file (called `a_file` here) will be deleted if it existed before. After this command ran, `a_file` contains what otherwise would have appeared in the terminal (the public key of USERNAME).

I also showed you how to append the output of a command to a file.

``` sh
$ curl https://github.com/USERNAME.keys >> a_file
```

> With two closing angle brackets (`>>`), the file (here: `a_file`) will not be overwritten. Instead the command's output (here: the public key) will be _appended_ to the file.

In the next session, I'll show you how you can use this technique to allow each other access to a specific account on one another's computers, so one of you can host the pair programming sessions in the future!

## The weekly image decoding excercise

You decoded a manually encoded image by another she.codes pair. This time it was a 1bpp (1 bit per pixel) RLE compressed image (also called _bitmap_). We discussed how RLE (run length encoding) works and how, in this case, it did not actually reduce the image data, but quite contrary, made it bigger. We talked about that it would have been more efficient when the image was larger (more pixels). RLE works by reducing stretches of repeated pixels (a `run`). If instead it would replace repeated _patterns_ of pixels with less bits (sort of like we use abbreviations in written langage), it would have been nore efficient. This is exaclty what the so-called `LZ77` algorithm does. It is used by the PNG image format and by compression programms such as `zip` and  `gzip`.

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
- digitally sign a message you have written and make sure it is not modified by a man-in-the-middle. (encode with your own private key)
- combine encryption with a digital signature

We discussed that encryption and authentication is especially important when remotely logging into a computer via the Internet.

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
(you typically find this command line in the command history of the `pair` user, so after `ssh`ing into the host computer, you simply press the `up arrow` key, and there it is!)

In a shared tmux session, all participants can type on their keyboard and the keystrokes are handled as if the host typed them. We all become one user (typically we use the user account of the person that hosts) and everyone sees the same terminal output, however in their own favorite font and colors, because your terminal emulator still rules over these aspects. To make this work, the shared area is as big as the smallest terminal in the session. All other terminals are artificially made smaler by filling some area with dots.

We discussed that this form of pair programming is superior to screen sharing (transfering a compressed image of the computer desktop over the network) in low-bandwidth situations or when the host computer has no graphical user interface (GUI), like a server for example. It also is more inclusive; even people that use a [Braille display](https://en.wikipedia.org/wiki/Refreshable_braille_display) can participate. If you are developing an application with a GUI however, it becomes harder to have a shared view of your work result.

We briefly experimented with opening `panels` (terminals in terminals) inside tmux and navigating between panels. Expect more `tmux` wizardry later.

## History of UNIX

I gave you a more detailed history of UNIX and we talked about some influencial persons and organisations and how development of UNIX and the Internet (not to be confused with the web) were closely connected.

Here's the timeline we went through:

- 1957-10-04 Launch of Sputnik 1 (first satellite) causes _Sputnik crisis_ in the US
- 1958-02 Eisenhower authorises creation of _ARPA_ (later DARPA) as a response to the launch of Sputnik 1, 13 employees manage an initial budget of $520 Mio (2.92 billion as of 2015)
- 1958-06 _NASA_ act signed
- 1964 _Gneral Electric (GE)_, _Bell Labs_ and _MIT_ collaborate on _Multics_, a time-sharing operating system (OS) for a GE mainframe, MIT's MAC project is sponsored by ARPA (resp. DARPA) since '63 with $2 Mio
- 1968 After visiting MIT and realising that keeping a phone line open is one of the major cost factors, Donald Davies develops the concept of _packet switching_ and presents it in Edinburgh
1969 Davies publication and the fact that Bob Tylor needed to use three different terminals in his Pentagon office to remote-control three ARPA-sponsored computers across the nation, inspire ARPANET
- 1969 Unhappy with Multics, MIT AI staff (including Richard M. Stallman) start work on their own OS: _ITS_ ("the incompatible time sharing system", a name that expresses frustration), a completely open (as in: no security), wiki-like, post-privacy OS.
- 1969 Bell Labs leaves multics consortium because they consider it to be too complex, complicated and unelegant
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
- 1978 Cornway teaches VLSI (very large scale integration, i.e. computer-aided chip design) at MIT
- 1979 MIT AI Lab dissolves, (founding of Symbolics and LMI), leaving Stallman without the community of **Hackers** (they invented the term; it means: programmers (that hack on keyboards), later the media incorrectly used the word to mean "people that commit cypercrime")
- 1979-05 2BSD contains csh and **vi** (both written by Joy) and Berknet, developed by Eric Schmidt as part of his master's thesis work. 2BSD was maintained until 2008
- 1979-09 3BSD contains the Unix port to VAX and a re-written kernel with virtual memory managment called _vmunix_
- 198x Lynn Cornway joins ARPA (from XEROX PARC)
    - ARPA's VLSI Project (founded by Lynn Cornway) funds further development of BSD and Stanford University Network (S.U.N. Workstation) to make it easier to design complex CPUs > 1k transistors. The basic idea is: separate design of logic circuits from the nitty-gritty details of physics, ley computers calculate the circuit layout, this requires software and hardware)
- 1980 Joy re-implements **TCP/IP** instead of integrating ARPA-sponsored BBN version
- 1981 _MOSIS_ service launched as one of the first services on the ARPANET (aka the Internet). Allows students to upload their chip-designs via FTP and they receive the finished chip by mail. This is the implementation of a VLSI system.
- 1981 Berkeley students publish their _RISC1_ design (ARPA VLSI funded, they uses MOSIS)
- 1982 Joy and Stanford students found _SUN Microsystems_
- 1984 After splitting up AT&T's local telephone businesses and therefore freed from antitrust regulations, they start to sell Unix as a commercial product ($16k for administrative use by universities and $800 for education with an addition per-CPU fee)
- 1983-09 **Richard Stallman** launches the **GNU project** and starts working on gnu emacs, **gcc**
- 1985-10 Stallman founds the **Free Software Foundation** (FSF)
    - FSF funds development of **bash** and many other userland tools
- 1989 Release of BSD "Net/1" (TCP/Stack only under a free license for non-AT&T licencees)
- 1988 First release of NextStep, an OS based on BSD
- 1990 Using NextStep, **Tim Berners-Lee** writes the first web browser. It's name: _WorldWideWeb_
- 1991 Release of BSD "Net/2", whole OS with AT&T code (almost) replaced
- 1991 **Linus Torvalds** releases first version of **Linux**, a kernel to be combined with a GNU userland
- 1992 Lawsuit AT&T vs BSDi, uncertainty of legal status of BSD, boosts GNU/Linux popularity
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

I gave you the task of converting a largish decimal number (1052) to binary (base2), octal (base8), hexadecimal (base16) and base64. We found out: the higher the base, the shorter the resulting notation. Therefore, to transfer a string of bits over a transmission line that can only transport 7-bit ASCII (telegraphy) the most space-efficient encoding is base64. It uses (nearly) all of the printable characters in the ASCII code table.

We also found out about a near relationship between binary and octal, binary and hex and binary and base64

- there are 8 ways to combine three bits -> three bits can be represented by one octal digit
- there are 16 ways to combine four bits -> four bits can be represented by one hex digit
- there are 64 ways to combine six bits -> six bits can be represented by one base64 digit

So, in addition to base64 (space-efficient), hex is pretty nice too, because eight bits (=one byte, which is a commonly used grouping of bits) can be represented by two hex digits.

We found another relationship:

- there are 64 ways to combine two octal digits -> two octal digits can be represented by one base64 digit

We used these observations to quickly convert a long binary number to hex, octal or base64 by grouping the bits into groups of 3, 4 or 6 respectively. You can then convert the groups separately, which is much simpler than dealing with the whole thing at once.

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

I presented you an encoded message from another she.codes team and we discussed possible encodings. First we talked about the base they might have used for number notation (turned out to be hex) then we talked about the meaning of those numbers. It turned out that the message is an 8-bit-wide bitmap (or raster image) where each 1 represents a dark pixel and each zero represents a transparent pixel. After successful decoding, you prepared a message for another team (maybe using a different encoding ... we won't disclose it here)

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

We talked about that symmetric ciphers only work after the secret key has been transferred between sender and receiver, which in turn requires a secure communication channel. Historically, this is solved by the two parties meeting in person to exchange the key.

We discussed that it would be much more practical if there was a way to securely exchange a secret over an unsecure channel.

### Homework
  - watch [this video](https://www.youtube.com/watch?v=3QnD2c4Xovk) on Diffie-Hellman Key Exchange.
  - With [this video](https://www.youtube.com/watch?v=GSIDS_lvRv4) on asymmetric cryptography

# Session #2 2015-9-24

## The relation between binary, octal and hexadecimal
We talked about how it is much easier to convert between binary and octal, or binary and hexadecimal than it is to convert between decimal and ... well, really everything else. It is our so familiar decimal system that is the outsider here. The reason for this: 3bits fit exactly into one octal digit and four bits fit exactly into one hexadecimal digit. 
>There are 8 ways to arrange 3 bits and octal has a symbol for each one of them. There are 16 ways to arrange 4 bits and hexadecimal has a symbol for each one.

## base60 and base64
We also talked about base 60 (used in clocks, like 1:43:32, meaning one hour, 43 minutes, 32 seconds. This is a base60 system where each symbol consists of two decimal digits. This is totally sick if you think about it. Certainly more complex than anything you find in the tidy world of programming!
There's a much less familiar one: base64, the biggest number system we can do using only visible characters from the ASCII table. It is the most compact way to transfer very large sets of bits over a traditional 7bit telegraphy line.

We talked about how this could be used for a telegraphy-age telefax, a thing that actually did exist! We talked about the _stack of encodings_ that is necessary to make this work:
- Pixels are encoded as ones and zeros
- six of them are grouped and their binary value is used to map them to a character in the set of characters used by base64 (2^6 = 64)
- these characters (all printable ASCII characters) can now be sent over a traditional telegraph wire, just like any other telegram.
- the receiver does the above steps on reverse.

This is a very common pattern you find everywhere in computing: different layers of encoding on top of each other.

## A Closer look at the Keyboard
On a terminal most keypresses result in a 7-bit binary number (the character's ASCII code) to be sent over the serial output line. However, there are some keys that are meant to be pressed only in combination with another key. They change that other key's effect by making accessible a 2nd or even 3rd key assignment. Those keys are called _meta-keys_. On the original terminals there were two meta-keys: Shift and Control.

On modern computers there are some additional meta-keys: Option (Apple) or Alt (other brands, alt stands for Alternative), Command (Apple), Alt-Gr (other brands, "Gr" stands for graphics) and on some Laptops: Fn (short for "Function"). On modern computers the effect of a meta-key combinations is defined by the operating system and/or the application that owns the active window. 

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

The shell is your interface to the machine. It let's you start programs and combine them, building complex data flows through multiple programs and more. You cannot interact with the kernel directly. But programs can. The kernel might however disallow certain programs from doing certain things, depending on their privilege level. (more on this later)

The _userland_ is where those programs live. It's also where the shell lives, because the shell simply is a program like all the others, it's just the one that happens to be in charge of the terminal input/output connections. Consequently the sum of all those little programs (like `ls`, `pwd`, `cd`) is called the userland.

## UNIX family tree
We talked about the evolution of Unix. A joint-venture of Bell Labs, MIT and General Electric started developing  Multics. DARPA (Defense Advanced Research Projects Agency) wanted an operating system with time-sharing (`mult` as in multi-user, multi-program). Bell Labs left the consortium after it became obvious that Mutics would become too complex. Dennis Ritchie and Ken Thompson, both working at Bell Labs were so much in love with the basic ideas (the _UNIX philosophy_) that they started a new, much simpler project with the same basic ideas. They called it "Unics" (uni as in the opposite of multi). The University of Berkeley made their own distribution (the first one infringing on AT&T's copyright). They called it BSD (Berkeley Software Distribution). BSD received DAROA funding. For the next release, they re-coded all the stuff that was property of AT&T (the employer of Ritchie and Thompson) and the University's lawyers wrote up the BSD license (it basically says: do what you want, we take no responsibilities. It allows commercial use, modification, redistribution and everything else you can think of). Since then, UNIX is free. FreeBSD, OpenBSD, NetBSD, PCBSD are all descendants of this distro (short for distribution). Darwin is another one, combined with their own kernel, this is the foundation of Mac OS X and iOS.

A former employee at MIT's AI Lab, Richard Stallman, founded the Free Software Foundation (FSF). He started to re-implement UNIX' shell and userland tools and was soon joined by many other programmers. That's what he calls the free software movement. The project's name is GNU (stands for "GNU's Not Unix"). Together with a kernel for IBM compatible PCs written by Linus Torvalds, this is the operating system known as "Linux". (However it _should_ be known as "GNU/Linux" because Linux is really just the kernel.)

So, all the operating systems are closely related, they all are Unix, or _unix-like_. The one exception being: everything from Microsoft. Windows is a descendant of DR-DOS by Digital Research which in turn is related to CP/M.

## foo, bar, baz
We talked about where the word `foo` comes from (a comic from the 30s) and what it means (nothing, it's nonsense). We also talked about the US military term FUBAR (fucked up beyond all repair). These two things seems to have interacted in people's head and lead to `foo` and `bar` being used very frequently as placeholders for words. Sort of like `lorem ipsum` but for single words rather than a whole body of text.

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
Event though each program can decide how it wants to interpret the characters passed on as arguments, there's a convention:

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

We talked about how Galvani discovered the battery by accidents by using two wires made of different metals.

We talked about early experiments to use electricity for communication purposes in Europe and that Samual Morse was introduced to this idea on a journey.

#### Morse
We talked about the tragic story of the death of Morse's wife which led to the invention of the `Morse Code` and the first digital network for long-distance communication in the US.
We talked about how electro-magnetic switches were used to relay the digital signals and that those electromagnetic switches are therefore called `relay`; together with a battery, they are the main component of a Morse relay station.
We talked about how batteries and relays made possible the first transcontinental telegraphy line in the US and the first transatlantic cable.

#### Baudot
We talked about how in 1870, French telegraph employee Émile Baudot improved upon the Morse system with a more convenient input device (5 key piano-like keyboard) which improved usability by putting less stress on the human sender because the singnal generation is done by a clock-driven device rather than the human operator herself. Baudot's decision to use impulses of just one length (as opposed to morse) turned out to have a huge impact, because it is the key to automation.
We talked about the 5-bit Baudot code and how its design was influenced by ergonomic concerns. The clock speed in a Baudot system is called `baud rate` and is equivalent to `bits per seconds` (`bps` for short). Until the 90s modem speed was meassured in baud (e.g. "I have a 2400 baud modem').

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
We talked about how different companies and vendors extended the Baudot code (to get rid of figure mode/letter mode?) to 7-bit code. They all did it in different ways, which hurt inter-operatability. To solve this issue, ASCII was introduced. (We found out that this happenend in the 60s, so the chain of events probably was a bit different than presented here.)
We talked about how ASCII's design could be free of ergonomic concerns, because of a better de-coupling between the human and the mechanics of signal-generation. Instead it is neatly organised into sections (lower-case letters, upper-case letters, numbers, control-codes)
You can identify the section by the first couple of bits and the rest of the bits tell you the number of the symbol within that section (1 for A etc). This makes it possible to read ASCII without having to memorize the entire code table (as opposed to Morse/Baudot)

- [Tom Scott Video: How to read binary ASCII](https://youtu.be/wCQSIub_g7M)

#### Proprietary extensions to ASCII
We talked about how the 7-bit ASCII code was extended with an eighth bit to make room for German Umlauts, French accents, the Copyright symbol etc., and how, again, each vendor introduced their _own_ code. We all remember the messed up Umlauts in emails sent from a Windows computer to a Mac. Lots of different 8-Bit encodings co-existed. They had names like `latin-1`, `mac-roman`, `iso-whatever`.

#### Unicode
We discussed how `Unicode` tries to solve this issue _once and for all_ by simply assigning a number to each character, glyph and symbol any human ever came up with. (many tens of thousands, including musical notes, emojis, dead languages). Unicode explicity _avoids_ to specify a number of bits to transfer those numbers. The problem of creating bit patterns for each `codepoint` (symbol number) is left to other standards.
We talked about how Microsoft uses a 16 bit code (sort of learning nothing from history) while UTF-8 is an elegant solutions that, by using 8 bit as default, minimises the amount of data for English text and expands the number of bits per character as needed.

- [Tom Scott's UTF8-Video](https://youtu.be/MijmeoH9LT4)

### Dull calculations

#### Ada Lovelace and Charles Babbage 
We talked about how Charles Babbage tried to build a machine that does simple calculus and how it was Ada Lovelace (his pen-friend) that realised the full potential of this idea and who envisioned the first programmable machine and thus invented the concept of software.

- [Great Minds – Ada Lovelace](https://youtu.be/uBbVbqRvqTM)

#### Konrad Zuse
Konrad Zuse worked at an airplane company. He had to calculate the most efficient shapes for wings. Tired of this work, he started to build a machine that would do this for him. This happened in his parent's living room in Berlin. Konrad used mechanical switches in his first version and relays (electromagnetic switches) in the later versions. The 3rd version (Z3) he built during World War II was the first working computer.

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
We talked about how a transistor is built from metal and silicon, a semi-conductor, and can replace relays and tubes at a much lower cost and much higher lifespan. It also switches a lot faster and consumes less energy.

- [How does a Transistor Work?](https://youtu.be/IcrBqCFLHIY)

#### Integrated Circuits
We briefly talked about how a novel photographic process lead to entire electric circuits on a tiny piece of Silicon (a "chip" of silicon). We looked at some, packaged in black plastic with a lot of metal legs, giving them the appearance of a bug.

- [historic film: Fairchild Semiconductor, inventor of the IC](https://www.youtube.com/watch?v=z47Gv2cdFtA)

#### Time Sharing
We talked about that the first computers were so buggy and expensive that universities or companies could only afford one of them (if at all). We talked about how users had to wait a long time before the computer would run their programs. Computing time was the limiting factor. Usually, depending on how important you were, you could hope that your program would maybe run sometime during the night and you would pick up the printed result (usually an error message) the next day. The idea of time sharing is to divide the computer's time into very short time slices and run a different user's program in each of those slices. By connecting multiple terminals to the same computer, users could work interactively and at the same time with one computer.

- [Time Sharing at MIT, historic film](https://youtu.be/Q07PhW5sCEk)

#### Magnetic storage
We briefly talked about the roles of magnetic tapes, harddisks and floppy disks to store bits. They were used instead of paper tape when large amounts of data needed to be stored and accessed (relatively) quickly.

#### Invention of UNIX at Bell Labs
We talked about how Dennis Ritchie and Ken Thompson worked on a time-sharing system at Bell Labs and how the project (a joint-venture of AT&T, MIT and General Electric) was finally cancelled. Dennis and Ken went on anyway because they liked the idea (and needed it to run the game they were developing) and re-wrote a much more simple system that first was called Unics, then UNIX. Unix turned out to take over the world.

- [Brian Kernighan about working at Bell Labs](https://youtu.be/QFK6RG47bww)

#### Video Terminals
 Cathode Ray Tubes (CRT) are being used as screens for TVs and for a new generation of "Glass" terminals or _video terminals_. The VT100 became a popular model in the the 70s. These video terminals almost look like personal computers, but they remain as "dumb" as the earlier models that look like mechanical typewriters (teletype, or TTY for short). They print everything they receive on a serial input line (basically the system that baudot invented) and, if you press a key, they send it over a serial output line. You can connect two of these terminals together to have a simple chat system, or you can connect them to a computer to feed keystrokes as input into a program and visualise the program's output.

- [The Unix revolution – computing in the 70s](https://youtu.be/-rPPqm44xLs)
