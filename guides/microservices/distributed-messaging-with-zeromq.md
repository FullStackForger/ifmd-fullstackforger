# Distributed messaging with ZeroMQ


## Messaging Patterns
Source [zguide:Messaging Patterns](http://zguide.zeromq.org/page:all#Messaging-Patterns)

* Request-reply - connects a set of clients to a set of services.
This is a remote procedure call and task distribution pattern.
* Pub-sub, which connects a set of publishers to a set of subscribers.
This is a data distribution pattern.
* Pipeline - connects nodes in a fan-out/fan-in pattern that can have multiple steps and loops. This is a parallel task distribution and collection pattern.
* Exclusive pair - which connects two sockets exclusively. This is a pattern for connecting two threads in a process, not to be confused with "normal" pairs of sockets.

## Valid socket combinations

The zmq_socket() man page is fairly clear about the patterns — it's worth reading several times until it starts to make sense. These are the socket combinations that are valid for a connect-bind pair (either side can bind):

* PUB and SUB
* REQ and REP
* REQ and ROUTER (take care, REQ inserts an extra null frame)
* DEALER and REP (take care, REP assumes a null frame)
* DEALER and ROUTER
* DEALER and DEALER
* ROUTER and ROUTER
* PUSH and PULL
* PAIR and PAIR
