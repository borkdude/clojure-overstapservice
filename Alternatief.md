# Clojure: a mature alternative to Java

This year we celebrate the 25th anniversary of Java, which currently is the most populair programming language. That is, according to the [TIOBE index](http://www.tiobe.com/index.php/content/paperinfo/tpci/index.html). The become the most popular programming language is a difficult feat, and surely in the beginning lots of people need to be convinced. 

This is the way Java was positioned as a better alternative to the language of choice at the time, C++:

> "We were after the C++ programmers. We managed to drag a lot of them about halfway to Lisp."
>-- Guy Steele, co-auteur van de Java specificatie

And so it came to be that many programmers crossed over to Java, moving halfway the path to enlightenment. This article is about the second part, switching to Lisp - or in any case: 'a' lisp, the pinnacle of productivity, according to Steele.

The lisp we are talking about is Clojure, a programming language for the JVM that was introduced in 2007 by Rich Hickey. At the 2014 edition of Java One, Hickey held a [talk](https://www.youtube.com/watch?v=VSdnJDO-xdg). Het started off with the following [quote](http://thenewstack.io/the-new-stack-makers-adrian-cockcroft-on-sun-netflix-clojure-go-docker-and-more/):

> "A lot of the best programmers and the most productive programmers I know are writing everything in Clojure and swearing by it, and then just producing ridiculously sophisticated things in a very short time. And that programmer productivity matters."
> -- Adrian Cockroft, (former) Netflix

The pitch of Rich was that Clojure enables you to write programs that are better and more flexible, and above all makes you much more productive than using Java.

It could certainly be the reason why a big company like Netflix embraced Clojure as a more mature alternative to Java. Another example is [WalmartLabs](http://blog.cognitect.com/blog/2015/6/30/walmart-runs-clojure-at-scale):

> "Clojure shrinks our code base to about one-fifth the size it would be if we had written in Java" 
> -- Anthony Marcar, architect, WalmartLabs

By now the language has proven itself in the industry. Perhaps is a good reason for you the switch to Clojure? We listed seven advantages of using Clojure for you.


## 1. Clojure is simple

Rich Hickey dedicated a whole year of his life to designing Clojure. He decided that the language needed to be simple. Not that it should be an _easy_ language. True, if Clojure was easy to learn, it might initially win over some Java programmers, but it's not. Indeed, the path to programmer's enlightenment is never easy. Perhaps even painful, with all those parens, and that silly prefix notation.

However, once you've reached Nirvana you will find that Clojure's syntax is actually very simple. One example of its simplicity is writing functions. Clojure encourages you to think pure functions - that is, functions that yield no side effects. Rich Hickey recognized that the feature of _immutability_ will help achieve this. Because if your variables are guaranteed to be immutable, your code will be better to reason about, and a lot more easy to test.


## 2. Clojure is data oriented

Simplicity also stems from the fact that Clojure is not object oriented, it is data oriented. Data is represented by immutable hashmaps instead of classes, like in Java. These data structures are easy to manipulate with functions, but they are mutually independent. Therefore Clojure is philosophically aligned with the following quote from Alan J. Perlis:

> "It is better to have 100 functions operate on one data structure than to have 10 functions operate on 10 data structures."

Object orientation forces you to continually re-invent the wheel, because it requires you to put your data into classes. Instead, with Clojure you can use the same limited set of data structures for all your use cases. Moreover, each new class is a language in itself: you have to learn its methods first in order to access the data structures that are lurking behind. Also, because maps in Clojure are immutable, you don't not have to worry about side effects.

Here's a small example. In [Ring](https://github.com/ring-clojure), a library for writing web applications, a handler is nothing other than a function that expects a hashmap (the request), and returns a hashmap (the response):

```clojure
(defn what-is-my-ip [request]
  {:status 200
   :headers {"Content-Type" "text/plain"}
   :body (:remote-addr request)})
```

The namespace, which is the scope for this handler, is not dependent upon the Ring library. The only thing we need to know is what a request in Ring should look like, i.e. which keys we may expect in the hashmap, and which keys we ought to return in order to return a valid response. All that is happening between request and response is nothing other than computations with normal hashmaps. This surely is a lot more simple that working with `HttpServletRequest` and `HttpServletResponse`.


## 3. Clojure requires less lines of code

When using Clojure you do not have to carry the burden of the immense overhead that occurs with object oriented languages. Such overhead consists of writing the interface and implementation code to 'protect' your data structures. This means that you need less Clojure lines of code than Java liness of code in order to achieve the same result.

Another reason why you need to write less lines of code is because Clojure is a _functional programming language_. Such languages are known to be more expressive than imperative programming languages. For instance, in Clojure you do not have to write loops or iterations for computations on collections. All you have to do is to apply _higher order_ functions, such as  map, filter, and reduce to your collections. Of course, as of version 8, the Java language already incorporated features such as lambda's, and higher order functions via the Streams API. 

Because of the fact that Clojure is a lisp, you can prevent writing boilerplate code by using _macro's_. These are functions that are executed at compile time, and that accept expressions as arguments. Expressions are being represented as data, that you can also transform using Clojure functions. All this amounts to code-generation, so you daon't have to write the boilerplate code yourself.

The website [TodoBackend](http://www.todobackend.com/) contains several different implementations of the Todo-list API. You can also use it to compare languges. When comparing the Clojure implementation with Java 7 and 8, we see the following:

* Clojure: 168 lines of Clojure (including build configuration)
* Java 7 + Spring MVC:  555 lines of XML, 228 Java lines of code, 56 lines of Groovy
* Java 8 + Spring 4 Boot: 200 lines of Java, 37 lines of Groovy

Here we did not include the lines of code from libraries and frameworks being used. If we did, we expect even better results for Clojure.

The advantages of less lines of code are evident: a smaller program contains less bugs, is easier to reason about, and - because of its simplicity - is more robust.


## 4. Clojure supports multiple platforms

The Clojure ecosystem enables you to reuse your knowledge of the language in multiple environments. For server side development you use Clojure on the JVM (or in .NET via ClojureCLR).

For front end development you use ClojureScript, which compiles to JavaScript. The ClojureScript compiler can be used for programs that run in the browser, but also server side - using NodeJS.

Much effort has been put in keeping Clojure implementations mutually consistent. The only diverge if this is being enforced by platform limitations. One example of such a limitation is the absence off multi-threading in the browser.

Many Clojure libraries offer support for both Clojure and ClojureScript. So you can reuse you knowledge on both platforms. Library developers can use _reader conditionals_, that offer the possibility to use the same code file for all target environments.

Application developers can also use reader conditionals to write one piece of code for all platforms. As an example, the following code contains a variable with value `NaN`. On the JVM platform ('clj') it is defined as `Double/NaN`, in JavaScript ('cljs') as `NaN` from the global namespace.


```clojure
(def not-a-number
  #?(:clj Double/NaN
     :cljs    js/NaN
     :default nil))
```


## 5. Clojure is interactief

Clojure is an interactive programming language. Through the REPL (read-eval-print loop) you receive immediate feedback. Whilst developing you can redefine you functions and re-test them, while the application is running - without even starting a new compile cyclus. Tools such as JRebel or Quarkus for hot code reloading is not necessary.

It's even possible to cross the network and hook up to a remote REPL-session in order to inspect the state of the application on a production server.

ClojureScript also supports REPL sessions that enables you to connect to the code running in the browser (or Node), so you can test and modify your program. A tool like [Figwheel](https://github.com/bhauman/lein-figwheel) also enables you to immediately view the result of the modified ClojureScript, without even having too refresh your browser.

All this means that Clojure is very suitable for live coding. With Clojure you can even produce live music. For this check out the library [Overtone](http://overtone.github.io/).


## 6. Clojure is full stack

Hoe je het ook wendt of keert, Java is niet full stack, het is een taal voor de back-end. Wel is geprobeerd om een plekje te veroveren aan de client side, maar wie ontwikkelt er tegenwoordig nog applicaties in AWT, Swing of JavaFX? Evenzo is Clojure begonnen als  alternatief voor back-end, maar met de komst van ClojureScript, en een hoop front-end libraries en tools kun je nu recht spreken van een full stack programmeertaal. Bovendien biedt de stack interessante elementen waarmee full stack ontwikkeling aanzienlijk kan worden verbeterd. 

### React
Met de komst van JavaScript library React is het bouwen van een SPA (single page app) in ClojureScript een fluitje van een cent. Reagent is een ClojureScript library die het schrijven van React-componenten vergemakkelijkt. Door middel van Hiccup-notatie, waarmee je uit een geneste Clojure-datastructuur HTML kan beschrijven, kun je met relatief weinig code een React-component schrijven. 

Dit is een voorbeeld van een component wat het aantal clicks telt op een knop en dit toont in een div:  

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

Met Om, een andere ClojureScript wrapper rondom React, zijn zelfs betere performance benchmarks gehaald dan React zelf. Dit heeft te maken met de efficiënte manier waarop verschillen in state kan worden bepaald in ClojureScript (maar ook in Clojure zelf).

## 7. Clojure voorkomt callback hell

Een van de problemen waar JavaScript-developers mee worstelen is het fenomeen callback hell. Een browser heeft maar één thread. Daarom moeten we werken met geneste callbacks. Als we callbacks  diep nesten, wordt de code steeds slechter leesbaar. 

Clojure geeft hiervoor een oplossing in de vorm van de core.async library, die het werken met asynchrone code vereenvoudigt. Uiteraard werkt ook deze library op zowel server als client. De bouwblokken van core.async zijn channels, buffers en go-blocks. Go-blocks zijn source-transformaties die de illusie geven van synchrone code, maar uiteindelijk geneste callbacks opleveren. 

Een voorbeeld. De volgende code haalt eerst het e-mailadres op van een gebruiker via een REST-API call. Hiervoor gebruiken we de library [http-cljs](https://github.com/r0man/cljs-http), die het maken van Ajax calls combineert met core.async. Als resultaat van een http-call krijgen we een een channel terug, waaruit we het resultaat uit kunnen lezen met de <! (take) operatie. Vervolgens gebruiken we het e-mailadres in een tweede call om de bestellingen van deze gebruiker op te halen. Als uiteindelijk resultaat geven we het aantal bestellingen terug.

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

## Slot

Java bestaat 20 jaar. Inmiddels zijn vele alternatieve programmeertalen verschenen waarmee je applicaties voor de JVM kunt schrijven. Van de Cambrische explosie van nieuwe talen die ontstond, halverwege het vorige decennium, is er een beperkt aantal die de status van volwassenheid hebben bereikt: Groovy, Scala, maar ook Clojure. In dit artikel hebben we een aantal voordelen benoemd waarmee Clojure zich onderscheidt van de rest. Mocht dit je hebben overtuigd, de beste manier om met Clojure aan de slag te gaan is er gewoon mee te beginnen. Om de drempel zo klein mogelijk te maken, kun je gebruik maken van onze [overstapservice](README.md).
