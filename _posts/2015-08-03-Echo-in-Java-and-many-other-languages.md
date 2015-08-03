---
layout: post
title: Echo in Java and many other languages
---

`Echo` in Java is simple, isn't it?
But have you ever thought about how many ways there are to implement Echo in Java?
I'll show you some.
And while I'm at it, I'll also show you Echo in bash, C, Clojure, Common Lisp, Go, Groovy, JavaScript, Objective-C, Pascal, Perl, PHP, Prolog, Python, Ruby, Scala and Scheme.
While this is mostly for developers who learn Java or another language, I hope that there will be one or two things that are also surprising for experienced developers in Java as well as other languages.

## What is `Echo`? Requirements?

The `echo` command is a command found in almost all CLIs, like UNIX shells (`sh`, `bash` etc.), Windows `cmd.exe` etc. which will simply print all its arguments, concatenated with space, followed by newline.

Here are a few examples

{% highlight bash linenos %}
christian@armor01:~$ echo

christian@armor01:~$ echo foo bar
foo bar
christian@armor01:~$ echo foo     bar
foo bar
christian@armor01:~$ echo "foo     bar"
foo     bar
christian@armor01:~$ echo 1 2 3 
1 2 3
christian@armor01:~$ echo "Hello, world!"
Hello, world!
{% endhighlight %}

So, arguments are concatenated with a single whitespace.
The last argument will be printed without a following whitespace.
In any case, the last thing printed will be a newline.

# Solutions

## Java 8

Let's look at Java 8 first, because it provides the most convenient solution.
If you're just interested in solving the problem nicely today, this is the preferred solution.

{% highlight java linenos %}
import static java.lang.String.join;
import static java.lang.System.out;

public class Echo {
    public static void main(final String... args) {
        out.println(join(" ", args));
    }
}
{% endhighlight %}

or, if you prefer without `import static`:

{% highlight java linenos %}
public class Echo {
    public static void main(final String... args) {
        System.out.println(String.join(" ", args));
    }
}
{% endhighlight %}

## Iterative Java 1-4

{% highlight java linenos %}
public class Echo {
    public static void main(final String[] args) {
        for (int i = 0; i < args.length; i++) {
            if (i > 0) System.out.print(" ");
            System.out.print(args[i]);
        }
        System.out.println();
    }
}
{% endhighlight %}

## Iterative Java 5-7

{% highlight java liennos %}
import static java.lang.System.out;

public class Echo {
    public static void main(final String... args) {
        for (final String arg : args) {
            if (arg != args[0]) out.print(" ");
            out.print(arg);
        }
        out.println();
    }
}
{% endhighlight %}

## Recursive Java 1-4

{% highlight java linenos %}
public class Echo {
    public static void main(final String[] args) {
        echo(args, 0);
        System.out.println();
    }
    public static void echo(final String[] args, final int num) {
        if (num >= args.length) return;
        if (num > 0) System.out.print(" ");
        System.out.print(args[num]);
        echo(args, num + 1);
    }
}
{% endhighlight %}

## Recursive Java 5-7 using `List`

{% highlight java linenos %}
import static java.lang.System.out;
import static java.util.Arrays.asList;
import java.util.List;

public class Echo {
    public static void main(final String... args) {
        echo(asList(args));
        out.println();
    }
    public static void echo(final List<String> args) {
        switch (args.size()) {
        case 0: return;
        case 1: out.print(args.get(0)); return;
        }
        out.print(args.get(0) + " ");
        echo(args.subList(1, args.size()));
    }
}
{% endhighlight %}

## Recursive Java 5-7 using `ListIterator`

{% highlight java linenos %}
import static java.lang.System.out;
import static java.util.Arrays.asList;
import java.util.ListIterator;

public class Echo {
    public static void main(final String... args) {
        echo(asList(args).listIterator());
        out.println();
    }
    public static void echo(final ListIterator<String> args) {
        if (!args.hasNext()) return;
        if (args.hasPrevious()) out.print(" ");
        out.print(args.next());
        echo(args);
    }
}
{% endhighlight %}

## Iterative Java 1-4 with less I/O overhead, w/o explicit StringBuffer

