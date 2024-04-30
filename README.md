# Reflection Tutorial 9

## What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

**Unary:** When the client only sends a single request and will only receive a single response

Most interaction between client and server are done in unary, since most of the time a request only needs little information to be sent to the server and most requests will only need a single piece or chunk of data to be returned. Whether it be getting a webpage, making changes to a database, getting some data from a database, most of the time these will only require little data to be sent to the server thus only requiring a single request message to be sent, and they would either return an html, some json data and sometimes just a status code, thus returning a single response

**Server Streaming:** When the client only sends a single request that will open a channel to the server, where the server will use that channel to send a stream of messages.

This is mostly used when the server has to send a large amount of data that can't be served in a single response message like a bunch of images or a video or if there needs to be realtime updates to the client, such as getting live notifications and watching a live stream. This is because Server Streaming makes it so that the server does not need to wait for further requests from the client to continue to serve the client with the large amount and/or real-time information that it requires

**Bi-directional Streaming:** When the client opens a channel between the client and server, where both the client and server will use that channel to send streams of messages to each other.

This is mostly used in realtime collaborative applications, where you need to be able to send all changes during the session with no need to refresh, while also needing to be able to update the client with any and all changes by other users (such as google docs, google slides and canva). Other uses are for realtime interactive communication systems, where you need to be able to send and receive messages to and from the person you are communicating in real time, such as any messaging and/or calling applications. Lastly are any applications or protocols that requires continuous bidirectional exchange, for example bitTorrent likely requires bidirectional streaming, because a client will receive requested video segments while also sending video segments to other peers.

