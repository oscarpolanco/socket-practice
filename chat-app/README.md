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

# Broadcast and disconnect events

## Broadcast event

When you want to send an event to all connections except the current one you need to use `broadcast`.
`socket.broadcast.emit("eventName", data);`

## Disconnect event

To detect when the connection close we need the build event `disconnect` inside the `connection` event.

```js
io.on("connection", socket => {
  socket.on("disconnect", () => {
    io.emit("eventName", data);
  });
});
```

## List of commit

- [Add a broadcast event for the users that join the chat](https://github.com/oscarpolanco/socket-practice/pull/6/commits/0fb772b1c5f7967a4f32ec1540f4d147caba79a5)
- [Add disconnect event](https://github.com/oscarpolanco/socket-practice/pull/6/commits/34dffadf6f611222180856fee7122ef1d93cfd08)

# Event acknowledge

We send an event and we aren't quite sure that the event was delivered successfully so we send some kind of data to acknowledge that we use that event.

To have this functionality we just need to send another parameter on our event(callback) that will run when we receive the event.

```js
// client
// dataSend = "Delivered!"
socket.emit("eventName", data, (dataSend) => {
    console.log("Message delivered", dataSend);
  });
}

// server
io.on("connection", socket => {
  socket.on("eventName", (data,  callback) => {
    io.emit("eventName", data);
    callback("Delivered!");
  });
});
```

## List of commits

- [Add a event acknowledge message](https://github.com/oscarpolanco/socket-practice/pull/8/commits/ee6af889a55d5d7dc38f7c3a381e0c60571e7cff)
- [Use event acknowledge to filter bad words](https://github.com/oscarpolanco/socket-practice/pull/8/commits/e89e746dd5c6f69da272c3af8475cd35bf4b9a5a)

# Button and form states

## List of commits
- [Add form and button logic when the user send a message](https://github.com/oscarpolanco/socket-practice/pull/9/commits/e6ac8252152da77bde966257a5928d8132fe72ec)
- [Add logic to the setLocation button when is press](https://github.com/oscarpolanco/socket-practice/pull/9/commits/30796725ede529ee67718ee75bb81904c625a77e)

# Render library of the example

We are using `mustache` for this example that will help users to quickly have a dynamic template without a lot of configuration.

To begin with `mustache` we add the `cdn` to the `index.html` file.
`<script src="https://cdnjs.cloudflare.com/ajax/libs/mustache.js/3.0.1/mustache.min.js"></script>`

Create an container for the content that you will render on your html.
`<div id="content-container"></div>`

Then create your template using an `id` to identify it.
```js
<script id="my-template" type="text/html">
  <div>
    <p>text</p>
  </div>
</script>
```

Then on your `js` file; target the elments that you need from the `html` file in this case the `script` tag that you create and the `content-container` then use the `Mustache` object and it render function `render`,
```js
const $container = document.querySelector('#content-container');
const myTemplate = document.querySelector("#my-template").innerHTML;

const html = Mustache.render(myTemplate, {
    data
});
  $container.insertAdjacentHTML('beforeend', html);
```

To have dynamic values we send an object as second parameter on the `render` function and on the `script` tag we need to put double square bracket `{{}}` with the property name inside of it.
```js
// html
<script id="my-template" type="text/html">
  <div>
    <p>{{data}}</p>
  </div>
</script>
```

## List of commits (Rendering the location message)

- [Render the messages using mustache](https://github.com/oscarpolanco/socket-practice/pull/10/commits/4018b9de0729f22cf7bf4afabcf8b05a6330ee63)
- [Add a dynamic template for the locatiion functionallity](https://github.com/oscarpolanco/socket-practice/pull/11/commits/8c8755e11d72c391bd67ddfa856784824a450de6)

## List of commits (Rendering the location message)

- [Separate the location message from the normal message event](https://github.com/oscarpolanco/socket-practice/pull/11/commits/9a34de8b7fbc21e74757b28e40ca621203cb80a6)
- [Add a dynamic template for the locatiion functionallity](https://github.com/oscarpolanco/socket-practice/pull/11/commits/8c8755e11d72c391bd67ddfa856784824a450de6)

# Working with time

We use `moment.js` to format the time.

## List of commits(Message event and template)

- [Add a timestamp to the message template and the message event](https://github.com/oscarpolanco/socket-practice/pull/12/commits/837ae61e124278b7e79e2d2887e6dc05a8ca7894)
- [Use moment to give format to our timestamp](https://github.com/oscarpolanco/socket-practice/pull/12/commits/4bfdce76cf482d99c5c135ee6b6fcd55ecc63278)

## List of commits(Location event and template)

- [Add a timestamp to the location template and event](https://github.com/oscarpolanco/socket-practice/pull/13/commits/5b1405a20acaf1746b76c3ade762a6aef6c963e3)

# Styling chat app

## List of commits
[Add style and update the page content](https://github.com/oscarpolanco/socket-practice/pull/14/commits/532d02ab3af932658f079ebafe89e9648c9610df)

# Join page

At this point you need to fill the homepage form to access the chat room

## List of commits

- [Add a join page and create the chat page with the old homepage content](https://github.com/oscarpolanco/socket-practice/pull/15/commits/765576c6b9b6f44265ba1729229be4a1835fc504)

# Socket.io rooms

We need that the events go to a specific `room` since we add a `join` page where you can enter a specific `room` so `socket.io` allows us to do this with the following methods.

## Parse query parameters

First we `parse` the `query` parameters that we send when the user submit information on the `join` page using the `queryString` library
`const { queryParameter1, queryParameter2 } = Qs.parse(location.search, { ignoreQueryPrefix: true});`

A second object is an options object; in this case, is to ignore the `?` on the string.

## Emit and receive the room to the server

### Emit the event

Then we create a event to send values to the server throw a event.
`socket.emit('join', { queryParameter1, queryParameter2 });`

### Receive the event and join the room

Use the `on` method to catch that event on the server and use the values of the query parameter.
```js
socket.on("join", ({ queryParameter1, queryParameter2 }) => {
  socket.join(queryParameter1);
});
```

`socket.join` allow us to `join` a giving room just specify the name sending a string.

### Emit events to a specific room

To emit an event to a specific `room` you just need to use the `to` method which only needs to specify the name of the `room` in a string.

```js
socket.on("join", ({ username, room}) => {
  socket.join(room);

  socket.broadcast.to(room).emit("message", generateMessage(`${username} has joined!`));
});
```

## List of commits

- [Parse the room and username from the url and create a event to send this values to the server](https://github.com/oscarpolanco/socket-practice/pull/16/commits/523b1f6e5431d0b81c1cad63c2428548855597c1)

- [Add the join event, use socket to join a specific room and send the welcome message to a specific room](https://github.com/oscarpolanco/socket-practice/pull/16/commits/e9a8f3645361d6d6d3b544d7124a4bf607755366)

# Storing user

Create a couple of functions to manage the users in the different rules

- [Add a function to store the users on a specific room](https://github.com/oscarpolanco/socket-practice/pull/17/commits/f63fc093d1a5a022379f839eb67e43b42de89654)

- [Create a remove user function](https://github.com/oscarpolanco/socket-practice/pull/17/commits/376ccb7a241cb23657314123aeb75bdc1fa65db4)

- [Add a getUser function](https://github.com/oscarpolanco/socket-practice/pull/17/commits/8d1bdafe136d45403e7a1d453245b83d77a5fa67)

- [Add getUser and getUserInRoom function](https://github.com/oscarpolanco/socket-practice/pull/17/commits/4f6cdb8add1efde7d629c10c88649d0a4fd034eb)
