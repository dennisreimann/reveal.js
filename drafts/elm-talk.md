# Elm – Funktionale Frontendentwicklung

## Status Quo – Meanwhile in JavaScript Land

Was ist aktuell der heiße Shit?

- React – das neue jQuery

Warum ist das so? Die Hauptgründe:

- Deklaratives UI: Stateless functions
- Lösung für State Management: Flux (Redux)
- Reduktion der Komplexität

<!-- Was ist der richtig heiße Shit? (abseits des Mainstream) - Reactivity as the only system for responding to interaction - apps that are highly interactive with a multitude of UI events related to data events - real-time events of every kind that enable a highly interactive experience to the user - reactive Programming raises the level of abstraction of your code so you can focus on the interdependence of events that define the business logic - ReactiveX: "Reactive Revolution: ReactiveX is more than an API, it's an idea and a breakthrough in programming." -->

Trend in Richtung Funktionale Programmierung:

- Pure functions -> Native

  - Basiert auf Input-Parametern, kein Outside State
  - Keine Seiteneffekte
  - Reusable, Composable, Testable, Cacheable, Parallelizable

- Higher-order functions -> Native
- Currying -> Ramda.js
- Immutable data structures -> Immutable.js
- Static type checking -> TypeScript / Flow
- No concept of Null -> folktale/data.maybe
- Reactivity -> Rx.js
- Declarative UI -> React.js
- Data flow and state management -> Redux

Kein JS Bashing -> Diagramm Treppe

- In JS erfordert der Funktionale Stil Disziplin
- Es kann trotzdem was schief gehen – kein Zwang oder Kontrolle
- Elm is different in one important way -- because it is a distinct programming language and not simply a framework, Elm can strictly enforce its opinions.

## Was ist Elm?

- Elm is a usability-focused functional programming language that compiles to high-performance JavaScript.
- You can use it with or without JavaScript to build user interfaces on the web.
- primary benefits compared to JavaScript are reliability, maintainability, and programmer delight.
- Static Typing

  - Alles in Elm hat einen Typ (auch `null`)
  - Beispiele "I expected this function to return a number, but it returned nil instead." "Sometimes when I call this function it fails, even though I gave it the exact same input." "Something is changing the value of my variable, and I'm not sure where it's happening."

- Pure Functions

- Immutability

### Warum Elm?

What distinguishes Elm from other compile-to-JS languages--like CoffeeScript, Dart, and ClojureScript--is not only what it adds, but also what it leaves out. This Model-View-Update architecture is not a suggestion in Elm; it's the only way to write applications! This means every library in Elm's package repository is built around this idea, and there's no backlog of alternative rendering strategies to decide between. There's just one extremely well-supported way to do it.

#### Compiler

Developing with Elm often feels like an ongoing conversation with the compiler, gently pushing you toward writing better code. Often, you don't even need to look at your application in the browser while implementing a feature — you can trust the compiler to tell you if you are going in the right direction.

Elm's compiler is widely praised for having the most helpful error messages in the business. "If it compiles, it typically just works," is a common sentiment, even after a serious refactor. This makes large Elm code bases much nicer to maintain than large JS ones.

Strong type checking means that most type errors will be caught when compiling. You will be forced to write safe code (handling all cases of a case, for instance), and won't have to worry about dangerous stuff, like type coercion. It'll make you trust your code more.

- Maintainability
- Reliability

