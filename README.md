# Golang backend assignment

## Table of contents

* [Assignment](#assignment)
* [Running and building](#running-and-building)
* [Testing](#testing)

## Assignment

Return your answer as a zip file containing all relevant files _with tests_ (including `.git`, so that we can see your commit history).
Do not fork this repo.
If you create your own git repo, please make sure it is private, so that the other candidates cannot access your solution.

Design and implement (with tests) a _message delivery system_ using the [Go](http://golang.org/) programming language,
including both the server and the client.
You are free to use any external libs if needed.
The protocol must be on top of pure TCP, don't use existing application level protocols like HTTP or WebSockets.

We don't value over-engineering.
Provide a readable minimalistic implementation that has understandable split to well-named source files and functions.
Impress us with simplicity, good unit tests and a working solution.

In this simplified scenario the message delivery system includes the following parts:

### Hub

The hub relays incoming message bodies to receivers based on user ID(s) defined in the message.
You don't need to implement authentication, the hub can for example assign arbitrary (unique) user ids to clients when they connect.

- user_id - unsigned 64 bit integer
- Connections to the hub must be made using pure TCP. The protocol doesnt require multiplexing.

### Clients

Clients are users who are connected to the hub. Client may send three types of messages which are described below.

#### Identity message
Clients can send a identity message which the hub will answer with the user_id of the connected user.

![Identity](docs/identity.seq.png)

#### List message
Clients can send a list message which the hub will answer with the list of all connected client user_id:s (excluding the requesting client).

![List](docs/list.seq.png)

#### Relay message
Clients can send a relay messages which body is relayed to receivers marked in the message.
Design the optimal data format for the message delivery system, so that it consumes minimal amount of resources (memory, cpu, etc.).
The message body can be relayed to one or multiple receivers.

- max 255 receivers (user_id:s) per message
- message body - byte array (text, JSON, binary, or anything), max length 1024 kilobytes

![Relay](docs/relay.seq.png)

*Relay example: receivers: 2 and 3, body: foobar*

## Running and building

The project already includes the necessary infrastructure for building and running the hub.
There is a Makefile with targets for building and testing both client and server; you should be able to run the server and multiple clients in order to exercise the functionality outlined here.

## Testing

The project contains integration and benchmark tests for the hub (including both client and server), 
so make sure your implementation is compatible with the tests and that the tests pass without making major changes to them.

