# V8 under the hood - Franziska Hinkelman

## JS engines everywhere

* mozilla - Spidermonkey
* microsoft - Chakra
* apple - JavascriptCore
* oracle - Nashorn (ex-Rhino)
* google - V8

## and they're fast

* see sunspider benchmarks - getting ever faster
* JS not that much slower than optimized c++ code (and faster than unoptimized)

## compilers

* lex -> parse -> translate -> compile
* AOT (ahead of time, e.g C++) vs JIT (just in time, JS)

## V8

* V8 uses two compilers: a quick JIT and optimizing JIT compiler
* hot functions are identified then passed to the optimizing compiler
* optimizing compiler spends a bit more but does more optimizations
* V8 optimizing compilers:
    * 2011 CrankShaft
    * 2015 TurboFan - work in progress
* `d8` - debug V8
* `d8 --trace-opt` - tracing optimizations

## optimizations

* V8 creteas hidden classes to re-use already compiled machine
* `d8 --allow-natives-syntax` - `#HaveSameMap(a,b) === true` if same hidden class
* inline caches - for property access: same hidden class, property at same memory offset
* dead code elimination
* V8 loves constructors and monomorphic functions (same number+type arguments)
* avoid try-catch, for-in, leaking vars, with, eval (seriously?)
* optimize with caution, clearer code first

# Lessons learned from building high traffic web-app  - Patrick Hund
* mobile.de 300 req/sec
* moving away from Java, breaking the monolith
* importance load testing, good monitoring (gatling, grafana)
* the joy of working with node for devs
* lessons learnt:
    * run in prod as early as possible
    * write testable code
    * code reviews are silver pairing is gold (harder but pays off)
    * keep it simple

# IPFS - David Dias

* node is one of the best places of innovation - community, etc

## problems

* web relies on connection to the backbone
* loosing connection, ddos
* location addressing (instead of content addressing)
* irresponsible bandwith usage:  sharing 200MB video - 8 hops, 30 clients, 48GB - ? surely networking stack optimizes something
* we're building more datacenter cables put data closer to user
* internet connection speeds still increasing faster than bndwith requirements but for how long?
* on slow connections inaccesible https sites: TLS handshake timeout
* third world countries
* offline collaboration in the same room?

## goals?

* apps with no origin
* leveraging network cache
* resilient to network partitions/splits
* internet - comes from intergalactic network, right?

## solution

distributed|merkle|permanent web

* merkledag - link of blobs, each blob starts with the hash of the previous one

> Money does grow on trees. It's called bitcoin.

* DHT - holy grail of p2p - discovery of peers, content locations

## serve data to everywhere?  - serve data from everywhere

# Hunting Performance Problems - Daniel Khan

* bad press - usually around perf
    * netflix - node in flames
    * walmart - node memory leak
* so what's node? V8, libuv, std library, bindings ...

## node.js memory model
* resident set size = heap + stack + code
* `process.memoryUsage()`

## garbage collection
* scavenge compact - quick, dirty
* mark sweep - proper one
* `node-gc-profiler`

## heap snapshots
* when you get process out of memory - probably a leak
* simple leak - obvious
* complex leak - closure holding onto whole context, even unused vars
* use `v8-profiler` to take a heap snapshot
* use chrome devtool compare heap snapshot

## node.js vs cpu
* main thread -> event loop -> thread pool
* blocking main thread is not fun
* `v8-profiler`, `startProfiling`, `endProfiling`
* chrome devtool - cpu profiles
* generate flamegraphs

## tips
* set `NODE_ENV`, otherwise defaults to development might cause some modules do weird shit
* build your own dashboard using tools above
* do load testing and measure/profile

# the magic dump - Luca Maraschi

* coredumps - process snapshots
* node has a post-mortems wg and a tracing wg
* check out indutny/llnode, node.js lldb debugger plugin
> bryan cantrill - production is war
* story of a production outage and debugging session:
    * elb trashing machine - terminating instances, hard to debug
    * stack: ec2 running nginx, node
    * taking a dump on linux, move it to smartos - to dtrace and mdb
    * restify + bunyan - implements dtrace probes
    * `prstat` for nice process stats
    * `bunyan -p <PID>` - enable logging w/o restart
    * `joyent/nhttpsnoop`
    * node flag  `--abort-on-uncaught-exception`
    * `pmap -x`, `gcore`
    * `mdb findleaks` - if you're lucky
