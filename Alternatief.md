# Clojure: a mature alternative to Java

This year we celebrate the 25th anniversary of Java, which currently is the most popular programming language. That is, according to the [TIOBE index](http://www.tiobe.com/index.php/content/paperinfo/tpci/index.html). To become the most popular programming language is a difficult feat, and surely in the beginning of Java, lots of people needed to be convinced.

This is the way Java was positioned as a better alternative to the language of choice at the time:

> "We were after the C++ programmers. We managed to drag a lot of them about halfway to Lisp."
>
> -- Guy Steele, co-author of the Java language specification

And so it came to be that many programmers crossed over to Java, moving to about midway on the path to enlightenment. This article is about the subsequent part, switching to Lisp... Or in any case: _a_ lisp, the pinnacle of productivity, according to Steele.

The lisp we are talking about is Clojure, a programming language for the JVM that was introduced in 2007 by Rich Hickey. At the 2014 edition of Java One, Hickey held a [talk](https://www.youtube.com/watch?v=VSdnJDO-xdg). He started off with the following [quote](http://thenewstack.io/the-new-stack-makers-adrian-cockcroft-on-sun-netflix-clojure-go-docker-and-more/):

> "A lot of the best programmers and the most productive programmers I know are writing everything in Clojure and swearing by it, and then just producing ridiculously sophisticated things in a very short time. And that programmer productivity matters."
>
> -- Adrian Cockroft, (former) Netflix

The pitch of Rich was that Clojure enables you to write programs that are better and more flexible, and above all makes you much more productive than using Java.

It could certainly be the reason why a big company like Netflix embraced Clojure as a more mature alternative to Java. Another example is [WalmartLabs](http://blog.cognitect.com/blog/2015/6/30/walmart-runs-clojure-at-scale):

> "Clojure shrinks our code base to about one-fifth the size it would be if we had written in Java"
>
> -- Anthony Marcar, architect, WalmartLabs

By now the language has proven itself in the industry. Perhaps a good reason for you to switch to Clojure? We listed ten advantages of using Clojure for you.


## 1. Clojure is simple

Rich Hickey dedicated a whole year of his life to designing Clojure. He decided that the language needed to be simple. Not that it should be an _easy_ language. True, if Clojure was easy to learn, it might initially win over some Java programmers, but it's not. Indeed, the path to programmer's enlightenment is never easy. Perhaps even painful, with all those parens, and that silly prefix notation.

However, once you've reached Nirvana you will find that Clojure's syntax is actually very simple. One example of its simplicity is writing functions. Clojure encourages you to write pure functions - that is, functions that yield no side effects. Rich Hickey recognized that the feature of _immutability_ will help achieve this. Because if your variables are guaranteed to be immutable, your code will be better to reason about, and a lot easier to test.


## 2. Clojure is data oriented

Simplicity also stems from the fact that Clojure is not object oriented, it is data oriented. Data is represented by immutable hash-maps instead of classes, like in Java. These data structures are easy to manipulate with functions, but they are mutually independent. Therefore Clojure is philosophically aligned with the following quote from Alan J. Perlis:

> "It is better to have 100 functions operate on one data structure than to have 10 functions operate on 10 data structures."

Object orientation forces you to continually re-invent the wheel, because it requires you to put your data into classes. Instead, with Clojure you can use the same limited set of data structures for all your use cases. Moreover, each new class in OO is a language in itself: you have to learn its methods first in order to access the data structures that are lurking behind. Also, because maps in Clojure are immutable, you don't not have to worry about side effects.

Here's a small example. In [Ring](https://github.com/ring-clojure), a library for writing web applications, a handler is nothing other than a function that expects a hashmap (the request), and returns a hashmap (the response):

```clojure
(defn what-is-my-ip [request]
  {:status 200
   :headers {"Content-Type" "text/plain"}
   :body (:remote-addr request)})
```

The namespace, which is the scope for this handler, is not dependent upon the Ring library. The only thing we need to know is what a request in Ring should look like, i.e. which keys we may expect in the hash-map, and which keys we ought to return in order to yield a valid response. All that is happening between the request and the response is nothing else than computations with normal hash-maps. This surely is a lot more simple that working with `HttpServletRequest` and `HttpServletResponse`.


## 3. Clojure requires less lines of code

When using Clojure you do not have to carry the burden of the immense overhead that occurs with object oriented languages. Such overhead consists of writing the interface and implementation code to 'protect' your data structures. This means that you need less Clojure lines of code than Java lines of code, in order to achieve the same result.

Another reason why you need to write less lines of code is because Clojure is a _functional_ programming language. Such languages are known to be more expressive than imperative programming languages. For instance, in Clojure you do not have to write loops or iterations for computations on collections. All you have to do is to apply _higher order_ functions, such as  map, filter, and reduce to your collections. 

Of course, as of version 8, the Java language already incorporated functional features such as lambda's, and higher order functions via the Streams API. But there's more. Because of the fact that Clojure is a lisp, you can prevent writing boilerplate code by using _macro's_. These are functions that are executed at compile time, and that accept expressions as arguments. Expressions are being represented as data, that you can also transform using Clojure functions. All this amounts to code-generation, so you don't have to write the boilerplate code yourself.

The website [TodoBackend](http://www.todobackend.com/) contains several different implementations of the Todo-list API. You can also use it to compare languages. When comparing the Clojure implementation with Java 7 and 8, we see the following:

* Clojure: 168 lines of Clojure (including build configuration)
* Java 7 + Spring MVC:  555 lines of XML, 228 Java lines of code, 56 lines of Groovy
* Java 8 + Spring 4 Boot: 200 lines of Java, 37 lines of Groovy

Here we did not include the lines of code from libraries and frameworks being used. If we did, we expect even better results for Clojure.

The advantages of less lines of code are evident: a smaller program contains less bugs, is easier to reason about, and - because of its simplicity - is more robust.


## 4. Clojure supports multiple platforms

The Clojure ecosystem enables you to reuse your knowledge of the language in multiple environments. For server side development you use Clojure on the JVM (or in .NET via ClojureCLR).

For front end development you use ClojureScript, which compiles to JavaScript. The ClojureScript compiler can be used for programs that run in the browser, but also server side - using NodeJS.

Much effort has been put in keeping Clojure implementations mutually consistent. They only diverge because of platform limitations. One example of such a limitation is the absence of multi-threading in the browser.

Many Clojure libraries offer support for both Clojure and ClojureScript. So you can reuse your knowledge on both platforms. Library developers can use _reader conditionals_, that offer the possibility to use the same code file for all target environments.

Application developers can also use reader conditionals to write one piece of code for all platforms. As an example, the following code contains a variable with value `NaN`. On the JVM platform ('clj') it is defined as `Double/NaN`, in JavaScript ('cljs') as `NaN` from the global namespace.


```clojure
(def not-a-number
  #?(:clj Double/NaN
     :cljs    js/NaN
     :default nil))
```


## 5. Clojure is interactive

Clojure is an interactive programming language. Through the REPL (read-eval-print loop) you receive immediate feedback. Whilst developing you can redefine you functions and re-test them, even while the application is running - without starting a new compile cycle. Tools such as JRebel or Quarkus for hot code reloading is not necessary.

It's even possible to cross the network and hook up to a remote REPL-session in order to inspect the state of the application on a production server.

ClojureScript also supports REPL sessions that enables you to connect to the code running in the browser (or Node), so you can test and modify your program. A tool like [Figwheel](https://github.com/bhauman/lein-figwheel) also enables you to immediately view the result of the modified ClojureScript, without even having to refresh your browser.

All this means that Clojure is very suitable for live coding. With Clojure you can even produce live music. For this check out the library [Overtone](http://overtone.github.io/).


## 6. Clojure is full stack

One way or the other, Java is not full stack. It is a back-end language. It's not that people didn't try to become relevant at the front end as well. But seriously, who is still developing applications using AWT, Swing or JavaFX? Likewise, Clojure was initially conceived as an alternative back end language. With the advent of ClojureScript, and the subsequent release of many front end libraries and tools, you can decidedly consider Clojure a full stack language. It also introduced a lot of interesting elements that considerably improves the experience for full stack developers.


### React

Piggybacking on the success of the JavaScript library React, it is now amazingly easy to build a(n) SPA (Single Page Application) in ClojureScript. [Reagent](http://reagent-project.github.io/) is a ClojureScript library that simplifies writing React components. The [Hiccup syntax](https://github.com/weavejester/hiccup/wiki/Syntax), for describing HTML markup using Clojure data structures, requires very few lines of code.

This is an example of a component that counts the number of clicks on a button, subsequently showing this in a div:

![component](https://raw.githubusercontent.com/finalist/clojure-overstapservice/master/img/component.png)

```clojure
(def count-state (atom 10))

(defn counter []
  [:div
   @count-state
   [:button {:on-click #(swap! count-state inc)}
    "x"]])

(reagent/render-component [counter]
                          (js/document.getElementById "app"))
```

With [Om](https://github.com/omcljs/om), another ClojureScript wrapper for React, it was possible to achieve even [better performance](https://swannodette.github.io/2013/12/17/the-future-of-javascript-mvcs) than React itself. This having to do with better efficiency in comparing different sets of state in ClojureScript (as well as in Clojure itself).


## 7. Clojure prevents callback hell

One of the problems that Javascript developers have to deal with is a phenomenon called 'callback hell'. A browser program only has one thread. That is the reason why we need to work with nested callbacks. As we apply deeper levels of nesting, code readability will be increasingly worse.

The Clojure solution consists of a library, [core.async](https://github.com/clojure/core.async), which - as you might have guessed by now - simplifies working with asynchronous code. It also goes without saying that this library works both on the server as in the client. Its building blocks are channels, buffers and go blocks. Go blocks are source transformations that offer the appearance of synchronous code, but ultimately translate to nested callbacks.

An example. The following code first retrieves a user's email address via a call to a REST API. For this the library [http-cljs](https://github.com/r0man/cljs-http) is being used, that combines the Ajax calls with core.async. As a return value we receive a channel, from which we can read the result using the `<!` (take) operation. Next we use the email address in a second call, to fetch the orders for this user.


```clojure
(go (let [email (:body
                 (<! (http/get
                      (str "/api/users/"
                           "123"
                           "/email"))))
          orders (:body
                  (<! (http/get
                       (str
                        "/api/orders-by-email/"
                        email))))]
      (count orders)))
```

## 8. Clojure has spec

Since Clojure 1.9, released at the end of 2017, Clojure comes with `spec` which is a library to describe the structure of data and functions with support for validation, error reporting, instrumentation and data generation. Developers coming from Java may find Clojure's lack of static typing hard to digest. While spec does not offer compile time guarantees beyond macro syntax checking, it does offer something much more powerful at runtime.

A small example. We can define a spec that expresses an integer which must be even and is greater than `1000`.

``` clojure
(s/def ::big-even (s/and int? even? #(> % 1000)))
```

We can use instrumentation to check if an argument to a function conforms to this specification:

``` clojure
(defn square-big-even
  "returns square of x, which must be a big even integer"
  [x]
  (* x x))

;; define spec for function arguments:
(s/fdef square-big-even :args (s/cat :x ::big-even))

;; instrument
(st/instrument `square-big-even)

(square-big-even 1)
;; => 1 - failed: even? at: [:x] spec: :user/big-even

(square-big-even 2000)
;; => 4000000
```

Error messages from spec can seem a bit cryptic. Libraries like [expound](https://github.com/bhb/expound) can be used to transform spec error data to human-readable text.

The same spec that is used for validation can also be used for data generation and generative testing:

``` clojure
(gen/sample (s/gen ::big-even))
;; => (150874 1402248 1562 165164 1146 2300 15226 1564 1686 3298)
```

## 9. Clojure is a good scripting language

In almost all software projects there will be a need to write shell scripts to automate tasks. Java isn't the most natural fit for a scripting language since it is quite verbose compared to popular choices like Bash and Python. It requires a compilation step and the startup time of the JVM is another hurdle to take. In recent years, several solutions have come up to get a Clojure scripting environment with good startup time:

-  [planck](https://github.com/planck-repl/planck): based on ClojureScript and runs on JavaScriptCode
- [joker](https://github.com/candid82/joker): a Clojure interpreter implemented in Go
- [babashka](https://github.com/borkdude/babashka/): a Clojure interpreter written in Clojure itself, compiled with [GraalVM](https://www.graalvm.org/) `native-image`

This is a Babashka script that prints the canonical paths for all directories in the current directory. As you may notice there is interop with the `java.io.File` class, one of the many classes that are packaged with babashka.

``` clojure
#!/usr/bin/env bb

(require '[clojure.java.io :as io])

(defn canonical-dirs [path]
  (->>
   (io/file path)
   (.listFiles)
   (filter #(.isDirectory %))
   (map #(.getCanonicalPath %))))

(doseq [dir (canonical-dirs ".")]
  (println dir))
```

This script takes about 19ms to execute on my (Michiel's) Macbook Pro 2019.

## 10. Clojure is stable

Like Java, Clojure has a strong focus on API stability. Programs written for Clojure 1.0 in 2009 are still likely to work with Clojure 1.10.1 in 2020. The Clojure community at large adopts the philosophy of Rich Hickey that APIs should grow over time by accretion (adding features), relaxation (requiring less), and bug fixing instead of making breaking changes. Watch the [Spec-ulation](https://www.youtube.com/watch?v=oyLBGkS5ICk) keynote if you want to learn more about this ([transcript](https://github.com/matthiasn/talk-transcripts/blob/master/Hickey_Rich/Spec_ulation.md)).

## Epilogue

Java, the programming language, is 25 years old. During all those years, many alternatives to Java have come and go. But from all the ones that have appeared since the 'Cambrian explosion' of languages during the previous decade, only a few have reached maturity: Groovy, Scala, but also Clojure. In this article we have listed a number of features that positively separates Clojure from the rest. If you are convinced, the best way to learn Clojure is to actually use it.
