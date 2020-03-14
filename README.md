# GP-CommunityEdition

The GP Blocks programming system forked and improved by the community.

GP is a "general purpose" blocks programming system similar to Scratch, but
with  extended features such as the ability to work with pixel and sound data,
process data, or build games and applications. What's more, GP can turn anything
you build into an application.

## How GP Works

GP runs on a virtual machine (VM) that includes a parser for the textual version of GP,
an intepreter for GP code, a garbage collector to manage memory, and a set of *primitives*
for things like graphics and sound I/O, file system access, support for language structures
such as functions and classes, arithmetic operations, etc. (You can get a full list of
primitives by typing 'help' to the GP command prompt.)

Most of GP is actually written in GP itself. That code is stored in textual form in a set
of .gp files stored in the runtime/lib folder. The textual form of GP maps to blocks,
so the GP library code can be viewed (or even edited) as blocks. Eventually, we hope
that we'll use the GP blocks editor to edit and extend the GP system itself. But when working
with larger amounts of code or making global changes to the entire library, its often more
practical to work with the textual form.

When GP is started, it reads all of the .gp files in the runtime/lib folder. It then reads
and executes the code in runtime/startup.gp, if that file exists. Finally, it looks for a
function named "startup" and runs it if it exists. The startup function is usually
defined in runtime/startup.gp.

## Using GP Read-Eval-Print Loop (REPL)

GP can also be run in interactive mode by typing:

```
gp-win -
```

When started in interactive mode, GP doesn't read runtime/startup.gp or run the "startup" function.
Thus, instead of opening GP's graphical editor, you'll see a GP welcome message and command prompt:

```
Loaded 127 library files from runtime/lib
Welcome to GP!
gp>
```

You can run commands such as:

```
exit - quit GP
help - print a list of primitive operations built into GP
print (3 + 4) - print the result of an expression
inspect (list 1 2 3) - inspect an object
go - restart the user interface
```

## Wiki

The Wiki on this repository will eventually include articles explaining how GP works,
how it loads the entire system from "runtime/lib" when it starts, and how to make changes.

## User Forum

Ask questions and share what you've created with others on the "GP Mods" section of the
GP Forum (http://gpblocks.org/forum). Unfortunately, due to a constant barrage of spammers
creating accounts, we had to disable automatic account creation; just drop an email to
"gpteam" at gpblocks.org.

## Discord
On discord you can join us to talk about GP, suggest features, ask for help and more: https://discord.gg/vUprKx4 

## License

All source code in this repository is under the Mozilla Public License 2.0.
You are allowed (and encouraged!) to fork and modify it, but you must share your
changes