{% highlight java linenos %}
public class Echo {
    public static void main(final String[] args) {
        String echo = "";
        for (int i = 0; i < args.length; i++) {
            if (i > 0) echo += " ";
            echo += args[i];
        }
        System.out.println(echo);
    }
}
{% endhighlight %}

## Iterative Java 1-4 with less I/O overhead, w/ explicit StringBuffer

{% highlight java linenos %}
public class Echo {
    public static void main(final String[] args) {
        final StringBuffer echo = new StringBuffer();
        for (int i = 0; i < args.length; i++) {
            if (i > 0) echo.append(" ");
            echo.append(args[i]);
        }
        System.out.println(echo);
    }
}
{% endhighlight %}

## Iterative Java 5-7 with less I/O overhead, w/o explicit StringBuilder

{% highlight java liennos %}
import static java.lang.System.out;

public class Echo {
    public static void main(final String... args) {
        String echo = "";
        for (final String arg : args) {
            if (arg != args[0]) echo += " ";
            echo += arg;
        }
        out.println(echo);
    }
}
{% endhighlight %}

## Iterative Java 5-7 with less I/O overhead, w/ explicit StringBuilder

{% highlight java liennos %}
import static java.lang.System.out;

public class Echo {
    public static void main(final String... args) {
        final StringBuilder echo = new StringBuilder();
        for (final String arg : args) {
            if (arg != args[0]) echo.append(" ");
            echo.append(arg);
        }
        out.println(echo);
    }
}
{% endhighlight %}

## Recursive Java 1-4 with less I/O overhead

{% highlight java linenos %}
public class Echo {
    public static void main(final String[] args) {
        System.out.println(echo(args, 0));
    }
    public static String echo(final String[] args, final int num) {
        if (num >= args.length) return "";
        if (num > 0) return " " + args[num] + echo(args, num + 1);
        return args[num] + echo(args, num + 1);
    }
}
{% endhighlight %}

## Recursive Java 5-7 using `List` with less I/O overhead

{% highlight java linenos %}
import static java.lang.System.out;
import static java.util.Arrays.asList;
import java.util.List;

public class Echo {
    public static void main(final String... args) {
        out.println(echo(asList(args)));
    }
    public static String echo(final List<String> args) {
        switch (args.size()) {
        case 0: return "";
        case 1: return args.get(0);
        default: return args.get(0) + " " + echo(args.subList(1, args.size()));
        }
    }
}
{% endhighlight %}

## Recursive Java 5-7 using `ListIterator` with less I/O overhead

{% highlight java linenos %}
import static java.lang.System.out;
import static java.util.Arrays.asList;
import java.util.ListIterator;

public class Echo {
    public static void main(final String... args) {
        out.println(echo(asList(args).listIterator()));
    }
    public static String echo(final ListIterator<String> args) {
        if (!args.hasNext()) return "";
        return args.next() + (args.hasNext() ? " " + echo(args) : "");
    }
}
{% endhighlight %}

# For comparison: Echo in other programming languages

## bash

{% highlight bash linenos %}
#!/bin/bash
printf "%s\n" "$*"
{% endhighlight %}

## C

### Iterative solution in C without counter

{% highlight C linenos %}
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    while (*++argv) {
        printf("%s", *argv);
        if (argv[1]) printf(" ");
    }
    printf("\n");
    return EXIT_SUCCESS;
}
{% endhighlight %}

### Iterative solution in C with counter

{% highlight C linenos %}
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    int i;
    if (argc > 1)
        printf("%s", argv[1]);
    for (i = 2; i < argc; i++)
        printf(" %s", argv[i]);
    printf("\n");
    return EXIT_SUCCESS;
}
{% endhighlight %}

### Recursive solution in C

{% highlight C linenos %}
#include <stdio.h>
#include <stdlib.h>

void echo(char *argv[])
{
    if (!*argv) return;
    printf("%s", *argv);
    if (argv[1]) printf(" ");
    echo(++argv);
}

int main(int argc, char *argv[])
{
    echo(++argv);
    printf("\n");
    return EXIT_SUCCESS;
}
{% endhighlight %}

## Clojure

{% highlight Clojure linenos %}
(println (clojure.string/join " " *command-line-args*))
{% endhighlight %}

As we can see, the solution in Clojure is very simple.

