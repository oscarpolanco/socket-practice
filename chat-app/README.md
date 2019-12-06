# Creating a chat app project

Here we have the basic `node/express` configuration of our app.

## List of commit:

- [Add package.json, express, public folder and source folder](https://github.com/oscarpolanco/socket-practice/pull/1/commits/399cf54fcd76ba54c37ba5da0952a16a97affd08)
- [Add the script to run the server](https://github.com/oscarpolanco/socket-practice/pull/1/commits/d2d38a40409b87dda39e63846fd8e3efa087e62d)

# WebSocket

Allow us to set communication where clients connect to a server. Here are some of the things that WebSockets allow us:


- Allow full-duplex(bidirectional) communication. The client can initiate communication with the client and vice-versa different that an `HTTP` request that only the client initiates the communication also the WebSocket is present along with as is needed.
- WebSocket is a separate protocol from `HTTP`
- Persistent connection between client and server

## List of commit:

- [Some inside of webSockets](https://github.com/oscarpolanco/socket-practice/pull/2/commits/da52ca7a880f9cd085ae27bd68c85cfd57021477)

# Socket.io

The `socket.io` provide us with all the configuration of the server and client to stablish a `websocket` communication.

## Start using socket.io

Here are the [socket.io](https://socket.io/) documentation.

### Add socket.io on the server

First install `socket.io` using this command:
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

Create a function that listen a event at this case `connection` that will be trigger when we got a new connection.
```js
io.on('connection', () => {
    console.log('New WebSocket connection');
});
```

### Add socket.io on the client

Add the client side of the app. First on your `public` html file add the following script
`<script src="/socket.io/socket.io.js"></script>`

This file will exist because we configure our server with `socket.io` and will allow use all the things of the `socket.io` library.

Create a new folder on your `public` directory that store your `js` files the add a file on this case call `chat.js` to store our `socket.io` function then add a `script` tag to index to use it.
`index.html` => `<script src="/js/chat.js"></script>`
`chat.js` => `io();`

Now you can see the message that you put on the `server` on the console.

## List of commit:
[Add socket.io and communicate the server with the client](https://github.com/oscarpolanco/socket-practice/pull/3/commits/3770c4a1f232d03d3d630fa6eae9294a2a288d55)
