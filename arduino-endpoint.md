---
title: summary An HTTP-based API endpoint for Arduino-style devices
labels: Arduino,HTTP,API,Microcontroller,Library
---
# Endpoint Arduino Library

The **Endpoint** library is a minimalist library to create an HTTP-based API endpoint on an Arduino device. This allows more complex systems (such as Python, Java, or web browsers) to connect to the device to issue a command or fetch data.

An Arduino application can use this library to receive commands and parameters over the network (represented as a set of bytes), perform an operation, and respond with pass/fail to the caller with optional return values.

## Design

Interacting with the Arduino is performed through a simple HTTP GET operation. URLs are constructed as: `/X/(hh)*` where X is a single alphanumeric character specifying the endpoint, and hh is a 2-character hexadecimal value specifying a set of parameters for the endpoint.

Responses are delivered as one of three options:

* 204 (No Content) -- the operation succeeded, with no further response data
* 200 (OK) -- the body contains N bytes of response data, as a series of bytes represented in hex
* 400 (Bad Request) -- the query was not formed properly

The HTTP connection remains open for successful operations, and is closed/shutdown on a 400 response. The HTTP protocol is observed, so this library should work with any client application/library, but only the barest minimum of the protocol is actually used.

## Code

Located at: https://gstein.googlecode.com/svn/trunk/arduino/endpoint/

## Size

The Endpoint library is a bit larger than 1000 bytes, when compiled for my ATmega328 processor. Another 10k bytes are pulled in for the Ethernet and SPI libraries.

## TODO

* more documentation
* package it as a real library
* submit to Arduino community "contrib" section
* expanded tests/example
