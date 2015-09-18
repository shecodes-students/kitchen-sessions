# kitchen-sessions
she.codes kitchen sessions Berlin

Documenting the 2nd generation (Nicole, Kathrin, Judith, Ela)

## Session #1 2015-9-17

We talked about how to pronounce the special characters, some of which have more than one name.
octothroph

We talked about how computing is the convergence of two threads: the need to communicate over long distances on one hand and the  desire to get rid of/accelerate dull calculation tasks. We focused on the history of communication first.

### Long-Distance Communication

We talked about the history of long-distance communication. Starting with smoke-signs, then people on hills with flags (semaphores), then chains of semaphore buildings on mountain tops. Here we introduced the term `relay` to refer to a link in a chain that re-amplifies a signal or message. (the term comes from sports)
Monty Python Semaphore video

We talked about the discovery of electricity (rubbing amber on a cat, electric fish) and electro magnetism, how "electricians" were booked by party organisers to entertain people. We talked about the battle between the Italians Volta and Galvani about the existence of _animal electricity_.

We talked about how Galvani doscovered the battery by accidents by using two wires made of different metals.

We talked about early experiments to use electricity for communication purposes in Europe and that Samual Morse was introduced to this idea on a journey.

We talked about the tragic story of the death of Morse's wife which led to the invention of the `Morse Code` and the first digital network for long-distance communication in the US.
We talked about how electro-magnetic switches were used to relay the digital signals and that those electromagnetic switches are therefor called `relay`; together with a battery, they are the main component of a Morse relay station.
We talked about how batteries and relays made possible the first trans-contiental telegraphy line in the US and the first transatlantic cable.

We talked about how the French post-office employee Baudot improved upon the Morse system with a more convenient input device (5 key piano-like keyboard) which improved usabality by putting less stress on the human sender because the singnal generation is done by a clock-driven device rather than the human operator herself.
We talked about the 5-bit Baudot code and how its design was influenced by ergnomic concerns. The clock speed in a Baudaut system is called `baud rate` and is equivalent to `bits per seconds` (`bps` for short). Until the 90s modem speed was meassured in baud (e.g. "I have a 2400 baud modem')

We talked about the resemblance of the Baudot code table to a paper tape with holes in it and how such paper tapes were indeed used to further improve usability of devices used for sending telegrams ofer the telegraph network.
These new devices resembled mechanic typewriters, but instead od printing characters on paper, they would punch holes (according to the Baudot code) into paper tapes. The resulting tape would then be "read" by a simple machine that  replaces the human at the Baudot-"piano". We talked about how this setup, using tha paper tape as a temporary storage or `Buffer`, allows the operator to type at a higher speed then the machine can send, building up some slack in the tape which allows her to have a zip or coffee or tiny breaks until the sending machine caught up and the paper tape becomes straightend again.
The paper tape in this setup is a `FIFO` (first-in, first-out) memory buffer. (In contrast, with the original Baudot system, the operator was forced to type in the "accords" at the exact pace of the machine)

We talked about how addresses at the start of a telegram message could be used to route a telegram through a network of stations. We gave the example of a message originating in Frankfurt that is transmitted to a station in London, then to the transatlantic cable station in Scotland, then to New York and then to San Francisco. We talked about how this routing could be done manually by humans or even automatically by machines.

We talked about how Telegraphy company became wealthy and powerful and the technology having a social an economic impact (banking/stock market), sort of like an early Internet.

We talked about how different companies and vendors extended the Baudot code (to get rid of figure mode/letter mode?) to 7-bit code. They all did it in different ways, which hurt inter-operatability. To solve this issue, ASCII was introduced. (We found out that is happend in the 60s, so the cain of events probably was a bit different than presented here.)
We talked about how ASCII's design could be free of ergonomic concearns, because of a better de-coupling between the human and the mechanics of signal-generation. Instead it is neatly organised into sections (lower-case letters, upper-case letters, numbers, control-codes)
You can identify the section by the first couple of bits and the rest of the bits tell you the number of the symbol within that section (1 for A etc). This makes it possible to read ASCII without having to memorize the entire code table (as opposed to Morse/Baudot)
Tom Scott Video: How to read ASCII

We talked about how the 7-bit ASCII code was extended with an eighth bit to make room for German Umlauts, French accents, the Copyright symbol etc., and how, again, each vendor introduced their _own_ code. We all remember the messed up Umlauts in emails sent from a Windows computer to a Mac. Lots of  different 8-Bit encodings co-existed. They had names like `latin-1`, `mac-roman`, `iso-whatever`.

We discussed how `Unicode` tries to solves this issue _once and for all_ by simply assigning a number to each character, glyph, symbol any human ever came up with. (many tens of thousands, including musical notes, emojis, dead languages). Unicode explicity _avoids_ to specify a number of bits to transfer those numbers. The problem of creating bit patterns for each `codepoint` (symbol number) is left to other standards.
We talked about how Microsoft uses a 16bit code (sort of learning nothing from history) while UTF-8 is an elegant solutions that, by using 8bit as default, minimises the amount of data for English text and expands the number of bits per character as needed.
Tom Scott's UTF-Video

