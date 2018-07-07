---
layout: post
title: "Moving forward with WebSockets"
redirect_from:
  - /moving-forward-with-websockets
  - /moving-forward-with-websockets/
  - /blog/2012/12/moving-forward-with-websockets
  - /blog/2012/12/moving-forward-with-websockets/
---

Two years ago I created [LibWebSocket][libwebsocket] - gem designed to abstract complicated WebSocket API and make it easy to use. During this time a lot things happened - several big gems started using LibWebSocket (like Selenium-Webdriver and Pusher), and it was downloaded nearly 1,5 million times. A lot changed in WebSocket world itself - couple drafts passed, specification was [standardized][standard] and most browsers implemented [native support][support] for WS protocol.

But time passes and old solutions needs updates. Unfortunately, because big changes in specification it’s impossible to keep LibWebSocket up to date - mostly because of new features that are incompatible with some design decisions made during creation of LibWebSocket. Because of that I decided that best option is to rewrite it from scratch, with new standards in mind. As a result I would like to announce new gem: [WebSocket-Ruby][websocket-ruby].

Most of you will ask “why we need another WebSocket implementation?”. That is great question, and let me answer it before moving forward. Currently we have multiple servers and maybe one or two clients for WebSocket. Each of them implements standard by itself, which results in multiple bugs and quirks - even most widely used EM-WebSocket have its own collection. In addition to that focusing on both server part and WebSocket API part make development a lot slower - I strongly believe that splitting responsibility to separate gems is best approach(for proof please see Rack and Thin separation).

In order to make sure that this implementation is reliable I created two additional gems - WebSocket [Client][em-client] and [Server][em-server]. At the time of writing this article both of them have full [Autobahn][autobahn] compatibility and best performance from all other Ruby implementations.

My next step is to start making pull requests for other major WebSocket clients and servers implementations - the more of them will use common implementation, the better it will be, and whole community will benefit. So if you are maintainer of such implementation, or you are willing to simply help transferring your favorite one to WebSocket-Ruby then I will gladly help.

[autobahn]: https://crossbar.io/autobahn/
[em-client]: https://github.com/imanel/websocket-eventmachine-client
[em-server]: https://github.com/imanel/websocket-eventmachine-server
[libwebsocket]: https://rubygems.org/gems/libwebsocket
[standard]: https://datatracker.ietf.org/doc/rfc6455/?include_text=1
[support]: https://caniuse.com/websockets
[websocket-ruby]: https://github.com/imanel/websocket-ruby
