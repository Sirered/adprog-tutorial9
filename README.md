# Reflection Tutorial 9

## What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

**Unary:** When the client only sends a single request and will only receive a single response

Most interaction between client and server are done in unary, since most of the time a request only needs little information to be sent to the server and most requests will only need a single piece or chunk of data to be returned. Whether it be getting a webpage, making changes to a database, getting some data from a database, most of the time these will only require little data to be sent to the server thus only requiring a single request message to be sent, and they would either return an html, some json data and sometimes just a status code, thus returning a single response

**Server Streaming:** When the client only sends a single request that will open a channel to the server, where the server will use that channel to send a stream of messages.

This is mostly used when the server has to send a large amount of data that can't be served in a single response message like a bunch of images or a video or if there needs to be realtime updates to the client, such as getting live notifications and watching a live stream. This is because Server Streaming makes it so that the server does not need to wait for further requests from the client to continue to serve the client with the large amount and/or real-time information that it requires

**Bi-directional Streaming:** When the client opens a channel between the client and server, where both the client and server will use that channel to send streams of messages to each other.

This is mostly used in realtime collaborative applications, where you need to be able to send all changes during the session with no need to refresh, while also needing to be able to update the client with any and all changes by other users (such as google docs, google slides and canva). Other uses are for realtime interactive communication systems, where you need to be able to send and receive messages to and from the person you are communicating in real time, such as any messaging and/or calling applications. Lastly are any applications or protocols that requires continuous bidirectional exchange, for example bitTorrent likely requires bidirectional streaming, because a client will receive requested video segments while also sending video segments to other peers.

## What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

Implementing gRPC on rust only facilitates method caling between client and server using HTTP, it does not handle security by default. Thus in terms of authentication, you have to make sure you handle authentication separately, whether you implement mutual TLS to secure the connection between client and server, use token-based authentication to authenticate clients or use client certificates to make sure the client that is being connected to is a client that the server can trust. Similarly, for authorisation gRPC doesn't implement such by default, you have to specifically set restrictions on what certain clients can and can't do within the code, otherwise nothing is stopping any client from being able to use all methods that it's respectiver server service offers. Lastly, by default gRPC only sends these messages between client and server via HTTP, in which case if people successfully sniff those HTTP messages, they will be able to get all of the information within these HTTP messages, which may include passwords and general confidential data. To combat this, we would have to encrypt the data before sending it through gRPC and decrypt it on the other end. Overall gRPC is only a too;l to communicate and send requests between different microservices and/or devices regardless of the language used, it does not natively provide much security.

## What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

**Advantages:** As it is part of the tokio package, it has been implemented in a way that works well and intuitively with other modules from the tokio, which from just looking at the import statements from all of our tutorials thus far, is a package we use a lot of. Hence, having the module we use to handle streams of messages also work well with all of our other implementations is very convenient. Secondly, as part of tokio, which is a package that likes to focus on asynchronous execution, the ReceiverStream also handles receiving streams of data asynchronously, which is incredibly useful considering the fact that we would only need to receive streams of data when we want real-time updates or are getting large files, in which case being able to handle other tasks at the same time is a godsend for us. This also allows ReceiverStream to integrate well with the idea of asyncronous functions and futures. 

**Disadvantages:** The main disadvantage of ReceiverStream goes back to the fact that receiving the stream is asynchronous and concurrent, bringing along all of the complexities that come with handling concurrency in programming, such as memory access management, resource management, thread safety etc. Currently we're working with a rather simple example that isn't inherently that complex, but when we want to utilise these streams in projects with a larger scale, we may run into problems that require experience and familiarity of Rust's asynchronous ecosystem to analyse and fix. For example, during this tutorial I was still unsure why we had to use await with a ? when we were getting information from server, but we use await with no question mark when reading console input and sending the message for the ChatService.

## In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

**Here are some steps I woul consider if we were to actually have the PaymentService record and make payments:**

First it would have to authenticate the user to see if they have the authority and the ability to make the payment

I would also add some logic that would actually log/record the payment happening in a shared list of transactions, so instead of get_transaction_history getting 30 made up transactions, it will actually give 

transactions made by making a payment.

I would assume we also have a data structure storing the products or the users that we have, in which case the paymentService will have to update those too.

Lastly, instead of just returning a response with only a success field, we should also have a field that describes why a payment is unsuccessful if success is false.


**For all of these changes we would have to:**

update the protoBuf struct for PaymentResponse

Add a few shared stores of data (here we may use the Singleton pattern to do so)

implement authentication on our services

And lastly would just be adding the logic in the process_payment method to handle all of these new details, along with error handling when something goes wrong, as well as trying to make sure no concurrency issues occur while adding this logic, as process_payment is an asynchronous function that can exhibit issues when handled incorrectly

## 
