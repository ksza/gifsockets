# gifsockets

"This library is the websockets of the '90s" - Somebody at Hacker News.

This library demoes how to achieve realtime text communication using GIF images as transport.

The interesting part is that you can even use IE6 with this library and get the data in Real Time (TM).

Of course this should have been delivered as an April's Fools joke but sadly we are in mid September here in the northern hemisphere.

## How does it work

The idea is pretty simple. We use Animated Gif images to stream data in real time to the browser. Since a gif image doesn't specify how many frames it has, once the browser opens it, it will keep waiting for new frames until you send the bits indicating that there's no more image to fetch.

Pretty simple uh!

And yes. It works in IE6.

## Usage

The usage now is a bit rudimentary and manual. Feel free to improve it and send a PR.

```bash
$ git clone
$ cd gifsockets
$ lein deps
% lein repl
```

Then perform the following commands on the Clojure REPL.

```clojure
;; in the repl do the following to import the libs
(use 'gifsockets.core)
(use 'gifsockets.server)
;;
;;Then we declare the tcp server
(def server (tcp-server :port 8081 :handler gif-handler))
(start2 server)
;; wait for a browser connection on port 8081
;; go and open http://localhost:8081/ in Safari or IE6
;; In Chrome it works a bit laggy and in Firefox it doesn't work at all
;;
;; Now let's create the gif encoder that we use to write messages to the browser.
(def encoder (create-gif (.getOutputStream client)))
;;
;;Now we are ready to send messages to that browser client
(add-message encoder "Hello gif-sockets")
;; now you should see a GIF image with the new message on it.
(add-message encoder "Zup zup zup")
(add-message encoder "And so forth")
;;
;; Now let's clean up and close the connection
(.finish encoder)
(.close client)
```

As you can see from the code this handles only one connection in what is called  an UPoC (Uber Proof Of Concept).

## Possible uses:

All joking aside I think this is a very low tech way to have say, an website where you could tail logs for instance and you need to do it with browser that have zero websockets support or something like that.

## License

Copyright © 2012 Alvaro Videla

The following classes:

- AnimatedGifEncoder.java
- GifDecoder.java
- LZWEncoder.java
- NeuQuant.java

Where taken from this website: [http://www.fmsware.com/stuff/gif.html](http://www.fmsware.com/stuff/gif.html).

And the server code was taken from here [https://github.com/weavejester/tcp-server](https://github.com/weavejester/tcp-server)

Distributed under the Eclipse Public License, the same as Clojure.