## Common Lisp

{% highlight Lisp linenos %}
(format t "~{~A~^ ~}~%" *args*)
{% endhighlight %}

## Go

{% highlight Go linenos %}
package main
import "fmt"
import "os"
import "strings"
func main() {
    fmt.Println(strings.Join(os.Args[1:], " "))
}
{% endhighlight %}

## Groovy

{% highlight Groovy linenos %}
println args.join(" ")
{% endhighlight %}

## JavaScript

{% highlight JavaScript linenos %}
console.log(process.argv.slice(2).join(" "));
{% endhighlight %}

## Objective-C

Basically the same as C, because no features of Objective-C are required.

{% highlight Objective-C linenos %}
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    while (*++argv) {
        printf("%s", *argv);
        if (argv[1]) printf(" ");
    }
    printf("\n");
    return EXIT_SUCCESS;
}
{% endhighlight %}

## Pascal

{% highlight Pascal linenos %}
PROGRAM Echo;

VAR
    I: Integer;
BEGIN
    for I := 1 to ParamCount - 1 do
        Write(ParamStr(I), ' ');
    if (ParamCount > 0) then
        Write(ParamStr(ParamCount));
    WriteLn();
END.
{% endhighlight %}

## Perl

{% highlight Perl linenos %}
#!/bin/perl

print "@ARGV\n"
{% endhighlight %}

## PHP

{% highlight PHP linenos %}
<?php
echo implode(" ", array_slice($argv, 1)), "\n";
?>
{% endhighlight %}

## Prolog

Behold the beauty of Prolog syntax!

{% highlight Prolog linenos %}
main(Argv) :- echo(Argv).

echo([]) :- nl.
echo([Last]) :- write(Last), echo([]).
echo([H|T]) :- write(H), write(' '), echo(T).
{% endhighlight %}

Prolog has no support for joining strings.
Still, the Prolog listing is very nice, it is beautiful.
See how pattern matching, list operations and recursion make it very simple and straight forward.
For an empty list, it outputs a newline.
For a list with only one element, it outputs that element and recurses to the empty list.
For any other list, it outputs the head element, a space, and recurses to the tail of the list.

## Python

{% highlight Python linenos %}
import sys
print ' '.join(sys.argv[1:])
{% endhighlight %}

## Ruby

{% highlight Ruby linenos %}
printf("%s\n", ARGV.join(' '))
{% endhighlight %}

## Scala

{% highlight Scala linenos %}
println(args.mkString(" "))
{% endhighlight %}

## Scheme

{% highlight Scheme linenos %}
(display (string-join (list-tail (command-line) 1) " "))
(newline)
{% endhighlight %}

# Conclusions

It's obvious that there's more than one way to solve a problem.

## Java 8 is awesome - almost!

What's really nice about Java 8 is that the solution provided by Java 8 is the cleanest, shortest and fastest at the same time.
That's what functional programming really is about.

However, Java 8 didn't manage to get rid of boilerplate.
As many other programming languages - most notably, because being somewhat designed for the JVM, Clojure, Groovy and Scala - demonstrate, this boilerplate isn't necessary at all.

## Recursion in Java sucks

And that's for more than just one reason.
Recursion works best in languages with the following attributes:

* Pattern matching method signatures
* Tail recursion optimization
* List slices
* More than one return value (return list)

As we can see, Java supports neither of these.
And that's what makes writing recursive programs hard.

## Before Java 8, Java provided too many ways how to solve the problem

Although Java 8 added yet one more way how to solve the problem, Java 8 made the situation much better.
Before Java 8, a problem as simple as `echo` already turned into a trade-off between readability and performance.
Clearly, for `echo`, performance wouldn't be an issue anyway.
I'm just saying, as there might be different, bigger problems.


TODO:2015-08-02:christian:3: Solutions in C++, C#, Assembly x86, Assembly x64, Assembly ARM, COBOL, Fortran, D, Scratch, Lua, F#, Erlang, Haskell, Awk, bash, Eiffel, Modula-2, OCaml, Oberon, ML, SmallTalk, TcL, R, Dart, Ada, Logo, Whitespace, Brainfuck, Befunge, LOLCODE, Malbolge, Cluster, Kotlin
