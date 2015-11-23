# V8 under the hood - Franziska Hinkelman

Enough to look at SunSpider benchmarks over the past few years to see how far we got with JS engines. We got a peek into how V8 works and what kind of optimizations we can apply to application code if we really need them. A great example was how hidden classes allow machine code re-use. Check out `d8` to get more V8 insights about your JS code.

# Lessons learned from building a high traffic web-app  - Patrick Hund

Mobile.de (part of eBay) has moved their homepage serving 300 requests a second to node from Java making coding fun again in the process. Lessons included the importance of getting to production early, lots of pair programming and keeping it simple. Also a great war story on last minute replacing a misbehaving node module based on load testing. **People, load test!**

# IPFS and the distributed web - David Dias

The web relies on our network backbones and centralized services way too much. Loosing backbone connection or a DDoS attack can seriously affect us all. It doesn't have to be that way. Protocol Labs is working on IPFS to upgrade the web using peer to peer technology built on Merkle DAGs (like git), DHTs (like BitTorrent) and much more.
The node community is one of the best places for innovation and we might be able to help with the distributed web revolution, too. Check out IPFS.

# Hunting performance problems - Daniel Khan

When node gets bad press it's usually around performance (netflix vs. express or the walmart memory leak). To tackle problems like this we first need to understand what node is, the memory model, how garbage collection works and so on. Heap snapshots are great at tracking down memory leaks. CPU profiles and generating flamegraphs can help understanding CPU related issues. v8-profiler and Chrome Dev Tools make it really easy to do the tasks above.

# the magic dump - Luca Maraschi

Luca told us a story of a production outage and tracking down the root cause. Lots of good examples on how to debug issues by taking a core dump and analyzing with dtrace and mdb on SmartOS. In the end he found the typo causing a memory leak :)

# Middleware evolution - Eugene Pshenichniy

Eugenes' slides had really cool realtime polls built-in giving input from the audience. Based on these people seem to like restify for building APIs, agree that generators are not really intuitive but prefer promises and like async-await, too

# robots - Julian Cheal

Julien demoed all sorts of hardware from RGB LED strips to dancing Parrot AR drones. I counted most laughs per talk here. I guess he could easily be a standup comedian. Libraries to check out include cylons.js johnnyfive or gobot (for go).

# Becoming a better node dev - Igor Soarez

You might started learning node for just fun or out of necessity but to keep up your motivation it's great to bet on the long run and enjoy taking it further.

Igor mentioning the right mentality to keep things simple (YAGNI, no IDEs, no boilerplate, no code generation) reminded me of a great talk from Rich Hickey: Simple made easy.

Also node has the perfect tools for 'turbo mode' learning by keeping your feedback keep really short. Anyone running tests with nodemon and growl? :)

An absolute necessity of course is having great JS skills but it's also great idea to become a T-shaped generalizing specialist and know things like frontend and a bit of ops.

We can map certain node skills to levels of capable, efficient and pro developer. An interesting idea here was README-driven development at a quite high level of maturity. Essentially breaking a problem down to small node modules in advance then writing READMEs, then tests, then code.

//TODO Encouraging to publish your code early. Great community, calling mentors, too. teaching, pairing rocks

# Greenkeeper demo - Stephan Bönnemann

Then we saw a short demo of greenkeeper. This tools sends you PRs when your  dependencies release a new version. The PR could then trigger a CI run thus also allowing to automatically detect non-semver-respecting dependencies breaking our own code.

//TODO NODECONFBCN free for a month for private repos - ask Stephan if it's public?

# Rapid idea devalidation - David Gruebl

We all know the feeling when a friend approaches with that amazing idea. Good to remember that a startup is basically just a bet on an idea, validate and fail early if need be.

# Coding education should be free - Michelle and Claire

Community is everything. Online resources are cool but pretty hard to stay motivated for the whole length of a course. Community learning is so much better.

//TODO We have the odd situation when learning to code is expensive and at the same time there's a huge skill shortage. Also more diversity is great for the industry.

//TODO Nodeschool events are ...
Founders and coders is an 8 week coding course with volunteers. Guardian article comments... Free doesn't mean no quality though. And of course you can't learn everything in that timerframe but it can be a great start.
mentor, start new event, contribute, be cool

# Async microservices with node - Bruno Pedro

The de-facto standard way of 'connecting' microservices is using HTTP. A message broker based approach might make this more decoupled and simpler in some cases.

AMQP (and RabbitMQ) is a great option offering lots of flexibility in the patterns used. To simplify things message consumption can actually happen via web-hooks as well using a RabbitMQ plugin. Bruno recommends amqplib as a great node module. Can't agree more. Also check out rascal.

# networking for node programmers - Aria Stewart

As node developers we have to be aware of the properties and architecture of the networking protocol stack our apps communicate through. After some history Aria covered the basics of the most important network protocols from all layers: Ethernet, ARP, IP, DNS, TCP. These are nothing new but stood the test of time and important to understand them.

On some more recent new IPv6 is finally really starting to happen and we could hear a bit about p2p once again, check out some of the torrent experiments from mafintosh and scuttlebutt from dominictarr.