- [Compilers as assistants](http://elm-lang.org/blog/compilers-as-assistants)

#### No runtime exceptions

Production Elm code has a reputation for never throwing runtime exceptions. A far cry from "undefined is not a function."

#### TEA

The idea is to divide your application into three simple pieces:

- Model
  - beinhaltet State
- View
  - Funktion, deren Input das Model ist
- Update
  - Transformiert das Model
  - Einzige Stelle, an der das Model bearbeitet wird

Diagramm und Erklärung:

Elm's runtime connects these pieces to form a reactive application. When a user clicks a button, there is only one permitted outcome: a message is sent to the update function. This yields a nice separation of concerns: all application logic is implemented through the update function, all rendering logic is implemented through the view function, and all application state is stored in the Model.

- inspired Redux
- about communication and data flow
- Only one way to do it
- Immutable state

- "No DOM" means that you never have to manipulate it. All you have to do is provide a function that builds your HTML from your model, as the HTML is regenerated at any interaction. It's faster than you think thanks to a very fast DOM diff implementation.

Skalierung und Modularität:

- Only one way to do it

When you want to separate out some rendering logic, you write another view function and have the main view function call it. If your Model gets uncomfortably large, make a smaller Model and nest it within your main Model. If you want an independent component that manages its own state, give it a Model, view, and update, and have its parent Model, view, and update delegate to the child as appropriate.

- Beispieldiagramm: Nested Components

Reactivity:

- zwingt zu sauberem Code

You write a subscriptions function which looks at the current Model, determines which events your application wants to subscribe to--anything from keyboard presses to data arriving from a websocket--and then translates those events into messages that get sent to your update function.

Fazit:

What distinguishes Elm from other compile-to-JS languages--like CoffeeScript, Dart, and ClojureScript--is not only what it adds, but also what it leaves out. This Model-View-Update architecture is not a suggestion in Elm; it's the only way to write applications! This means every library in Elm's package repository is built around this idea, and there's no backlog of alternative rendering strategies to decide between. There's just one extremely well-supported way to do it.

[Blazing fast HTML – Round Two](http://elm-lang.org/blog/blazing-fast-html-round-two)

#### Semantic versioning

elm-package enforces semantic versioning automatically. If a package author tries to make a breaking API change without bumping the major version number, elm-package will detect this and refuse to publish it. No other known package manager enforces semantic versioning this reliably.

[Elm Package Manager](http://elm-lang.org/blog/announce/package-manager)

If anyone attempts to publish a package with a breaking API change, the package manager rejects it unless the change includes a bump to the major version number. If you're curious what API changes took place between any two versions of any packages, you can run something like elm-package diff NoRedInk/elm-rails 2.0.0 3.0.0 to see what changed between versions 2.0.0 and 3.0.0 of that package.

#### Powerful tooling

##### elm-format

- formats source code according to a community standard
- no more arguing over style conventions: consistent between all programmers and projects
- combined with the fact that all Elm programs follow the same architecture, make it possible to understand a random Elm project much more easily than a random javascript project
- developer experience

##### elm-test

- ships with batteries-included support for both unit testing and fuzz testing
- everything is a pure function, so every test is a unit test !

##### elm-css

- lets you write Elm code that compiles to a .css file
- typesafe and checked: guaranteed that your constants never get out of sync

#### Currying

Da Elm Funktionen standardmäßig curried sind, eigenen sie sich besser für einen funktionalen Stil

„Elm and JavaScript both support either curried or tupled functions. The difference is which they choose as the default:

· In JavaScript, functions are tupled by default. If you'd like them to support partial application, you can first curry them by hand--like we did in our JavaScript viewThumbnail implementation above. · In Elm, functions are curried by default. If you'd like to partially apply them...go right ahead! They're already set up for it. If you'd like a tupled function, write a curried function that accepts a single Tuple as its argument, then destructure that tuple."

#### JavaScript Interoperability

Elm code can also interoperate with JavaScript, meaning you can introduce it in small doses to your JS code base so that you can still leverage the enormous JS ecosystem and avoid reinventing the wheel.

Elm's JavaScript interoperation system is designed to preserve these guarantees. Rather than sharing code with JavaScript--code that might very well crash--an Elm application communicates with JavaScript code the way it would communicate with a server or Web Worker: by sending data back and forth. The only difference is that instead of transmitting this data over the network, or to and from a Web Worker, Elm transmits the data to and from a different language.

Teams talk of "Elm Land" and "JavaScript Land," two parts of the code base that play by different rules. In Elm Land, you can relax, confident Elm's compiler will keep your code from crashing. In JavaScript Land, you're necessarily less confident--in the back of your mind, you always know there could be a missing null check somewhere that escaped test coverage.

By keeping the two separate, and communicating only through data, Elm can provide a top-shelf reactive programming experience while still having access to the vast ecosystem of JavaScript libraries out there.

### Gotchas

- Functional Functional and statically typed
- new concepts
- syntax similar to Haskell
- erschwert Prototyping: Compiler

### Downsides

- Young ecosystem
- SSR missing
- Private Packages

## Zusammenfassung

### Was mag ich an Elm?

#### Developer Experience

- Compiler
- Fehlermeldungen
- SemVer

#### Best practices eingebacken (Rails)

- Immutability
- Testing einfach

#### Community und Approach

- Movin fast
- Focus: simplicity
- [Code is the easy part](https://www.youtube.com/watch?v=DSjbTC-hvqQ)

### Ausblick

Even if elm doesn't become mainstream, your time won't be wasted, having learned to write code in a more functional way, which is essential with reactive front-end development.

Anders herum: "It can't be unseen" – Exposes bad habits in other languages

Selbst wenn man Elm nicht nutzt, gibt es für JS eine Menge daraus zu lernen:

- Currying
- Immutability
- vgl. Redux
- Pure Functions -> Testing

- Elm is designed for front-end web developers. It's aimed at making their lives easier.

- Functional Programming ideas that have been around for over 40 years will be rediscovered to solve our current software complexity problems.

  - Functional Programming will make you a better programmer. The ideas in this article are only the tip of the iceberg. You really need to see them in practice to really appreciate how your programs will shrink in size and grow in stability.

- The state of hardware, e.g. gigabytes of cheap memory and fast processors, will make functional techniques viable.

- CPUs will not get faster but the number of cores will continue to increase.

- Mutable state will be recognized as one of the biggest problems in complex systems.

### Links

- [How to use Elm at work](http://elm-lang.org/blog/how-to-use-elm-at-work)
- [After React: Elm?](https://medium.com/wraptime-io/after-react-elm-152bd6559cb1#.mwi4am3jh)
- [Elm for Beginners](http://courses.knowthen.com/p/elm-for-beginners) - Videokurs
- [Elm in Action](https://www.manning.com/books/elm-in-action) - Buch
- [Elm Guide](https://guide.elm-lang.org/)
- [ElmBridge Tutorial](https://elmbridge.github.io/curriculum/)
- [Elm – Funktionale Frontendentwicklung](https://dennisreimann.de/articles/elm.html)
