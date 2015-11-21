# v8 under the hood - Franziska Hinkelman

## JS engines everywhere
* firefox monkeys, spider, trace, odin-monkey
misoft - chakra
apple - JavascriptCore
oracle nashorn (ex-rhino)
google v8

## and they're fast
some sunspider benchmarks - js engines getting faster
c++ vs js - js not that much slower than optimized c++ code
d8 headless node, v8 debug

## compilers
lexer,parser,translator then compilers
AOT (ahead of time) vs JIT (just in time)
hidden classes - e.g for functions 
same hidden class - same machine code
`d8 --allow-natives-syntax` then #HaveSameMap(a,b)
inline caches - same hidden class, property at same offset

JIT and optimized JIT - spend a bit more time optimizing hot functions
optimized JIT might have to fallback if not specific case
optimized compiler: crankshaft and turbofan
d8 --trace-opt

but monomorphic vs polymorphic is just one optimization
dead-code-optimization

avoid try-catch, for-in, leaking vars, with
premature opt warnings all over the place!
v8 loves classes and monomorphic functions

# lessons learned high traffic express webapp - patrick hund 
mobile.de 300 req/sec
from Java, breaking the monolith
lessons learnt:
* run in prod as early as possible
> gatling, kibana, grafana
* write testable code
* code reviews are silver pairing is gold
> pairing is harder but pays off
> joy of switching to node
* keep it simple

# ipfs - david dias
distributed first - better web applications
node is one of the best places of innvoation
web relies on connection to the backbone
loosing connection, ddos

location addressing vs content addressing
sharing photo - presenter to audience
or video - 200MB - 8 hops, 30 clients, 48GB 
> ? surely networking stack optimizes something?
responsible network usage
more datacenter cables put data closer to user 
internet connection speeds still increasing faster than bndwith requirements
slow connections, TLS handshake timeout
third world countries
offline collaboration 
low bwith, interference, congestion, 
web out of IoT as wasn't designed for offline - service workers?


apps with no origin
leverage network cache
resilient to network partitions/splits

internet - comes from intergalactic network

distributed - merkle - permanent web

merkledag - kinks between blobs, beginning of each blob the hash of the previous one

> aliens sad, want to use net, money grows on trees bitcoin

DHT - holy grail of p2p 

serve data to everywhere?  - serve data from everywhere

# hunting performance problems - daniel khan

bad press - usually around perf
netflix node in flames, walmart memory leak
startup noone knows vs featured on TC

what's node? v8, libuv, std library... 

node memory model - resident set size = heap + stack + code
process.memoryUsage()

gc
scavenge compact - quick, dirty
mark sweep - proper one
node-gc-profiler

when things go wrong
memory leaks

complex leak - closure holding onto whole context, even unused vars
v8-profiler

heap snaphost, chrome devtool compare heap snapshots





















