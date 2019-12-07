# Creating a chat app project

Here we have the basic `node/express` configuration of our app.

## List of commit:

- [Add package.json, express, public folder, and source folder](https://github.com/oscarpolanco/socket-practice/pull/1/commits/399cf54fcd76ba54c37ba5da0952a16a97affd08)
- [Add the script to run the server](https://github.com/oscarpolanco/socket-practice/pull/1/commits/d2d38a40409b87dda39e63846fd8e3efa087e62d)

# WebSocket

Allow us to set communication where clients connect to a server. Here are some of the things that WebSockets allow us:

- Allow full-duplex(bidirectional) communication. The client can initiate communication with the client and vice-versa different that an `HTTP` request that only the client initiates the communication also the WebSocket is present along with as is needed.
- WebSocket is a separate protocol from `HTTP`
- The persistent connection between client and server

## List of commit:

- [Some inside of webSockets](https://github.com/oscarpolanco/socket-practice/pull/2/commits/da52ca7a880f9cd085ae27bd68c85cfd57021477)

# Socket.io

The `socket.io` provide us with all the configuration of the server and client to stablish a `websocket` communication.

## Start using socket.io

Here are the [socket.io](https://socket.io/) documentation.

### Add socket.io on the server

First, install `socket.io` using this command:
`npm install socket.io`

Import the `http` library
`const http = require('http');`

Wrap the `app` using the `http` library using the `createServer` function(`express` does this begin the scene).
`const server = http.createServer(app);`

Change the `listen` function

```js
server.listen(port, () => {
  console.log(`Server is up on port ${port}`);
});
```

Add the `socket.io` library using:
`const socketio = require('socket.io');`

Create an instance of the `socket.io` library. When we do this we get back a function that we need to send the server so we create the `server` constant to past it to this function.
`const io = socketio(server);`

Create a function that listens to an event at this case `connection` that will be trigger when we got a new connection.

```js
io.on("connection", () => {
  console.log("New WebSocket connection");
});
```

### Add socket.io on the client

Add the client side of the app. First on your `public` html file add the following script
`<script src="/socket.io/socket.io.js"></script>`

This file will exist because we configure our server with `socket.io` and will allow us all the things of the `socket.io` library.

Create a new folder on your `public` directory that store your `js` files the add a file on this case call `chat.js` to store our `socket.io` function then add a `script` tag to index to use it.
`index.html` => `<script src="/js/chat.js"></script>`
`chat.js` => `io();`

Now you can see the message that you put on the `server` on the console.

## List of commit:

[Add socket.io and communicate the server with the client](https://github.com/oscarpolanco/socket-practice/pull/3/commits/3770c4a1f232d03d3d630fa6eae9294a2a288d55)

# Socket.io event

At this moment we only communicate with the client and server using an event called `connection`. Now we want to use events to pass values between the client and the server.

## Get socket information

The first thing to do is update our socket callback to receive an object that will have information on a connection like this.

```js
io.on("connection", socket => {
  console.log("New WebSocket connection");
});
```

## Send information

We send events between the server and the client. To do this we use the `emit` function that will at least have one parameter that is the `event` name.
`socket.emit('eventName', data);`

## Receive information

To recive the event we use the `on` function that recive an event and a function.
`socket.on('eventName', (data) => {});`

The event name needs to be the same on the `server` and the client.

## Update all the current connections

To `emit` a event to all the connections instead of just one we use the `io.emit`.
`io.emit('eventName', data);`

## Client socket

On the client, we need to store the return value from the `io` function to use it.
`const socket = io();`

## Note

- The `connection` socket will run one time for each client so if we got 5 clients it will run 5 times.
- You can create your custom event.

## List of commits

- [Add a custom event from the server to the client, to begin with, a counting example](https://github.com/oscarpolanco/socket-practice/pull/4/commits/a0c47004f26b1b1dfbdcafe89036170a3b2d4671)
- [Send data from the client to the server](https://github.com/oscarpolanco/socket-practice/pull/4/commits/e866f56bb8573bffef5a87496dc0d612a26c1eb7)
- [Emit an event from the client, catch the event from the server and update value from the server](https://github.com/oscarpolanco/socket-practice/pull/4/commits/6c722ab1894122169a087bd7c7ffcd49c706f4c3)
- [Emit the count event to all connections](https://github.com/oscarpolanco/socket-practice/pull/4/commits/0d21d6bbba2eaad968f6136d345c5be4fb379b85)
