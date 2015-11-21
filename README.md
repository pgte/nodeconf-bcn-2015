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
