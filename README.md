Pure Python OPC UA / IEC 62541 Client and Server Python 2, 3 and pypy.
http://freeopcua.github.io/, https://github.com/FreeOpcUa/python-opcua

[![Build Status](https://travis-ci.org/FreeOpcUa/python-opcua.svg?branch=master)](https://travis-ci.org/FreeOpcUa/python-opcua)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/FreeOpcUa/python-opcua/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/FreeOpcUa/python-opcua/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/FreeOpcUa/python-opcua/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/FreeOpcUa/python-opcua/?branch=master)
[![Codacy Badge](https://api.codacy.com/project/badge/grade/f7f9a138ee7c4541b3b794b86e61e929)](https://www.codacy.com/app/olivier-roulet/python-opcua)
[![Code Climate](https://codeclimate.com/github/FreeOpcUa/python-opcua/badges/gpa.svg)](https://codeclimate.com/github/FreeOpcUa/python-opcua)

[![PyPI Package](https://badge.fury.io/py/opcua.svg)](https://badge.fury.io/py/opcua)

**The library is in maintenance model. The plan is now to focus on the asyncio version of this library https://github.com/FreeOpcUa/opcua-asyncio which also has a sync wrapper, with very few changes in API  https://github.com/FreeOpcUa/opcua-asyncio/blob/master/asyncua/sync.py**

**PR to python-opcua are welcome, but if you want to write new fancy features or architectural changes please look at opcua-asyncio**

**Why asyncio? because it makes code easier to read and safer(read: less chances for bugs) and since python is monoprocess it might even be faster. It is also a good timing to remove all Python2 code**


OPC UA binary protocol implementation is quasi complete and has been tested against many different OPC UA stacks. API offers both a low level interface to send and receive all UA defined structures and high level classes allowing to write a server or a client in a few lines. It is easy to mix high level objects and low level UA calls in one application.

Most low level code is autogenerated from xml specification, thus adding missing functionality to client or server is often trivial.

coverage.py reports a test coverage of over 95 % of code, most of non-tested code is autogenerated code that is not used yet.


# Installation

With pip (note: the package was ealier called freeopcua)

    pip install opcua

Ubuntu:

    apt install python-opcua        # Library
    apt install python-opcua-tools  # Command-line tools

Dependencies:
* Python > 3.4: `cryptography`, `dateutil`, `lxml` and `pytz`. 
* Python 2.7 or pypy < 3: you also need to install `enum34`, `trollius` (`asyncio`), and `futures` (`concurrent.futures`),
  with pip for example.


# Documentation

Some documentation is available on [ReadTheDocs](http://python-opcua.readthedocs.org/en/latest/).

A simple GUI client is available: https://github.com/FreeOpcUa/opcua-client-gui

Examples: https://github.com/FreeOpcUa/python-opcua/tree/master/examples

Minimal client example: https://github.com/FreeOpcUa/python-opcua/blob/master/examples/client-minimal.py
Minimal server example: https://github.com/FreeOpcUa/python-opcua/blob/master/examples/server-minimal.py

A set of command line tools also available: https://github.com/FreeOpcUa/python-opcua/tree/master/tools
* `uadiscover `(find_servers, get_endpoints and find_servers_on_network calls)
* `uals `(list children of a node)
* `uahistoryread`
* `uaread `(read attribute of a node)
* `uawrite `(write attribute of a node)
* `uacall `(call method of a node)
* `uasubscribe `(subscribe to a node and print datachange events)
* `uaclient `(connect to server and start python shell)
* `uaserver `(starts a demo OPC UA server)  
  `tools/uaserver --populate --certificate cert.pem --private_key pk.pem`

How to generate certificate: https://github.com/FreeOpcUa/python-opcua/tree/master/examples/generate_certificate.sh

## Client

What works:

* connection to server, opening channel, session
* browsing and reading attributes value
* getting nodes by path and nodeids
* creating subscriptions
* subscribing to items for data change
* subscribing to events
* adding nodes
* method call
* user and password
* history read
* login with certificate
* communication encryption
* removing nodes

Tested servers: freeopcua C++, freeopcua Python, prosys, kepware, beckhoff, winCC, B&R, …

Not implemented yet:

* localized text feature
* XML protocol
* UDP
* maybe automatic reconnection...


## Server

What works:

* creating channel and sessions
* read/set attributes and browse
* getting nodes by path and nodeids
* autogenerate address space from spec
* adding nodes to address space
* datachange events
* events
* methods
* basic user implementation (one existing user called admin, which can be disabled, all others are read only)
* encryption
* certificate handling
* removing nodes
* history support for data change and events

Tested clients: freeopcua C++, freeopcua Python, uaexpert, prosys, quickopc

Not yet implemented:

* more high level solution to create custom structures
* UDP
* session restore
* alarms
* XML protocol
* views
* localized text features
* better security model with users and password


### Running a server on a Raspberry Pi

Setting up the standard address-space from XML is the most time-consuming step of the startup process which may lead to
long startup times on less powerful devices like a Raspberry Pi. By passing a path to a cache-file to the server constructor,
a shelve holding the address space will be created during the first startup. All following startups will make use of the
cache-file which leads to significantly better startup performance (~3.5 vs 125 seconds on a Raspberry Pi Model B).


# Development

Code follows PEP8 apart for line lengths which should be max 120 characters and OPC UA structures that keep camel case
from XML definition.

All protocol code is under opcua directory

- `opcua/ua` contains all UA structures from specification, most are autogenerated
- `opcua/common` contains high level objects and methods used both in server and client
- `opcua/client` contains client specific code
- `opcua/server` contains server specific code
- `opcua/utils` contains some utilities function and classes
- `opcua/tools` contains code for command lines tools
- `schemas` contains the XML and text files from specification and the python scripts used to autogenerate code
- `tests` contains tests
- `docs` contains files to auto generate documentation from doc strings
- `examples` contains many example files
- `tools` contains python scripts that can be used to run command line tools from repository without installing

## Running tests:

```
./run-tests.sh
```

## Coverage

```
coverage run tests.py
coverage html
firefox htmlcov/index.html
```
