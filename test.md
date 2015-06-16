---
layout: default
title: Lesson: Lines and Line Endings
---

# Lesson: Lines and Line Endings

You create an (almost?) empty file named empty.md. You create it the following way:
    $ touch empty.md
Is it really empty? How can you verify whether it is really empty?
How many characters does it contain?
How many lines does it contain?
How can you verify how many characters or lines it contains? (Hint: There’s a UNIX command which can do that for you.)

You create an (almost?) empty file named empty2.md. You create it the following way:
    $ echo >empty2.md
Is it really empty?
How many lines does it contain?
How many characters does it contain?
How can you print which characters it contains? (Hint: There’s a UNIX command which can dump binary files for you.)

You create an (almost?) empty file named empty3.md. You create it the following way:
    $ echo -n >empty3.md
Is it really empty?
How many lines does it contain?
How many characters does it contain?
Which characters does it contain?

You create a file named hello.md. You create it the following way:
    $ echo hello >hello.md
How many lines does it contain?

You create a file named twoHellos.md. You create it the following way:
    $ echo hello >twoHellos.md
    $ echo hello >>twoHellos.md
How many lines does it contain?

You create a file named hello2.md. You create it the following way:
    $ echo -n hello >hello2.md
What does option -n do?
How many lines does it contain?
What is the difference between hello.md and hello2.md?

You open the same file hello.md in two different text editors, both of which also show line numbers. Here’s what these two editors display.

<table class="bordertable">
<caption>File `hello.md` as shown in two different text editors</caption>
<thead><tr><th>Editor 1</th><th>Editor 2</th></tr></thead>
<tbody><tr><td><pre>1 hello</pre></td><td><pre>1 hello
2 </pre></td></tr></tbody>
</table>

Why is the same file shown differently by two text editors?
Within the text editors available to you, identify at least one editor that shows the file similar to editor 1, and one editor that shows the file similar to editor 2.
What would you see if you open the file hello2.md in these two text editors?
Does the display tell something about the end of the file? What does it tell?

Fill in the following table

ASCII
Code
Name
Number
CR




LF




Table: Most relevant line ending characters

Fill in the following table

Language
Escape Sequence for
CR
LF
bash




C




C++




C#




Java




JavaScript




Perl




PHP




Python




Ruby




Table: Escape sequences in popular programming languages


Fill in the following table

Operating System
Line Ending Character Sequence
POSIX (UNIX, Linux, Mac OS)


MS-DOS, Microsoft Windows


Mac OS (before Darwin)


Table: Line Ending Character Sequences on various operating system families
Tasks / Exercises:
How many different line ending characters are there in ASCII?
How many different line ending characters are there in ISO 8859-1 / -15?
How many different line ending characters are there in Unicode (7.0)?

