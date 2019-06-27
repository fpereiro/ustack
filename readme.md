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

One of the main consequences of `mES5` is that the ustack is compatible with browsers that support ES5 javascript (the first of which were released about a decade ago).

## The libraries

### [dale](https://github.com/fpereiro/dale): loops as functions (150 lines)

Status: stable & complete.

In programming there's a lot of repetition. You want to take lists of things and then do something about them. For example, if you have a list of 30 users you want to create a table with one row per user.

The fundamental construct required for these cases is a loop. The main problem with loops, however, is that [they are statements, not expressions](https://dl.acm.org/citation.cfm?id=359579).

To solve this problem, `dale` offers a set of eight functions to create and execute functional loops. This means that through a function invocation, you can create loops that return object literals. And because loops are wrapped in a function invocation, you can put their results directly in an object literals. In other words, `dale` readily converts loops into data and helps us keep our code as expression-centered as possible.

Besides this, `dale` does two other things: 1) allow to iterate not just arrays, but also objects, using the same functions; 2) fix some quirks related to javascript loops.

### [teishi](https://github.com/fpereiro/teishi): validation (390 lines)

Status: stable & complete.

One of the most powerful characterstics of javascript is the fact that any variable can hold a value of any type - and that value can later be replaced by other value of another type. This freedom enables short and expressive code.

Many people, however, prefer to add type declarations and checks on top of javascript, to catch some errors statically (before the code is executed).

`teishi` proposes an alternative to allow for greater quality and thorough error checking. It borrows the essential concept of *auto-activation* from the Toyota Production System, which states that any unexpected error should stop operations so that it can be fixed and its root cause prevented in the future. On top of that, it considers the function to be the essential unit of a program - and provides tools to functions for validating its input thoroughly. If the input of a function is valid, then its the function's duty to process it correctly - and if it is invalid, its duty is instead to return a properly constructed error.

`teishi` allows not just for type checks, but for equality/inequality, range and regex match. It also allows for constructing validation functions that can be used as checks, too. In this, it proves to be more versatile and flexible than a type system.

`teishi` makes heavy use of object literals - indeed, validation rules are expressed as data.

### [lith](https://github.com/fpereiro/lith): HTML/CSS generation (270 lines)

Status: stable & complete.

`lith` creates the HTML and CSS that draw views in a browser. It receives object literals that represent HTML and CSS and generates corresponding, well, HTML and CSS. It is fully built on top of `dale` and `teishi` and historically it was their starting point, since those two libraries emerged to solve the iteration and validation problems encountered by `lith`.

`lith` allows for creating dynamic applications on the client - rather than serving HTML and CSS statically, `lith` generates it dynamically on the client. While this requires javascript, it still enables performant interfaces for browsers released in the last decade.

### [recalc](https://github.com/fpereiro/recalc): events (200 lines)

Status: stable & complete.

`recalc` is an attempt to find the general case of functional programming. It does it through a pretty heretical approach, which eschews object orientedness, pure functions and return values; it instead employs events, event listeners, and literals and function invocations to represent both. And a global object to store all the data.

`recalc` events require both a `verb` and a `path`, just like REST verbs. This allows for reusing similar logic that affects different parts of the data. Event listeners are matched against events being fired, and are executed sequentially and synchronously. Multiple event listeners can be executed in response to an event, and their sequence of execution can be specified. Asynchronous event handlers can also be executed and the overall sequence of events still preserved, much as if they were written synchronously.

`recalc`'s global state, coupled with listeners, allow a single event to trigger cascades of actions in a predictable and replicable manner, even when these actions generate further event calls and asynchronous function invocations.

While `recalc` can be used in the backend, it finds its core use case in the creation of interfaces for the browser (see `gotoB` below), where every user interaction (or side effect) can potentially trigger multiple actions in the interface and the server.

### [cocholate](https://github.com/fpereiro/cocholate): DOM manipulation & AJAX (280 lines)

Status: mostly stable & mostly complete.

