# V8 under the hood - Franziska Hinkelman

Enough to look at SunSpider benchmarks over the past few years to see how far we got with JS engines. We got a peek into how V8 optimizes JS and what kind of optimizations we can apply to application code if we really need them. A great example was how hidden classes allow machine code re-use. Check out `d8` to get more V8 insights about your JS code.

# Lessons learned from building a high traffic web-app  - Patrick Hund

Mobile.de (part of eBay) has moved their homepage serving 300 requests a second to node from Java making coding a lot of fun again in the process. Lessons included the importance of getting to production early, lots of pair programming and keeping it simple. Also a great war story on last minute replacing a misbehaving node module based on load testing. **People, load test!**

# IPFS and the distributed web - David Dias

The web relies on our network backbones and centralized services way too much. Loosing your connection or a DDoS attack can seriously affect us all. It doesn't have to be that way but how do you serve data to everywhere? Just serve it **from everywhere**. Protocol Labs is working on IPFS to upgrade the web using peer to peer technology built on Merkle DAGs (like git), DHTs (like BitTorrent) and much more.
The node community is one of the best places of innovation and we can help kicking off the distributed web revolution, too. Check out IPFS.

# Hunting performance problems - Daniel Khan

When node gets bad press it's usually around performance (netflix vs. express or the walmart memory leak).

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
