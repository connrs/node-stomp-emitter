# Stomp Emitter [![Build Status](https://secure.travis-ci.org/connrs/node-stomp-emitter.png?branch=master)](http://travis-ci.org/connrs/node-stomp-emitter)

An EventEmitter like interface for a stomp client

## Getting Started
Install the module with: `npm install stomp-emitter`

## Usage

This module expects you to pass in an existing stomp client. The stomp client should follow the same interface of the `stomp-client` npm module. So either `npm install stomp-client --save` or create a similar interface.

    var client = require('./my-stomp-client-instance.js');
    var StompEmitter = require('stomp-emitter');
    var emitter = new StompEmitter(client, process.pid); // The client is a fully instantiated stomp-client instance and process.pid is used as the UID.

You may use any UID when initialising the emitter as long as you can ensure that it will be unique among the procsses emitting data to the stomp server.

## Emitting global events

To emit an event that may be picked up by any other client, simply emit any event name followed by the raw data. If you wish to emit JSON and have it be processed by another stomp-emitter client instance, provide a third parameter to the emit method with an object key of `content-type: application/json'`:

    emitter.emit('my_event', 'THIS IS MY EVENT MESSAGE'); // 'THIS IS MY EVENT MESSAGE' is received by clients subscribed to the my_event event.

    emitter.emit('my_json_event', JSON.stringify([1, 2, 3]), { 'content-type': 'application/json' }); // [1, 2, 3] is received by clients subscribed to the my_json_event event.

## Emitting local events

Sometimes, you may wish to have events which are only processed within the local process. To restrict events locally, prefix the event name with a tilde character `~` when publishing and subscribing:

    emitter.on('~thisislocal', localHandlerFunc);

    emitter.emit('~thisislocal', 3.14); // This will only be handled by localHandlerFunc