*   * shown lots of other mdb command
    * in the end it was a typo causing self referencing handler

# middleware evolution - Eugene Pshenichniy

* slides had cool builtin realtim polls
* poll: people love promises
* poll: people love restify
* 'middleware' - series of functions connected together like unix pipes
* implementing 'middlewares':
    * callbacks - hell,
    * generators - not intuitive (poll: what?!)
* co - turn generators into promises
* when implemented with async-await
    * return promise
    * only with babel of course
    * poll people like it

# robots - Julian Cheal

* libraries: cylons.js johnnyfive or gobot (for go)
* hello world - blinking led
* devices:
    * RGB LED
    * RGB LED strip
    * arduino
    * digispark
    * raspi
    * photon
    * an iBeacon
    * parrot drone

# Becoming a better node dev - Igor Soarez

## motivation
* short term: necessity, fun
* grow fond of it, bet on the long run

## attitude

## mentality
* simplicty, YAGNI, no IDEs, no boilerplate, no code generation, etc
* reminds me easy vs simple

## learning
* turbo mode: short feedback loop
* invest in tools but don't overoptimize

## great node skills
* basic js
* network protocols
* T-shaped

## levels

* capable: modules, troubelshooting, debugging, no cb hell, use streams
* efficient: advanced flow control, understand event loop, IO throttling, work queues, custom streams
* pro: understand libuv, v8, custom TCP protocols, README driven development, small modules, README-tests-code

## community
* standards, conventions
* contributing to open source
* publish your code early
* teach

## pairing

# Greenkeeper demon - Stephan Bönnemann

* automatic break-detection for non-semver respecting deps`
* NODECONFBCN free for a month for private repos

# rapid idea devalidation - David Gruebl

* we are 'that friend' - to implement that starup idea
* a startup is a bet on an idea, validate ASAP

# coding education should be free - Michelle and Claire

* community is everything
* online resources are cool but community learning better
* skill shortage but learning to code is $$$
* diversity is better for the industry

## nodegirls_LDN

## online resources

* nodeschool, freecodecamp
* hard to stay motivated alone

## founders and coders
* 8 weeks of group learning with volunteer teachers

## critics
* free !== bad
* resentment towards newcomers
* you can't learn everything in a day, but introduction is possible

## mentor, start new event, contribute, be cool

# async microservices with node - Bruno Pedro

## microservices
* organize service around business capabilities
* designed for resilience and decentralized ownership and deployment
* loosely coupled, limited responsibility

* connected through common interface, usually HTTP
* can also use a broker to decouple even more and simplify

## AMQP
* advanced pub-sub, transactional
* publish: fire-and-forget, or confirmed
* configurable routing and subscribe
* can even consume via web-hook (rabbit plugin)

## amqplib

## patterns
work queue, pubsub, webhook, routing, backpressure, RPC

# networking for node programmers - Aria Stewart

* brief history on the begginings of networking

## Ethernet
* 1980s local networks converged to Ethernet
* OSI model, layer 2 ethernet (layer 3 ip)

## ARP
* layer 2-3 link - Ethernet - IP
* who has this ip? then caches
* if not local IP to talk to then gateway

## IP
* address for a single interface
* getting packets across networks, just reading a few bytes of the beginning of packet
* 4 bytes, network part, host id, A, B, C, D, etc class, /16 /24
* e.g. /24 network: 5.6.7 host: .1
* local network - usually not too many routes
* on the internet: broad routes to send traffic to EU, US, etc

## IPv6
* starting to happen
* 128 bit, 64 bit for host and network, each
* routing bit more complex though

## traceroute
* e.g. `traceroute -an 24.75.24.253`
* backbone routers don't always increment hop count
* also don't always advertise who they are

## DNS

## TCP
* connection is defined by 2 IPs, 2 ports
* source port usually chosen randomly
* each conn tracks how much buffer space left to send
* drop packets are rarer these dates, but delayed is common
* TCP connection setup kind of slow

## new constraints: mobile phones
* high bandwidth but delay

## convergence
* IPv6 starting to happen

## unevenly distributed future
* e.g. 2G connection speeds common in India

## network scale limits
* no more ipv4 addresses
* ISP doing NAT, same IP for lots of customers

# network disintermediation
* hybrid p2p - centralized
* webRTC, scuttlebutt, torrent experiments
