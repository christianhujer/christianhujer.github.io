---
layout: post
title: Gradle versus Make
---

Recently I've started using [Gradle](https://gradle.org/) for the first time in my life.
As a frequent user of GNU `make`, I'm curious about its features, and here I'm sharing my experiences.

# Comparison of key roundtrip times

The first thing that I'm looking at when comparing GNU `make` and Gradle is the key roundtrip times.
Performance measurements are done on a AMD Turion(tm) X2 Ultra Dual-Core Mobile ZM-82 with 4 GB RAM running Ubuntu 12.04.5 LTS.
I'm interested in how fast I can develop using this tool.

## Rountrip Time #1 - Invokation with a non-existent task

You mistyped the task - how long does it take for the tool to tell you that you invoked it in the wrong way?

* Roundtrip time of `make foo`: <1secs
* Roundtrip time of `gradle foo`: 11 secs

## Roundtrip Time #2 - List of tasks (first time)

You ask the tool what it can do for you - how long does it take for the tool to answer?

* List of tasks using `make help`: <2 secs
* List of tasks using `./gradlew tasks`: 13 mins 11.286 secs

Gradle was downloading all the required libraries for the project.
I understand that Gradle does so when I want to build something, and this is a nice feature.
What I do not understand is why Gradle is doing so when all I want is just a list of tasks.

Note: For `make help`, I refer to [makehelp](https://github.com/christianhujer/makehelp).

## Roundtrip Time #3

Okay, the first time in Gradle is special, so let's run that again.

* List of tasks using `make help`: <2 secs
* List of tasks using `./gradlew tasks`: 15.612 secs

If I would actually use gradle for the TDD Red-Green-Refactor cycle, I fear it would slow me down significantly.
I'd need either a faster machine or simply have to use something else.

# Summary

<table class="bordertable">
    <caption>Comparison of roundtrip times between Gradle and GNU make</caption>
    <thead>
        <tr>
            <th rowspan="2">Task</th>
            <th colspan="2">Time in seconds for</th>
        </tr>
        <tr>
            <th>Gradle</th>
            <th>GNU make</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Wrong invocation</td>
            <td>11</td>
            <td>1</td>
        </tr>
        <tr>
            <td>list of tasks (first time)</td>
            <td>791</td>
            <td>2</td>
        </tr>
        <tr>
            <td>list of tasks</td>
            <td>15</td>
            <td>2</td>
        </tr>
    </tbody>
</table>

# Conclusion
