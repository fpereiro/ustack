# The ustack

> "No one sits down to write megabytes of code." --[Chuck Moore](http://www.ultratechnology.com/moore4th.htm)

The ustack is a set of libraries to build web applications. It aims to be fully understandable by those who use it.

## Why

Programming is something magic. The web is something vibrant. Programming web applications should be both magic and vibrant.

Complexity, however, gets in the way. To make a web application, you need to understand a ton of libraries that barely manage to coexist with each other. This is even more the case now than five or ten years ago.

Some time ago, I suspected that these libraries were way more complex than necessary, and that real applications could be created with a set of libraries that was one (or two) orders of magnitude simpler than mainstream libraries.

The ustack was written as a result of the search for very simple, yet fully functional libraries.

Making the libraries as simple as possible makes them easier to be understood. And, what's more valuable, is that simple solutions tend to be more fundamental - in the end, you're not learning the library so much as you're learning about the problem it tries to solve.

A [complete understanding](https://prog21.dadgum.com/129.html) of web applications (taking the browser and node.js as our base) is possible.

I find that when I understand the libraries I use, everything flows naturally. Not only I'm way more productive, I'm also enjoying the process much more. My sincere hope is that through the ustack, you will be more productive and enjoy the programming of web applications.

## How

The ustack has been forged through [radically minimizing the lines of code](http://steve-yegge.blogspot.com/2007/12/codes-worst-enemy.html) of each of its libraries. The entire ustack actually cannot be more than 4096 lines (2048 frontend & 2048 backend).

The way we achieve code shortening is not by [golfing](https://en.wikipedia.org/wiki/Code_golf) or putting tons of statements into one line; rather, it is achieved through understanding the essence of the problems that each librarie solves, and then make the most straightforward solution possible to it.

Not only we minimize the lines of code; we also minimize the number of dependencies of each of the libraries.

A fundamental assumption is that properly written software can be [(almost) finished](http://www.federicopereiro.com/asymptotic-software/) and that once in that state, it can remain useful for years and even decades.

## What

The ustack is composed of ten libraries. All of them are consistent in style and they build on each other:

- Foundation: `dale` and `teishi` are the foundation for all the other libraries.
- Frontend: `lith`, `recalc` and `cocholate` are the foundation for `gotoB`, which is the ustack's frontend framework.
- Backend: `cicek` and `giz` allow to write a web server. `astack` and `kaboot` allow for testing and provisioning it.

By using the ustack, you can create a complete web application. The one exception to ustack's completeness is that it does not provide libraries for interacting with a database, which is something that you need in any real application.

The defining stylistic approach of all the libraries is the use of [dsDSLs](https://www.toptal.com/software/declarative-programming).

## mES5, a javascript subset

The ustack doesn't use post-2009 modern javascript. It takes pride in being uncool - and on being able to run in old browsers without any compilation step.

In particular, the ustack uses a subset of [ES5 javascript](https://en.wikipedia.org/wiki/ECMAScript#5th_Edition), which we name `mES5`:

- No use of object-oriented techniques: no `new`, no `this`, no accessing or modifying prototypes.
- Use of some Douglas Crockford's recommendations: use of strict equality/inequality (`===` & `!==`), no `switch`.
- In the client, we use a few global variables defined at the top of the file to reference our libraries. Other than this, no global variables are used.

## The libraries

### [dale](https://github.com/fpereiro/dale): loops as functions (150 lines)

Status: stable & complete.

In programming there's a lot of repetition. You want to take lists of things and then do something about them. For example, if you have a list of 30 users you want to create a table with one row per user.

The fundamental construct is a loop. The main problem with loops, however, is that [they are statements, not expressions](https://dl.acm.org/citation.cfm?id=359579).

To solve this problem, `dale` offers a set of eight functions to create and execute functional loops.

### [teishi](https://github.com/fpereiro/teishi): validation (390 lines)

Status: stable & complete.

### [lith](https://github.com/fpereiro/lith): HTML/CSS generation (270 lines)

Status: stable & complete.

### [recalc](https://github.com/fpereiro/recalc): events (200 lines)

Status: stable & complete.

Also backend.

### [cocholate](https://github.com/fpereiro/cocholate): DOM manipulation & AJAX (280 lines)

Status: mostly stable & mostly complete.

### [gotoB](https://github.com/fpereiro/gotob): a frontend framework (700 lines)

Status: mostly stable & mostly complete.

### [cicek](https://github.com/fpereiro/cicek): a web server (830 lines)

Status: unstable & mostly complete.

### [giz](https://github.com/fpereiro/giz): auth functions (170 lines)

Status: somewhat stable & somewhat complete.

### [astack](https://github.com/fpereiro/astack): asynchronicity (390 lines)

Status: unstable.

Also frontend.

### [kaboot](https://github.com/fpereiro/kaboot): test & deploy applications (??? lines)

Status: unstable & incomplete. In the meantime, for HTTP testing, refer to [hitit](https://github.com/fpereiro/hitit) instead.

## License

The ustack is written by Federico Pereiro (fpereiro@gmail.com) and released into the public domain.