`cocholate` is a set of tools for dealing with the DOM in a somewhat more consistent way. It provides its own minmalistic polyfill.

### [gotoB](https://github.com/fpereiro/gotob): a frontend framework (700 lines)

Status: mostly stable & mostly complete.

The ustack's frontend framework. After raging against frameworks for years, it was natural I'd have to write my own and promote it to unsuspecting devs. You're encouraged to suspect and question. `gotoB` exists because web applications (as opposed to web pages) store state and redraw the page without refreshes. This means that despite my former hopes, plain HTML/CSS generation (even on the browser) is not enough to create web applications.

`gotoB` is built on top of the five libraries we've already covered. It expects views to be functions that return `lith` structures that represent HTML/CSS. Views are event listeners that depend on a part of the global state; and views can trigger (or contain elements that trigger) events themselves. It is, then, essentially a set of functions on top of `lith` and `recalc`.

`gotoB` has another trick up its sleeve: it uses the [Myers diff algorithm](https://en.wikipedia.org/wiki/Diff#Variations) (the same one that you use when running `git diff`) to compare an updated view with its old version, and find out what's the minimum amount of changes that should be applied to it. This allows for decently fast redrawing of views that have few changes, without requiring the view function to add any identifiers or conform to any heuristics. Once `gotoB` computes the diff for the view, it applies those changes to the DOM directly and attempts to recycle DOM elements.

### [cicek](https://github.com/fpereiro/cicek): a web server (830 lines)

Status: unstable & mostly complete. Will be rewritten at some point during 2019.

A fully featured web server, including the kitchen sink: cookies, logging, JSON & multipart body parsing, serving of static assets, cluster. The idea is to find the minimal set of core components that make a web server, and then present them in the most succint, consistent and solid way possible.

`cicek` uses object literals to represent HTTP routes.

### [giz](https://github.com/fpereiro/giz): auth functions (170 lines)

Status: somewhat stable & somewhat complete.

A few auth functions that use the excellent [bcryptjs](https://github.com/dcodeIO/bcrypt.js) to create hashes of passwords and tokens for password recovery. By default, `giz` stores its data in redis, but you can make it store data in other databases as well by replacing a few functions.

### [astack](https://github.com/fpereiro/astack): asynchronicity (390 lines)

Status: unstable. A [new version](https://github.com/altocodenl/acpic/blob/master/lib/astack.js) is currently being tested and will be published in 2019.

`astack` takes object literals to their ultimate frontier: representing code itself. `astack` allows for sequences of functions to be defined and executed as arrays of functions and arguments. It allows for sequential, conditional, iterative and interrupted execution. Both synchronous and asynchronous functions can be converted to `astack`'s conventions and then used on these functions.

`astack` emerges from the need to write asynchronous functions as if they were sequential. In contrast to promises and other async toolsets, asynchronous sequences are represented with expressions, not statements.

While `astack` can be used in the frontend, it finds its core use case in the backend, both within server routes and when testing or provisioning an API (see `kaboot` below).

`astack` is chiefly a solution to the async problem - and yet, quite unexpectedly, allows to view of the general patterns of computation in a new light, by making us write code in terms of literals (or functions that return literals).

### [kaboot](https://github.com/fpereiro/kaboot): test & deploy applications (??? lines)

Status: unstable & incomplete. In the meantime, for HTTP testing, refer to [hitit](https://github.com/fpereiro/hitit) instead. It will be done one day, but not before the other libraries.

Historically, the first library I worked on. Ironic that I'm leaving it for last.

The complexity of devops toolsets is legendary, and in my minority view, absolutely unjustifiable. `kaboot` intends to be a small, minimalistic and flexible toolset that allows you to do what most toolsets really enable: calls to the network and calls to the OS.

`kaboot` provides two main functions: `k.hit`, for performing HTTP requests, and `k.run`, for executing calls to the OS.

It is built on top of `astack`, since all the operations are asynchronous.

## License

The ustack is written by Federico Pereiro (fpereiro@gmail.com) and released into the public domain.
